# /pr-description

変更内容からPull Request説明文を自動生成するスキル。

## 使い方

```
/pr-description
```

## 動作

1. コミット履歴・変更ファイルを読み取る
2. PR説明文を自動生成
3. GitHub PRに貼り付け

## 出力例

```markdown
## Summary
Notion検索機能を実装しました。キーワードで知識ベースを検索できるようになります。

## Changes
- `/api/search` エンドポイント追加
- Notion MCP連携ロジック実装
- テストケース追加（検索精度95%以上）

## Testing
- 単体テスト: 全て通過
- 実装ログ: Notion に記録済み

## Related Issues
- Notion統合ルール: .claude/rules/notion-integration.md

🤖 Generated with [Claude Code](https://claude.com/claude-code)
```

## 使用例

```bash
git push origin feature-branch
# GitHubでPRを作成するとき、説明文に貼り付け
/pr-description
```

## 推奨事項

- PR作成前に `/pr-description` で内容を確認
- テスト状況・関連Issue・Notionへの記録状況を記載
