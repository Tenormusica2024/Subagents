# Claude Code サブエージェント - 完全リスト（日本語版）

## 📋 実装済みサブエージェント

### 1. code-reviewer（コードレビュー専門家）

**説明**: コード完成後に品質・セキュリティ・ベストプラクティスを自動的にレビュー。重要なコード変更後に自動起動。

**利用可能ツール**: Read, Grep, Glob, Bash, Edit, MultiEdit

**モデル**: inherit（継承）

**行数**: 157行

**主要機能**:
- **セキュリティスキャン**
  - ハードコードされたシークレット・APIキー検出
  - SQLインジェクション脆弱性
  - XSS脆弱性
  - 安全でないデシリアライゼーション
  - 入力検証の欠落
  - 弱い暗号化実装

- **コード品質分析**
  - 可読性（シンプル・明確な命名）
  - DRY原則（重複コードなし）
  - 関数長（50行以下推奨）
  - 複雑性（循環的複雑度 < 10）
  - エラーハンドリング（適切なtry-catch、検証）

- **パフォーマンスレビュー**
  - N+1クエリ問題
  - 非効率なループ（ネストループ、大規模反復）
  - メモリリーク
  - ブロッキングI/O操作
  - 不要なオブジェクト生成

- **テストカバレッジ**
  - 新規関数のユニットテスト存在確認
  - エッジケースのカバー
  - エラー条件のテスト
  - API統合テスト

**自動起動条件**:
- 50行以上の重要なコード完成時
- 複数ファイルの変更時
- セキュリティ関連コード（認証、データベース、API）記述時
- "review"、"check"等のキーワード使用時

**使用方法**:
```
@code-reviewer
```

**レポート形式**:
- 🔴 Critical Issues（即座修正必須）
- 🟠 High Priority（修正推奨）
- 🟡 Medium Priority（修正検討）
- 🟢 Suggestions（任意）
- ✅ Strengths（優れている点）

---

### 2. deployment-specialist（デプロイ専門家）

**説明**: Cloud Run デプロイ・ロールバック・検証の専門家。デプロイ後のキャッシュクリア強制実行とスクリーンショットによる動作確認を担当。

**利用可能ツール**: Bash, Read, Edit, MultiEdit, mcp__playwright__playwright_screenshot, mcp__playwright__playwright_navigate, mcp__playwright__playwright_click, mcp__mobile__mobile_take_screenshot

**モデル**: inherit（継承）

**行数**: 328行

**主要機能**:

1. **Cloud Run デプロイ自動実行**
   ```bash
   cd "[PROJECT_PATH]"
   gcloud run deploy [SERVICE] --source . --quiet
   ```

2. **デプロイ後の3ステップ検証**
   - ステップ1: キャッシュクリア（Ctrl+Shift+R / シークレットウィンドウ）
   - ステップ2: スクリーンショット撮影（Playwright/Mobile）
   - ステップ3: Read確認（画像内容の実際の確認）

3. **緊急ロールバック手順**
   - 前リビジョンへの即座復帰
   - バックアップからの復元
   - トラフィック分割による段階的復旧

4. **AI FM Podcast プロジェクト固有の検証**
   - 認証テスト（新規ユーザー登録）
   - Library機能確認
   - コンソールエラー確認
   - HTTPステータスコード確認

5. **バックアップ作成・管理**
   - タイムスタンプ付きディレクトリ作成
   - 完全コピーディレクトリ作成
   - VERSION_METADATA.json生成

**CLAUDE.md プロトコル連携**:
- ✅ Cache Clear Protocol 準拠
- ✅ Proactive Screenshot Protocol 準拠
- ✅ Cloud Run Re-deployment Protocol 準拠
- ✅ Task Completion Validation Protocol 準拠

**自動起動条件**:
- "deploy"、"デプロイ"、"ロールバック"等のキーワード
- Cloud Run 関連の作業
- "更新が確認できない"問題発生時
- 5xx/401/403エラー発生時

**使用方法**:
```
@deployment-specialist
```

**デプロイ前チェックリスト**:
- [ ] ワーキングディレクトリ確認
- [ ] cloudbuild.yaml または Dockerfile 確認
- [ ] requirements.txt 確認
- [ ] 環境変数確認
- [ ] Git commit/push 完了確認

**デプロイ後検証基準**:
- HTTP 200 ステータスコード
- ログインテスト成功
- 主要機能の動作確認
- JavaScriptエラーなし

---

### 3. github-integration-specialist（GitHub統合専門家）

**説明**: GitHub Issue・PR・コミット管理の専門家。task_complete_private.py による自動報告、コミットメッセージ生成、PR作成を担当。

**利用可能ツール**: Bash, Read, Edit, MultiEdit, Grep, Glob

**モデル**: inherit（継承）

**行数**: 318行

**主要機能**:

