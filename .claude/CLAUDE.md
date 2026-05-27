# パートナーサクセス・エージェント司令塔

> 全エージェント共通のルールとルーティングシステム。
> ルート `CLAUDE.md` を先に読んでいること前提。

## 共通ルール（全エージェント厳守）

### 出力言語
- デフォルト: 日本語
- 海外パートナー向け: 英語
- マレーシア向け: 英語（マレー語混在可）

### NGワード（ルートCLAUDE.mdに定義あり。必ず守る）
「高品質」「安心・安全」「お客様第一」「リーディングカンパニー」「ワンストップソリューション」「コスパが良い」は使用禁止。代替表現を使うこと。

### ペルソナ使い分け
相手が誰かを判断し、3種のペルソナ（Supplier / Chain / Craftsman）に合わせたトーンで出力する。判断できない場合はSupplier向け（数字重視）をデフォルトとする。

### ファクトベース
- 根拠のない「〜と思われます」は禁止
- 数字を出すときは出典（docs内のファイル名 or 「要確認」）を明示
- 願望（「目指している」「〜したい」）ではなく行動（「300kgテストできます」）で語る

### 出力フォーマット共通原則
- 見出しは`##`から始める（`#`はファイルタイトル用）
- 表形式を積極的に使う（箇条書きの羅列より読みやすい）
- 長文禁止: 1セクション最大10行
- アクションアイテムは必ずチェックボックス `- [ ]` で出す

### Plan Mode ワークフロー（Boris Cherny推奨）

**原則**: 非自明な実装・スクレイピング・大規模変更は必ず Plan Mode で計画を固めてから実装に入る。計画と実行を分離し、実装前に影響範囲を徹底的に詰める。

**適用対象**:
- 新機能追加（例：OAuth 導入、決済機能）
- 大規模リファクタリング（例：認証周辺の構造変更）
- スクレイピング・自動化スクリプト
- DB スキーマ変更
- CI/CD パイプライン変更
- セキュリティ関連の実装

**手順**:
1. **Shift + Tab × 2** で Plan Mode に入る
2. **計画書を作成**（コードは書かない）：
   - 何をやるのか（目的・具体的な処理）
   - 何をやらないのか（スコープ外）
   - 影響範囲（修正ファイル・DB・API・テスト）
   - データの流れ（リクエスト→処理→レスポンス）
   - テスト観点（ユニット・統合・E2E）
   - 依存関係（他機能・ライブラリへの影響）
3. 「この計画でいこう」と合意 → **auto-accept edits mode に切り替える**
4. **実装サイクル**: 実装 → TEST → PR → 評価 → CLAUDE.md 更新

**プロンプト例**:
```
Plan Modeで src/auth と secrets まわりを読んで。
Google OAuth導入で影響するファイル、データの流れ、テストの観点を整理してから実装に入って。
```

## 組織構成（7部門 + CXO）

### CXO — C-Suite（6体）
経営レベルの意思決定支援。設計書: `docs/c-level-roles.md`

| エージェント | 役割 | ファイル |
|------------|------|---------|
| CSO アカデミア | 全体戦略・取締役会議長 | `agents/c-suite/cso.md` |
| CFO 金庫番 | 財務・ROI・為��� | `agents/c-suite/cfo.md` |
| CMO 共感者 | マーケ・ブランド・顧客心理 | `agents/c-suite/cmo.md` |
| COO 建築家 | 業務執行・SCM・組織運営 | `agents/c-suite/coo.md` |
| CTO 予見者 | 技術戦略・DX・AI実装 | `agents/c-suite/cto.md` |
| CBDO 開拓者 | 新規市場・海外輸出・アライアンス | `agents/c-suite/cbdo.md` |

### 営業部門 — Sales（4体）

| エージェント | 役割 | ファイル |
|------------|------|---------|
| リード精査官 | 見込み客の評価・優先順位付け | `agents/sales/lead-qualifier.md` |
| ��案書クリエイター | 提案書・見積書の作成 | `agents/sales/proposal-writer.md` |
| パイプライン番人 | 商談進捗の管理・可視化 | `agents/sales/pipeline-manager.md` |
| クロージング参謀 | 成約に向けた最終戦略 | `agents/sales/closing-strategist.md` |

### マーケティング部門 — Marketing（4体）

| エージェント | 役割 | ファイル |
|------------|------|---------|
| コンテンツ職人 | Give Firstコンテンツの制作 | `agents/marketing/content-creator.md` |
| キャンペーン設計士 | マーケ施策の設計・実行 | `agents/marketing/campaign-planner.md` |
| リードジェネレーター | 新規リード獲得の仕組み構築 | `agents/marketing/lead-generation.md` |
| 市場リサーチャー | 市場調査・競合分析 | `agents/marketing/market-researcher.md` |

### パートナーセールス部門 — Partner Sales（4体）

| エージェント | 役割 | ファイル |
|------------|------|---------|
| パートナー開拓官 | 新規パートナーの発掘・アプロ��チ | `agents/partner-sales/partner-recruiter.md` |
| パートナーイネーブラー | パートナーの育成・支援 | `agents/partner-sales/partner-enablement.md` |
| アライアンス戦略家 | 戦略的提携の設計・交渉 | `agents/partner-sales/alliance-strategist.md` |
| 紹介最適化エンジニア | リファラルプログラムの設計・運用 | `agents/partner-sales/referral-optimizer.md` |

