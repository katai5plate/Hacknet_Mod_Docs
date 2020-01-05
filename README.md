# Hacknet Extensions Guide

## 翻訳元
https://steamcommunity.com/sharedfiles/filedetails/?id=914587661

## コントリビュート募集中
- 翻訳を手伝っていただける人を募集中です。
- 協力していただける方は、このプロジェクトを fork し、プルリクエストを送ってください。

# 以下翻訳

## はじめに

まずはプリインストールされている`IntroExtension`を試してみることをお勧めします。
このガイドはMOD構築の参考に使用されることを想定しています。
`IntroExtension`自体は、紹介とチュートリアルです。

また、`Hacknet Extensions Discord` に参加して、
他のMOD開発者と対話したり、テスターを見つけたり、問題解決することもできます。

- [Hacknet Extensions Discord](https://discord.gg/7cHhVT9)

主な参考ファイルは次のとおりです。

- [Computer](https://github.com/katai5plate/Hacknet_Mod_Docs/blob/master/Computer.xml)
- [Mission](https://github.com/katai5plate/Hacknet_Mod_Docs/blob/master/Mission.xml)
- [Faction/Actions](https://github.com/katai5plate/Hacknet_Mod_Docs/blob/master/FactionAndActions.xml)

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

## ExtensionInfo.xml

すべてのMODは、`ExtensionInfo.xml`というファイルを含む、
決まった構成のフォルダを必要とします。

このファイルはMODのメタデータを設定するために記述するものです。

[サンプル](https://github.com/katai5plate/Hacknet_Mod_Docs/blob/master/ExtensionInfo.xml)

この内容をコピーし、自分のMODに合う設定に変更しましょう。

## コンピュータの基本

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

### security

ハッキングの難易度を`0～5`の数値で表します。
1～4までは、打開に多くのポートが必要になるでしょう。

4以上であれば、簡単にスケールアップできるよう、
他のセキュリティが自動追加されます。（推奨）

### AllowsDefaultBootModule

デフォルトで`true`になっています。
ノードに接続すると自動的にプログラムを起動して
ディスプレイモジュールに表示します。

プログラムとは、このファイルの最後に定義されたデーモンのことです。

### icon

|||||
|-|-|-|-|
|**DEFAULT**||||
|laptop|chip|kellis||
|tablet|ePhone|ePhone2||
|**LABYRINTHS**||||
|Psylance|PacificAir|Alchemist||
|DLCLaptop|DLCPC1|DLCPC2|DLCServer|

- 未定義の場合、セキュリティレベルで設定されたデフォルトが使用されます。

### type

|値|説明|
|-|-|
|1|企業(corporate)|
|2|自宅(home)|
|3|サーバー(server)|
|4 / empty|空(empty)|

## セキュリティ

これらのタグをコンピュータファイルに追加することで、
セキュリティ機能を記述できます。

### アカウント

#### 管理者パスワードの設定

```xml
<adminPass pass="password" />
```

#### アカウント追加

```xml
<account username="Matt" password="testpass" type="ALL" />
```

##### type

|値|説明|
|-|-|
|0 / ADMIN||
|1 / ALL|ファイルの削除が可能|
|2 / MAIL|メールアカウント専用|
|3 / MISSIONLIST|ミッションリストアカウント専用|

### セキュリティ詳細

#### ポートの設定

```xml
<ports>21, 22, 25, 80, 1433, 104, 6881, 443, 192, 554</ports>
```

|番号|port<br/>Remap<br/>用略称|説明|対応|ワイルドカード|
|-|-|-|-|-|
|21|ftp|FTP protocols|FTPBounce<br/>FTPSprint|#FTP_CRACK#<br/>#FTP_FAST_EXE#|
|22|ssh|SSH protocols|SSHcrack|#SSH_CRACK#|
|25|smtp|SMTP protocols|SMTPoverflow|#SMTP_CRACK#|
|80|web|HTTP WebServer|WebServerWorm|#WEB_CRACK#|
|1433|sql|SQL Server|SQL_MemCorrupt<br/>SQLBufferOverflow|#SQL_CRACK#|
|104|medical|Medical Services|KBT_PortTest<br/>KBTPortTest|#MEDICAL_PROGRAM#|
|6881|torrent|BitTorrent|TorrentStreamInjector<br/>trnt|#TORRENT_EXE#|
|443||HTTPS (SSL)|SSLTrojan<br/>WIP_SSLTrojan|#SSL_EXE#|
|192||Pacific Dedicated|PacificPortcrusher|#PACIFIC_EXE#|
|554||RTSP|RTSPCrack|#RTSP_EXE#|

##### 任意のポート番号に変更する

左辺は上の表の略称でも、ポート番号でも構いません。

```xml
<portRemap>web=1234,22=2</portRemap>
```

#### プロキシ

- `time`: 打開にかかる時間を`30 * time`秒に設定します。
- 無効にする場合は`time="-1"`を設定します。

```xml
<proxy time="2" />
```

#### PortHackに必要なポート数

- `val`: ポート数
- 100以上の場合、`INVIOLABILITY ERROR`を表示します。

```xml
<portsForCrack val="0" />
```

#### ファイアウォール

- `level`: 解決に必要な回数
- `solution`: 回答、
- `additionalTime`: ステップあたりの遅延時間
- 無効にする場合は`level="-1"`を設定します。

```xml
<firewall level="6" solution="Scypio" additionalTime="1.0"/>
```

#### 自動修復

disconnectした直後、自動でポートとスタッフをリセットします。

- `type`:
  + `basic`: 15秒ほどでリセット
  + `progress`: 管理者権限未掌握の場合、ポート・ファイアウォール・プロキシの進捗をリセットする
  + `fast`: リセットされるまでの間隔が短い
  + `none`: 無効にする
- `resetPassword`: パスワードをリセットするか
- `isSuper`: fastモードで間髪入れずパスワードをリセットするか

```xml
<admin type="fast" resetPassword="false" isSuper="false"/>
```

#### パッシブトレース

侵入ログが残存した状態でdisconnectすると、
ハッカーに逆探知され、リセット攻撃を仕掛けて来るようにします。

```xml
<tracker />
```

### 接続

#### 別のコンピュータとリンクする

scanしたとき、他のコンピュータを追加するようにします。

- `target`: コンピュータ名

```xml
<dlink target="advExamplePC_1" />
<dlink target="advExamplePC_2" />
<dlink target="advExamplePC_3" />
```

##### 位置の設定

見栄えを良くするために、
親コンピュータを中心に円をとり、子コンピュータを均等に配置します。

- `position`: `total`のうちの位置インデックス
- `total`: 親の周囲にいくつコンピュータを存在させるか
- `extraDistance`: 半径。`-0.6～0.3`の範囲推奨。最適値は`0.1`。
- `force`: 他のコンピュータと位置が重なっていても、強制的に配置します。

```xml
<dlink target="advExamplePC2" />
<positionNear target="advExamplePC2" position="1" total="3" extraDistance="0.1" force="false"/>
```

### ファイル構成

```xml
<file path="home" name="Test_File.txt">これはホームディレクトリのテストファイルです。</file>
```
```xml
<file path="home" name="asdf.txt">これは
複数行
ファイル。

左側に空白が「ない」ことに注意してください。
そうでないと、フォーマットの問題を引き起こします！</file>
```
```xml
<file path="home" name="downloadFile.txt">これはExampleMission.xmlのいくつかの目的のためのファイルです。</file>
```
```xml
<file path="home" name="changeFile.txt">これも！</file>
```
```xml
<file path="bin" name="Binary_File">#BINARY#</file>
```

`#`で囲まれた`ワイルドカード`は、
ロード時に特定の内容に自動置換されるものです。

ワイルドカードの書式は常に Screaming snake case です。

```xml
<file path="home/NewDirectory" name="Test_File">
これは、pathに何かを追加することによって新しいディレクトリを作成する大きなファイルです。

    #BINARY#
    2000文字のバイナリ文字列を返す
    
    #BINARYSMALL#
    1000文字のバイナリ文字列を返す

    #PLAYER_IP#
    プレイヤーのIPの文字列を返す

    #PLAYERNAME#
    プレイヤーの名前の文字列を返す

    #RANDOM_IP#
    生成されたランダムなIPの文字列を返す

    てすとてすと
  </file>
```
```xml
<file path="bin" name="SSHCrack.exe">#SSH_CRACK#</file>
<file path="bin" name="FTPBounce.exe">#FTP_CRACK#</file>
<file path="bin" name="WebServerWorm.exe">#WEB_CRACK#</file>
<file path="bin" name="SMTPOverflow.exe">#SMTP_CRACK#</file>
<file path="bin" name="SQLBufferOverflow.exe">#SQL_CRACK#</file>
<file path="bin" name="HexClock.exe">#HEXCLOCK_EXE#</file>
<file path="bin" name="Clock.exe">#CLOCK_PROGRAM#</file>
<file path="bin" name="Decypher.exe">#DECYPHER_PROGRAM#</file>
<file path="bin" name="DECHead.exe">#DECHEAD_PROGRAM#</file>
<file path="bin" name="KBTPortTest.exe">#MEDICAL_PROGRAM#</file>
<file path="bin" name="ThemeChanger.exe">#THEMECHANGER_EXE#</file>
<file path="bin" name="eosDeviceScan.exe">#EOS_SCANNER_EXE#</file>
<file path="bin" name="SecurityTracer.exe">#SECURITYTRACER_PROGRAM#</file>
<file path="bin" name="Tracekill.exe">#TRACEKILL_EXE#</file>
```

#### MOD専用のプログラム

- `RTSPCrack`: ポート554を解除
- `ESequencer`: `ExtensionInfo.xml`によってカスタマイズ可能なプログラム
- `OpShell`: 現在のシェル設定を保存し、あとで再展開。`-s`,`-o`のオプション。

```xml
<file path="bin" name="RTSPCrack.exe">#RTSP_EXE#</file>
<file path="bin" name="ESequencer.exe">#EXT_SEQUENCER_EXE#</file>
<file path="bin" name="OpShell.exe">#SHELL_OPENER_EXE#</file>
```

#### テーマ

```xml
<file path="sys" name="White-Theme.sys">#WHITE_THEME#</file>
<file path="sys" name="Green-Theme.sys">#GREEN_THEME#</file>
<file path="sys" name="Yellow-Theme.sys">#YELLOW_THEME#</file>
<file path="sys" name="Teal-Theme.sys">#TEAL_THEME#</file>
<file path="sys" name="Base-Theme.sys">#BASE_THEME#</file>
<file path="sys" name="LE-Theme.sys">#PURPLE_THEME#</file>
<file path="sys" name="Mint-Theme.sys">#MINT_THEME#</file>
```

#### カスタムテーマ

```xml
<customthemefile path="sys" name="Custom_x-server.sys" themePath="Themes/TestTheme2.xml"/>
```

#### 暗号化ファイル

`DECHead.exe`でトレースし、`Decypher.exe`で復号することができます。

```xml
<encryptedFile path="home" name="encrypted_File.dec" extension=".txt" ip="192.168.1.1" header="This is the header" pass="decryptionPassword">
    This generates an encrypted file that can be decrypted using the password above. It decrypts to have the extension .txt
  </encryptedFile>

  <encryptedFile path="home" name="easy_encrypted_File.dec" ip="192.168.1.1" header="This is the header">
    By simply not providing a password like this one, it can be decrypted without a password
  </encryptedFile>
  
  <encryptedFile path="home" name="asdf2.dec" ip="192.168.1.1" header="This is the header" pass="password">
    This is an encrypted file referenced in ExampleMission.xml
  </encryptedFile>
```

### デーモン

ノード上で実行されるプログラムです。

一般的にはノードごとに単一のデーモンだけ使用するのがベストですが、
好きなだけ追加することができます。

#### データベース

##### 死刑囚

事前設定されたレコードのコレクションをロードします。

```xml
<deathRowDatabase />
```

##### 大学

`People.All`から引き出されます。

```xml
<academicDatabase />
```

##### 医療

`People.All`から引き出されます。

```xml
<MedicalDatabase />
```

##### ポイントクリッカーゲーム

`People.All`から引き出されます。

```xml
<PointClicker />
```

#### ISP
```xml
<ispSystem />
```

#### アップローダー
```xml
<uploadServerDaemon name="Upload Dropbox" folder="Drop" 
                      needsAuth="false" color="204,116,212"/>
```

#### WEBサーバー(HTML)
```xml
<addWebServer name="Website Server" url="Web/ExampleWebsite/ExampleWebsite.html" />
```

#### 掲示板
```xml
<messageBoard name="Custom Board Name!">
    <thread>Docs/MessageBoardThreads/ExampleThread1.txt</thread>
    <thread>Docs/MessageBoardThreads/ExampleThread2.txt</thread>
</messageBoard>
```

#### メールサーバー
```xml
<mailServer name="Example Mail Server" color="50,237,212" generateJunk="true">
    <email recipient="mailGuy" sender="Sender Guy" subject="Adding an email!">
This is how you add emails to the mail server - logging in with someone's account
will show these just like the way the player gets emails.
    </email>
    <email recipient="mailGuy" sender="Spam" subject="amazing features">
You can have as many of these as you want
    </email>
    <email recipient="Matt" sender="Spam" subject="re: amazing features">
Different users too
    </email>
<email recipient="Matt" sender="Spam" subject="re: re: amazing features">
The generateJunk flag will specify if this server will generate junk emails for non-player accounts on it.
    </email>
  </mailServer>
```

#### ペースメーカー
- `patient`: 患者名。死亡している場合は`DEAD`。

警告: 実在の人間を殺す表現のためにこれを使用しないでください。

```xml
<HeartMonitor patient="J_Stalvern"/>
```

#### BGMチェンジャー
```xml
<SongChangerDaemon />
```

#### 一覧ページ

エントロピーのミッション一覧や、SlashBotのようなニュース記事を表示します。

```xml
<variableMissionListingServer name="example listing server" iconPath="Logo.png" articleFolderPath="Docs/ListingServerArticles" color="120,200,2" assigner="false" public="true" title="This is the rendered title of the server" />
```

CSEC風:

```xml
<missionHubServer groupName="ExTech" serviceName="Example Tech Contract Hub" missionFolderPath="Missions/Misc" themeColor="200,10,10" lineColor="255,80,80" backgroundColor="20,20,20" allowAbandon="false"/>
```

#### クレジット

クレジット用に保存されたファイルをロードします。

```xml
<CreditsDaemon Title="intro Extension Ending Credits" ButtonText="Complete" ConditionalActionSetToRunOnButtonPressPath="Actions/CreditsRunActions.xml"/>
```

#### FastActionHost (翻訳中)

An optimized Action host daemon

this daemon does nothing except host delayable actions

but it is much more efficient at doing that than other servers.

If you have lots of actions looping or in delay at once (more than 50, say) it might be worth moving them to be delay hosted on one of these if you encounter performance issues.

If you're going to use this, make sure this tag is the first one under the <Computer> tag!

```xml
<FastActionHost />
```

### eOSデバイス (翻訳中)

```xml
<eosDevice name="Deliliah's ePhone 4S" id="eosIntroPhone" icon="ePhone2" empty="true" passOverride="notAlpine">
    <note>TestNote
More text</note>
    <note>Note filenames
Note filenames are generated automatically by taking the first line of the file
(in this case "Note Filenames") and replacing spaces with underscores.</note>
    <mail username="test@jmail.com" pass="thisIstheaccountpass" />
    <mail username="test2@jmail.com" pass="YouCanHaveLotsOfThese" />
    <file path="eos/test" name="crackedFile.txt">This is mostly useful for jailbroken phones</file>
  </eosDevice>
```

Adding this section will create a second computer on load, attached to this which is an eos device, all set up, with these files on it. It'll also automatically generate some apps and save files and things for flavor.

You can have more than one eOS device per computer!

Note: The above code must be added Inside of the <Computer> tags! It's described as a part of a computer, and gets split out into it's own thing when this file is loaded in.

For example, a very simple computer with an eOS device might look like this:

```xml
<?xml version = "1.0" encoding = "UTF-8" ?>
<Computer id="eosTestComp"
     name="eOS Host Computer" 
     security="3" >

    <eosDevice name="testePhone" id="testePhone" icon="ePhone2" >
        <note>TestNote</note>
        <mail username="test@jmail.com" pass="thisIstheaccountpass" />
  </eosDevice>

</Computer>
```

### Labyrinths専用要素: コンピュータ (翻訳中)

The following Computer features require Labyrinths to work.

```xml
  <Memory>
    <Commands>
      <Command>cd /log</Command>
      <Command>rm *.log</Command>
      <Command>cd /log</Command>
      <Command>rm *.log</Command>
    </Commands>
    <Data>
      <Block>This appears in the "files" section</Block>
      <Block>
        more
        and longer, multi line notes!
        !
        asdf
      </Block>
    </Data>
    <Images>
      <Image>Images/ExampleImage.png</Image>
    </Images>
  </Memory>
```

This is the memory that will be turned into a memory dump using MemDumpGenerator.exe

```xml
  <memoryDumpFile name="testDump.md" path="home">
    <Memory>
      <Data>
        <Block>test string one</Block>
        <Block>test string two</Block>
      </Data>
      <Commands>
        <Command>connect 123.123.123.123</Command>
      </Commands>
      <Data>
        <Block>1234432</Block>
        <Block>gfdgfdgdf</Block>
        <Block>asdf</Block>
      </Data>
    </Memory>
  </memoryDumpFile>
```

Memory dump file - this can be downloaded and analyzed with MemForensics.exe

### Labyrinths専用要素: デーモン (翻訳中)

```xml
 <DHSDaemon groupName="NewFaction Hub" addsFactionPointOnMissionComplete="true" autoClearMissionsOnPlayerComplete="true" themeColor="255, 255, 161" allowContractAbbandon="false">
    <agent name="Tyson" pass="rockemsockem" color="209,74,48"/>
    <agent name="DependableSkeleton" pass="d832n3msad" color="112,198,255"/>
    <agent name="HA0" pass="kolgateryu" color="58,202,146"/>
  </DHSDaemon>
```

Creates a bibliotheque (DLC IRC server) style message board. Can be used to inject missions and messages into via the faction system.

```xml
<CustomConnectDisplayDaemon />
```

Changes the default connect display to look like the one the "ricer" had in Labyrinths

```xml
<LogoDaemon Name="Logo Display Test" ShowsTitle="true" TextColor="0, 220, 220, 200" LogoImagePath="Logo.png">
Lines
Here
Appear under
The Logo
  </LogoDaemon>
```

Displays a big logo on the front of the server with optional messages underneath. If you do not provide a logo image path, it'll display a fancy loading spinner instead.

```xml
<LogoCustomConnectDisplayDaemon logo="Logo.png" title="Logo.png" overdrawLogo="true" buttonAlignment="left" />
```

Custom connect display with a nameplate logo and title image like PacificAir had in Labyrinths.
Button alignment can be "left" "middle" or "right"

```xml
<IRCDaemon themeColor="67,204,148" name="Misc IRC Channel" needsLogin="false">
    <user name="Vegas" color="0,209,232"/>
    <user name="bprm" color="202,98,0"/>
    <user name="Care_ey" color="0,196,82"/>

    <post user="Vegas">Post IRC Messages here!</post>
    <post user="bprm">dont forget to set the users</post>
    <post user="Care_ey">Yep, this can also be used in the faction scripts to add messages etc to.</post>
    <post user="Vegas">That's so cool!</post>
  </IRCDaemon>
```

Creates an IRC daemon (without missions etc - just the chat). This can be dynamically added to later with Actions!

#### Whitelist Servers

Whitelist servers are kind of tricky. This same tag can mean that this server checks against a remote whitelist to see who to let on, or that it itself is that whitelist server. This can be a bit confusing, so i'm just going to outline a few of the cases here:

```xml
<WhitelistAuthenticatorDaemon SelfAuthenticating="false" />
```

This is your basic whitelist server "host" type - it wont check incoming connection to itself against it's list. It only serves a protective function against *other* servers.

```xml
<WhitelistAuthenticatorDaemon Remote="RemoteServerID"/>
```

This one connects to a remote host and checks against that host's whitelist.

```xml
<WhitelistAuthenticatorDaemon SelfAuthenticating="true"/>
```

This one protects itself against all connections not on the list. Generally "unbreakable" without scripts.

#### Databases

```xml
<DatabaseDaemon Permissions="private" DataType="GitCommitEntry" Foldername="database" Color="85,0,150" AdminEmailAccount="Matt@TestExtensionMail.com" AdminEmailHostID="advExamplePC" Name="Test database">

    <GitCommitEntry>
      <EntryNumber>8613</EntryNumber>
      <ChangedFiles>
        <String>Neopals.php</String>
      </ChangedFiles>
      <Message>Reverted Minor updates because it broke everything</Message>
      <UserName>T.Champer</UserName>
      <SourceIP>192.168.1.1 (Localhost)</SourceIP>
    </GitCommitEntry>
    
    <GitCommitEntry>
      <EntryNumber>8611</EntryNumber>
      <ChangedFiles>
        <String>Content.pak</String>
      </ChangedFiles>
      <Message>REMOTE COMMIT: Decreased pelvis bulge on Sparkle costume set :(</Message>
      <UserName>A.Wallin</UserName>
      <SourceIP>54.192.234.65</SourceIP>
    </GitCommitEntry>
  
  </DatabaseDaemon>
```

Database daemons display a list of records of any datatype in Hacknet. Providing no datatype will give the "API Access" screen like Pacific air had.

```
Current Datatypes:
*all basic C# datatypes from .NET*
Everything in the Hacknet codebase

Easy templated ones below:

Git Commit entry, as seen sbove

"TextRecord"
<TextRecord>
<Title>Record Title</Title>
<Data>Body Data</Data>
</TextRecord>

"OnlineAccount"
<OnlineAccount>
<ID>1234</ID>
<Username>asdf</Username>
<BanStatus>very yes</BanStatus>
<Notes>notes here</Notes>
</OnlineAccount>

"CAROData"
UserID
Headshots
Kills
Rank
Crowbars
InventoryID
BanStatus

"Account"
string ID;
string Cash;
string Bank;
string Apartments;
string Vehicles;
string PegasusVehicles;
string Rank;
string RP;
string Kills;
```

### Labyrinths専用要素: プログラム (翻訳中)

Labyrinths Programs

```xml
<file path="bin" name="TorrentStreamInjector.exe">#TORRENT_EXE#</file>
<file path="bin" name="SSLTrojan.exe">#SSL_EXE#</file>
<file path="bin" name="FTPSprint.exe">#FTP_FAST_EXE#</file>
<file path="bin" name="SignalScramble.exe">#SIGNAL_SCRAMBLER_EXE#</file>
<file path="bin" name="MemForensics.exe">#MEM_FORENSICS_EXE#</file>
<file path="bin" name="MemDumpGenerator.exe">#MEM_DUMP_GENERATOR#</file>
<file path="bin" name="PacificPortcrusher.exe">#PACIFIC_EXE#</file>
<file path="bin" name="NetmapOrganizer.exe">#NETMAP_ORGANIZER_EXE#</file>
<file path="bin" name="ComShell.exe">#SHELL_CONTROLLER_EXE#</file>
<file path="bin" name="DNotes.exe">#NOTES_DUMPER_EXE#</file>
<file path="bin" name="Tuneswap.exe">#DLC_MUSIC_EXE#</file>
```

## ミッションの基本

Hacknet missions, unlike computers, need to be defined with items in the correct order. Their parser is a bit more delicate, so be very careful when making new ones! Not changing this was part of my "No re-writes" policy for Hacknet, so some of the elements are very strict about their ordering and case sensitivity. Be careful, and test!

A complete mission file with everything discussed here in correct order can be found here: https://pastebin.com/WtFLbQvH

```xml
<mission id="testMission0" activeCheck="true" shouldIgnoreSenderVerification="false">
```
- id : the mission ID - this isn't used in-code but is sometimes useful in debugging and testing.
- activeCheck : if this is set to true, when this mission is active, Hacknet will test for this mission being complete every frame, instead of only when the player responds to an email. This is very useful for creating delays, and for situations where the player is contacted from outside an existing chain.
- shouldIgnoreSenderVerification : This prevents the check to ensure that the player responded to the right sender when checking for mission completion.

## Mission Goals
A mission can be defined with one or more goals in it. All goals added to a mission have to be complete before the mission will complete.

```xml
<goal type="filedeletion" target="advExamplePC" file="asdf.txt" path="home"/>
```
Task to delete the file on node target, at path/file

This entry asks for deletion of home/asdf.txt on machine with ID "missionTestNode"

```xml
<goal type="clearfolder" target="advExamplePC" path="home"/>
```
Task to delete ALL files on node target, in the folder at path

```xml
<goal type="filedownload" target="advExamplePC" file="downloadFile.txt" path="home"/>
```
Task to download the file at the path

This entry asks for the player to download (SCP) home/asdf.txt on machine with ID "missionTestNode"

```xml
<goal type="filechange" target="advExamplePC" file="changeFile.txt" path="home" keyword="extension" caseSensitive="true"/>
```
Task to add the text specified by keyword to the specified file. This is usually achieved by the "replace" command. It's possible to make the keyword a larger block and task the player to replace one file with another.

This entry asks for the player to add the test "extension" top the file home/asdf.txt on machine with ID "missionTestNode"

```xml
<goal type="filechange" target="advExamplePC" file="changeFile.txt" path="home" keyword="data" removal="true"/>
```
Task to remove the text specified by keyword to the specified file. This is usually achieved by the "replace" command. The extra attribute "removal" tasks the player with removing a block of text from a file. Combining this with another filechange task can require a "replacement" of text (removing some, adding a different block).

This entry asks for the player to add the test "extension" top the file home/asdf.txt on machine with ID "missionTestNode"

```xml
<goal type="getadmin" target="advExamplePC"/>
```
Task to get admin on the target system.

NOTE: This will only be 'passed' if the player responds *while they are the admin of the system*

Servers that have an "admin" field that resets the password automatically will make this mission not complete if it's reset in the time that the player takes to disconnect from the system and respond.

```xml
<goal type="getstring" target="password" />
```
Task to require a string passed in via the "Additional Info" field in the reply email screen.

This task requires the player to reply with the string "password" added.

```xml
<goal type="delay" time="10.0"/>
```
This task will not complete until [time] seconds after the first attempt to complete it has been made. This is mostly only useful for use with the "activeCheck" flag on the mission set to true.

A mission with a delay goal, that is silenced and that's set to auto-check essentially acts as a delay timer buffer between responses for missions

This can add a human-feeling response time to missions.

```xml
<goal type="hasflag" target="flagName"/>
```
This will only accept if the target flag has been set.

Mission end functions can use the function addFlags:[flagname],[flagname2],[etc] to set flags, then you can have a mission only complete if a flag is set.

Some daemons add flags when events happen. For example, a pacemaker server adds the flag "PATIENT_NAME:DEAD" when the patient dies.

```xml
<goal type="fileupload" target="advExamplePC" file="asdf.txt" path="home" destTarget="introFactionHomeNode" destPath="Drop/Uploads"/>
```
Task to upload the file on server bitMission00 at bin/target_filename.txt to server with ID "destComp" to folder bin.

The folder name Dest/Uploads is the folder that files are uploaded to when using an Upload server daemon, so it's the most useful location normally.

```xml
<goal type="fileupload" target="advExamplePC" file="asdf2.dec" path="home" destTarget="introFactionHomeNode" destPath="home" decrypt="true" decryptPass="password"/>
```
For upload tasks, adding the flag decrypt=true requires the player the decypher the file before uploading it. If that file is password protected, you must include the password

in the goal with decryptPass="password"

For uploading encrypted files - put the encrypted name here (ending with .dec usually). It doesn't matter what the filename of the decrypted one becomes

The code will check for the right content.

```xml
<goal type="AddDegree" owner="John Stalvern" degree="Masters in Digital Security" uni="Manchester University" gpa="3.0"/>
```
Task to add the degree matching the degree name and uni and GPA details for the listed owner. This task *requires* an academic server to exist.

An owner is defined by name - to add a person to the simulation, define them in the People folder and refer to them by name here

The academic server must have the following properties:

1. ID must be "academic"
2. It must contain an Academic Database Daemon

```xml
<goal type="wipedegrees" owner="John Stalvern"/>
```
Task to remove all degrees from the academic server for a specified owner. Note that for this one, you *MUST* have the ID of your database be "academic" or it wont find it.

```xml
<goal type="sendemail" mailServer="jmail" recipient="mailuser123" subject="Email Subject!"/>
```
Task to have an email sent to a specified address with a defined subject. This one requires an email be sent to mailuser123@jmail.com with the subject "Email Subject!". More specifically, it requires that file to exist in their inbox folder on the server ID "jmail" that contains a mail server.

This is mostly useful for detecting sent medical records from a medical database.

Medical records have the subject line: "MedicalRecord - Lastname_Firstname"

```xml
<goal type="getadminpasswordstring" target="advExamplePC"/>
```
Requires the player to respond with the current admin password for the linked server. This is useful for servers that change passwords - like ones with password resetting admins, and databases that allow for admin password reset.

## Mission Functions
Mission functions can be run from Actions, or from a mission starting or ending. To trigger a mission function within a mission:

```xml
<missionStart val="7">changeSong</missionStart>
```
This will activate as soon as the mission is accepted from a hub server, or it's started in an email chain. val lets you pass integer data (whole numbers with no decimal points) to the function. This is optional, and is only useful with a few of these (like changeSong and addRank, which need a number passed into them)..

```xml
<missionEnd val="1">addRank</missionEnd>
```
Same as mission start (this is also optional), but this happens when the mission is completed.

Note that this will never trigger if the mission is abandoned or otherwise lost.

### Available Mission Functions

```
setFaction:FACTION_ID
```
Used like setFaction:entropy, sets the player's current faction to the faction with the given idName

```
addRank
```
This adds val in rank to the player's current active faction.

Important note: This will fail if the player hasn't been given a faction yet! Be very careful to make sure the player has a faction assigned before calling this!

```
addRankSilent
```
Same as addRank, but it won't send a faction status update email.

```
addRankFaction:[FACTION NAME]
```
this adds the val to the specified faction. I.E: "addRankFaction:CSEC"

```
addFlags:flagname1,flagname2,flagname3,etc
```
Adds the defined (1 or more) flags to the current session. Flags can unlock missions and do other tasks.

```
removeFlags:flagname1,flagname2,flagname3,etc
```
Removes the listed flags if they exist.

```
changeSong
```
Changes to the song defined in val:
- case 1: Revolve
- case 2: The_Quickening
- case 3: TheAlgorithm
- case 4: Ryan3
- case 5: Bit(Ending)
- case 6: Rico_Puestel-Roja_Drifts_By
- case 7: out_run_the_wolves
- case 8: Irritations
- case 9: Broken_Boy
- case 10: Ryan10
- case 11: tetrameth

```
loadConditionalActions:DLC/ActionScripts/HermeticAlchemistsActions.xml
```
Loads in a conditional action set. These will be covered later in the guide.

```
setHubServer:[SERVER ID]
```
Sets the server with the given ID to be the new hub server visually on the netmap.

this adds the little spinny things around it, like CSEC has in the base game.

```
setAssetServer:[SERVER ID]
```
Sets the server with the given ID to be the new asset server visually on the netmap.

this adds the little star inside it

```
playCustomSong:[PATH_TO_SONG]
```
Fades the music into the song file specified. Needs the whole path like: Music/Song.ogg

Songs must be in Ogg Vorbis (.ogg) format

Custom songs will not play on the XNA branch!

```
playCustomSongImmediatley:[PATH_TO_SONG]
```
Plays the specified song above without the fade in - instantly snaps to it.

### Labyrinths-Only Mission Functions
```
changeSongDLC
```
Values:
- case 1: Remi2
- case 2: snidelyWhiplash
- case 3: Slow_Motion
- case 4: World_Chase
- case 5: HOME_Resonance
- case 6: Remi_Finale
- case 7: RemiDrone
- case 8: DreamHead
- case 9: Userspacelike
- case 10: CrashTrack

## Mission Continuations and Branches
To define what happens when this mission is completed, there's the nextMission tag:

```xml
 <nextMission IsSilent="false">NONE</nextMission>
```
This defines the path to the next mission file. The mission file at this path will be loaded, and email sent when the current mission is completed.

The IsSilent flag is... admittedly in a terrible place here. It silenced THIS mission, not the next one. This means that if you set IsSilent to be true here, this mission will not send and email when it is loaded.

Important detail about mission completion: For a reply to work, the mission the player is currently on my have the same email sender field as the email they reply to in their inbox. This means that if you have a silent connector mission, make sure it has the same sender filled in in the email field.

If you want there to be no mission following this one (For example, the email mission that introduces you to a hub server, set nextMission to "NONE".


### Branch missions (Optional)
Branch missions are loaded during the load step of this mission, and are held in parallel.

Every frame (if ActiveCheck) or every time an email is responded to, the mission system will check the goals of the current mission (this file), and if it doesn't pass them, it will check (in order) the goals of all the branch missions defined here. If successful, the branch mission's "nextMission" will be loaded.

```xml
 <branchMissions>
    <branch>Missions/BranchExample/TestBranchMission.xml</branch>
    <!-- You can have more than one branch mission here -->
  </branchMissions>
```
This allows you to put in branching story paths, variable responses, and allow different responses for various goals. It's a bit finicky to use, so I recommend testing all branch structures you put in very carefully.

Note that the branch mission's email will not be sent - they are all loaded in silent mode.

## Mission Postings and Emails
### Mission Postings

The "Posting" of a mission is what gets read and put up on a mission listing board like CSEC or Entropy in base Hacknet.

```xml
<posting title="Do the Extension Test Mission" reqs="someCustomFlag" requiredRank="3" >
```
This is the body text of the posting that will appear when the mission is clicked on. It should contain a basic outline, with any warnings the player needs.

Once accepted, the email should contain full details.

```xml
</posting>
```
title is the Mission title line that's displayed in the list.

reqs is a comma separated list of required flags before this mission can be accepted from a Hub server. Before the reqs and required rank are met, the mission will appear locked (like Junebug).

requiredRank is the faction rank the player needs to have before this mission is unlocked (player rank must be >= this number)

### Email

This is the data of the email the player will be sent when this mission is accepted or continued to.

```xml
<email>
    <!-- The sender field is very important for branching or silent mission chains. Even if this file you're editing isn't the email on the server the player will be responding to, it will still run it's completion checks as long as the player is responding to any email from the same sender. This doesn't affect basic and normal missions, so if you're not doing anything too fancy, don't worry about it.-->
    <sender>Matt</sender>
    <subject>Test Mission Email</subject>
    <body>This is the body of the email.
Be careful with your formatting! The Hacknet parser does not account for auto-whitespace added to the left here.
Email contents are very important - small changes in wording can dramatically change how easy or hard a mission is.
Be very conscious about how much you are hinting at and guiding the player, and understand that it will be much harder
for everyone that's not you.

Good luck,
-Matt
    </body>
    <!-- The attachments are the clickable items at the bottom of the email with a small "+" You can have as many attachments as you want, of any kind, but I recommend keeping the number small whenever possible, so the player can be focused on a single goal, and not be too overwhelmed. Even if you have no attachments, make sure you include the opening and closing tag (with nothing inside).
They are required for the formatting.-->
    <attachments>
      <note title="An example note">Experiment with note formatting!
Remember that the note space is very small, so text overflow onto new lines happens quickly.

Guide the player! Hacknet missions are very frustrating when the player has too little direction and cant continue.
      </note>
      <!-- A link takes in the idName of a computer and links to it on the netmap -->
      <link comp="missionTestNode" />

      <!-- this scans for the computer with ID "missionTestNode", then searches it's accounts for the provided username and password, and adds it to the list of known accounts for auto completion. It's a good way of providing the password to a server. -->
      <account comp="missionTestNode" user="TestUser" pass="testpass" />
      
    </attachments>
  </email>
```

## Basic Actions
Actions in Hacknet can be triggered in a few ways:

- MissionStart or MissionEnd functions
- Faction rank progressing
- ConditionalActionSets having a condition met

You'll see how to make use of these soon, but for now it's best to understand what actions can do. Here are the basic actions you can use:

```xml
<RunFunction FunctionName="changeSong" FunctionValue="6" />
```
Runs a 'mission function' - for a list of these and how to use them, see the example mission file.

```xml
<LoadMission MissionName="Missions/ExampleMission.xml" />
```
Immediately loads a mission, replacing whatever's currently there and sending the email.

```xml
<AddAsset FileName="MemForensics.exe" FileContents="#MEM_FORENSICS_EXE#" TargetComp="advExamplePC" TargetFolderpath="bin" />
```
Creates a file with the listed content on the computer with ID targetComp, in the folder path specified.

The folder specified must exist before this runs!

The file contents field here doesn't support newlines or super long content - this is recommended for just creating items with wildcard data (like programs)

For a complete list of wildcards, see the example computer file.

```xml
<CopyAsset DestFilePath="linkNode1" DestComp="advExamplePC" SourceComp="advExamplePC" SourceFileName="Test_File.txt" SourceFilePath="home" />
```
This copies a file from one computer to another.

DestFilePath + DestComp are the DESTINATION computer - where the file will end up

SourceFileName, SourceFilePath and SourceComp are the SOURCE computer - where the file exists and is taken from This will do nothing if the file does not exist to be copied.

This is very useful for adding complex files to a server after the session is created - create a new hidden server that's impossible to find, define all the files you want on there, then copy them over to where you want later.

```xml
<AddMissionToHubServer MissionFilepath="Missions/TestExtensionMission1.xml" TargetComp="advExamplePC" AssignmentTag="HA0"/>
```
This adds a new mission to a hub server. This works for both missionHubServer servers (CSEC Style), missionListingServers (Entropy style) and also DHSDaemon servers (Bibliotheque style).

**CSEC Style:**

```xml
<AddMissionToHubServer MissionFilepath="Missions/TestExtensionMission1.xml" TargetComp="advExamplePC" />
```
Bibliotheque Style: Assignment tag is who the mission starts assigned to of the server's user list. If this tag exists, the player cant claim the mission!

Note that adding a mission to an entropy style server, you can set the AssignmentTag to be "top" to add to the top of the list. Otherwise it'll be added to the bottom.

```xml
<RemoveMissionFromHubServer MissionFilepath="Missions/TestExtensionMission1.xml" TargetComp="advExamplePC" />
```
This removes the mission from the hub server, if it exists. Works for all three specified above.

```xml
<AddThreadToMissionBoard ThreadFilepath="Nodes/MessageBoardThreads/ExampleThread1.txt" TargetComp="advExamplePC" />
```
Adds a thread to a /el message board - thread file should be a UFT-8 plaintext document.

```xml
<AddIRCMessage Author="HA0" TargetComp="advExamplePC" Delay="3.0">Hey @#PLAYERNAME# - check out the new asset that got added a few lines ago.</AddIRCMessage>
<AddIRCMessage Author="DependableSkeleton" TargetComp="advExamplePC" Delay="8.0">@channel this notifies everyone!</AddIRCMessage>
```
Adds a message to a DHSDaemon or IRCDaemon IRC. Delay is seconds until the post appears.

```xml
<AddIRCMessage Author="P4rt1cul4rity" TargetComp="advExamplePC" Delay="3.0">!ATTACHMENT:note#%#Note Title#%#Note Content
Note line 2
etc</AddIRCMessage>

<AddIRCMessage Author="P4rt1cul4rity" TargetComp="advExamplePC" Delay="3.0">!ATTACHMENT:link#%#ComputerName#%#123.123.123.123</AddIRCMessage>

<AddIRCMessage Author="P4rt1cul4rity" TargetComp="advExamplePC" Delay="3.0">!ATTACHMENT:account#%#TargetCompID#%#69.58.186.118#%#admin#%#password</AddIRCMessage>
```
You can made notes, links and attachments in IRC Messages like this!

```xml
<CrashComputer TargetComp="linkNode1" CrashSource="advExamplePC" DelayHost="advExamplePC" Delay="5.5" />
```
Crashes computer with the ID TargetComp. If this is "playerComp" it will crash the player's own computer (bluscreening it) instantly.

If it's another computer, this will remove it from the netmap for 15 seconds or so, and disconnect the player if they were connected to it.

The CrashSource is the ID of a computer that the crash log will point to.

NOTE: Players are very likely to rm * entire log folders, so don't make this part of any critical path - it's almost impossible to find. Secrets only!

DelayHost is the ID of a computer that can host delayable actions. Valid computers need to have one of these daemons on them:
IRCDaemon or DLCHubServer

```xml
<DeleteFile TargetComp="advExamplePC" FilePath="bin" FileName="Binary_File" DelayHost="advExamplePC" Delay="5.5"/>
```
Deletes a file from the target computer. Does nothing if that file does not exist.

```xml
<AddConditionalActions Filepath="ExampleConditionalActionSet.xml" DelayHost="advExamplePC" Delay="5.5"/>
```
Loads in a new set of conditional actions from a file. Check out the file listed here for an explanation on what you can do with that.

```xml
 <SaveGame DelayHost="advExamplePC" Delay="20.5"/>
```
Saves the game! The player should be saved frequently, just incase their power dies or something. If you have extremely long missions without email replies or any other save triggers, use this to keep the player saved and up to date.

## Advanced Actions

These can be used just like the basic actions, but there were too many to fit in the word limit for steam guide's single chapter limit, so here are the more advanced ones:

```xml
<LaunchHackScript Filepath="HackerScripts/ExampleHack.txt" DelayHost="advExamplePC" Delay="5.5" SourceComp="advExamplePC" TargetComp="playerComp" RequireLogsOnSource="false" RequireSourceIntact="true"/>
```
Loads in the Hacker script defined below to execute 5.5 seconds from now, coming from source advExamplePC and targeting playerComp (The player node ID).

These can target whatever you like, and be ally scripts! RequireLogsOnSource means that this will only execute if the source comp has logs on it from the player deleting, moving, or copying a file. You can use the ID or the IP for the source and target - either works.

RequireSourceIntact means that if the target is missing it's networking file from /sys, the hack wont happen. This means a crippled computer cant hack you back.

```xml
<SwitchToTheme ThemePathOrName="Themes/ExampleTheme.xml" FlickerInDuration="3.0" DelayHost="advExamplePC" Delay="15.5" />
```
Switches to the listed theme. You can specify any of the core Hacknet themes by name directly :

TerminalOnlyBlack, HacknetBlue, HacknetTeal, HacknetYellow, HacknetWhite, HacknetPurple, HacknetMint, HackerGreen
Or you can specify a path to a custom theme file

```xml
<StartScreenBleedEffect AlertTitle="Title" CompleteAction="Actions/HackerScriptActions.xml" TotalDurationSeconds="12.5" DelayHost="advExamplePC" Delay="3.5">Line one
Line two
Line Three!</StartScreenBleedEffect>
```
Starts a 'screen bleed' effect! This is where the red line moves down from the top of the screen dramatically.

The title and the first 3 lines of the content of this tag are what's shown in the bottom left alert window. The CompleteAction conditional action set is run *only if* the red hits the bottom of the screen and the player runs out of time.TotalDurationSeconds is the total time it takes for the screen bleed to go from top to bottom.

I recommend syncing this up with various other conditions that allow you to cancel the effect if the player completes some task.

```xml
<CancelScreenBleedEffect DelayHost="advExamplePC" Delay="9.5" />
```
Cancels a screen bleed effect started with the tag above.

```xml
<AppendToFile DelayHost="advExamplePC" Delay="12.5" TargetComp="advExamplePC" TargetFolderpath="Whitelist" TargetFilename="list.txt">#PLAYER_IP#</AppendToFile>
```
Appends the content of the tag onto the file specified. Useful for adding extra data into things, making the world feel more dynamic, adding people to whitelists etc.

```xml
<KillExe DelayHost="advExamplePC" Delay="15.5" ExeName="ssh" />
```
Kills all exes that contain the string mentioned, or * for all.

This is case insensitive. So the following would kill "SSHCrackExe",

but changing it to "s" would kill all exes with an s in their name.

This is useful for doing things like killing ESequencer.exe once it's running,
or having a rival hacker close all of the player's shells.

```xml
<HideNode DelayHost="advExamplePC" Delay="15.5" TargetComp="advExamplePC" />
```
Hides the target computer node from the netmap if it's visible

```xml
<GivePlayerUserAccount DelayHost="advExamplePC" Delay="15.5" TargetComp="advExamplePC" Username="mailGuy" />
```
Reveals this user account to the player so it's details show up in the autocomplete options when the player goes to log in.

```xml
<ChangeIP DelayHost="advExamplePC" Delay="15.5" TargetComp="advExamplePC" NewIP="111.111.123.124" />
```
Changes the IP Address of the computer with ID in targetComp to the new IP address provided.

Set NewIP to be blank, or #RANDOM_IP# for a random one!

```xml
<HideAllNodes DelayHost="advExamplePC" Delay="21.5"/>
```
Un-reveals all visible nodes from the netmap

```xml
<ShowNode DelayHost="advExamplePC" Delay="21.5" Target="advExamplePC" />
```
Reveals a node on the netmap.

```xml
<SetLock DelayHost="advExamplePC" Delay="21.5" Module="terminal" IsLocked="true" IsHidden="false" />
```
Sets a UI module to be locked/unlocked and hidden/visible. Module names are:

terminal, ram, netmap, display

### Changing the Mail Icon
The final Action lets you change the mail Icon.

```xml
<ChangeAlertIcon Target="advExamplePC" Type="irc" DelayHost="advExamplePC" Delay="9.5"/>
```
Changes the alert icon (top right) to point to a new server!

Target is the ID of the server it should connect to.

Type can be one of 4 options: mail, irc, irchub, board

mail is a mail server, like Jmail. If you use this, the mail server that it points to MUST HAVE A MAIL ACCOUNT FOR THE PLAYER.

If it doesn't, it will crash when the player clicks on the icon and tries to go to their non-existent account.

board is a /el style message board. Whenever a message or thread is added that contains any other the above IRC mention strings, the player will be notified.

#### Labyrinths users only.
irchub is a DLCHubServer like Labyrinths uses. Labyrinths players only.

irc is a standard IRC server. Whenever an IRC log with @#PLAYERNAME# or @Channel in it is added, the player will get a notification.

IMPORTANT NOTE: Changing the type to anything other than mail is only supported with the Hacknet Labyrinths DLC installed.

### Conditional Action Sets
A conditional action set is a set of conditions that each contain a list of Actions to run if that condition is met.

You can load a conditional action set in via the Action:
```xml
<AddConditionalActions Filepath="ExampleConditionalActionSet.xml" DelayHost="advExamplePC" Delay="5.5"/>
```

And mission function:
```
loadConditionalActions:Path/To/Actions.xml
```

A complete conditional action set example file is here: https://hastebin.com/rodiyumepa.xml


### Avaliable Conditions

```xml
<OnConnect target="advExamplePC" needsMissionComplete="true" requiredFlags="decypher">
    <!-- You can put any action that was described in the Actions section here -->
</OnConnect>
```
This one is checked when the player connects to the PC with ID in the target field. needsMissionComplete and reqiuredFlags are optional additional checks that you can apply.

```xml
<HasFlags requiredFlags="decypher,otherFlag">
    <!-- Actions here -->
</HasFlags>
```
Triggers once the player has *all* of the flags in a comma separated list in requiredFlags

```xml
<OnAdminGained target="advExamplePC">
    <!-- Actions here -->
</OnAdminGained>
```
Triggered once the player gets admin access to the target machine.

```xml
<Instantly>
    <!-- Actions here -->
</Instantly>
```
This triggers immediately, as soon as this is finished loading! Actions will happen in the order they appear in this file.

```xml
<Instantly needsMissionComplete="true">
    <!--You can also have the needs mission complete flag on instantly actions-->
    <AddIRCMessage Author="DependableSkeleton" TargetComp="advExamplePC" Delay="0.3">Second, if mission complete!</AddIRCMessage>
  </Instantly>


 <DoesNotHaveFlags Flags="SomeFlag,MoreFlags">
    <!-- Actions here -->
</DoesNotHaveFlags>
```
Triggers if the player has none of the flags in the list.

```xml
<OnDisconnect target="advExamplePC">
    <!--Actions here-->
</OnDisconnect>
```
Triggers when the player disconnects from something, or connects to their own computer.

You can remove the target tag (or set it to "NONE") to have this trigger when the player disconnects from anything.

## Factions

A faction is essentially just a few things:

A name, a number representing the player's position in the faction, and a set of actions that can trigger to make changes to the game state in response to actions.

They are useful for sequencing events, launching Action sets and guiding the player to new things.

A complete example faction file can be found here: https://pastebin.com/n3i1faEe

### Defining a Faction

For a faction to be loaded it, it must be added to the IntroExtension.xml

With one of these tags:

```xml
<Faction>Path/To/Faction.xml</Faction>
```

Your faction file should start with this:

```xml
<CustomFaction name="Example Faction" id="examplefaction" playerVal="0">
```
You can change the starting player value with the playerVal field above.

From there it can contain multiple Action sets that are triggered by a faction-specific condition. Available conditions are:

```xml
<Action ValueRequired="1">
    <!-- Actions Here! -->
</Action>
```
The value required conditional action is the most basic one. This triggers once the player's value moves from below this number to equal to or more than it.

```xml
<Action Flags="decypher">
    <!-- Actions Here! -->
</Action>
```
This action trigger as soon as the player is given the "decypher" flag

```xml
<Action ValueRequired="5" Flags="decypher">
     <!-- Actions Here! -->
</Action>
```
And this one only triggers once both the player has the flag, and the score is greater than or equal to the required value

## Custom Themes

First, check out the example theme file that generates the theme in the screenshot above: https://pastebin.com/ch66vkUb

You'll notice it has a bunch of tags with the name of the color, and 3 or 4 numbers which represent the color itself, like this:

```xml
<defaultHighlightColor>255,41,63</defaultHighlightColor>
```

These numbers define a color using the premultiplied-alpha method, so: R, G, B, A

Where A is opacity and additivity. These are bytes, so they range from 0 to 255 without decimal places. This is a little complex, so I'll break it down with some examples.

- Pure black is 0, 0, 0, 255
- Pure White is 255, 255, 255, 255

If the number on the far right (alpha) is 0, the color values provided will ADD to the pixel underneath it. This lets you do highlights that brighten things behind them, and look "sort of" transparent.

This is a cool effect, and Hacknet uses it a lot, but remember to be tasteful with it - backgrounds like this make text hard to read, for example.

There are a few quirks to what the colors do, as the theme setup was added super early in development, and some things pull colors from strange places. For example, the Proxy/Firewall bars on the probe screen draw their colors from the shell and top bar colors.

### Layouts
You can change the layout of the windows in your theme with the tag:

```xml
<themeLayoutName>mint</themeLayoutName>
```
Valid layouts: blue, green, white, mint, greencompact, riptide, colamaeleon and riptide2

### Testing Functions
you can use the command "reloadtheme" if you have debug commands enabled to instantly reload the theme you currently have active and see any changes you've made.

### Custom Backgrounds
You can include your own images in the extension and reference them to be used as the background in custom themes. Note that if you want to distribute your mod through steam workshop, you'll need the rights to the images you use! Make sure you get permission.

Backgrounds should be 1920x1080px and wither .png or .jpg format.

If no background is specified, Hacknet will automatically generate one out of hex cells.

### Adding themes to your Extention

You can add your themes to computers ingame using the tag:
```xml
<customthemefile path="sys" name="Custom_x-server.sys" themePath="Path/To/Theme.xml"/>
```

Or you can auto-switch the player to it with the Action:

```xml
<SwitchToTheme ThemePathOrName="Path/To/Theme.xml" FlickerInDuration="3.0" DelayHost="advExamplePC" Delay="15.5" />
```

## Music
Custom songs can be played with the mission function "playCustomSong:[SONG_PATH]"

You can also use playCustomSongImmediatley:[SONG_PATH] to skip the fade between songs and start it immediately.

Songs must be in Ogg Vorbis (.ogg) format!

Also note that for now, songs will not work on the XNA branch - this is because the .ogg format handling code is done within FNA, so the XNA version doesn't have the code to support those files.

I'll address this when I can, but due to how few people use that branch, it's a lower priority for now. Sorry!

A strategy for this is to specify a base game song to play first, the instantly override it with your desired custom song - this way XNA players will still get a music change.

You can also do this from conditional action sets, using the "runFunction" tag.

## People System
Several daemons use the people system to populate their datasets.

The people system defines (and generates) a set of data representing a single individual that has accounts and records across multiple servers. This helps create a sense of consistency and identity within the game, and allows players to track down all of the data points about a person on various servers.

To define a person that will get loaded in and added to relevant servers, add a person descriptor file into the /People/ folder.

You can see an example person file here: https://hastebin.com/eqayupuxiv.xml

You can add as many people files as you want. Hacknet will automatically generate random people so that the list is filled up to be at least 100 people on new session load, to make the other servers feel full.

Daemons that use data form the people system:
- Databases
- Academic Daemons
- Medical Daemons
- PointClicker
- JMail junk accounts

## Websites
Websites. The porting nightmare. The buggy mess. The endless hacks.

These have been nothing but nightmares from day 1, and due to apparently all web rendering services that work cross platform being buggy as anything, many players use the -disableweb launch argument, preventing websites from rendering.
This means you need to make 100% sure that your missions are completable from these people reading the source of the website.

Websites in Hacknet are restricted to being in only one folder for security reasons, so *everything* you reference within your websites needs to be relative to Web/Cache within your extension folder.

Before you look into the files, first visit the website itself, linked below.

Your websites themselves (the HTML files) can be stored wherever you like within your extension folder, but all images and .css files you reference must be within the /Web/Cache folder (or a subfolder therein).

Note that websites have a few rendering issues (like stretching for some resolutions and themes). I'll work these kinks out as much as possible before launch, but they may remain, so try to design around them.

## Website Template
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html>
<head>
<!-- put stylesheet references here -->
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<link rel="stylesheet" href="<FILEPATH HERE>" type="text/css" charset="utf-8" />
<head>
<body>
<img src="<FILEPATH HERE>">
</body>
</html>
```

### Summary
Create your website HTML file wherever you like.

Add all images and CSS files that you want to use in it to the folder Web/Cache or a subfolder inside of it.

Reference those assets using relative paths only within the website.

Add a web server daemon to your node with the URL pointing to your html file.

Replacing Jmail, ISP servers, and the Player Computer

### Jmail
Jmail can be replaced by setting the id of any computer to be "jmail". If you aren;t changing your mail icon to something else, it will default point to this server. Note that if you want to use your newly crated server as the default mail icon destination, it MUST contain a MailServer daemon, and a mail account for the player, like so:

```xml
<account username="#PLAYERNAME#" password="reptile" type="mail" />

<mailServer name="Example Mail Server" color="50,237,212">

</mailServer>
```
Note that you can still add in extra accounts etc here as normal.

### Player Computer
The player computer can be overwritten by setting the ID of any computer to be "playerComp". Note that this is *always* the ID of the player computer, for referencing it elsewhere by ID.

There aren't any special requirements for this one, so go nuts. Note that the computer will probably not be in the normal spot on the netmap if you do this!

### ISP Servers
Make a computer with the id "ispComp" and make sure it has an ISP server on it, and this should work!

## Hacker Scripts

Following is an example hacker script with all available options listed in it:

```
config [TARGET_COMP] [SOURCE_COMP] 0.2 $#%#$
writel The 0.2 at the end of the line above this one is the baseline delay between actions in the script $#%#$
writel you can hardcode in source and target ip addresses above if you want, but this lets you use the script more dynamically. $#%#$

connect $#%#$
writel connect will connect from the source comp the the target comp $#%#$

delay 2 $#%#$
writel delay will pause execution for that many seconds. In this case, 2. $#%#$

openPort 21 $#%#$
writel Opens a port on the target! $#%#$

stopMusic $#%#$
writel Stops the music of the player, if they are the target. $#%#$

startMusic $#%#$
writel Starts the music of the player, if stopped, and they are the target. $#%#$

clearTerminal $#%#$
writel Clear the terminal of the player, if they are the target. $#%#$

write WHO $#%#$
write DO $#%#$
write YOU $#%#$
write THINK $#%#$
write YOU $#%#$
write ARE $#%#$
write ? $#%#$
writel You can write out to terminal onto the same line like that above, or a full line like this one! $#%#$

write_silent Shhhh $#%#$
writel_silent these two let you write thigns without the beeps and flashes $#%#$

writel $#%#$
writel An empty writeline just pushes out newlines to the terminal. Looks cool. $#%#$

hideNetMap $#%#$
hideRam $#%#$
hideDisplay $#%#$
writel These commands hide stuff if the player is connected to them. BE VERY CAREFUL WITH THIS - player cant get it back alone. $#%#$

showNetMap $#%#$
showRam $#%#$
showDisplay $#%#$
showTerminal $#%#$
writel These commands bring the windows back $#%#$

trackseq $#%#$
writel This flags it so that the player will have to do the ETAS if they fail the next forkbomb that they receive, if they have the CSEC flag $#%#$

instanttrace $#%#$
writel This is a brutal command that instantly starts the ETAS for the player $#%#$

forkbomb $#%#$
writel Launches a forkbomb on the target. Crashing it instantly if non-player. $#%#$

flash $#%#$
writel Flashes the UI in a dramatic way, if targeting the player $#%#$

delete /sys x-server.sys $#%#$
writel Deletes a file. You can specify any folder and file here, or * for all files in that folder. $#%#$

setAdminPass newpass  $#%#$
writel Sets the admin password of this computer to be 'newpass' or whatever you set that to. $#%#$

makeFile home Filename.txt Everything past the first space there goes into the file! $#%#$
writel Makes a new file in the folder named in the first argument (cant do subfolders, sorry) with the name FIlename.txt and the shown content $#%#$

makeFile bin SSHCrack.exe #SSH_CRACK# $#%#$
writel You can add programs like this. See the full list of wildcard names in ExampleComputer.xml $#%#$

openCDTray $#%#$
writel YEP! $#%#$

delay 5 $#%#$

closeCDTray $#%#$

disconnect $#%#$
```

## Development Debug Commands
To Autostart your mod while in development, start Hacknet with the argument
```
-extstart [YOUR_MOD_FOLDERNAME]
```

To start Hacknet on your second monitor, if you have one, use the argument
```
-altmonitor
```

You probably also want to develop with force complete on:
```
-enablefc
```

Debug commands too:
```
-enabledebug
```

Once you have Debug commands enabled:

```
fh
```
Instantly breaks into any computer

```
dscan [ID]
```
reveals a computer by ID name

```
ra
```
reveals everything on the netmap at once

```
reloadext
```
reloads all the nodes in the current extension from source files,
and updates them in the current save.

```
printflags
```
ouputs all the flags the player has to the terminal

```
runhackscript Script/Path/Name.txt
```
Replace the path with the filepath of your own hacker script to test it out - this also prints errors to the terminal.

## Publishing to Steam Workshop and the Nexus
To make the workshop publishing tools visible within Hacknet, add the launch argument:

```
-allowextpublish
```

From your extension page, you'll now be able to create the workshop item and upload your content to it. Make sure to read the Workshop information at the bottom of ExtensionInfo.xml in the example extension to know what to include!

I suggest you create it as private first, then go to your own workshop page on steam to look over it and polish it up before making it public.

Once uploaded, there's a few things you'll need to check. First, check to see if your extension requires the Labyrinths DLC or not. You can keep track of this yourself during development, but the best way to know for sure is to disable your copy of Labyrinths if you have it (uncheck it's box in the DLC section of your Hacknet library page), then run the tests on your extension and play through it.

If it does require Labyrinths, make sure you add that as a requirement on your workshop page!

### The Nexus
Once you've published to the Steam workshop, you should upload your extension to The Nexus so that others with DRM Free copies of Hacknet can play it. Upload them here: https://www.nexusmods.com/hacknet

