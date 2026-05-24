# スキル・エージェント・コマンドの使い分けガイド

## 3つの仕組みの違い

| 観点 | エージェント（agents/） | スキル（skills/） | コマンド（commands/） |
|------|----------------------|------------------|---------------------|
| 目的 | 特定の役割を持つ「人格」 | 業務手順書・マニュアル | 短い定型操作 |
| 起動方法 | 司令塔が自動ルーティング | エージェントが参照 | `/command` で手動実行 |
| 出力 | 役割に応じた判断・提案 | 手順に沿った実行結果 | 単一タスクの結果 |
| 例 | 「提案書を作って」→ proposal-writer | proposal-writerが sales-playbook を参照 | `/review` でPRレビュー |

## いつ何を使うか

### エージェントを使う場面
- 「〇〇して」という依頼 → 司令塔がルーティング
- 判断・提案・分析が必要な業務
- 複数ステップの作業（エージェントが自律的に進める）

### スキルを使う場面
- エージェントが**参照する業務マニュアル**
- 手順の標準化（誰がやっても同じ品質）
- ユーザーが直接呼ぶことはない（エージェント経由）

### コマンドを使う場面
- 定型的で頻度の高い短いタスク
- ユーザーが `/command名` で明示的に呼ぶ
- 現状このリポジトリでは未整備（将来追加可能）

## 関係図

```
ユーザーの依頼
    ↓
司令塔（.claude/CLAUDE.md）がルーティング
    ↓
エージェント（agents/部門/名前.md）が応答
    ├── スキル（skills/マニュアル.md）を参照
    └── ドキュメント（docs/ファイル.md）を参照
```

## 8つのスキルの対応表

| スキル | 主に使うエージェント |
|--------|-------------------|
| sales-playbook | sales/ 全4体 |
| marketing-operations | marketing/ 全4体 |
| partner-management | partner-sales/ 全4体 |
| customer-success-guide | customer-success/ 全3体 |
| kpi-analytics | analytics/ 全4体、CFO |
| strategy-planning | strategy/ 全4体、CSO |
| meeting-facilitation | agenda-designer、one-on-one-coach |
| brand-guidelines | content-creator、CMO |

## 将来の拡張

### コマンド追加の候補
- `/weekly-report` — 週次KPIサマリーの自動生成
- `/prospect-check` — リードスコアリングのクイック実行
- `/price-calc` — 見積計算（CIF/FOB）

### エージェント追加の基準
- 既存エージェントでカバーできない新しい「役割」が明確に存在する
- 週1回以上使う見込みがある
- 32体を大幅に超えないこと（管理コスト増）
