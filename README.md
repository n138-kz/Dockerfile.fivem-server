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
	git clone https://github.com/n138-kz/fivem-server
	cd fivem-server/
	```

2. 環境変数ファイル（ファイル名： `.env` ） を作成・編集する。（詳細は公式ドキュメントを参照）

	```c:.env
	MYSQL_ROOT_PASSWORD="mysql-password"
	ROOT_PASSWORD="shroot-password"
	```

3. コンテナの生成と起動

```bash
docker compose up -d --build 
```

> [!TIP]
> `docker compose logs -f` を実行して、下記 が表示されれば起動完了。（初回起動時のみ）
>
> ```sh
> docker compose logs -f
> ```
> ```
> ┌────────────────────────────────────┐
> │                                    │
> │     All ready! Please access:      │
> │    http://your-public-ip:40120/    │
> │    http://203.0.113.100:40120/     │
> │                                    │
> │   Use the PIN below to register:   │
> │                0001                │
> │                                    │
> └────────────────────────────────────┘
> ```

4. Webブラウザで txadmin にアクセスし、セットアップ行う（初回起動時のみ）

	1. txadminにアクセス
	- [http://your-public-ip:40120/](http://203.0.113.100:40120)
	- [http://203.0.113.100:40120/](http://203.0.113.100:40120)

	2. コンソールに表示されているPINコードを入力して、`Link Account`を押下する
	3. Cfx.reにログインする。（すでにログインしている場合は`CONTINUE`を押下）
	4. `Discord ID`と`Backup Password`を入力し、利用規約同意にチェックし、`Register`押下

	> [!TIP]
	> - [Where can I find my User/Server/Message ID?](https://support.discord.com/hc/en-us/articles/206346498-Where-can-I-find-my-User-Server-Message-ID)
	> - [ユーザー/サーバー/メッセージIDはどこで見つけられる？](https://support.discord.com/hc/ja/articles/206346498-%E3%83%A6%E3%83%BC%E3%82%B6%E3%83%BC-%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC-%E3%83%A1%E3%83%83%E3%82%BB%E3%83%BC%E3%82%B8ID%E3%81%AF%E3%81%A9%E3%81%93%E3%81%A7%E8%A6%8B%E3%81%A4%E3%81%91%E3%82%89%E3%82%8C%E3%82%8B)

5. `Review Recipe` で追加するmodを記載する

	- [あとからmod導入](#modscfxfivem)する場合は `/txData/QBCoreFramework_*.base/resources/` 配下に置く
	- セットアップ時にmod追加する場合は以下（メジャーmodのみ記載）
	
	<details>
		<summary>動作しないけどこの書式のはず（Bad Archive）</summary>
		
		- [Postal Code Map & Minimap](https://forum.cfx.re/t/release-postal-code-map-minimap-new-improved-v1-3/147458)
	
		1. Review Recipe に下記追記
		- `# Clean up`(だいたい422行目付近) より前に記載する
		- yamlの書式に合わせ記載する（インデントなど）

		```text
		- action: download_file
		  path: ./tmp/Postal_Code_Map.zip
		  url: https://www.dropbox.com/s/lb22r7rb4gwh44o/Postal%20Code%20Map.zip
		- action: unzip
		  dest: ./resources/[mods]
		  src: ./tmp/Postal_Code_Map.zip
		```

		2. server.cfg に下記追加
		```
		ensure [mods]
		```
	</details>

## コンテナ停止

```bash
docker compose down
```

> [!IMPORTANT]
> ワールドデータは永続保持される設定のため、サーバ再構築などデータをすべて初期化する場合は、下記コマンド を実行する。
> ```
> docker compose down --rmi all --volumes --remove-orphans
> ```

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

### Mods list

| Name | Price(Orig) | Price(JPN) | Tag | Download Src | Description |
| ---- | ----------- | ---------- | --- | ------------ | ----------- |
| [Postal Code Map & Minimap](https://forum.cfx.re/t/release-postal-code-map-minimap-new-improved-v1-3/147458) | $0 | \0 | Map | cfx.re | postal map |
| [ox_lib](https://github.com/overextended/ox_lib) | $0 | \0 | library | github | |
| [bb-dices](https://github.com/BarBaroNN/bb-dices) | $0 | \0 | Mini-game | github | サイコロ振り |
| [QB-Disable-Radio](https://github.com/IanToujou/QB-Disable-Radio) | $0 | \0 | Disable-Util | github | ラジオ無効 |
| [CL-ThermiteMission](https://github.com/NevoSwissa/CL-ThermiteMission) | $0 | \0 | Mission | github | テルミットミッション |
| [TimeAndDateDisplay-FiveM](https://github.com/JokeDevil/TimeAndDateDisplay-FiveM) | $0 | \0 | | github | 画面右上に日時を表示 |
| [cd_drawtextui](https://forum.cfx.re/t/free-release-draw-text-ui/1885313) | $0 | \0 | | github | 3DTextの最適化 |
| [cdn-fuel](https://github.com/CodineDev/cdn-fuel) | $0 | \0 | | github | 燃料システム |
| [Staxzs skateboard](https://forum.cfx.re/t/esx-qb-standalone-staxzs-skateboard/4863487/1) | €3 | \485.58 | | cfx.re | スケートボード |
| [VoiceRangeMarker](https://github.com/TomPecs/vrm) | $0 | \0 | | github | ボイスチャットの範囲を段階的に切り替える |

- [某有名鯖のスクリプト一覧 | ポテト](https://docs.google.com/spreadsheets/d/1Mr2r4rjVWrBoeGrOhW8OJJkt7JY_BoAO3AS8lbD-OUs/edit?usp=sharing)

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
