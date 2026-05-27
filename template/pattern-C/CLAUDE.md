# Case[CASE_NUMBER] — [COMPANY_NAME] × TG GLOBAL

## ミッション

**[PERSON_NAME]（[PERSON_ROLE]、[COMPANY_NAME]）との取引を成立させる。**

[OPPORTUNITY_REASON]

起点: THAIFEX 2026 [THAIFEX_DAY] での接触。

## Quick Reference

| 項目 | 内容 |
|---|---|
| 会社名 | [COMPANY_NAME] |
| 担当者 | [PERSON_NAME] / [PERSON_ROLE] |
| 国・都市 | [COUNTRY_CITY] |
| 業種 | [INDUSTRY] |
| 顧客層 | [CUSTOMER_TIER] |
| 使用言語 | [LANGUAGE] |
| 接触方法・連絡先 | [CONTACT_INFO] |
| 確度 | [CONFIDENCE_SCORE] |
| 決裁権 | [DECISION_AUTHORITY] |
| 意思決定者 | [DECISION_MAKER] |
| 関心商品 | [PRODUCTS] |
| 今すぐやること | [IMMEDIATE_ACTIONS]（期限: [ACTION_DEADLINE]） |

---

## Go/No Go 判断基準

| ステップ | アクション | Go（次へ進む） | No Go（止まる・変える） |
|---|---|---|---|
| Step 1 | [IMMEDIATE_ACTIONS] | 返信あり・反応あり | 72時間無反応 → 追いフォロー1回。それでも無反応なら低確度に移動 |
| Step 2 | 見積提示・商品提案 | 価格や条件について具体的な質問 / 商談アポ確定 | 2週間反応なし → クローズ判断 |
| Step 3 | クロージング | 発注書または試験発注の受領 | 1ヶ月経過・進展なし → Case を「休眠」としてアーカイブ |

---

## 決裁情報

- **決裁権**: [DECISION_AUTHORITY]
- **意思決定者**: [DECISION_MAKER]
- **ルート**: [DECISION_AUTHORITY] が「上司・会議が決める」場合は、[PERSON_NAME] を内部推薦者として育て、意思決定者への接触機会をつくる

---

## 関心商品

[PRODUCTS]

詳細な価格・在庫状況は `Case001_EV-THAIFEX2026/docs/booth-product-list.md` を参照。

---

## 戦略

<!-- Claude がintake回答（特にQ4業種・Q9顧客層・Q11確度）から推論して1〜2段落で記述する。
     テンプレートのまま残さないこと。以下は記述例。 -->

[CUSTOMER_TIER] のペルソナに対し、[PRODUCTS] を起点に関係を構築する。
[CONFIDENCE_SCORE] の確度であるため、まず [IMMEDIATE_ACTIONS] で接触を維持し、
次に見積もり → 試験発注のサイクルに乗せることを最優先とする。

Case001（タイ市場戦略全体）の「[CUSTOMER_TIER]」向け訴求軸を適用する:
- 第1層: 「豊洲を通さない唯一のルート・産地直接ストーリー」
- 第2層: 「タイ国内在庫あり・即納可能・100kgから対応」
- 第3層: 「安定供給・品揃え・大阪港集約」

---

## フェーズ

### フェーズ0（今すぐ ← 現在）

- [ ] [IMMEDIATE_ACTIONS] → 期限: [ACTION_DEADLINE]
- [ ] `docs/followup-plan.md` の72時間以内フォロー文面を確認して送信

### フェーズ1（1〜2週間以内）

- [ ] 見積もり送付（関心商品: [PRODUCTS]）
- [ ] 返信・反応を `docs/phase0-intelligence.md` に記録
- [ ] Go/No Go Step 2 の判定

### フェーズ2（1ヶ月以内）

- [ ] 試験発注の打診
- [ ] サンプル手配（必要な場合）
- [ ] クロージング → 発注書受領

---

## エージェントの役割

1. **コミュニケーター** — [LANGUAGE] でのフォローアップメッセージ・見積もりメール・LINE文面を起草する。翻訳が必要な場合は「要ネイティブ確認」と明示
2. **提案設計者** — [PRODUCTS] の見積もり・提案文・価格シートを作成する。Case001の商品情報と価格を参照
3. **情報分析官** — 案件の進捗記録・Go/No Go判定の材料整理・次のアクション提案を行う

---

## 連絡先

| 手段 | 情報 |
|---|---|
| [CONTACT_INFO] | （上記参照） |
| Kenny（TG GLOBAL） | kenny@shunnhokkaido.jp |

---

## 参照

| リソース | 内容 |
|---|---|
| `Case001_EV-THAIFEX2026/CLAUDE.md` | タイ市場戦略全体・3層顧客モデル・競合分析 |
| `Case001_EV-THAIFEX2026/docs/booth-product-list.md` | 商品在庫・価格（THB） |
| `docs/phase0-intelligence.md` | この案件の初期情報・接触記録 |
| `docs/followup-plan.md` | フォローアップ計画・メッセージ文面 |
| `C:\Users\kenzo\Desktop\Warp_Repositories\standard-seafood-wholesale-overseas` | TGプリンシパル（ブランドルール・会社情報） |

---

## 未確認事項

- [ ] [DECISION_MAKER] の詳細な役職・権限の確認
- [ ] 最低発注量（MOQ）・支払い条件の確認
- [ ] [PRODUCTS] の具体的な規格・グレードの希望確認
