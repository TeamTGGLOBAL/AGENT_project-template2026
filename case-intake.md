# Case Intake — 新規個別商談Case作成ガイド

> **Claude Codeへ**: 新規Caseを作成するよう指示されたとき、必ずこのファイルを最初に読んでください。
> このファイルに従って質問し、すべての回答が揃ってから一括でCaseを生成してください。

---

## このファイルの使い方

**トリガー**: ユーザーが「Case作って」「新しいCase」「見込み客を登録したい」などと言ったとき。

**実行手順**:
1. 下記の「Batch 1〜3 質問フロー」に従い、AskUserQuestion で3回に分けて合計12問を聞く
2. 全回答が揃ったら「Case番号の決定」に従って番号を決める
3. 「生成するファイル」に従ってCaseディレクトリ一式を作成する
4. 「git セットアップ」に従ってリポジトリ化 → 親リポジトリにサブモジュール追加
5. 完了を報告する

---

## Batch 1 — クライアント基本情報（4問）

Claude Codeへの指示: 以下の4問をAskUserQuestionで一度に投げる。

```
Q1: 会社名（または個人名）
    → Other で自由入力

Q2: 担当者名・役職
    → Other で自由入力（例: 「Somchai Jinda、仕入れ部長」）

Q3: 国・拠点都市
    選択肢:
    - タイ（バンコク）
    - タイ（バンコク以外）
    - 日本在住
    - Other（別の国・都市を入力）

Q4: 業種
    選択肢:
    - レストラン・飲食店
    - ホテル・ホスピタリティ
    - 食品卸・ディストリビューター
    - スーパー・小売
    - 食品メーカー・加工業
    - Other（別の業種を入力）
```

---

## Batch 2 — 接触・商談情報（4問）

```
Q5: 接触方法・連絡先
    → Other で自由入力
    （例: 「名刺交換 + LINE: @somchai_bkk」、「WhatsApp: +66-81-234-5678」）

Q6: 使用言語
    選択肢:
    - 英語
    - タイ語
    - 中国語（繁体字）
    - 中国語（簡体字）
    - 日本語
    - Other

Q7: 関心を示した商品（複数選択可）
    選択肢:
    - ホタテ（活き）
    - ホタテ（3D冷凍貝柱）
    - ホタテ（生玉）
    - いくら（醤油漬け）
    - タラバガニ
    - ズワイガニ
    - Other（別の商品を入力）

Q8: 決裁権・意思決定者
    選択肢:
    - 本人に決裁権あり
    - 上司・会議が決める（→ Otherで意思決定者名を入力）
    - 不明
```

---

## Batch 3 — チャンス評価・ネクストアクション（4問）

```
Q9: 顧客層（Case001の3層モデルに照らして判断）
    選択肢:
    - 第1層（コンテナ単位・北海道直送ルート志向 / Mr. Chen型）
    - 第2層（100kg〜500kg・即納重視 / Mr. Somchai型）
    - 第3層（タイ国内パートナーサプライヤー / 高田氏型）
    - 未判断

Q10: なぜビジネスチャンスだと感じたか
    → Other で自由入力（1〜3行、具体的に）

Q11: 確度
    選択肢:
    - 1（様子見・興味はあるが購買意欲不明）
    - 2（可能性あり・もう少し話したい様子）
    - 3（前向き・価格や商品をもっと知りたい）
    - 4（具体的・見積もりを欲しがっている）
    - 5（即商談・今すぐ動ける）

Q12: 今すぐやること + 期限
    複数選択可:
    - 見積もりを送付する
    - サンプルを手配する
    - LINE / WhatsApp で初回メッセージを送る
    - 次回商談のアポを調整する
    - Other（具体的なアクションを入力）
    ※ 選択後に「期限はいつか」もOtherで聞く
```

---

## Case番号の決定

