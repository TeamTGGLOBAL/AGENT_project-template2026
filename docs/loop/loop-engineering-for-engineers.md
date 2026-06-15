# ループエンジニアリング ― エンジニアのための実装ガイド

**作成日：2026年6月15日　／　対象：エンジニア・テックリード**

---

## 1\. 何が変わったのか

プロンプトエンジニアリングは「1回の対話」を最適化する。ループエンジニアリングは「多数の対話を回す反復プロセスそのもの」を設計する。プロンプトはその中の一部品に降格する。

「もうコーディングエージェントにプロンプトを打つべきじゃない。エージェントに指示を出すループを設計すべきだ。」 ― Peter Steinberger（OpenClaw 作者、2026/6/7 の投稿が発端）

ポイントは、**Claude Code と Codex がほぼ同じプリミティブに収束した**こと。ループの「形」がツール非依存になったので、どちらに座っていても同じ設計が通る。アディ・オスマニはこれを既存概念の「harness engineering（単一エージェントの実行環境を作る）」の一階上＝**タイマーで回り、ヘルパーを生成し、自分自身に餌を与える harness** と位置づけている。

---

## 2\. ループの構成要素（5＋1）と各ツールの対応

| プリミティブ | ループ内での役割 | Codex app | Claude Code |
| :---- | :---- | :---- | :---- |
| **Automations** | スケジュールで発見＋トリアージ | Automations タブ（project/prompt/cadence/environment を設定、結果は Triage inbox へ）、`/goal` | スケジュールタスク／cron、`/loop`、`/goal`、hooks、GitHub Actions |
| **Worktrees** | 並列作業の隔離 | スレッドごとに組み込みの worktree | `git worktree`、`--worktree`、subagent への `isolation: worktree` |
| **Skills** | プロジェクト知識の codify | Agent Skills（`SKILL.md`、`$name` か暗黙呼び出し） | Agent Skills（`SKILL.md`） |
| **Plugins / Connectors** | 既存ツールへ接続 | Connectors（MCP）＋配布用 plugins | MCP サーバ＋plugins |
| **Sub-agents** | 立案と検証を分離 | `.codex/agents/` に TOML で定義 | `.claude/agents/`、agent teams |
| **State（記憶）** | 完了/残タスクの追跡 | Markdown または Linear（connector経由） | Markdown（`AGENTS.md`、進捗ファイル）または Linear（MCP） |

名前は違うが capability は同じ。MCP コネクタは両者で互換性が高く、片方で書いたものはもう片方でもだいたい動く。

---

## 3\. 各プリミティブの勘所

**Automations ＝ ループの心臓。** これが無いと「1回走らせただけ」で終わる。Codex は Automations タブで project/prompt/cadence/local-or-worktree を設定、ヒットした run は Triage inbox に、空振りは自動アーカイブ。OpenAI 社内では日次の issue triage、CI 失敗の要約、commit briefing、バグ探しなどに使っている。automation からは skill を呼べるので、巨大な指示文をスケジュールに貼り付けず `$skill-name` を叩く形に保つ。Claude Code は `/loop`（一定間隔で再実行）、cron、hooks（ライフサイクルの特定地点でシェル実行）、長時間化したいなら GitHub Actions。

**`/loop` と `/goal` の違いは重要。** `/loop` は cadence で再実行するだけ。`/goal` は**検証可能な停止条件が真になるまで自走**し、各ターン後に別の小さなモデルが完了判定する（＝書いたモデルと採点するモデルを分離）。例：「test/auth のテスト全通過 かつ lint clean」を渡して離席。Codex の `/goal` も同等で、pause/resume/clear を備える。

**Worktrees ＝ 並列のカオス回避。** 複数エージェントが同一ファイルを触ると、人間が同じ行に無連絡コミットするのと同じ衝突が起きる。`git worktree` は履歴を共有しつつ別ブランチ・別作業ディレクトリなので物理的に衝突しない。ただし機械的衝突が消えても**レビュー帯域（人間）が同時実行数の上限**である点は変わらない。

**Skills ＝ intent を外部化して毎回のコストを消す。** エージェントは毎セッション cold start で、intent の穴を自信満々の推測で埋める。`SKILL.md` に規約・ビルド手順・「あの障害があったからこうはしない」を一度書けば、毎 run 読み込まれて知識が複利で効く。skill は**オーサリング形式**、plugin は**配布形式**（複数repo間で共有・バンドルする時に plugin 化）。tight で退屈な description ほど暗黙呼び出しの精度が上がる。