1. **Task Complete Report（最重要）**
   ```bash
   cd "C:\Users\Tenormusica\Documents\github-remote-desktop"
   python task_complete_private.py "[完全な報告テキスト]"
   ```
   - 完全な報告テキスト記載（要約禁止・省略禁止）
   - すべてのタスク完了時に必須実行
   - 修正内容・変更点・期待される動作を全て含める

2. **Git コミットメッセージ生成**
   - 絵文字プレフィックス使用
   - 📋, 🎯, 🐛, ⚡, 📝, 🔧, ✨, 🚀, 🔒, 🧪 等
   - 変更内容の正確な反映
   - 関連Issue番号の自動付与
   - Co-Authored-By: Claude 記載

3. **Pull Request 作成**
   ```bash
   gh pr create --title "タイトル" --body "本文"
   ```
   - Summary（1-3箇条書き）
   - Test Plan（テスト手順チェックリスト）
   - 🤖 Generated with Claude Code 記載

4. **GitHub Actions 管理**
   - ワークフロー作成支援
   - トラブルシューティング
   - ログ確認・再実行
   - 失敗時の自動修正提案

5. **リポジトリ管理**
   - ブランチ管理（作成・削除・マージ）
   - タグ管理（リリースタグ作成）
   - リモート操作（fetch・pull・push）

**CLAUDE.md プロトコル連携**:
- ✅ Task Completion Validation Protocol 完全対応
- ✅ 完全な報告テキスト記載（要約禁止）
- ✅ すべてのタスク完了時に必須実行
- ✅ FALSE SUCCESS CLAIMS 完全禁止プロトコル準拠

**自動起動条件**:
- タスク完了時（必須・例外なし）
- "GitHub Issue報告"、"コミット"、"PR作成"等のキーワード
- Git 操作時
- 確認・質問がある時
- エラー発生時

**使用方法**:
```
@github-integration-specialist
```

**コミットメッセージフォーマット**:
```bash
🎯 [機能名] - [詳細な説明]

🤖 Generated with [Claude Code](https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

---

## 📊 サブエージェント統計

| サブエージェント | 行数 | ファイルサイズ | 優先度 | ステータス | 主要用途 |
|------------------|------|----------------|---------|-----------|----------|
| code-reviewer | 157 | 4.4KB | 🟡 中 | ✅ 実装済み | コード品質・セキュリティレビュー |
| deployment-specialist | 328 | 9.9KB | 🔴 最高 | ✅ 実装済み | Cloud Run デプロイ・検証 |
| github-integration-specialist | 318 | 7.7KB | 🟠 高 | ✅ 実装済み | GitHub Issue・PR・コミット管理 |

**合計**: 3つのサブエージェント、803行

---

## 🎯 実装予定サブエージェント

### Phase 2（1週間以内実装予定）

#### 4. ui-verification-specialist（UI検証専門家）
**優先度**: 🟠 高

**目的**: UI修正後の視覚的検証専門家

**主要機能**:
- Playwright自動スクリーンショット撮影
- 画像内容のRead確認（必須）
- UI差異検出
- 修正前後の比較レポート
- デプロイ後のキャッシュクリア強制実行

**CLAUDE.md プロトコル連携**:
- ✅ FALSE SUCCESS CLAIMS 完全禁止プロトコル準拠
- ✅ スクリーンショット撮影後の必須Read確認
- ✅ 目視確認を常に正とする
- ✅ JavaScript実行結果より画像内容を優先

**自動起動条件**:
- UI修正完了時
- フロントエンド変更時
- "修正確認"、"表示確認"等のキーワード

#### 5. backup-manager（バックアップ管理専門家）
**優先度**: 🟡 中

**目的**: バックアップ作成・管理の専門家

**主要機能**:
- 自動バックアップディレクトリ作成
- 完全コピーディレクトリ作成（必須）
- VERSION_METADATA.json生成
- バックアップ履歴管理
- タイムスタンプ付き命名規則

**CLAUDE.md プロトコル連携**:
- ✅ バックアップ作成プロトコル準拠
- ✅ 全く同じコピーディレクトリ作成（必須）

**自動起動条件**:
- "バックアップ"、"復元"等のキーワード
- 重要なコード変更前
- デプロイ前

### Phase 3（必要に応じて実装）

#### 6. automation-debugger（自動化デバッグ専門家）
**優先度**: 🟡 中

**目的**: 自動化スクリプトのデバッグ専門家

**主要機能**:
- スクリーンショット付きデバッグ
- エラーログ分析
- セレクタ修正提案
- タイムアウト問題の解決
- PyAutoGUI/Playwright スクリプト最適化

**対象スクリプト**:
- X.com投稿・リツイートシステム
- ブラウザ自動操作
- UI自動化テスト

**自動起動条件**:
- 自動化スクリプトエラー発生時
- "デバッグ"、"自動化失敗"等のキーワード

---

## 🔧 インストール・設定

### ファイル配置場所

#### User-level（全プロジェクト共通）
**Windows**:
```
C:\Users\[ユーザー名]\.claude\agents\
```

**Mac/Linux**:
```
~/.claude/agents/
```

#### Project-level（プロジェクト固有）
```
[プロジェクトルート]/.claude/agents/
```

### 設定ファイルフォーマット

```markdown
---
name: [サブエージェント名]
description: [日本語説明]
tools: [使用可能ツール名]
model: inherit
---

