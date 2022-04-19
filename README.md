# my-home-server

## 手順

### treafik

```sh
% docker-compose -f traefik/docker-compose.yml --env-file ./.env up -d
```

### MinIO

```sh
% docker-compose -f minio/docker-compose.yml --env-file ./.env up -d
```

### gitea

```sh
% docker-compose -f gitea/docker-compose.yml --env-file ./.env up -d
```

- 設定したドメインにアクセス
- 「初期設定」のページが表示される
- 「オプション設定」→「管理者アカウントの設定」の項目を入力
- 「Gitea をインストール」を押す
- 注）「Bad Gateway」が出ることがあるが問題なく動く

### drone

- **gitea**の UI にアクセス
- 右上のアイコン →「設定」→「アプリケーション」→「OAuth2 アプリケーションの管理」に以下の項目を入力
  - アプリケーション名: （自由）
  - リダイレクト URI: `http://<droneのドメイン>/login`
- アプリケーション作成
- 以下の値を`.env`に記載
  - 「クライアント ID」: `DRONE_GITEA_CLIENT_ID`
  - 「クライアント シークレット」: `DRONE_GITEA_CLIENT_SECRET`

```sh
% docker-compose -f drone/docker-compose.yml --env-file ./.env up -d
```

## MEMO

### docker-compose コマンド

- ファイル指定オプション: `-f`
  - https://docs.docker.jp/compose/reference/docker-compose.html
- 環境変数ファイル指定オプション: `--env-file`
  - https://matsuand.github.io/docs.docker.jp.onthefly/compose/environment-variables/#using-the---env-file--option
  - `config`コマンドで埋め込みの確認も可能

### Treafik

- [Docker 環境でドメインごとにアクセスするコンテナを切り替える（複数ポートも可能） - Qiita](https://qiita.com/error484/items/d5ff9da8385ea2ee720a?utm_source=pocket_mylist)
- [Docker - Traefik](https://doc.traefik.io/traefik/routing/providers/docker/#port)
- [【入門】Traefik とは？基本的な使い方を解説 - エンジニアの本棚](https://coders-shelf.com/traefik-intro/?utm_source=pocket_mylist)
- `8080`ポート: 管理画面

### MinIO

- https://docs.min.io/docs/minio-docker-quickstart-guide.html
- [MinIO Server — MinIO Baremetal Documentation](https://docs.min.io/minio/baremetal/reference/minio-server/minio-server.html#minio.server.-address)

### gitea

- 環境変数一覧: [Config Cheat Sheet - Docs](https://docs.gitea.io/en-us/config-cheat-sheet/#server-server)
