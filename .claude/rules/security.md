# セキュリティルール

## 認証情報の管理

- Notion Integration Token は絶対にコミットしない
- `CLAUDE.local.md` で管理し、`.gitignore` で除外
- `.mcp.json` も同様に除外

## 機密情報の扱い

- コード内にシークレット・APIキーをハードコードしない
- 環境変数や設定ファイルで管理
- Notionページに機密情報を記載する際は注意

## Notion API利用

- 権限のあるデータベースのみアクセス
- 不必要なデータベースへのアクセスは控える
- Notionの規約を守る

## ローカル環境

- `CLAUDE.local.md` は共有しない
- `.env` ファイルも ``.gitignore` 対象
