---
title: Windows Terminalの導入とWSLの追加
categories: [Blogging, Tutorial]
tags: [Windows Terminal, WSL]
math: true
---
# Windows Terminalとは
Microsoft社が開発しているWindows 10用のターミナルです．
2020年5月にバージョン1.0がリリースされています．
ソースコードは[Github](https://github.com/microsoft/terminal)にて公開されています．

# インストール
公式の推奨は[Microsoft Store](https://aka.ms/terminal)からインストールする方法です．

その他の方法でもインストールできます．
詳しくは[README](https://github.com/microsoft/terminal/blob/master/README.md#other-install-methods)を参照してください．

# 設定
設定を開くにはタイトルバーの \\(\lor\\) をクリックすると表示されるドロップダウンリストから`Setting`を選択するか，ショートカットキーの`Ctrl + ,`を押します．
設定は`profiles.json`を編集し，ファイルを保存することで即時反映されます．
設定に誤りがある場合には，Windows Terminal側に次の画像のように表示されます．

![Windows Terminal Error Message](/assets/img/posts/2020-08-31-windows-terminal/windows-terminal-error-message.jpg)

## Global Settings
### ウィンドウサイズの設定
```json
"initialCols": 96,
"initialRows": 28,
```

### 起動時の座標
```json
"initialPosition": "200, 100",
```

### デフォルトターミナルの設定
```json
"defaultProfile": "{GUID}",
```

### 複数のタブを同時に閉じる際の挙動
```json
"confirmCloseAllTabs": false,
```

## Profiles
`"defaults"`に記述した設定はすべてのターミナルに反映されます．
ターミナルごとに個別の設定をする場合は`"list"`内に記述します．

```json
"defaults":
{
    "fontsize": 11,
    "fontFace": "Cascadia Code",
    "backgroundImage": "<path>",
    "backgroundImageOpacity": 0.4,
},
```

## Color Schemes
カラースキームを設定にするには次のように記述します．
詳しくは[Windows ターミナルの配色](https://aka.ms/terminal-color-schemes)を参照してください．
```json
"schemes: [
    {
        "name" : "Campbell",
        "cursorColor": "#FFFFFF",
        "selectionBackground": "#FFFFFF",
        "background" : "#0C0C0C",
        "foreground" : "#CCCCCC",
        "black" : "#0C0C0C",
        "blue" : "#0037DA",
        "cyan" : "#3A96DD",
        "green" : "#13A10E",
        "purple" : "#881798",
        "red" : "#C50F1F",
        "white" : "#CCCCCC",
        "yellow" : "#C19C00",
        "brightBlack" : "#767676",
        "brightBlue" : "#3B78FF",
        "brightCyan" : "#61D6D6",
        "brightGreen" : "#16C60C",
        "brightPurple" : "#B4009E",
        "brightRed" : "#E74856",
        "brightWhite" : "#F2F2F2",
        "brightYellow" : "#F9F1A5"
    },
]
```

## Key Bindings
キーバインドもカスタム可能です．
詳しくは[Windows ターミナルのカスタム キー バインド](https://aka.ms/terminal-keybindings)を参照してください．

以下ではよく使うであろうキーバインドを紹介します．

### タブ

|キーバインド|概要|
|---|---|
|Ctrl + Shift + t|デフォルトターミナルを新しいタブで開く|
|Ctrl + Shift + d|タブを複製|
|Ctrl + Tab|次のタブに移動|
|Ctrl + Shift + Tab|前のタブに移動|
|Ctrl + Shift + w|タブを閉じる|

### 画面分割

|キーバインド|概要|
|---|---|
|Alt + Shift + -|水平方向に画面を分割|
|Alt + Shift + +|垂直方向に画面を分割|
|Alt + \\(\rightarrow\\)|右へ移動|
|Alt + \\(\leftarrow\\)|左へ移動|
|Alt + \\(\uparrow\\)|上へ移動|
|Alt + \\(\downarrow\\)|下へ移動|
|Alt + Shift + \\(\rightarrow\\)|分割線を右へ移動|
|Alt + Shift + \\(\leftarrow\\)|分割線を左へ移動|
|Alt + Shift + \\(\uparrow\\)|分割線を上へ移動|
|Alt + Shift + \\(\downarrow\\)|分割線を下へ移動|


# Windows Subsystem for Linux(WSL)とは
[What is the Windows Subsystem for Linux?](https://aka.ms/wsl)より，
> The Windows Subsystem for Linux lets developers run a GNU/Linux environment -- including most command-line tools, utilities, and applications -- directly on Windows, unmodified, without the overhead of a traditional virtual machine or dualboot setup.

本稿執筆時点では`WSL2`がリリースされていますが，本稿では`WSL1`について説明します．
`WSL2`へのアップデートは[WSL 2 に更新する](https://docs.microsoft.com/ja-jp/windows/wsl/install-win10#update-to-wsl-2)を参照してください．

# Windows Subsystem for Linuxの有効化
管理者として PowerShell を開き，次のコマンドを実行します．
```shell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
実行が完了したら再起動します．

# Ubuntu 18.04 LTSのインストール
[Microsoft Store](https://aka.ms/wslstore)からLinuxディストリビューションを選択してインストールします．
今回は[Ubuntu 18.04 LTS](https://www.microsoft.com/store/apps/9N9TNGVNDL3Q)をインストールします．

# セットアップ
新しくインストールしたLinuxディストリビューションの初回起動時にセットアップが必要になります．

また，[新しい Linux ディストリビューションのユーザー アカウントとパスワードを作成](https://docs.microsoft.com/ja-jp/windows/wsl/user-support)します．

# Windows Terminalで開く
Windows TerminalからWSLを起動した際に，起動ディレクトリがWindows側のユーザディレクトリになってしまいます．
そのため，次の設定をプロファイルに追記します．

`<username>`の部分はセットアップ時に設定したユーザ名に置き換えてください．(ユーザ名が`foobar`の場合は`"//wsl$Ubuntu-18.04/home/foobar"`とします．)
```json
"list": [
    {
        "guid": "{GUID}",
        "name": "Ubuntu-18.04",
        "hidden": false,
        "source": "Windows.Terminal.Wsl",
        "startingDirectory": "//wsl$Ubuntu-18.04/home/<username>",
    }
],
```
