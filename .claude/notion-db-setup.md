# Notion Database セットアップガイド

X投稿閉ループシステム用の3つのDatabaseを作成・設定する手順です。

---

## 概要

既に作成済みの3つのDatabaseを「コンテンツ生成」用に再構成します：

| Database | ID | 用途 |
|---|---|---|
| ソースDB | 33d93e3d-9d44-8037-85a5-000bc9d1f570 | アイデア・実験ログ・記事まとめ |
| Output DB | 33d93e3d-9d44-8133-9811-000ba66ddc22 | 生成済みX投稿ネタ（下書き） |
| フィードバックDB | 33d93e3d-9d44-81e0-95e5-000b6bdee204 | 投稿結果・反応分析 |

---

## 1. ソースDB（Knowledge Base） - 33d93e3d-9d44-8037-85a5-000bc9d1f570

### 目的
アイデア・実験ログ・読んだ記事・トレンド情報を集約。生成時のソース。

### プロパティ設定

| プロパティ | 型 | 説明 |
|---|---|---|
| **Title** | Text | 「◯◯を実装してみた」「Claude Code MCP完全ガイド」など |
| **Category** | Select | `実装ログ` / `記事まとめ` / `トレンド` / `失敗ログ` / `アイデア` |
| **Date** | Date | 記事作成日 or 実装日 |
| **Source URL** | URL | 元記事リンク or コード例URL |
| **Key Insight** | Text | 「何が重要か」を1〜2行で |
| **X投稿ネタ度** | Select | `高` / `中` / `低` / `未確認` |
| **Status** | Select | `新規` / `生成済み` / `投稿済み` |

### 使用例

| Title | Category | Key Insight | X投稿ネタ度 |
|---|---|---|---|
| Claude Code で Notion 自動化スクリプト書いた | 実装ログ | Pro の範囲内で十分動く。100行程度 | 高 |
| OpenAI DevDay 2024 まとめ | 記事まとめ | 他社の動き。対比で自分のアプローチを発信できる | 中 |
| Claude MCP 導入に1時間かかった話 | 失敗ログ | 失敗も発信すると反応良い傾向 | 高 |
| 「AIエージェント時代、何が必要か」という考察 | アイデア | ブログ向け。実装ネタというより思考ネタ | 低 |

### 日々のメンテナンス
- **毎日追加**：実装ログ、読んだ記事、思いついたアイデア
- **週1回確認**：「X投稿ネタ度」を振り直す
- **生成時に使用**：「ネタ度が高い」「Status = 新規」でフィルタ

---

## 2. Output DB（Implementation Log） - 33d93e3d-9d44-8133-9811-000ba66ddc22

### 目的
生成済みのX投稿ネタ（下書き）を管理。投稿実行・反応記録の拠点。

### プロパティ設定

| プロパティ | 型 | 説明 |
|---|---|---|
| **Title** | Text | X投稿タイトル案（140字以内） |
| **Post Draft** | Text | 本文ドラフト（実投稿可能な長さ） |
| **Blog Version** | Text | より深い解説版（任意） |
| **Expected Reaction** | Text | なぜ刺さるのか、の分析 |
| **Required Actions** | Checkbox list | 画像生成、コード例、リンク作成 など |
| **Source** | URL | ソースDB のリンク |
| **Status** | Select | `下書き` / `投稿済み` / `反応確認済み` |
| **Posted At** | Date | X 投稿日時 |
| **Posted URL** | URL | X のリンク |
| **Impressions** | Number | X インプレッション数 |
| **Likes** | Number | いいね数 |
| **Replies** | Number | リプライ数 |
| **Retweets** | Number | リツイート数 |
| **Feedback Note** | Text | 反応分析メモ |

### 使用例（1ページの構成）

```
Title: Claude Code で Notion 検索機能を実装してみた
Post Draft: 
  Claude Code で Notion 検索スクリプト書いてみたら、手作業が90%削減できた。
  Pro の範囲内で全部動く。MCP + TS で実装。
  実験系好きな人は試してみるといい。

Status: 下書き
Source: [ソースDB の「Claude Code で Notion 自動化スクリプト書いた」へのリンク]
```

投稿後：
```
Status: 投稿済み
Posted At: 2025-04-10
Posted URL: https://x.com/zelta_lab/status/...
Impressions: 1200
Likes: 45
Replies: 8
Feedback Note: 実装系は反応良い。技術名（Notion MCP）を入れると刺さる傾向
```

