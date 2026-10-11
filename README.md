# b-guru-skill

[B-guru](https://bsm.backspace.fm) — backspace.fm 有料会員コミュニティを AI エージェント（Claude / ChatGPT / Hermes など）から使うためのスキルです。

## できること

- 📖 タイムライン・スレッド・チャットの読み取り・検索・要約
- ✍️ 自分名義での投稿・返信（`via: agent` マーク付き）
- 😀 絵文字リアクション・ブックマーク・既読管理
- 📊 部活ごとの盛り上がり分析・ふりかえり記事のネタ収集

## セットアップ

1. **トークン発行**: [bsm.backspace.fm/settings/agent](https://bsm.backspace.fm/settings/agent) にアクセス（B-guru 会員ログイン済み）→ トークンを発行（`bga_` で始まる文字列・1 回だけ表示）
2. **接続**:
   - **Claude**: [bsm.backspace.fm/connect](https://bsm.backspace.fm/connect) の案内に従ってコネクタ URL `https://bsm.backspace.fm/mcp` を Claude に追加（認証ヘッダにトークン）
   - **Hermes**: `~/.hermes/config.yaml` に `mcp_servers: bguru: {url, headers: {Authorization}}` を追加して再起動
   - **cURL/REST**: `Authorization: Bearer bga_...` で `/api/agent/*` を直接呼び出し
3. 詳細は [SKILL.md](SKILL.md) と [references/](references/) を参照

## 要件

- backspace.fm の**有料会員**であること（退会するとトークンは自動失効します）
- トークンは他人に渡さない・公開しない（[settings/agent](https://bsm.backspace.fm/settings/agent) でいつでも失効できます）

## 構成

```
SKILL.md               # スキル本体（セットアップ・ツール一覧・規約）
references/api.md      # REST API リファレンス
references/use-cases.md# ユースケース集（要約・分析・収集レシピ）
```

## ライセンス

MIT
