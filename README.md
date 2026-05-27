# AGENT_project-template2026

TG GLOBALのAI×営業プロジェクト立ち上げフレームワーク。

## フレームワークの思想

### なぜAI×営業プロジェクトをリポジトリで管理するか

営業活動は「情報」と「判断」の連続だ。AIをうまく使えば、情報の整理・分析・メッセージ作成が劇的に速くなる。しかし、AIに単発の質問をするだけでは文脈が蓄積されない。

リポジトリで管理することで：

- **情報が蓄積される** — 誰がどんなことを言ったか、何を試みたか
- **判断の根拠が残る** — Go/No Goを決めた理由
- **AIが文脈を持てる** — CLAUDE.mdがあれば次の会話でも即座に動ける

### 3つの原則

1. **土俵を変える** — 価格競争に引き込まれる前に、「TGにしか出せないもの」で差別化する
2. **意思決定者へのルートを設計する** — 末端ではなく、決める人に届ける
3. **止まる条件を先に決める** — Go/No Goを定義してから動く

## フェーズ構造

```
フェーズ0：偵察
  → 組織構造・意思決定者・購買プロセス・競合を把握する
  → 情報源を特定し、評価する

フェーズ1：武器を揃える
  → 市場リサーチ・価格インテリジェンス・商品戦略を完成させる
  → 接触シナリオ・ヒアリング設計を準備する

フェーズ2：接触
  → Go/No Goを確認しながらステップを進める

フェーズ3：波及・拡大
  → 最初の実績を起点に、本命ターゲットへ到達する
```

## テンプレートの選び方

| パターン | 向いている状況 | 所要時間 | 場所 |
|---|---|---|---|
| **C：個別商談Case（推奨）** | 展示会・商談で接触した見込み客を1社ずつ管理したい | **10〜15分** | `template/pattern-C/` |
| **A：穴埋め式** | 新市場全体の戦略を立てる。ターゲット・商材が決まっている | 60〜90分 | `template/pattern-A/` |
| **B：説明と空欄式** | 新市場全体の戦略。商材・国・業種が異なる。まず思考を整理したい | 60〜90分 | `template/pattern-B/` |

### Pattern-C を使うとき（最速）

```
"case-intake.md を読んで、新しいCaseを作成してください。"
```

Claude Codeが12問を順に聞き、すべての回答からCaseを自動生成します。

## ファイル構成

```
WORKFLOW.md                         # Case○○○セットアップ手順（ステップ0〜9）
case-intake.md                      # ★ 個別商談Case作成の質問定義 + Claude指示書
template/
├── pattern-C/                      # ★ 個別商談Case専用（10〜15分で生成）
│   ├── CLAUDE.md                   # 1対1商談管理テンプレート
│   ├── MEMORY.md                   # 別PC引き継ぎ用メモリテンプレート
│   └── docs/
│       ├── phase0-intelligence.md  # 接触記録・初期情報
│       └── followup-plan.md        # 72時間以内フォロー計画
├── pattern-A/                      # 穴埋め式（市場戦略Case用）
│   ├── CLAUDE.md
│   ├── MEMORY.md
│   └── docs/
│       ├── phase0-intelligence.md
│       ├── phase1-weapons.md
│       ├── phase1-approach-target.md
│       └── phase1-hearing-questions.md
└── pattern-B/                      # 説明と空欄式（市場戦略Case用）
    ├── CLAUDE.md
    ├── MEMORY.md
    └── docs/
        ├── phase0-intelligence.md
        ├── phase1-weapons.md
        ├── phase1-approach-target.md
        └── phase1-hearing-questions.md
```

## 新規案件を始めるとき

`WORKFLOW.md` を読んでから作業を開始する。9ステップで Case○○○ のセットアップ（README・CLAUDE.md・MEMORY.md・docs/）が完成する。