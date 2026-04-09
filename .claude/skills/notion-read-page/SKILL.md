# /notion-read-page

Notionのページ内容を全て読み込むスキル。

## 使い方

```
/notion-read-page <page_url_or_id>
```

## 動作

1. 指定されたNotionページを取得
2. ページタイトル・プロパティ・本文内容を全て抽出
3. 整形して表示

## 例

```
/notion-read-page https://www.notion.so/API-Design-...
/notion-read-page 33d93e3d-9d44-8037-85a5-000bc9d1f570
```

## 使用場面

- `/notion-search` で見つけたページの詳細を確認
- 仕様書・設計ドキュメントの全体を把握
- 過去の決定事項を確認してから実装

## 対象Database

- **Knowledge Base** (知識ベース): 仕様書・設計ドキュメント
- **Implementation Log** (実装ログ): 過去の実装結果

## 制限

- 読み取り専用