# [サブエージェント名]

[サブエージェントの詳細実装内容（日本語）]

## 主要機能

## 自動起動条件

## 使用例
```

---

## 📚 使用方法

### 方法1: @メンション（推奨）
```
@code-reviewer
@deployment-specialist
@github-integration-specialist
```

### 方法2: 自然言語指示
```
code-reviewerサブエージェントを使ってコードをチェックしてください
deployment-specialist でデプロイしてください
GitHub Issue に報告してください
```

### 方法3: 自動トリガー（プロアクティブ）
各サブエージェントの自動起動条件に基づいて、Claude Codeが自動的にサブエージェントを起動します。

**例**:
- 50行以上のコード完成 → code-reviewer 自動起動
- "デプロイ"キーワード → deployment-specialist 自動起動
- タスク完了 → github-integration-specialist 自動起動

### 方法4: /agents コマンド
```
/agents
```
サブエージェント選択インターフェースが開き、リストから選択可能。

---

## ✅ 動作確認方法

### 1. サブエージェント認識確認
Claude Code 再起動後、プロンプトで `@` を入力し、候補にサブエージェント名が表示されることを確認。

### 2. テスト実行
```
@code-reviewer
```
サブエージェントが応答することを確認。

### 3. 登録済みサブエージェント一覧確認
```
/agents
```
全ての登録済みサブエージェントが表示されることを確認。

---

## 🔄 バージョン履歴

### v1.0.3（2025-10-01）
- **deployment-specialist** 追加（最優先度）
  - Cloud Run デプロイ自動化
  - 3ステップ検証プロセス
  - AI FM Podcast 固有検証
- **github-integration-specialist** 追加（優先度高）
  - Task Complete Report 自動実行
  - Co-Authored-By 記載
  - PR作成自動化

### v1.0.2（2025-10-01）
- **Serena MCP** 導入（軽量設定）
  - 会話コンテキスト永続化
  - MAX_MEMORIES: 100件
  - 7日自動クリーンアップ
- MCP合計10個に拡張

### v1.0.1（2025-10-01）
- **code-reviewer** を JSON から Markdown 形式に移行
- Claude Code v1.0.60+ 対応
- 自動起動条件の明確化

### v1.0.0（2025-10-01）
- 初期実装
- code-reviewer サブエージェント（Markdown形式）
- 4軸レビュー（セキュリティ・品質・パフォーマンス・テスト）

---

## 🛠️ カスタマイズ方法

### 利用可能ツールの変更
YAMLフロントマターの `tools` フィールドを編集:

```yaml
---
name: code-reviewer
tools: Read, Grep, Glob, Bash, Edit, MultiEdit, WebFetch
model: inherit
---
```

### モデルの指定
特定のモデルを使用する場合:

```yaml
---
name: code-reviewer
model: claude-sonnet-4
---
```

親エージェントから継承する場合:

```yaml
---
model: inherit
---
```

---

## 📝 実装例

### code-reviewer 実装例
```markdown
---
name: code-reviewer
description: コード品質・セキュリティ・パフォーマンスを自動レビュー
tools: Read, Grep, Glob, Bash, Edit, MultiEdit
model: inherit
---

# Code Reviewer Agent

重要なコード変更後に自動的に起動し、以下をレビュー:
- セキュリティ脆弱性
- コード品質
- パフォーマンス問題
- テストカバレッジ
```

---

## 🔗 関連リンク

- **Claude Code 公式ドキュメント**: https://docs.anthropic.com/en/docs/claude-code
- **サブエージェント詳細ガイド**: https://docs.anthropic.com/en/docs/claude-code/sub-agents
- **GitHub リポジトリ**: https://github.com/Tenormusica2024/Subagents
- **CLAUDE.md プロトコル**: プロジェクトルートの CLAUDE.md 参照

---

## 🆘 トラブルシューティング

### サブエージェントが認識されない
1. ファイル配置場所を確認
   - User-level: `~/.claude/agents/`
   - Project-level: `.claude/agents/`
2. Claude Code を再起動
3. YAMLフロントマターの構文を確認

### @メンションで候補に表示されない
1. ファイル名が `[name].md` になっているか確認
2. `name:` フィールドが正しく設定されているか確認
3. Claude Code v1.0.60+ を使用しているか確認

### 自動起動しない
1. 自動起動条件を README.md で確認
2. キーワードが正しく使用されているか確認
3. CLAUDE.md プロトコルが正しく設定されているか確認

---

## 📞 サポート

使用方法やカスタマイズについての質問は、Claude Code セッション内で自由にお尋ねください。

---

## 📝 ライセンス

Private Use Only（個人利用のみ）

---

**最終更新**: 2025-10-01  
**バージョン**: 1.0.3  
**作成者**: Claude Code  
**言語**: 日本語（完全版）
