# README

## 概要

![version:fivem](https://img.shields.io/badge/version-fivem-brightgreen)  
![framework:QBCore](https://img.shields.io/badge/framework-QBCore-brightgreen)  

- [README](#readme)
	- [概要](#概要)
	- [使用機材](#使用機材)
	- [コンテナ停止](#コンテナ停止)
	- [ポート用途](#ポート用途)
	- [License Key](#license-key)
	- [mods(cfx/fivem)](#modscfxfivem)
		- [Mods list](#mods-list)
		- [setup](#setup)
		- [QBCoreの日本語翻訳](#qbcoreの日本語翻訳)
		- [Postal Code Map \& Minimap](#postal-code-map--minimap)
	- [参考](#参考)
		- [docker-compose](#docker-compose)
		- [Github/README](#githubreadme)
		- [fivem-core](#fivem-core)
			- [server.cfg](#servercfg)
				- [日本語フォント可視化](#日本語フォント可視化)
		- [fivem-database](#fivem-database)

コンテナ型仮想環境で、FiveMサーバーを構築します。  
使用するコンテナイメージは Docker 社が運営する公開レジストリの Docker Hub から取得します。  

## 使用機材

- Docker Hub
	- [fivem-fileshell](https://hub.docker.com/_/ubuntu/tags#latest)
	- [fivem-database](https://hub.docker.com/_/mysql#latest)
	- [fivem-database-admin](https://hub.docker.com/r/phpmyadmin/phpmyadmin)
	- [fivem-core](https://hub.docker.com/r/spritsail/fivem)


1. Docker のインストール

[インストール方法](https://docs.docker.jp/engine/installation/linux/index.html)  


1. Compose ファイル を編集する。

```sh
git clone https://github.com/n138-kz/fivem-server
cd fivem-server/
```
```sh
docker pull ubuntu
docker pull mysql
docker pull phpmyadmin/phpmyadmin
docker pull spritsail/fivem
docker pull spritsail/fivem:13227
```


1. 環境変数ファイル（ファイル名： `.env` ） を作成・編集する。（詳細は公式ドキュメントを参照）

```c:.env
MYSQL_ROOT_PASSWORD="mysql-password"
ROOT_PASSWORD="shroot-password"
txadmin_version="13227"
```
[txadmin:build:13227](https://forum.cfx.re/t/txadmin-v8-0-update-changelog/5312071)


1. コンテナの生成と起動

```sh
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
> │                0000                │
> │                                    │
> └────────────────────────────────────┘
> ```


1. Webブラウザで txadmin にアクセスし、セットアップ行う（初回起動時のみ）

1. txadminにアクセス
- [http://your-public-ip:40120/](http://your-public-ip:40120)
- [http://203.0.113.100:40120/](http://203.0.113.100:40120)

1. コンソールに表示されているPINコードを入力して、 `Link Account` を押下する
1. Cfx.reにログインする。（すでにログインしている場合は `CONTINUE` を押下）
1. `Discord ID` と `Backup Password` を入力し、利用規約同意にチェックし、 `Register` 押下

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

| Name | Price(Orig) | Price(JPN) | Tag | Download Src | Require SQL | Description |
| ---- | ----------- | ---------- | --- | ------------ | ----------- | ----------- |
| [Postal Code Map & Minimap](https://forum.cfx.re/t/release-postal-code-map-minimap-new-improved-v1-3/147458) | $0 | \0 | Map | [cfx.re](https://www.dropbox.com/s/lb22r7rb4gwh44o/Postal%20Code%20Map.zip?dl=0) | false | postal map |
| [ox_lib](https://github.com/overextended/ox_lib) | $0 | \0 | library | github | false | |
| [bb-dices](https://github.com/BarBaroNN/bb-dices) | $0 | \0 | Mini-game | github | false | サイコロ振り |
| [QB-Disable-Radio](https://github.com/IanToujou/QB-Disable-Radio) | $0 | \0 | Disable-Util | github | false | ラジオ無効 |
| [CL-ThermiteMission](https://github.com/NevoSwissa/CL-ThermiteMission) | $0 | \0 | Mission | github | false | テルミットミッション |
| [TimeAndDateDisplay-FiveM](https://github.com/JokeDevil/TimeAndDateDisplay-FiveM) | $0 | \0 | | github | false | 画面右上に日時を表示 |
| [cd_drawtextui](https://forum.cfx.re/t/free-release-draw-text-ui/1885313) | $0 | \0 | | github | false | 3DTextの最適化 |
| [cdn-fuel](https://github.com/CodineDev/cdn-fuel) | $0 | \0 | | github | true | 燃料システム |
| [VoiceRangeMarker](https://github.com/TomPecs/vrm) | $0 | \0 | | github | false | ボイスチャットの範囲を段階的に切り替える |
| [Staxzs skateboard](https://forum.cfx.re/t/esx-qb-standalone-staxzs-skateboard/4863487/1) | €3 (EUR) | \485.58 | | cfx.re | false | スケートボード |
| [pma-voice](https://github.com/AvarianKnight/pma-voice/releases/tag/v6.6.2) | $0 | \0 | | github | false | 声の範囲 |
| [dpemotes](https://forum.cfx.re/t/dpemotes-1-7-390-emotes-walkingstyles-keybinding-dances-expressions-and-shared-emotes/843105) | $0 | \0 | | [github](https://github.com/andristum/dpemotes) | false | Emotes / Animations |
| [SuperMarket script - Hydrus - [FREE][ESX]](https://forum.cfx.re/t/supermarket-script-hydrus-free-esx/5076338) | 0 | 0 | | cfx.re then [googledrive](https://drive.google.com/file/d/1u-IEVWjz1ABTJMA5_HLLpWgx6-lsqMca/view?usp=sharing) | false | |
| [ps-ui](https://github.com/Project-Sloth/ps-ui) | $0 | \0 | library | github | false | |
| [AV Union Heist](https://forum.cfx.re/t/av-union-heist/5139656) | $0 | \0 | Mission | [github](https://github.com/avilchiis/av_union) | false | ユニオンミッション |
| [FiveM-Clothes](https://github.com/xchopin/FiveM-Clothes/) | $0 | \0 | 服屋 | github | FiveM: Clothing Shops addon |
| [Group Jobs Tablet.](https://forum.cfx.re/t/free-esx-qb-group-jobs-tablet-rep-scripts/5041937) | $0 | \0 | Job | [github](https://github.com/Rep-Scripts/rep-tablet) | Job |

- [某有名鯖のスクリプト一覧 | ポテト](https://docs.google.com/spreadsheets/d/1Mr2r4rjVWrBoeGrOhW8OJJkt7JY_BoAO3AS8lbD-OUs/edit?usp=sharing)
- [fivem(QBcore)のおすすめのScript | Qiita](https://qiita.com/ae86jr225kei/items/d6d2f269c6e434668678)

### setup

```sh
docker compose exec -it fivem-fileshell ls -l /media/host/assets/\[mods\] /txData/QBCore_6CAFFA.base/resources/\[mods\]
docker compose exec -it fivem-fileshell cp -r /media/host/assets/\[mods\] /txData/QBCore_6CAFFA.base/resources/
docker compose exec -it fivem-fileshell ls -l /media/host/assets/\[mods\] /txData/QBCore_6CAFFA.base/resources/\[mods\]
```

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
