# reverseproxy-docker

## dnsmasq インストール （for Mac）

```bash
brew install dnsmasq
```

```text
==> Fetching dnsmasq
==> Downloading https://ghcr.io/v2/homebrew/core/dnsmasq/manifests/2.89
################################################################################################################################################################################################################################################# 100.0%
==> Downloading https://ghcr.io/v2/homebrew/core/dnsmasq/blobs/sha256:490265bd8d3e8392380fff3b0fbb4caf65f918366b5cf8c613372d21844860aa
################################################################################################################################################################################################################################################# 100.0%
==> Pouring dnsmasq--2.89.arm64_ventura.bottle.tar.gz
==> Caveats
To start dnsmasq now and restart at startup:
  sudo brew services start dnsmasq
Or, if you don't want/need a background service you can just run:
  /opt/homebrew/opt/dnsmasq/sbin/dnsmasq --keep-in-foreground -C /opt/homebrew/etc/dnsmasq.conf -7 /opt/homebrew/etc/dnsmasq.d,*.conf
==> Summary
🍺  /opt/homebrew/Cellar/dnsmasq/2.89: 10 files, 646.4KB
==> Running `brew cleanup dnsmasq`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
```

```bash
cat << EOS > /opt/homebrew/etc/dnsmasq.d/my.conf
port=53
address=/priv/127.0.0.1
bogus-priv
domain-needed
EOS
```

```bash
sudo brew services start dnsmasq
```

※ 何かやってしまったのか、すでにいる的なこと言われたから

```bash
ps aux | grep -e dnsmasq -e USER
```

```bash
sudo kill <pid>
```

Mac でのドメイン解決（ネットワークから DNS に 127.0.0.1 を追加しても良いが）

```bash
sudo mkdir -p /etc/resolver

echo "nameserver 127.0.0.1" | sudo tee /etc/resolver/priv

ping priv
ping xxx.priv
ping abc.xxx.priv

# dig は、`/etc/resolver/` を見に行かない。
# ping が失敗する場合は、DNS キャッシュの削除を試す。
```

（Docker で dnsmasq を動かそうとしたが、`udp` がホスト側からアクセスできず。。）

## リバースプロキシとお試しアプリを起動

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