**Sub-agents ＝ maker と checker を分ける。** 書いたモデルは自分の宿題に甘い。別 instructions（時に別モデル）の検証役が、最初のモデルが言いくるめた箇所を捕まえる。典型は explore / implement / verify の3分割。トークンは余計に食うので、second opinion に金を払う価値がある所に絞る。`/goal` の停止判定が別モデルなのは、この maker/checker 分離を停止条件自体に適用したもの。

**State ＝ ループの背骨。** モデルは run 間で全部忘れる。何を試し、何が通り、何が未対応かを Markdown か Linear に置けば、翌朝の run が続きから拾える。「エージェントは忘れるが、リポジトリは忘れない」。

---

## 4\. 最小ループの一例

```
毎朝 automation が repo 上で起動
  → triage skill が「前日のCI失敗・open issues・直近commit」を読み、findings を markdown/Linear に書く
  → 対応すべき finding ごとに isolated worktree を開く
      → sub-agent A が修正を draft
      → sub-agent B が project skills と既存テストに照らして review
  → connector が PR を開き、Linear チケットを更新
  → ループが処理しきれないものは triage inbox に残す
  → state ファイルが「試したこと・通ったこと・残り」を保持し、翌朝の run が継続
```

これを**一度設計するだけ**で、各ステップに人がプロンプトを打たない。Codex でも Claude Code でも部品が同じなので、ほぼ同じループが通る。

---

## 5\. 落とし穴（ループが良くなるほど鋭くなる）

- **検証は依然として自分の責任。** 無人で動くループは無人でミスする。verifier sub-agent を分けるのは「done」に意味を持たせるためだが、それでも「done」は主張であって証明ではない。**自分で動作確認したコードを出す**のが仕事。  
- **理解が腐る（comprehension debt）。** 自分が書いていないコードを速く出すほど、「存在するもの」と「自分が把握しているもの」の差が開く。ループが滑らかなほど速く広がる。read what the loop made。  
- **cognitive surrender に注意。** ループが自走すると、意見を持つのをやめて出力を丸呑みしたくなる。同じ「ループを設計する」行為が、判断を伴えば治療薬、思考回避なら加速剤になる。  
- **トークンコスト。** sub-agent は各自モデル・ツールを回すぶん費用が嵩む。token rich/poor で挙動が大きく変わるので上限と監視を必ず併設。

---

## 6\. ループの本質 ― 組織の責務構造をエージェント化する

maker/checker の分離は最小版にすぎない。本質は、**組織が持っていた「各工程に責務を負う人＋承認ゲート」という構造を、そのままエージェント群に写すこと**だ。code review / design review / launch review は作業ではなく accountability の分担であり、これを役割ごとのエージェント（それぞれ別 instructions、必要なら別モデル・別 reasoning effort）に割り当て、ゲートで受け渡す。`.claude/agents/`・`.codex/agents/` の agent teams と `/goal` の停止判定は、この「役割×ゲート」を実装する道具立てそのもの。

**決定的な線引き：agent 化するのは役割の“労働”であって“説明責任”ではない。** レビューの実行はエージェントに委譲できるが、「このゲートを通した責任は誰か」は人間が握り続ける。停止条件（`/goal` の done 判定）を verifier モデルに任せても、**不可逆なゲート（本番マージ・本番反映・課金・顧客への確定回答）は人間が承認する**設計にする。

### human-in-the-loop → human-on-the-loop

完全な無人化（out of the loop）はできないが、立ち位置は in the loop（毎ターン操作）から on the loop（監督＋ゲート承認）へ移る。設計上の問いは常にこれ：「**どのゲートを人間に残し、どのターンを手放すか**」。可逆・低リスクな操作（下書き生成、定型一次対応、探索的リサーチ）は手放し、不可逆・高リスク（本番反映、課金、顧客への確定回答、ブランドを左右する配信）はゲートとして残す。この線引きを業務単位で決めることが、ループ設計の中身そのもの。

### ゲートの質が moat になる