### 日々のメンテナンス
- **生成直後**：新規ページ作成、ドラフト貼り付け
- **投稿直前**：Status を「投稿済み」に、URL記載
- **24h後**：反応数入力、フィードバック分析

---

## 3. フィードバックDB（Tasks） - 33d93e3d-9d44-81e0-95e5-000b6bdee204

### 目的
投稿結果・反応パターン分析。次の生成に反映するデータ基盤。

### プロパティ設定

| プロパティ | 型 | 説明 |
|---|---|---|
| **Title** | Text | 投稿タイトル（Output DBと同じ） |
| **Post Content** | Text | 投稿本文 |
| **Posted Date** | Date | 投稿日 |
| **Impressions** | Number | インプレッション数 |
| **Total Engagement** | Formula | `Likes + Replies + Retweets` |
| **Engagement Rate** | Formula | `Total Engagement / Impressions * 100` |
| **Pattern** | Select | `実装系` / `発見系` / `思考系` / `失敗系` / `その他` |
| **Used Expressions** | Text | 使った表現（「かなりヤバい」「実装してみた」など） |
| **Reaction Summary** | Text | 反応の特徴（「エンジニア層の反応強い」など） |
| **Improvements** | Text | 次回の改善ポイント |
| **Output DB Link** | URL | Output DB へのリンク |

### 使用例

```
Title: Claude Code で Notion 検索機能を実装してみた
Posted Date: 2025-04-10
Impressions: 1200
Likes: 45
Replies: 8
Total Engagement: 53
Engagement Rate: 4.4%

Pattern: 実装系
Used Expressions: 「実装してみた」「Pro の範囲内で」「手作業が90%削減」
Reaction Summary: エンジニア層の反応が強い。具体的な数字（90%削減）が効果的
Improvements: 次回も「Pro で十分」というニュアンスを入れる。技術名は有効
```

### 日々のメンテナンス
- **投稿24h後**：反応データを入力
- **週1回**：「Engagement Rate が高い投稿」の共通点を分析
- **生成時に参考**：「過去のフィードバック」として Voice Guide プロンプトに含める

---

## セットアップ完了チェック

```
□ ソースDB に「Category」「Date」「Key Insight」「X投稿ネタ度」を設定
□ Output DB に「Post Draft」「Status」「Posted URL」「Impressions」などを設定
□ フィードバックDB に「Pattern」「Engagement Rate」「Reaction Summary」を設定
□ 各DB の説明（Description）を記載（何に使うDB か明記）
□ Output DB と ソースDB でリンク設定（双方向）
□ フィードバックDB と Output DB でリンク設定
```

---

## 運用フロー（目安）

### 日次
- ソースDB に1〜3件追加（実装ログ・記事・アイデア）
- `/generate-content` 実行（毎朝など固定時間）

### 投稿直後
- Output DB のページ作成・ドラフト貼り付け

### 投稿24h後
- フィードバックDB に反応データ入力
- 「反応パターン」を1〜2行で分析

### 週1回（毎週末など）
- フィードバックDB を眺めて「反応が良かった投稿の共通点」をまとめる
- ソースDB の「X投稿ネタ度」を見直す

### 月1回
- Voice Guide を見直す（トーン・表現パターンを更新）
- 前月の投稿分析（最高反応数、平均 Engagement Rate など）

---

## Tips

### 「ソースDB を充実させるコツ」
- 毎日「読んだ記事」「やってみた実装」を1件は記録する癖をつける
- 「失敗ログ」も追加する（失敗も発信すると反応良い）
- 月に1〜2件は「トレンド」「思考系」のアイデアを入れる

### 「反応が出ない時」
1. フィードバックDB を眺める
2. 「反応が良かった投稿」の パターン を3〜5個抽出
3. Voice Guide の「過去のフィードバック」セクションにそれを含める
4. 再生成実行

### 「ネタが思いつかない時」
1. ソースDB を眺める
2. 「X投稿ネタ度」でフィルタ
3. 「生成済み」でないものをピックアップ
4. 再度生成実行

---

## まとめ

この3つのDB は閉ループの要。毎日のメンテナンスは5分程度。
データが溜まるほど、Voice Guide の生成精度が上がる構図。
