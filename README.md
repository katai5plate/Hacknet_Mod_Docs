# Hacknet Extensions Guide

翻訳元: https://steamcommunity.com/sharedfiles/filedetails/?id=914587661

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

