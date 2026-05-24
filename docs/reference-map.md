# ドキュメント参照マップ

## 概要

各エージェント / CXO が業務遂行時に参照すべきドキュメントの対応表。
エージェントは自分の担当タスクに関連するdocsを読んでから応答する。

## docs/ ファイル一覧と用途

| ファイル | 内容 | 主な参照元 |
|---------|------|-----------|
| `company-overview.md` | 財務・沿革・競合・技術資産・リスク | 全CXO、strategy全体 |
| `brand-positioning.md` | ポジショニング・ペルソナ3種・NGワード・競合マップ | CMO、marketing全体、sales/proposal-writer |
| `c-level-roles.md` | CXO 6体の詳細設計（KPI・口調・意思決定基準） | c-suite全体 |
| `architecture.md` | バックエンド構成・MCP・OAuth | CTO、analytics全体 |
| `memory-operations.md` | メモリ運用ルール | 司令塔（ルーティング時の参考） |
| `reference-map.md` | 本ファイル（参照設計） | 司令塔 |

## エージェント別 必須参照ドキュメント

### CXO（c-suite/）

| CXO | 必須参照 | 追加参照 |
|-----|---------|---------|
| CSO | company-overview, c-level-roles | 全docs |
| CFO | company-overview, c-level-roles | — |
| CMO | brand-positioning, c-level-roles | company-overview |
| COO | company-overview, c-level-roles | architecture |
| CTO | architecture, c-level-roles | company-overview |
| CBDO | company-overview, brand-positioning, c-level-roles | — |

### 営業部門（sales/）

| エージェント | 必須参照 | スキル |
|------------|---------|--------|
| lead-qualifier | company-overview（市場・パートナー情報） | sales-playbook |
| proposal-writer | brand-positioning, company-overview | sales-playbook |
| pipeline-manager | company-overview（財務目標） | sales-playbook, kpi-analytics |
| closing-strategist | brand-positioning | sales-playbook |

### マーケティング部門（marketing/）

| エージェント | 必須参照 | スキル |
|------------|---------|--------|
| content-creator | brand-positioning | brand-guidelines, marketing-operations |
| campaign-planner | brand-positioning, company-overview | marketing-operations |
| lead-generation | company-overview（市場情報） | marketing-operations |
| market-researcher | company-overview | marketing-operations |

### パートナーセールス部門（partner-sales/）

| エージェント | 必須参照 | スキル |
|------------|---------|--------|
| partner-recruiter | company-overview（パートナー情報） | partner-management |
| partner-enablement | brand-positioning | partner-management |
| alliance-strategist | company-overview | partner-management, strategy-planning |
| referral-optimizer | company-overview | partner-management |

### カスタマーサクセス部門（customer-success/）

| エージェント | 必須参照 | スキル |
|------------|---------|--------|
| onboarding-specialist | brand-positioning | customer-success-guide |
| retention-manager | company-overview（財務） | customer-success-guide, kpi-analytics |
| expansion-planner | brand-positioning, company-overview | customer-success-guide |

### アナリティクス部門（analytics/）

| エージェント | 必須参照 | スキル |
|------------|---------|--------|
| kpi-tracker | company-overview（財務・目標） | kpi-analytics |
| pipeline-analyst | company-overview | kpi-analytics |
| forecast-modeler | company-overview（財務） | kpi-analytics |
| dashboard-builder | company-overview | kpi-analytics |

### 経営戦略部門（strategy/）

| エージェント | 必須参照 | スキル |
|------------|---------|--------|
| business-planner | company-overview | strategy-planning |
| agenda-designer | — | meeting-facilitation |
| competitive-analyst | company-overview, brand-positioning | strategy-planning |
| initiative-designer | company-overview | strategy-planning |

### ピープル部門（people/）

| エージェント | 必須参照 | スキル |
|------------|---------|--------|
| one-on-one-coach | — | meeting-facilitation |
| performance-reviewer | company-overview（KPI） | kpi-analytics |
| team-builder | company-overview（組織文化） | — |
