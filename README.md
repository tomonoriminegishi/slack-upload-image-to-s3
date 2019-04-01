## overview / 概要

It is an App that automatically saves the image uploaded to Slack channel to S3.

Slackのチャンネルにアップロードされた画像をS3に自動で保存するAppです。

## Why？ / なぜ作成したか？

Images will be collected as learning data used in machine learning, and image data was recruited in the company.
At that time, I started collecting it using Slack, which is adopted in-house, but there was a problem.
It took a lot of time to transfer uploaded images to AWS, which has a machine learning basis.
Therefore, I thought that the problem would be solved if I could upload it to S3 automatically from Slack.

機械学習で利用する学習データとして画像を集めることになり社内で画像データを募集しました。
その際に社内で採用されているSlackを利用して集め始めたのですが問題がありました。
それは、アップロードされた画像を機械学習の基盤があるAWSに移すのに手間がかかってしまったことです。
そこで、Slackから自動でS3にアップロードできれば問題は解消されると思い開発しました。

## Getting Started / スタートガイド

1. AWSへ環境を構築する

2. Create an app with Slack / Slackでappを作成する 

![FireShot Capture 235 - Slack API - Slack - https___api slack com_](https://user-images.githubusercontent.com/11880332/55311853-fd22e800-549e-11e9-9e6f-0e761dda23e4.png)


Go to [https://api.slack.com/](https://api.slack.com/) and press the "Start Building" button.

[https://api.slack.com/](https://api.slack.com/)へアクセスし「Start Building」ボタンを押す。

![FireShot Capture 234 - Slack API_ Applications - Slack - https___api slack com_apps_new_app=1](https://user-images.githubusercontent.com/11880332/55311888-22175b00-549f-11e9-916d-26aaf54da013.png)

Set 「App Name」and 「Development Slack Workspace」and press "Create App" button

「App Name」と「Development Slack Workspace」を設定し「Create App」ボタンを押す

3. Create an app with Slack / OAuth & Permissionsを確認 & スコープを設定する

4. Event Subscriptionsより、Enable Evendsを設定する

## Built With / 利用するシステムのリスト

 * Slack
 * Python 3.7
 * Amazon S3
 * Amazon API Gateway
 * AWS Lambda
 * AWS CloudFormation

## License / ライセンス

This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/tomonoriminegishi/slack-upload-image-to-s3/blob/master/LICENSE) file for details.

このプロジェクトは MIT ライセンスの元にライセンスされています。 詳細は [LICENSE.md](https://github.com/tomonoriminegishi/slack-upload-image-to-s3/blob/master/LICENSE) をご覧ください。

