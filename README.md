# claude-notion-integrated-project

Notion MCPを統合したClaude Code開発環境。Notionを知識ベース・仕様書・実装ログとして活用します。

## 構成

```
.claude/
├── rules/       # コーディング・セキュリティルール
├── skills/      # 再利用可能なスキル（Notion連携など）
├── commands/    # スラッシュコマンド（/notion-*, /plan, /review など）
├── agents/      # サブエージェント
├── hooks/       # 自動ガードレール
└── memory/      # 長期記憶
.github/         # GitHub Actions
```

## セットアップ

### 1. Notion MCP設定（最初に実行）

```bash
claude mcp add --transport http --scope user notion https://mcp.notion.com/mcp
```

### 2. 認証

ブラウザでNotionにOAuth認証します。指示に従ってください。

### 3. Notion統合トークン設定

`CLAUDE.local.md` に以下を記載：
- Notion Integration Token
- 各データベースのID（知識ベース、タスク管理、実装ログ）

## 使い方

### 知識ベース検索
```
/notion-search "仕様書"
```

### 実装ログを記録
```
/notion-create-page "実装ログ" "機能名：XXX完了"
```

詳細は `.claude/commands/` を参照。

## ワークフロー

1. `/plan` で設計
2. `/notion-search` で関連仕様確認
3. 実装
4. `/review` でチェック
5. `/notion-create-page` で記録
6. Commit & Push
