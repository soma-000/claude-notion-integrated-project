# /commit-message

変更内容を自動でConventional Commits形式に変換するスキル。

## 使い方

```
/commit-message
```

## 動作

1. ステージされた変更を読み取る
2. Conventional Commits形式でメッセージを生成
3. 提案されたメッセージをコピペで使用

## 例

出力：
```
feat: Notion検索機能の実装

- キーワード検索エンドポイント /api/search を追加
- 複数キーワードでのAND検索に対応

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
```

## Conventional Commits形式

- `feat:` — 新機能
- `fix:` — バグ修正
- `refactor:` — リファクタリング
- `chore:` — ビルド・設定・ドキュメント更新
- `docs:` — ドキュメント変更

## 使用例

```bash
git add .
/commit-message
# メッセージをコピー
git commit -m "feat: ..."
```
