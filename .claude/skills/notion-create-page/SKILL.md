# /notion-create-page

Notionに新規ページを作成・実装ログを記録するスキル。

## 使い方

```
/notion-create-page <database_type> <title> <content>
```

## パラメータ

- `database_type`: 
  - `knowledge` (知識ベース)
  - `implementation` (実装ログ) ← 最も使用頻度高い
  - `task` (タスク管理)
- `title`: ページのタイトル
- `content`: ページ本文

## 例

```
/notion-create-page implementation "機能X実装完了" "APIエンドポイント /api/v1/users を実装。レート制限対応完了。"

/notion-create-page knowledge "パフォーマンス最適化ガイド" "キャッシング戦略..."

/notion-create-page task "フロントエンド実装" "ユーザー画面のUI作成"
```

## 動作

1. 指定されたDatabaseに新規ページを作成
2. タイトル・内容を記載
3. 作成日時・ステータスなどのプロパティを自動設定
4. NotionのURLを返す

## 対象Database

- **Implementation Log** (実装ログ): 実装完了時に記録（推奨）
- **Knowledge Base** (知識ベース): ドキュメント・ノウハウ追加時
- **Tasks** (タスク管理): タスク作成（将来拡張）

## 使用ルール

- 実装完了後に `/notion-create-page implementation` で自動記録
- 重要な決定事項・学習内容は `/notion-create-page knowledge` で記録
- 作成前に内容を確認すること
