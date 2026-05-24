# タスク管理ガイド — Claude Code TaskCreate

> TG GLOBAL の全タスクは Claude Code の **TaskCreate ツール** で管理します。
> このガイドはチーム全体向けの運用ルールです。

## なぜ TaskCreate か

| 従来（メモ・スプレッド） | TaskCreate（Claude Code） |
|----------------------|-------------------------|
| 手動で進捗を更新 | AIが自動で進捗を認識 |
| セッション外で見えない | セッション内で常に可視化 |
| 依存関係が不明 | ブロック関係を明示 |
| チーム間で分散 | 単一の真実のソース |

## タスクの分類

### ✅ TaskCreate を使う

- 🆕 新機能追加（例：OAuth 導入）
- 🔧 大規模リファクタリング（例：認証周辺の構造変更）
- 🤖 スクレイピング・自動化スクリプト
- 💾 DB スキーマ変更
- 🔀 CI/CD パイプライン変更
- 🔐 セキュリティ実装
- 📝 複雑なドキュメント作成
- 🐛 複数ステップの bug fix

### ❌ TaskCreate は不要

- タイポ修正
- 1行の設定値変更
- ドキュメントの軽微な更新
- 即座に完了できる単純なタスク

## ライフサイクル

### 1️⃣ タスク作成（TaskCreate）

**タイミング**: 作業開始前

**入力項目**:
```
subject:      "命令形で簡潔に"
              例：「Google OAuth 導入の計画を固める」
              
description:  "背景・アウトプット・関連タスク"
              例：「Plan Modeで影響範囲・データ流・テスト観点を詰める。
                  完了後は実装タスクに進む」
              
activeForm:   "動作中の表現" (省略可)
              例：「Plan Modeで計画を詰めている」
```

**例**:
```
TaskCreate(
  subject="Google OAuth 導入の計画を固める",
  description="Plan Mode で src/auth・secrets を読んで、
              影響ファイル・データ流・テスト観点を整理。
              完了後は実装タスクに進む",
  activeForm="Plan Mode で計画を詰めている"
)
```

### 2️⃣ 開始（TaskUpdate → in_progress）

**タイミング**: タスクに実際に着手する直前

```
TaskUpdate(
  taskId="<タスクID>",
  status="in_progress"
)
```

### 3️⃣ 完了（TaskUpdate → completed）

**タイミング**: アウトプットが確定した直後

- ✅ コード実装完了 → TEST 完了 → PR マージ後
- ✅ 計画書作成 → Kennyが「この計画でいこう」と合意後
- ✅ ドキュメント作成 → レビュー完了・確定後

```
TaskUpdate(
  taskId="<タスクID>",
  status="completed"
)
```

### 4️⃣ 依存関係を張る（TaskUpdate → addBlockedBy）

**用途**: タスク A が完了してから タスク B を開始する場合

```
TaskUpdate(
  taskId="<実装タスクID>",
  addBlockedBy=["<計画タスクID>"]
)
```

**ブロック構造の例**:
```
計画タスク（完了）
  ↓ unblocks
実装タスク（進行中）
  ↓ unblocks
テスト・PR タスク
  ↓ unblocks
CLAUDE.md 更新タスク
```

## 実装例：Plan Mode ワークフロー

### シーン：「Google OAuth 導入」

**Step 1: 計画タスク作成**
```
TaskCreate(
  subject="Google OAuth 導入の計画を固める",
  description="Plan Mode で以下を詰める：
    - 影響するファイル（src/auth, src/oauth, secrets）
    - データの流れ（認可 → Token取得 → ユーザー作成）
    - テスト観点（ユニット・E2E）
    - 既存認証との互換性
    完了後は実装タスクを開始",
  activeForm="Plan Mode で設計を詰めている"
)
→ taskId: "PLAN-001" が返される
```

**Step 2: 計画タスク開始**
```
TaskUpdate(
  taskId="PLAN-001",
  status="in_progress"
)
```

**Step 3: 計画完成 → 計画タスク完了**
```
TaskUpdate(
  taskId="PLAN-001",
  status="completed"
)
```

**Step 4: 実装タスク作成（計画タスクがブロッカー）**
```
TaskCreate(
  subject="Google OAuth の実装",
  description="計画書に基づいて以下を実装：
    - OAuth2.0フロー
    - Token 管理
    - Error Handling
    完了後は TEST・PR・CLAUDE.md 更新を行う",
  activeForm="OAuth 導入を実装している"
)
→ taskId: "IMPL-001" が返される

TaskUpdate(
  taskId="IMPL-001",
  addBlockedBy=["PLAN-001"]  # 計画完了後に開始可能
)
```

**Step 5: 実装開始**
```
TaskUpdate(
  taskId="IMPL-001",
  status="in_progress"
)
```

**Step 6: 実装完了（TEST・PR 込み）**
```
TaskUpdate(
  taskId="IMPL-001",
  status="completed"
)
```

**Step 7: CLAUDE.md 更新タスク**
```
TaskCreate(
  subject="CLAUDE.md に OAuth 導入の記録を追加",
  description="実装完了後、以下を記載：
    - 実装パターン（どのファイルをどう変更したか）
    - 今後の拡張ポイント",
  activeForm="CLAUDE.md を更新している"
)

TaskUpdate(
  taskId="UPDATE-001",
  addBlockedBy=["IMPL-001"]
)
```

## ベストプラクティス

### ✅ DO

- 🎯 **subject は具体的・動作的に** — 「OAuth 対応」ではなく「Google OAuth 導入の計画を固める」
- 📊 **ブロック関係を積極的に張る** — 依存する順序を明示
- 🔄 **status を逐一更新** — in_progress / completed のタイミングを明確に
- 📝 **description に背景とアウトプットを記載** — 数日後に見直した時に分かるように

### ❌ DON'T

- ❌ 抽象的な subject（「改善」「対応」「修正」）
- ❌ ブロック関係を無視（依存順序が不明になる）
- ❌ status を更新しないまま放置
- ❌ 長すぎる description（簡潔に）

## チーム向け通達

### 全員に周知する項目

| 項目 | 通達内容 |
|------|--------|
| **タスク作成** | 複雑な作業は TaskCreate で作成。メモに頼らない |
| **進捗更新** | 開始時に in_progress / 完了時に completed に更新 |
| **依存関係** | ブロック関係を張って、タスク順序を明示 |
| **参照先** | このガイド + CLAUDE.md の「タスク管理」セクション |

### Kennyへの報告

- 大型タスク（> 1日）は TaskCreate で作成 → Kenny に taskId を伝える
- 完了時に TaskUpdate で status: completed に → 自動で通知
- 計画と実装のブロック関係を明示 → 後戻りを防ぐ

## Q & A

**Q: 既に始まっているタスクがある場合は？**
A: 今この瞬間から TaskCreate を使い始めて OK。遡及的に過去タスクを作る必要はありません。

**Q: セッション外で進捗が進んだら？**
A: 次のセッション開始時に TaskList で確認して、status を updated します。

**Q: タスクを削除したい場合は？**
A: 必要に応じて status: deleted で削除。重大な変更がない限りは完了タスクのまま残す。

**Q: 個人用の TODO は？**
A: 個人的な細粒度タスク（買い物リスト など）は TaskCreate の対象外。プロジェクト作業のみ。

---

**更新**: 2026-05-24
**責任者**: Kenny
**参照**: CLAUDE.md / .claude/CLAUDE.md
