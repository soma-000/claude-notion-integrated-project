# CLAUDE.md

このファイルはClaude Codeがこのプロジェクトで作業する際のガイドラインです。

## プロジェクト概要

Notion MCPを統合したClaude Code開発環境。Notionを知識ベース・仕様書・実装ログとして活用し、Claude Codeが直接読み書きできる構成です。

## 技術スタック

- **言語**: TypeScript
- **ランタイム**: Node.js (v20+)
- **MCP**: Notion Official Hosted Server (`https://mcp.notion.com/mcp`)
- **GitHub**: プライベートリポジトリ管理

## Notion連携の仕組み

### 読み取り：知識ベース参照
- Notionの仕様書・設計ドキュメント・過去の決定事項をClaude Codeから検索・参照
- `/notion-search` で特定ページを検索
- `/notion-read-page` でページ内容を取得

### 書き込み：実装結果の記録
- 実装完了時に `/notion-create-page` で新規ページ作成
- `/notion-update-page` で既存ページを更新
- 重要な決定事項・実装ログをNotionに自動記録

### タスク管理（将来拡張）
- 現在：最小限のサポート
- 将来：Notionタスクから直接コード生成 → 完了時にステータス更新

## ワークフロー

```
1. Plan
   ↓
2. Notion検索で関連仕様・過去の決定を参照
   ↓
3. Code実装
   ↓
4. Review
   ↓
5. 実装結果をNotionに記録（/notion-* コマンド）
   ↓
6. Commit & Push
```

## Notion MCPルール

- **読み取り専用操作は自由に** — いつでも知識ベースを検索できる
- **書き込み前に確認** — 重要なページ更新は実行前に内容を確認する
- **テンプレート活用** — Notionのテンプレートを使って統一フォーマットで記録
- **認証情報管理** — Notion Integration Token は `CLAUDE.local.md` で管理

## セキュリティ

- Notion Integration Token は絶対にコミットしない
- `.mcp.json` （MCP設定）は `.gitignore` に追加
- 本番データベースへのアクセスは慎重に

## コーディング規約

詳細は `.claude/rules/` を参照。基本原則：
- 関数は1責務のみ
- 変数名は自明に
- テスト駆動開発（TDD）推奨

## Claude Codeを使う上でのルール

- `/plan` で設計・影響範囲を整理後、実装開始
- 実装前に `/notion-search` で関連仕様を確認
- 実装後は `/review` で品質チェック
- 重要な実装は `/notion-create-page` で記録
