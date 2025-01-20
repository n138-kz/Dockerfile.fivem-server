# README

## 概要

![version:fivem](https://img.shields.io/badge/version-fivem-brightgreen)  
![framework:QBCore](https://img.shields.io/badge/framework-QBCore-brightgreen)  

コンテナ型仮想環境で、FiveMサーバーを構築します。  
使用するコンテナイメージは Docker 社が運営する公開レジストリの Docker Hub から取得します。  

## 事前準備

デバイスやソフトウェアは以下を用いる。  

- サーバ機（Ubuntu Server、Docker）
- Docker Hub
	- [mysql](https://hub.docker.com/_/mysql)
	- [spritsail/fivem](https://hub.docker.com/r/spritsail/fivem)

1. Docker のインストール
    
	[インストール方法](https://docs.docker.jp/engine/installation/linux/index.html)  

	<details>
		<summary>Tips for collapsed sections</summary>

	</details>

3. データ格納用ディレクトリの用意

> [!IMPORTANT]
> 以降のコマンド操作は設定したディレクトリで行うものとする。

## コンテナ作成

1. Compose ファイル を編集する。

```bash
git clone https://github.com/n138-kz/Dockerfile.git
cd Dockerfile/n138-kz/fivem-server/
```

2. 環境変数ファイル（ファイル名： `.env` ） を作成・編集する。（詳細は公式ドキュメントを参照）

```c:.env
MYSQL_ROOT_PASSWORD="mysql-password"
ROOT_PASSWORD="shroot-password"
```

## コンテナの生成と起動

```bash
docker compose up -d --build
```

> [!TIP]
> `docker compose logs -f` を実行して、`Done (*.***s)! For help, type "help"` が表示されれば起動完了。

## コンテナ停止

```bash
docker compose down
```

> [!IMPORTANT]
> ワールドデータは永続保持される設定のため、サーバ再構築などデータをすべて初期化する場合は、`docker compose down --rmi all --volumes --remove-orphans` を実行する。

> [!CAUTION]
> `docker compose down --rmi all --volumes --remove-orphans` はワールドデータも削除するため、取り扱いには十分注意すること。

## ポート用途

| Port Global | Port Local | Usage          | External | 
| ----------- | ---------- | -------------- | -------- | 
|             | 3306       | MySQL Database | false    | 
| 30120       | 30120      | FiveM          | true     | 
| 40120       | 40120      | TxAdmin        | true     | 
| 30000-65535 | 80         | MySQL Admin    | true     | 

## License Key

1. Jump to [Cfx.re Keymaster](https://keymaster.fivem.net/login)
2. Signin your account.
3. `Server Owners` > `New server`
4. Input the server display name & passing the Captcha then click `Generate`.
5. Copy Key to clipboard on the local pc.

## mods(cfx/fivem)

### QBCoreの日本語翻訳
- [QBCoreの日本語翻訳](https://gamesv.agepote.jp/download/fivemserver-jp-translation)

### Postal Code Map & Minimap

> [!WARNING]
> セットアップ完了後（`/txData/QBCoreFramework_XXXXXX.base/`フォルダが生成された後）以降に実施すること

```sh
cd /tmp && curl -OL 'https://www.dropbox.com/s/lb22r7rb4gwh44o/Postal%20Code%20Map.zip?dl=0' && unar Postal%20Code%20Map.zip && mv Postal%20Code%20Map/Server\ Resource/map /txData/QBCoreFramework_*.base/resources/
```

## 参考

### docker-compose

- [《滅びの呪文》Docker Composeで作ったコンテナ、イメージ、ボリューム、ネットワークを一括完全消去する便利コマンド](https://qiita.com/suin/items/19d65e191b96a0079417)

### Github/README

- [GitHubでQiitaの:::noteみたいな強調をする](https://qiita.com/lobmto/items/d02532134782f34c0e2f)

### fivem-core

- [【FiveM】VeZaGTAサーバーに入ったらすることと進め方](https://veza.jp/gta/tips/playserver)
- [【GTA5】QBCoreのゲーム設定やコマンド、日本語化を紹介します！【徹底解説】#admin-menu](https://gamesv.agepote.jp/game-server/qbcore-setting#admin-menu)

#### server.cfg

- [Discord Developer Portal](https://discord.com/developers/applications)
- [`sv_enforceGameBuild [build]`](https://docs.fivem.net/docs/server-manual/server-commands/#sv_enforcegamebuild-build)

##### 日本語フォント可視化

- [【GTA5】QBCoreのゲーム設定やコマンド、日本語化を紹介します！【徹底解説】#index_id9](https://gamesv.agepote.jp/game-server/qbcore-setting#index_id9)

```bash
find /txData -type f -name '*.lua' -exec sed -ie 's/SetTextFont(4)/SetTextFont(0)/g' {} \;;
```

### fivem-database