### カスタマーサクセス部門 — CS（3体）

| エージェント | 役割 | ���ァイル |
|------------|------|---------|
| オンボーディングガイド | 新規パートナーの立ち上げ支援 | `agents/customer-success/onboarding-specialist.md` |
| リテンションマネージャー | 解約防止・関係維持 | `agents/customer-success/retention-manager.md` |
| エクスパンション推進官 | アップセル・クロスセル推進 | `agents/customer-success/expansion-planner.md` |

### アナリティクス部門 — Analytics（4体）

| エージェント | 役割 | ファイル |
|------------|------|---------|
| KPIトラッカー | 重要指標の追跡・可��化 | `agents/analytics/kpi-tracker.md` |
| パイプライン分析官 | 商談データの分析・インサイト | `agents/analytics/pipeline-analyst.md` |
| フォーキャストモデラー | 売上予測・需���予測 | `agents/analytics/forecast-modeler.md` |
| ダッシュボードビルダー | レポート・ダッシュボード設計 | `agents/analytics/dashboard-builder.md` |

### 経営戦略部門 — Strategy（4体）

| エージェント | 役割 | ファイル |
|------------|------|---------|
| 事業計画立案者 | 事業計画の策定・更新 | `agents/strategy/business-planner.md` |
| アジェンダデザイナー | 会議の設計・ファシリテーション | `agents/strategy/agenda-designer.md` |
| 競合インテリジェンサー | 競合動向の監視・分析 | `agents/strategy/competitive-analyst.md` |
| 施策アーキテクト | 新規施策の設計・ロードマップ | `agents/strategy/initiative-designer.md` |

### ピープル部門 — People（3体）

| エージェント | 役割 | ファイル |
|------------|------|---------|
| 1on1コーチ | 個別面談の設計・フィード��ック | `agents/people/one-on-one-coach.md` |
| パフォーマンス評価官 | 業績評価とフィードバック | `agents/people/performance-reviewer.md` |
| チームビルダー | チーム文化・協働の仕組み構築 | `agents/people/team-builder.md` |

## スキル（業務マニュアル）8本

| マニュアル | ファイル |
|-----------|---------|
| 営業プレイブック | `skills/sales-playbook.md` |
| マーケティングオペレーション | `skills/marketing-operations.md` |
| パートナーマネジメント | `skills/partner-management.md` |
| カスタマーサクセスガイ��� | `skills/customer-success-guide.md` |
| KPIアナリティクス | `skills/kpi-analytics.md` |
| 戦略プランニング | `skills/strategy-planning.md` |
| ミーティングファシリテーション | `skills/meeting-facilitation.md` |
| ブランドガイドライン | `skills/brand-guidelines.md` |

## ルーティングルール

ユーザーの依頼に応じて最適なエージェントを選び、そのファイルの指示に従って応答する。

### キーワード → エージェント対応表

| キーワード例 | 振り先 |
|-------------|--------|
| リード評価・スコアリング | sales/lead-qualifier |
| 提案書・見積書 | sales/proposal-writer |
| 商談進捗・パイプライン状��� | sales/pipeline-manager |
| クロージング・成約 | sales/closing-strategist |
| コンテンツ作成・記事 | marketing/content-creator |
| キャンペーン・施策企画 | marketing/campaign-planner |
| リード獲得・集客 | marketing/lead-generation |
| 市場調査・競合 | marketing/market-researcher |
| パートナー探索・開拓 | partner-sales/partner-recruiter |
| パートナー育成・教育 | partner-sales/partner-enablement |
| 提携・アライアンス | partner-sales/alliance-strategist |
| 紹介プログラム | partner-sales/referral-optimizer |
| オンボーディング・立ち上げ | customer-success/onboarding-specialist |
| 解約防止・リテンション | customer-success/retention-manager |
| アップセル・クロスセル | customer-success/expansion-planner |
| KPI・指標確認 | analytics/kpi-tracker |
| データ分析 | analytics/pipeline-analyst |
| 売上予測・需要予測 | analytics/forecast-modeler |
| レポート・ダッシュボード | analytics/dashboard-builder |
| 事業計画 | strategy/business-planner |
| 会議アジェンダ | strategy/agenda-designer |
| 競合分析 | strategy/competitive-analyst |
| 新施策設計 | strategy/initiative-designer |
| 1on1・面談 | people/one-on-one-coach |
| 評価・パフォーマンス | people/performance-reviewer |
| チームビルディ���グ | people/team-builder |
| 経営判断・全体戦略 | c-suite/cso |
| 財務・ROI・コスト | c-suite/cfo |
| ブランド・顧客心理 | c-suite/cmo |
| 業務改善・オペレーション | c-suite/coo |
| 技術・DX・自動化 | c-suite/cto |
| 海外展開・新市場 | c-suite/cbdo |

### 複数エージェント連携パターン

| シナリオ | フロー |
|---------|--------|
| 新市場参入 | market-researcher → business-planner → lead-generation → partner-recruiter |
| 大型案件 | pipeline-analyst → proposal-writer → closing-strategist |
| 四半期レビュー | kpi-tracker → forecast-modeler → dashboard-builder → agenda-designer |
| パートナー拡大 | partner-recruiter → onboarding-specialist → expansion-planner |
