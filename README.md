# クローン後の処理

```
パッケージのインストール
npm install 

起動
npm run dev
```

# GCP Google App Engine デプロイ方法

- 日付:2022/10/18

- Google App Engineへのデプロイテスト

- 目的:next.jsで作成した[飼い主検定]を無料枠で公開したい

[GCP](https://console.cloud.google.com/)

[Google cloud CLIをインストール(初期設定)](https://cloud.google.com/sdk/docs/install-sdk)

[app.ymlファイルの構成(本番用ではない)](https://cloud.google.com/appengine/docs/flexible/nodejs/configuring-your-app-with-app-yaml)

[これでいけるかわからないyoutube](https://www.youtube.com/watch?v=C_9lpcfDCHs)

[502 Bad Gateway](https://cloud.google.com/endpoints/docs/openapi/troubleshoot-response-errors)

[参考資料:Google App Engine（GAE）とは 概要や機能、活用事例を5分で入門](https://cloud-ace.jp/column/detail251/)

[GAE（Google App Engine）を使ってみた](https://zenn.dev/seyama/articles/40f74a7320ea5b)

[Google App Engine(GAE)を無料枠で収めるための勘所](https://www.serversus.work/topics/p1uaj4jrv8b5x70hwe6p/)

[GAEがいきなり502 Server Errorで落ちた](https://oisham.hatenablog.com/entry/2018/11/10/111546)

[コスト管理の自動レスポンスの例](https://cloud.google.com/appengine/docs/flexible/nodejs/testing-and-deploying-your-app?hl=ja)

## デプロイコマンド
```
gcloud app deploy
```

デプロイURL

https://gcloud-test-365910.dt.r.appspot.com/


## デプロイせずにテストurlを作成するコマンド

```bush
gcloud app deploy --no-promote
```

## ブラウザを起動
```
gcloud app browse
```

## -作業の様子-

- Google CLIの設定時(ターミナル)

```txt
Welcome to the Google Cloud CLI!

To help improve the quality of this product, we collect anonymized usage data
and anonymized stacktraces when crashes are encountered; additional information
is available at <https://cloud.google.com/sdk/usage-statistics>. This data is
handled in accordance with our privacy policy
<https://cloud.google.com/terms/cloud-privacy-notice>. You may choose to opt in this
collection now (by choosing 'Y' at the below prompt), or at any time in the
future by running the following command:

    gcloud config set disable_usage_reporting false

Do you want to help improve the Google Cloud CLI (y/N)?  

To install or remove components at your current SDK version [405.0.0], run:
  $ gcloud components install COMPONENT_ID
  $ gcloud components remove COMPONENT_ID

To update your SDK installation to the latest version [406.0.0], run:
  $ gcloud components update


Modify profile to update your $PATH and enable shell command completion?

Do you want to continue (Y/n)?  

The Google Cloud SDK installer will now prompt you to update an rc file to bring
 the Google Cloud CLIs into your environment.

Enter a path to an rc file to update, or leave blank to use 
[/Users/masa/.zshrc]:  

==> Start a new shell for the changes to take effect.


For more information on how to get started, please visit:
  https://cloud.google.com/sdk/docs/quickstarts
```


- gcloudとプロジェクトの紐付け
```
Welcome! This command will take you through the configuration of gcloud.

Your current configuration has been set to: [default]

You can skip diagnostics next time by using the following flag:
  gcloud init --skip-diagnostics

Network diagnostic detects and fixes local network connection issues.
Checking network connection...done.                                            
Reachability Check passed.
Network diagnostic passed (1/1 checks passed).

You must log in to continue. Would you like to log in (Y/n)?  

Please enter numeric choice or text value (must exactly match list item):  
Please enter a value between 1 and 23, or a value present in the list:  

Your current project has been set to: [gcloud-test-365910].

Not setting default zone/region (this feature makes it easier to use
[gcloud compute] by setting an appropriate default value for the
--zone and --region flag).
See https://cloud.google.com/compute/docs/gcloud-compute section on how to set
default compute region and zone manually. If you would like [gcloud init] to be
able to do this for you the next time you run it, make sure the
Compute Engine API is enabled for your project on the
https://console.developers.google.com/apis page.

Created a default .boto configuration file at [/Users/masa/.boto]. See this file and
[https://cloud.google.com/storage/docs/gsutil/commands/config] for more
information about configuring Google Cloud Storage.
Your Google Cloud SDK is configured and ready to use!

* Commands that require authentication will use masa455.wv@gmail.com by default
* Commands will reference project `gcloud-test-365910` by default
Run `gcloud help config` to learn how to change individual settings

This gcloud configuration is called [default]. You can create additional configurations if you work with multiple accounts and/or projects.
Run `gcloud topic configurations` to learn more.

Some things to try next:

* Run `gcloud --help` to see the Cloud Platform services you can interact with. And run `gcloud help COMMAND` to get help on any gcloud command.
* Run `gcloud topic --help` to learn about advanced features of the SDK like arg files and output formatting
* Run `gcloud cheat-sheet` to see a roster of go-to `gcloud` commands.
```


- "app.yml"ファイルがなかった時のエラー
```
910
ERROR: (gcloud.app.create) The project [gcloud-test-365910] already contains an App Engine application in region [asia-northeast2].  You can deploy your application using `gcloud app deploy`.

ERROR: An app.yaml (or appengine-web.xml) file is required to deploy this directory as an App Engine application. Create an app.yaml file using the directions at https://cloud.google.com/appengine/docs/flexible/python/configuring-your-app-with-app-yaml (App Engine flexible environment) or https://cloud.google.com/appengine/docs/standard/python/config/appref (App Engine standard environment) under the tab for your language.
ERROR: (gcloud.app.deploy) [/Users/masa/Desktop/gcloud_test] could not be identified as a valid source directory or file.

 [1] Re-initialize this configuration [default] with new settings 
 [2] Create a new configuration
```

- "yml"ファイルの"runtime"がおかしい時

 これは"runtime"だけじゃなくて"ymlファイル"全体を見た方がいい

```
Do you want to continue (Y/n)?  Y

Beginning deployment of service [default]...
Created .gcloudignore file. See `gcloud topic gcloudignore` for details.
╔════════════════════════════════════════════════════════════╗
╠═ Uploading 15 files to Google Cloud Storage               ═╣
╚════════════════════════════════════════════════════════════╝
File upload done.
ERROR: (gcloud.app.deploy) INVALID_ARGUMENT: Invalid runtime 'nodejs' specified. Accepted runtimes are: [php, php55, python27, java, java7, java8, gcf preprod, gcf prod, gcf staging, go111, go112, go113, go114, go115, java11, java8 canary, nodejs10, nodejs12, nodejs14, php72, php73, php74, php55 canary, python37, python38, python39, ruby25, ruby26, ruby27]
```

- GCPで"Cloud Build"が設定されていない可能性あり

これは"ymlファイル"の設定が悪い可能性あり
```
ERROR: (gcloud.app.deploy) Error Response: [7] Access Not Configured. Cloud Build has not been used in project gcloud-test-365910 before or it is disabled. Enable it by visiting https://console.developers.google.com/apis/api/cloudbuild.googleapis.com/overview?project=gcloud-test-365910 then retry. If you enabled this API recently, wait a few minutes for the action to propagate to our systems and retry.
```

- これは"ymlファイル"の設定が悪い可能性あり
```
ERROR: (gcloud.app.deploy) INVALID_ARGUMENT: Instance class (F1) is only allowed with the automatic scaling value.
- '@type': type.googleapis.com/google.rpc.BadRequest
  fieldViolations:
  - description: Instance class (F1) is only allowed with the automatic scaling value.
    field: version.instance_class
```


## ブラウザが"402 Bad Geteway"の場合

- "GCP"の"App Engine"のページでエラーログがあるので参考にするといいかも

- ローカル環境で初回buildができていない可能性があるのでbuildしてください


## デプロイ方法

作業ブランチ：feature/~~~

確認ブランチ：develop

デフォルトブランチ：main

## github actions 設定

[GitHub Actions で GAE に自動デプロイ](https://curucuru.co.jp/blog/20220922-github-actions-cd)

[【GAE】githubへのpushをトリガーに自動デプロイを実行する](https://scrapbox.io/blockbase/%E3%80%90GAE%E3%80%91github%E3%81%B8%E3%81%AEpush%E3%82%92%E3%83%88%E3%83%AA%E3%82%AC%E3%83%BC%E3%81%AB%E8%87%AA%E5%8B%95%E3%83%87%E3%83%97%E3%83%AD%E3%82%A4%E3%82%92%E5%AE%9F%E8%A1%8C%E3%81%99%E3%82%8B)

[GitHub Actions でプルリクのマージでワークフローを実行する](https://qiita.com/okazy/items/7ab46f2c20ec341a2836)

[GitHub google-github-actions/deploy-appengine](https://github.com/google-github-actions/deploy-appengine)

[GitHub ワークフローをトリガーするイベント](https://docs.github.com/ja/actions/using-workflows/events-that-trigger-workflows)

[GitHub アクションのワークフロー構文](https://docs.github.com/ja/actions/using-workflows/workflow-syntax-for-github-actions#onpushpull_requestpull_request_targetpathspaths-ignore)

[GitHub アクションについて](https://docs.github.com/ja/actions/learn-github-actions/understanding-github-actions)

## GEA接続プロジェクトの切り替え

[Google Cloudデータセンターはどこにありますか？](https://kinsta.com/jp/knowledgebase/google-cloud-data-center-locations/)

[gcloudコマンドのアカウントやプロジェクトを切り替える方法](https://qiita.com/hnw/items/1e8ba8b6f64214205d89)


## GEAカスタムドメインの接続

[Google App Engine との統合](https://support.google.com/domains/answer/6009957?hl=ja&ref_topic=9142939)

[GAEへのカスタムドメインとロードバランサ](https://recruit.gmo.jp/engineer/jisedai/blog/gae-domain-loadbalancer/)
