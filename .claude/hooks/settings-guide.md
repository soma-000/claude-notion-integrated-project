# Hooks・Settings ガイド

`.claude/settings.local.json` に設定されているHooks・Permissions・その他の自動化機能について。

## Hooks（自動化の要）

### 1. onStop（完了通知）
**何をするか**: Claude応答完了時にmacOS通知を送信

```
「Claude Code完了」という通知が画面右上に表示される
```

**用途**: 複数セッション並列実行時に便利。どのセッションが終わったか一目瞭然。

---

### 2. preToolUse（外部送信ガード）

**何をするか**: 危険なコマンド（curl、git push、deploy等）を実行する前に警告

```
⚠️ 外部送信コマンドを実行しようとしています。内容を確認してください
```

**重要**: ブロック（deny）ではなく警告（warn）のため、確認した上で実行可能。

**対象**: curl, wget, git push, deploy, aws, netlify など

---

### 3. postToolUse（編集後自動フォーマット + 監査ログ）

#### 3a. Prettier自動実行
- **トリガー**: Editツール使用後
- **対象**: JS/TS/JSX/TSXファイル
- **効果**: 自動でコード整形
- 前提条件: `npm install prettier` が必要

#### 3b. 監査ログ記録
- **トリガー**: Bash実行後
- **記録先**: `~/.claude/audit-YYYY-MM-DD.jsonl`
- **内容**: タイムスタンプ・コマンド・終了コード
- **検索**: `grep "特定コマンド" ~/.claude/audit-*.jsonl`

---

### 4. userPromptSubmit（コンテキスト自動注入）

**何をするか**: 入力内容を分析して、自動で関連情報を注入

**例**: X/Twitterリンクを入力 → 自動で「Notion記録を検討」というメモが追加される

**用途**: プロンプトの前処理を自動化

---

## Permissions（権限制御）

### bypassPermissions: true
- **意味**: 毎回の許可確認をスキップ（効率↑）
- **前提**: `denyRules` で危険操作をブロック

### Allow Rules（許可）
```
✅ プロジェクトフォルダ内の操作
✅ Git操作（push --force除く）
✅ npm操作
```

### Deny Rules（ブロック）
```
❌ rm -rf / （ルート削除）
❌ git push --force （強制push）
❌ chmod 777 * （パーミッション全開）
❌ sudo （管理者操作）
```

---

## MCP設定

### toolSearch: "auto:5"
- **効果**: MCPで接続したツール定義を自動圧縮
- **利点**: コンテキストの無駄を削減
- **動作**: 詳細は必要時に自動取得

---

## 環境変数

| 変数 | 値 | 用途 |
|---|---|---|
| `MAX_THINKING_TOKENS` | 31999 | 深い思考を許可（最大値） |
| `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` | 1 | Agent Teams有効化（複数Agent協調） |
| `CLAUDE_CODE_DISABLE_TERMINAL_TITLE` | 1 | ターミナルタイトル自動変更を無効化 |
| `CLAUDE_CODE_SUBAGENT_MODEL` | haiku | サブエージェントをコスト最適化 |
| `CLAUDE_CONFIG_DIR` | ~/.claude/sessions | セッションログ一元管理 |

---

## その他

### Cleanup
- **cleanupPeriodDays**: 365
- **意味**: セッションログを1年間保持（自動削除は365日後）

### Attribution
- **機能**: コミット・PR作成時に自動で `Co-Authored-By: Claude Sonnet 4.6` を付与
- **目的**: Claudeによる変更を記録・追跡可能に

---

## 注意点・やってはいけないこと

❌ **dangerouslySkipPermissions**: コンテナ外では絶対に使わない（Anthropic公式警告）

❌ **設定の過剰**: 機能を盛りすぎると不安定になる。必要最小限が吉

✅ **定期的な監査ログ確認**: `grep` で過去の実行内容を検索可能

---

## トラブルシューティング

### Prettierが実行されない
→ プロジェクトに `npm install prettier` が必要

### 通知が表示されない
→ macOS設定でClaude Code通知を許可しているか確認

### 監査ログが記録されない
→ `~/.claude/` ディレクトリの書き込み権限を確認
