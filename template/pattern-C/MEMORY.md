# MEMORY — Case[CASE_NUMBER] / [COMPANY_NAME]

> **別PCからの作業開始方法**: `@MEMORY.md` → `@CLAUDE.md` → `@docs/followup-plan.md` の順で読み込む。

---

## 案件基本情報

| 項目 | 内容 |
|---|---|
| 案件名 | Case[CASE_NUMBER] — [COMPANY_NAME] × TG GLOBAL |
| 担当者（先方） | [PERSON_NAME] / [PERSON_ROLE] |
| 会社 | [COMPANY_NAME]（[COUNTRY_CITY]） |
| 業種 | [INDUSTRY] |
| 連絡先 | [CONTACT_INFO] |
| 顧客層 | [CUSTOMER_TIER] |
| 使用言語 | [LANGUAGE] |
| 確度 | [CONFIDENCE_SCORE] |
| 起点 | THAIFEX 2026 [THAIFEX_DAY] |
| TG担当者 | TSUJI KENZO（Kenny）|

---

## 戦略の核心（3行）

1. [OPPORTUNITY_REASON]（なぜチャンスか）
2. [IMMEDIATE_ACTIONS] を起点に関係を維持し、見積提示 → 試験発注へつなぐ
3. 最終ゴール: [PRODUCTS] の定期発注ルートを確立する

---

## 意思決定ログ

| 日付 | 決定事項 | 理由 |
|---|---|---|
| [THAIFEX_DAY] | Case[CASE_NUMBER]として起票 | THAIFEX 2026での接触を商談として管理するため |

---

## 現在の状態（[THAIFEX_DAY]時点）

**フェーズ0: 初回フォロー段階。**

完了済み ✅
- THAIFEX 2026での接触・名刺交換 / 連絡先取得

未完了 🔲
- 🔴 [IMMEDIATE_ACTIONS]（期限: [ACTION_DEADLINE]）
- 🟡 見積もり送付（[PRODUCTS]）
- 🟡 返信受領 → Go/No Go Step 1 判定
- 🟢 試験発注打診

---

## 関心商品

[PRODUCTS]

---

## 重要連絡先

| 相手 | 連絡先 | 備考 |
|---|---|---|
| [PERSON_NAME] | [CONTACT_INFO] | 先方担当者 |
| Kenny（TG GLOBAL） | kenny@shunnhokkaido.jp | 自社担当 |
| Case001 LINE OA | https://lin.ee/UUvyvbZw | タイ向け公式LINE |

---

## ブランドルール（TGプリンシパル抜粋）

- **マスターポジショニング**: 「漁港からキッチンまで、間にいるのは1人。」
- **NGワード**: 「高品質」「安心」「コスパが良い」「新鮮さをお届け」
- **代わりに使う言葉**: 「〇〇漁港直送」「豊洲を通さないから15%安い」「CIF $11-12/kg」

---

## このリポジトリの構造

```
Case[CASE_NUMBER]_[COMPANY_CODE]/
├── CLAUDE.md                       # ← ミッション・戦略・Go/No Go
├── MEMORY.md                       # ← このファイル（別PC引き継ぎ用）
└── docs/
    ├── phase0-intelligence.md      # ← 接触記録・会話ログ・追加情報
    └── followup-plan.md            # ← 今すぐ〜1週間のフォロー計画
```

---

## 別PCで作業を始めるとき

```
@MEMORY.md        ← まずこれを読んで状態を把握
@CLAUDE.md        ← ミッション・戦略・Go/No Goを確認
@docs/followup-plan.md   ← 次にやることを確認
```