ループが滑らかになるほどゲートを素通りさせたくなる（cognitive surrender）。だが verifier sub-agent を分離し、人間のゲートを意味あるものに保つことが、無人運転から安全に walk away できる唯一の条件。誰もが下書きを量産する時代には、**信頼できる検証ゲートを持っていること自体が差別化**になる。

---

## 7\. 学習ループ＝企業のIP（private eval / RL / institutional memory）

ここまでは loop を「動かす（operate）」話だった。最後の一段は loop を「**学習する（learn）**」システムとして設計すること。ナデラの整理に沿えば、**loop そのものが企業の新しい IP** であり、複利で成長する hill-climbing machine になる。

**2つの資本。** 人的資本（判断・関係・創造・パターン認識）と、トークン資本（自社が構築・所有する agentic capability）。トークン資本は **swap 可能なベースモデルの上に積む、自社所有の層**であって、ベースモデルそのものではない。だから設計目標は「best model を選ぶ」ことではなく、「**人的資本×トークン資本が複利で増える学習ループを所有する**」こと。タスクは委譲できても learning は委譲しない。

**アーキテクチャ要件 ― ジェネラリストは差し替え、ベテランは残す。** 汎用フロンティアモデルを差し替えても、ループに encode された「社内ベテランの専門知」を失わない構成にする。これが主権のテスト。具体的には：

- **Private eval。** 公開ベンチマークではなく、**自社の事業成果に紐づく eval harness**（例：このループ変更で自社カタログの成約率・受注が改善したか）。eval セットは実際のトレースから組む。  
- **Private RL / 内部トレースからの強化。** ゲートでの承認・却下・人手修正は、そのままラベル付き軌跡（trajectory）。これを学習信号として回し、自社の実際の運用からモデルを強化する。  
- **Institutional memory。** トレースを検索可能なナレッジベース化 → 組織の記憶を retrieval でき、トークン使用も効率化する。

**実装の勘所。** 各ゲート判定が必ずトレース（入力・下書き・人手修正・承認/却下・結果）を吐くようにし、state に永続化する。前章までの state（「何が done か」）を、ここで **学習信号ストア**へ格上げする。スキルが「intent を人手で外部化する」手段なのに対し、学習ループは「intent をトレースから自動で回収する」手段 ― 両者を併用する（型は手で書き、軌跡は自動で収穫する）。

**設計上の含意：** 前章で「ゲートの質が moat」と言ったが、正確には **ゲートを通るたびに学習信号が生まれ、暗黙知が複利で積み上がること**が moat。早く回し始めた者ほど、ベースモデルの優劣に関係なく模倣困難な優位を持つ。

---

## 8\. 着手手順

1. 繰り返し業務を1つ選び、`/loop` か automation で**最小ループを1本**回す。  
2. プロジェクト知識を `SKILL.md` に書き出す（複利で効き始める）。  
3. `/goal` ＋ verifier sub-agent で**停止条件と検証**を仕込み、離席できる状態にする。  
4. 既存ツールを MCP コネクタで接続し、PR作成・チケット更新まで自動化する。  
5. コスト上限・効果測定をセットで運用し、拡大可否を判断する。

---

## 参考文献

- Addy Osmani「Loop Engineering」 [https://addyosmani.com/blog/loop-engineering/](https://addyosmani.com/blog/loop-engineering/)  
- The Neuron「Claude Code's Creators Explain Agent Loops」 [https://www.theneuron.ai/explainer-articles/claude-code-creators-boris-cherny-and-cat-wu-explain-how-to-use-agent-loops/](https://www.theneuron.ai/explainer-articles/claude-code-creators-boris-cherny-and-cat-wu-explain-how-to-use-agent-loops/)  
- Medium（Oleg）「From Prompts to Loops: Codex and Claude」 [https://medium.com/@KilgortTrout/from-prompts-to-loops-a-practical-guide-to-building-agentic-workflows-in-codex-and-claude-0b57234452ed](https://medium.com/@KilgortTrout/from-prompts-to-loops-a-practical-guide-to-building-agentic-workflows-in-codex-and-claude-0b57234452ed)  
- MindStudio「What Is Loop Engineering?」 [https://www.mindstudio.ai/blog/what-is-loop-engineering-ai-coding-agents](https://www.mindstudio.ai/blog/what-is-loop-engineering-ai-coding-agents)

