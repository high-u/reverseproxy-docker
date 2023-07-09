# reverseproxy-docker

## 起動

```bash
docker network create reverseproxy
```

```bash
docker compose up -d
```

```bash
docker compose -f docker-compose.lowcode.yaml up -d

docker compose -f docker-compose.graphql.yaml up -d
```

## ブラウザにルート証明書を登録

### Chrome

- 『設定』
- 『プライバシーとセキュリティ』
- 『セキュリティ』
- 『デバイス証明書の管理』
- 『ログイン』
- メニュー『ファイル』→『読み込む...』
    - `./caddy/authorities/root.crt`
- 赤バツ（✕）の付いている『Caddy Local Authority - 2023 ECC Root』をダブルクリック。
- 『信頼』を開いて『この証明書を使用するとき』を『常に信頼』に。
- ウィンドウを閉じるとパスワードを聞かれるので入力。
    - 青プラス（＋）に変わる。

## 開いてみる

```bash
# open http://lowcode.priv
open https://lowcode.priv
```

```bash
# open http://hasura.graphql.priv
open https://hasura.graphql.priv
```

```bash
# open http://adminer.graphql.priv
open https://adminer.graphql.priv
```
