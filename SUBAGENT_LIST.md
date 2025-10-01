# Claude Code Subagents - Complete List

## 📋 実装済みサブエージェント

### 1. code-reviewer (コードレビュー専門家)

**説明**: Proactively review code after completion for quality, security, and best practices. Triggered automatically after significant code changes.

**ツール**: Read, Grep, Glob, Bash, Edit, MultiEdit

**モデル**: inherit

**行数**: 157行

**主要機能**:
- セキュリティスキャン（ハードコードされたシークレット、SQLインジェクション等）
- コード品質分析（可読性、DRY原則、関数長、複雑性）
- パフォーマンスレビュー（N+1クエリ、非効率なループ等）
- テストカバレッジ確認

**自動起動条件**:
- 50行以上の重要なコード完成時
- 複数ファイルの変更時
- セキュリティ関連コード記述時
- "review", "check" 等のキーワード使用時

**使用方法**:
```
@code-reviewer
```

---

### 2. deployment-specialist (デプロイ専門家)

**説明**: Cloud Run デプロイ・ロールバック・検証の専門家。デプロイ後のキャッシュクリア強制実行とスクリーンショットによる動作確認を担当

**ツール**: Bash, Read, Edit, MultiEdit, mcp__playwright__playwright_screenshot, mcp__playwright__playwright_navigate, mcp__playwright__playwright_click, mcp__mobile__mobile_take_screenshot

**モデル**: inherit

**行数**: 328行

**主要機能**:
- Cloud Run デプロイ自動実行
- デプロイ後の3ステップ検証（キャッシュクリア→スクリーンショット→Read確認）
- 緊急ロールバック手順
- AI FM Podcast プロジェクト固有の検証
- バックアップ作成・管理

**CLAUDE.md プロトコル連携**:
- Cache Clear Protocol 準拠
- Proactive Screenshot Protocol 準拠
- Cloud Run Re-deployment Protocol 準拠

**自動起動条件**:
- "deploy", "デプロイ", "ロールバック" 等のキーワード
- Cloud Run 関連の作業
- "更新が確認できない" 問題発生時

**使用方法**:
```
@deployment-specialist
```

---

### 3. github-integration-specialist (GitHub統合専門家)

**説明**: GitHub Issue・PR・コミット管理の専門家。task_complete_private.py による自動報告、コミットメッセージ生成、PR作成を担当

**ツール**: Bash, Read, Edit, MultiEdit, Grep, Glob

**モデル**: inherit

**行数**: 318行

**主要機能**:
- Task Complete Report（task_complete_private.py 自動実行）
- Git コミットメッセージ生成（絵文字プレフィックス使用）
- Pull Request 作成（gh pr create）
- GitHub Actions 管理
- リポジトリ管理（ブランチ・タグ・リモート操作）

**CLAUDE.md プロトコル連携**:
- Task Completion Validation Protocol 完全対応
- 完全な報告テキスト記載（要約禁止）
- すべてのタスク完了時に必須実行

**自動起動条件**:
- タスク完了時（必須）
- "GitHub Issue報告", "コミット", "PR作成" 等のキーワード
- Git 操作時

**使用方法**:
```
@github-integration-specialist
```

---

## 📊 サブエージェント統計

| サブエージェント | 行数 | ファイルサイズ | 優先度 | ステータス |
|------------------|------|----------------|---------|-----------|
| code-reviewer | 157 | 4.4KB | 中 | ✅ 実装済み |
| deployment-specialist | 328 | 9.9KB | 🔴 最高 | ✅ 実装済み |
| github-integration-specialist | 318 | 7.7KB | 🟠 高 | ✅ 実装済み |

**合計**: 3つのサブエージェント、803行

---

## 🎯 実装予定サブエージェント

### Phase 2 (1週間以内)

#### 4. ui-verification-specialist (UI検証専門家)
**優先度**: 🟠 高

**目的**: UI修正後の視覚的検証専門家

**主要機能**:
- Playwright自動スクリーンショット
- 画像内容のRead確認
- UI差異検出
- 修正前後の比較レポート

**CLAUDE.md プロトコル連携**:
- FALSE SUCCESS CLAIMS 完全禁止プロトコル準拠
- スクリーンショット必須確認
- 目視確認を常に正とする

#### 5. backup-manager (バックアップ管理専門家)
**優先度**: 🟡 中

**目的**: バックアップ作成・管理の専門家

**主要機能**:
- 自動バックアップディレクトリ作成
- 完全コピーディレクトリ作成
- VERSION_METADATA.json生成
- バックアップ履歴管理

**CLAUDE.md プロトコル連携**:
- バックアップ作成プロトコル準拠
- タイムスタンプ付き命名規則

### Phase 3 (必要に応じて)

#### 6. automation-debugger (自動化デバッグ専門家)
**優先度**: 🟡 中

**目的**: 自動化スクリプトのデバッグ専門家

**主要機能**:
- スクリーンショット付きデバッグ
- エラーログ分析
- セレクタ修正提案
- タイムアウト問題の解決

---

## 🔧 インストール・設定

### ファイル配置場所

**User-level（全プロジェクト共通）**:
```bash
Windows: C:\Users\[Username]\.claude\agentsMac/Linux: ~/.claude/agents/
```

**Project-level（プロジェクト固有）**:
```bash
.claude/agents/
```

### 設定ファイルフォーマット

```markdown
---
name: [サブエージェント名]
description: [説明]
tools: [使用可能ツール]
model: inherit
---

# [サブエージェント名]

[サブエージェントの詳細実装内容]
```

---

## 📚 使用方法

### Method 1: @-mention
```
@code-reviewer
@deployment-specialist
@github-integration-specialist
```

### Method 2: 自然言語指示
```
Use the code-reviewer subagent to check my changes
deployment-specialist でデプロイしてください
```

### Method 3: 自動トリガー
各サブエージェントの自動起動条件に基づいて自動的に起動

### Method 4: /agents コマンド
```
/agents
```
サブエージェント選択画面から選択

---

## 🔄 バージョン履歴

### v1.0.3 (2025-10-01)
- **deployment-specialist** 追加（最優先度）
- **github-integration-specialist** 追加（優先度高）

### v1.0.2 (2025-10-01)
- Serena MCP 導入（軽量設定）
- MCP合計10個に拡張

### v1.0.1 (2025-10-01)
- code-reviewer を JSON から Markdown 形式に移行
- Claude Code v1.0.60+ 対応

### v1.0.0 (2025-10-01)
- 初期実装
- code-reviewer サブエージェント（Markdown形式）

---

## 🔗 関連リンク

- **Claude Code 公式ドキュメント**: https://docs.anthropic.com/en/docs/claude-code
- **サブエージェント詳細**: https://docs.anthropic.com/en/docs/claude-code/sub-agents
- **GitHub リポジトリ**: https://github.com/Tenormusica2024/Subagents

---

## 📝 ライセンス

Private Use Only

---

**最終更新**: 2025-10-01  
**バージョン**: 1.0.3  
**作成者**: Claude Code
