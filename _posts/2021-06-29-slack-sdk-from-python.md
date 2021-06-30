---
title: Python用 Slack公式 SDK の slack_sdk を使ってシミュレーション終了の通知を画像とともにお知らせする
categories: [Blogging, Tutorial]
tags: [Python, Slack, slack_sdk]
date: 2021-06-30 00:00:00 +0900
---

# はじめに
強化学習シミュレーションが終了した際にSlackに通知させる方法を調べていたら，Slack公式のSDKがあることを知ったので使ってみました．

Slackアプリを作成する際にマニフェストを用いて作成する方法がありますが，ベータ版ということもあり本稿では使用しないので注意してください．また，Pythonでの実装を説明する際に，プロジェクトの作成や仮想環境の構築については省略させていただきます．

公式の開発者の方がQiitaに[チュートリアル記事を公開](https://qiita.com/seratch/items/8f93fd0bf815b0b1d557)しているので参照してみてください．

# やりたいこと
今回実装したい機能は以下のようなものです．

- シミュレーション終了時にSlackに通知
- シミュレーション終了時のログをメッセージとして送信
- シミュレーション結果を概観できる画像を送信

これらの目的を達成する手段として，[`WebClient.files_upload`](https://slack.dev/python-slack-sdk/api-docs/slack_sdk/web/client.html#slack_sdk.web.client.WebClient.files_upload)を使えば良さそうです．

# Slack Appを作成
[こちらのリンク](https://api.slack.com/apps)から新規アプリを作成してください．

作成したら **Features > OAuth & Permissions** にある **Scopes** セクションで，**Bot Token Scopes** に `files:write` を追加してください．

スコープの設定が完了したら **Settings > Install App** からワークスペースにインストールしてください．
このとき **Bot User OAuth Token** という名前で `xoxb-` から始まるトークンが発行されていることを確認してください．

このトークンを環境変数に設定してください．
```terminal
export SLACK_BOT_TOKEN=xoxb-
```

# Python側の実装
## 準備とテスト
まず pip で `slack-sdk` をインストールします．
```terminal
pip install slack-sdk
```

インストールできたらテストを実行してパッケージが利用できることを確認します．
```terminal
python -c 'from slack_sdk.web import WebClient
client = WebClient()
response = client.api_test()
print(response)'
```

以下のコマンドでOAuthのテストも行います．
```terminal
python -c 'import os
from slack_sdk.web import WebClient
client = WebClient(token=os.environ["SLACK_BOT_TOKEN"])
response = client.auth_test()
print(response)'
```

最後に，作成したアプリをチャンネルに追加します．
この作業をしておかないと `not_in_channel` エラーが出ます．

アプリの追加はチャンネル詳細画面からできます．

![アプリをチャンネルに追加する](/assets/img/posts/2021-06-29-slack-sdk-from-python/add-app.png){: height="450"}

## ファイルをアップロード
ファイルをアップロードするには `WebClient.files_upload` を利用します．

- [slack_sdk.web.client API Documentation](https://slack.dev/python-slack-sdk/api-docs/slack_sdk/web/client.html#slack_sdk.web.client.WebClient.files_upload)
- [files.upload method \| Slack](https://api.slack.com/methods/files.upload)

それではさっそく実装を見てみます．
エラーハンドリングなどを無視すれば以下のシンプルな実装でSlackにファイルをアップロードできます．
```python
from slack_sdk import WebClient

client = WebClient(os.environ['SLACK_BOT_TOKEN'])

response = client.files_upload(
    channels='#random',
    file='/path/to/file',
    initial_comment='This is AWESOME!!',
)
```

- `channels` は投稿先のチャンネルを指定します．
- `file` はアップロードするファイルのパスを指定します．
- `initial_comment` はアップロード時のメッセージを指定します．

うまくアップロードされるとこんな感じになります．
![ファイルをアップロード](/assets/img/posts/2021-06-29-slack-sdk-from-python/upload.png){: width="500"}

# おわりに
PythonからSlackに通知するためのライブラリがたくさんありますが，公式のSDKも使いやすくドキュメントもしっかりしているので良かったです．

そして，ファイルのアップロードも簡単にできたので，毎回SFTPでファイルを取得しなくてもシミュレーション結果を概観できるのは作業が捗りそうです．

# 参考文献
- [公式ドキュメント](https://slack.dev/python-slack-sdk/)
- [APIドキュメント](https://slack.dev/python-slack-sdk/api-docs/slack_sdk)
- [日本語のチュートリアル記事](https://qiita.com/seratch/items/8f93fd0bf815b0b1d557)
