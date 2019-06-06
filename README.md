## Overview / 概要

It is an App that automatically saves the image uploaded to Slack channel to S3.

[日本語]

Slackのチャンネルにアップロードされた画像をS3に自動で保存するAppです。

## Why？ / なぜ作成したか？

Images will be collected as learning data used in machine learning, and image data was recruited in the company.
At that time, I started collecting it using Slack, which is adopted in-house, but there was a problem.
It took a lot of time to transfer uploaded images to AWS, which has a machine learning basis.
Therefore, I thought that the problem would be solved if I could upload it to S3 automatically from Slack.

[日本語]

機械学習で利用する学習データとして画像を集めることになり社内で画像データを募集しました。
その際に社内で採用されているSlackを利用して集め始めたのですが問題がありました。
それは、アップロードされた画像を機械学習の基盤があるAWSに移すのに手間がかかってしまったことです。
そこで、Slackから自動でS3にアップロードできれば問題は解消されると思い開発しました。

## Built With / 利用するシステムのリスト

 * Slack
 * Amazon API Gateway
 * AWS Lambda（Python 3.7）
 * Amazon S3
 * AWS CloudFormation
 
## System configuration / システム構成

![名称未設定ファイル (3)](https://user-images.githubusercontent.com/11880332/55383835-1e014100-5564-11e9-9cb5-a01e71893901.png)

## Getting Started / スタートガイド

### 1. Slack settings / Slackの設定

#### Create an app with Slack / Slackでappを作成する 

![FireShot Capture 235 - Slack API - Slack - https___api slack com_](https://user-images.githubusercontent.com/11880332/55311853-fd22e800-549e-11e9-9e6f-0e761dda23e4.png)

Go to [https://api.slack.com/](https://api.slack.com/) and press the "Start Building" button.

[日本語]

[https://api.slack.com/](https://api.slack.com/)へアクセスし「Start Building」ボタンを押す。

![FireShot Capture 234 - Slack API_ Applications - Slack - https___api slack com_apps_new_app=1](https://user-images.githubusercontent.com/11880332/55311888-22175b00-549f-11e9-916d-26aaf54da013.png)

Set 「App Name」and 「Development Slack Workspace」and press "Create App" button

[日本語]

「App Name」と「Development Slack Workspace」を設定し「Create App」ボタンを押す

#### Check OAuth & Permissions / OAuth & Permissionsを確認

![FireShot Capture 236 - Slack API_ Applications - Showc_ - https___api slack com_apps_AHAFTD6BA_oauth_censored](https://user-images.githubusercontent.com/11880332/55373833-63604700-5541-11e9-9171-2ce16a834e8f.jpg)

Remember "OAuth Access Token" and "Bot User OAuth Access Token"

[日本語]

"OAuth Access Token"と"Bot User OAuth Access Token"を記憶しておく

OAuth Access Token
#### Set the scope / スコープを設定する

![FireShot Capture 237 - Slack API_ Applications - Showc_ - https___api slack com_apps_AHAFTD6BA_oauth_censored](https://user-images.githubusercontent.com/11880332/55373925-bb974900-5541-11e9-8ff8-a5804fd9cf87.jpg)

Set files:read

[日本語]

files:readを設定する

### 2. AWSへ環境を構築する

Build an AWS environment from AWS Cloudformation.

Items required to build an environment with AWS Cloudformation

| parameter | Description |
|:-----------|:------------|
| OAUTH_TOKEN       | OAuth Access Token |
| BOT_TOKEN     | Bot User OAuth Access Token |
| CHANNEL_NAME       | Slack channel name |
| S3_BUCKET         | S3 bucket name |


[日本語]

AWS CloudformationからAWSの環境を構築する。

AWS Cloudformationで環境構築時に必要な項目

| パラメータ | 説明 |
|:-----------|:------------|
| OAUTH_TOKEN       | OAuth Access Token |
| BOT_TOKEN     | Bot User OAuth Access Token |
| CHANNEL_NAME       | Slackのチャンネル名 |
| S3_BUCKET         | 保存先S3のバケット名 |

### 3. Set Slack Event Subscriptions / SlackのEvent Subscriptionsを設定する

![FireShot Capture 238 - Slack API_ Applic_ - https___api slack com_apps_AHAFTD6BA_event-subscriptions_censored](https://user-images.githubusercontent.com/11880332/55373867-7ffc7f00-5541-11e9-953d-7cd5e4457cfd.jpg)

Turn on Enable Events

Set API URL in AWS API Gateway to Request URL

Verified is OK

Add "file_shared" in Add Workspace Event

[日本語]

Enable EventsをOnにする

Amazon APIGatewayに作成されているAPIのURLをRequest URLに設定する

VerifiedならOKです。

Add Workspace Eventで、"file_shared"を追加する

### 4. Confirm operation / 動作確認する

Post the image to the Slack channel and make sure that the image is uploaded to S3

[日本語]

Slackのチャンネルに画像を投稿し、S3に画像がアップロードされていることを確認する

## Point / ポイント

 * If you set the request URL for event subscription, the ownership of the URL is confirmed.
Correspondence to this confirmation

 * Slack's Events API is a specification that does not respond within 3 seconds and does a retry. Therefore, when built without a server,
If it takes time to start lambda, Events API will be retried and multiple Lambdas will be started.
It is inevitable that the serverless configuration is retried but the same process is not performed.

>  [https://api.slack.com/events-api](https://api.slack.com/events-api) 
> 
> Responding to Events
> Your app should respond to the event request with an HTTP 2xx **within three seconds.** If it does not, we'll consider the event delivery attempt failed. After a failure, we'll retry three times, backing off exponentially.

[日本語]

 * Event SubscriptionsのRequest URLを設定するとURLの所有権の確認が行われます。
この認証に対応している事

 * SlackのEvents APIは3秒以内に応答を返さないとリトライを行う仕様です。そのためサーバーレスで構築した際、
lambdaが起動するまでに時間がかかった場合にEvents APIがリトライされ複数のLambdaが起動してしまう。
サーバーレスの構成上リトライされるのは仕方ないが同じ処理が行われないようにしていること。

>  [https://api.slack.com/events-api](https://api.slack.com/events-api) 
> 
> イベントへの対応
> あなたのアプリは**3秒以内**にHTTP 2xxでイベントリクエストに応答するべきですそれがそうでなければ、我々はイベント配信の試みが失敗したとみなします。失敗した後は、3回再試行して指数関数的にバックオフします。


## License / ライセンス

This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/tomonoriminegishi/slack-upload-image-to-s3/blob/master/LICENSE) file for details.

[日本語]

このプロジェクトは MIT ライセンスの元にライセンスされています。 詳細は [LICENSE.md](https://github.com/tomonoriminegishi/slack-upload-image-to-s3/blob/master/LICENSE) をご覧ください。
