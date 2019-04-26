# Hacknet Extensions Guide

## はじめに

まずはプリインストールされている`IntroExtension`を試してみることをお勧めします。
このガイドはMOD構築の参考に使用されることを想定しています。
`IntroExtension`自体は、紹介とチュートリアルです。

また、`Hacknet Extensions Discord` に参加して、
他のMOD開発者と対話したり、テスターを見つけたり、問題解決することもできます。

- [Hacknet Extensions Discord](https://discord.gg/7cHhVT9)

主な参考ファイルは次のとおりです。

- [Computer](https://pastebin.com/NwtgTdPW)
- [Mission](https://pastebin.com/WtFLbQvH)
- [Faction/Actions](https://pastebin.com/n3i1faEe)

## Getting started

まずは、あなたのHacknetインストール先のExtensionsフォルダに移動します。

```
...\Steam\steamapps\common\Hacknet\Extensions
```

Extensionsフォルダに入ったら、`BlankExtension`フォルダ全体を同フォルダにコピーし、
好きな名前に変更しましょう。

これでMODを新規作成することができます。

そのフォルダを開き、その中にある
`EDIT_ME_ExtensionInfo.xml`を`ExtensionInfo.xml`に名前を変更してください。

これにより、Hacknetを開きし`Extensions`を選択すると、
このMODが出現するようになります。（この時点では、`BlankExtension`として。）

ここからは、[ExtensionInfo.xmlの章](#extensioninfoxml)を参照し、
設定を変更しましょう。

これが終わったら、エラーの有無を確認するため、検証テストを行います。

Hacknetを開き、開発中のMODを選んだ先の画面に移動しましょう。
そこに`Run verification tests`というボタンがあるので、クリックすることで確認することができます。

このボタンを押すことに慣れておきましょう。

これであなたはミッションとコンピュータを追加するための基礎を手に入れました！

この先のガイドでは、あなたができる全てを伝授します。

# ExtensionInfo.xml

すべてのMODは、`ExtensionInfo.xml`というファイルを含む、
決まった構成のフォルダを必要とします。

このファイルはMODのメタデータを設定するために記述するものです。

[サンプル](https://pastebin.com/6fc1fpJn)

この内容をコピーし、自分のMODに合う設定に変更しましょう。

# コンピュータの基本

Hacknetのコンピュータはそれぞれ、単一のXMLファイルで定義されています。

これにタグを追加・削除したり、値を変更したりすることで、
機能やセキュリティなどが反映されます。

[そのすべての機能を完全に記述された例はこちら](https://pastebin.com/NwtgTdPW)

基礎となる開始タグは次のように記述します。

```xml
<Computer id="advExamplePC" 
          name="Extension Example PC" 
          ip="167.194.132.7"
          security="2"
          allowsDefaultBootModule="false"
          icon="chip" 
          type="1" > 
```

## security

ハッキングの難易度を`0～5`の数値で表します。
1～4までは、打開に多くのポートが必要になるでしょう。

4以上であれば、簡単にスケールアップできるよう、
他のセキュリティが自動追加されます。（推奨）

## AllowsDefaultBootModule

デフォルトで`true`になっています。
ノードに接続すると自動的にプログラムを起動して
ディスプレイモジュールに表示します。

プログラムとは、このファイルの最後に定義されたデーモンのことです。

## icon

|||||
|-|-|-|-|
|**DEFAULT**||||
|laptop|chip|kellis||
|tablet|ePhone|ePhone2||
|**LABYRINTHS**||||
|Psylance|PacificAir|Alchemist||
|DLCLaptop|DLCPC1|DLCPC2|DLCServer|

- 未定義の場合、セキュリティレベルで設定されたデフォルトが使用されます。

## type

|値|説明|
|-|-|
|1|企業(corporate)|
|2|自宅(home)|
|3|サーバー(server)|
|4 / empty|空(empty)|

# セキュリティ

これらのタグをコンピュータファイルに追加することで、
セキュリティ機能を記述できます。

## アカウント

### 管理者パスワードの設定

```xml
<adminPass pass="password" />
```

### アカウント追加

```xml
<account username="Matt" password="testpass" type="ALL" />
```

#### type

|値|説明|
|-|-|
|0 / ADMIN||
|1 / ALL|ファイルの削除が可能|
|2 / MAIL|メールアカウント専用|
|3 / MISSIONLIST|ミッションリストアカウント専用|

## セキュリティ

### ポートの設定

```xml
<ports>21, 22, 25, 80, 1433, 104, 6881, 443, 192, 554</ports>
```

|使用可能な番号|説明|対応|ワイルドカード|
|-|-|-|-|
|21|FTP protocols|FTPBounce / FTPSprint|#FTP_CRACK# / #FTP_FAST_EXE#|
|22|SSH protocols|SSHcrack|#SSH_CRACK#|
|25|SMTP protocols|SMTPoverflow|#SMTP_CRACK#|
|80|HTTP WebServer|WebServerWorm|#WEB_CRACK#|
|1433|SQL Server|SQL_MemCorrupt / SQLBufferOverflow|#SQL_CRACK#|
|104|Medical Services|KBT_PortTest / KBTPortTest|#MEDICAL_PROGRAM#|
|6881|BitTorrent|TorrentStreamInjector / trnt|#TORRENT_EXE#|
|443|HTTPS (SSL)|SSLTrojan / WIP_SSLTrojan|#SSL_EXE#|
|192|Pacific Dedicated|PacificPortcrusher|#PACIFIC_EXE#|
|554|RTSP|RTSPCrack|#RTSP_EXE#|