1. 親プロジェクトの最上位ディレクトリを `ls` して `Case○○○_` で始まるディレクトリを確認する
2. 最大番号を探して +1 する（例: Case001が最大なら → Case002）
3. ディレクトリ名は `Case[番号]_[会社名コード]` とする
   - 会社名コードは英数字で短く（例: `Case002_BKhoonsomchai`）
   - 会社名が長い場合は略称を使う

---

## 生成するファイル一覧

以下のファイルを `template/pattern-C/` をベースに生成する。
PLACEHOLDERはすべてintake回答で置換すること。

```
Case[N]_[会社名コード]/
├── CLAUDE.md           # template/pattern-C/CLAUDE.md から生成
├── MEMORY.md           # template/pattern-C/MEMORY.md から生成
└── docs/
    ├── phase0-intelligence.md   # template/pattern-C/docs/phase0-intelligence.md から生成
    └── followup-plan.md         # template/pattern-C/docs/followup-plan.md から生成
```

---

## PLACEHOLDER → intake回答 対応表

| PLACEHOLDER | 対応するQ | 例 |
|---|---|---|
| `[CASE_NUMBER]` | 自動採番 | `002` |
| `[COMPANY_NAME]` | Q1 | `Khun Somchai Restaurant Group` |
| `[PERSON_NAME]` | Q2（名前部分） | `Somchai Jinda` |
| `[PERSON_ROLE]` | Q2（役職部分） | `仕入れ部長` |
| `[COUNTRY_CITY]` | Q3 | `タイ・バンコク` |
| `[INDUSTRY]` | Q4 | `レストラン・飲食店` |
| `[CONTACT_INFO]` | Q5 | `名刺交換 / LINE: @somchai_bkk` |
| `[LANGUAGE]` | Q6 | `英語・タイ語` |
| `[PRODUCTS]` | Q7 | `ホタテ（活き）、いくら` |
| `[DECISION_AUTHORITY]` | Q8 | `本人に決裁権あり` |
| `[DECISION_MAKER]` | Q8（Other） | `本人（Somchai氏）` |
| `[CUSTOMER_TIER]` | Q9 | `第2層（即納小口）` |
| `[OPPORTUNITY_REASON]` | Q10 | `（自由記述そのまま）` |
| `[CONFIDENCE_SCORE]` | Q11 | `4（見積もりを欲しがっている）` |
| `[IMMEDIATE_ACTIONS]` | Q12 | `見積送付・LINE初回メッセージ` |
| `[ACTION_DEADLINE]` | Q12（期限） | `2026-05-28中` |
| `[THAIFEX_DAY]` | ユーザーの文脈から推定 | `Day 1（5/26）` |

---

## git セットアップ

全ファイル作成後に以下を実行する（Bashツール使用）:

```bash
# 1. Case ディレクトリを git リポジトリ化
cd [親プロジェクトのパス]/[CaseディレクトリのパスのCase部分]/
git init
git add .
git commit -m "Case[N]: フェーズ0開始 — intake情報から初期セットアップ"

# 2. 親リポジトリにサブモジュールとして追加
cd [親プロジェクトのパス]
git submodule add [CaseディレクトリのLocalパス] [CaseディレクトリのLocalパス]
git add .gitmodules [CaseディレクトリのLocalパス]
git commit -m "Case[N]_[会社名コード] をサブモジュールとして追加"
```

> **注意**: `git submodule add` にはURLが必要。ローカルパスをURLとして渡す場合は
> `git submodule add [フルパス] [相対ディレクトリ名]` の形式を使う。

---

## 完了報告の形式

生成完了後、以下の形式でユーザーに報告する:

```
Case[N]_[会社名コード] を作成しました。

作成ファイル:
- CLAUDE.md（ミッション・Go/No Go・戦略）
- MEMORY.md（別PCからの引き継ぎ用）
- docs/phase0-intelligence.md（初期情報）
- docs/followup-plan.md（72時間以内アクション含む）

今すぐやること:
1. [Q12のアクション1] → 期限: [期限]
2. [Q12のアクション2] → 期限: [期限]

確度: [Q11] / 顧客層: [Q9]
```
