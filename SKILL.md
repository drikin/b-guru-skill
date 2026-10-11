---
name: b-guru
description: B-guru (bsm.backspace.fm) — backspace.fm 有料会員向けコミュニティの読み取り・投稿・リアクションを AI エージェントから行う
version: 1.0.0
author: drikin
license: MIT
metadata:
  hermes:
    tags: [BSM, backspace.fm, community]
---

# B-guru スキル

backspace.fm 有料会員コミュニティ「B-guru」を AI エージェントから使うためのスキルです。
タイムラインの読み取り・検索・要約・投稿・リアクション・既読管理ができます。

## セットアップ（初回のみ）

### Claude Desktop / Claude Code の場合

1. [bsm.backspace.fm/settings/agent](https://bsm.backspace.fm/settings/agent) にアクセス（B-guru にログイン済みであること）
2. 「トークンを発行」でスコープを選んでトークンを発行（`bga_` で始まる文字列・**1 回だけ表示**）
3. [bsm.backspace.fm/connect](https://bsm.backspace.fm/connect) の URL（`https://bsm.backspace.fm/mcp`）を Claude のコネクタに追加
   - claude.ai: 設定 → コネクタ → カスタムコネクタを追加 → URL を貼り付け → 認証ヘッダに `Authorization: Bearer bga_...` を設定
   - Claude Desktop: Settings → Connectors → Add custom connector
4. これで「B-guru のタイムライン要約して」が動きます

### Hermes Agent の場合

`~/.hermes/config.yaml` に追加して再起動:

```yaml
mcp_servers:
  bguru:
    url: "https://bsm.backspace.fm/mcp"
    headers:
      Authorization: "Bearer bga_あなたのトークン"
```

### cURL / REST で使う場合

トークンを Bearer として `/api/agent/*` を直接叩けます。
エンドポイント一覧: `GET https://bsm.backspace.fm/api/agent/openapi.json`

## 利用規約

- **会員限定**: BSM 有料会員のトークンでのみ動作します。退会すると自動的に無効化されます
- **トークンの取り扱い**: トークンは自分のアカウントの鍵です。他人に渡さない・公開チャットに貼らない。[settings/agent](https://bsm.backspace.fm/settings/agent) でいつでも失効できます
- **投稿には `via: agent` マーク**が付きます（エージェント経由と分かる）
- **レート制限**: 読み取り 120 回/分・書き込み 10 回/時。超過すると 429 が返るので `hint` に従って待ってください

## ツール一覧

| ツール | 説明 | スコープ |
|---|---|---|
| `get_me` | 自分の user_id / スコープ / トークン期限 | read |
| `get_posts` | タイムライン取得（club/search/since_id/fav/bm フィルタ・ページング） | read |
| `get_thread` | 投稿 1 本と返信一覧 | read |
| `search_posts` | 全文検索 | read |
| `get_clubs` | 部活カタログ | read |
| `get_notifications` | 自分の通知 | read |
| `get_chat` | チャット履歴（24h 以内） | read |
| `post_message` | 新規投稿・返信（最大 5000 文字） | write |
| `edit_post` | 自分の投稿を編集（text のみ・メディアは変更不可） | write |
| `delete_post` | 自分の投稿を削除 | write |
| `add_reaction` | 絵文字リアクション付与/解除 | write |
| `mark_read` | 既読マーク | read_state |
| `toggle_bookmark` | ブックマーク | write |

## ユースケース例

詳細は [references/use-cases.md](references/use-cases.md) を参照:

- 「今週のタイムラインを要約してレポートにして」
- 「AI 部の今月の盛り上がりを分析して」
- 「このキーワードに関する過去スレッドを全部拾って議論を整理して」
- 「自分の投稿を振り返ってふりかえり記事のネタにして」

## エラーへの対処

| code | 意味 | 対処 |
|---|---|---|
| `token_expired` / `unauthorized` | トークン期限切れ・無効 | settings/agent で再発行 |
| `membership_ended` | 会員資格が確認できない | backspace.fm で会員状況を確認 |
| `rate_limited` | レート制限 | hint の秒数待って再試行 |
| `invalid_request` | 入力ミス | エラー文言を確認 |
