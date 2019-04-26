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
好きな名前に変更しましょう。これでMODを新規作成することができます。

そのフォルダを開き、その中にある
`EDIT_ME_ExtensionInfo.xml`を`ExtensionInfo.xml`に名前を変更してください。

これにより、Hacknetを開きし`Extensions`を選択すると、
このMODが出現するようになります。（この時点では、`BlankExtension`として。）

ここからは、[ExtensionInfo.xmlの章](#ExtensionInfo.xml)を参照し、
設定を変更しましょう。

これが終わったら、エラーの有無を確認するため、検証テストを行います。
Hacknetを開き、開発中のMODを選んだ先の画面に移動しましょう。
そこに`Run verification tests`というボタンがあるので、クリックすることで確認することができます。
このボタンを押すことに慣れておきましょう。

これであなたはミッションとコンピュータを追加するための基礎を手に入れました！
この先のガイドでは、あなたができる全てを伝授します。







