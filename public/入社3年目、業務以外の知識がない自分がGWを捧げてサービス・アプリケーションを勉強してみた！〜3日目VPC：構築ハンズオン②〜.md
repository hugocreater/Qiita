---
title: 入社3年目、業務以外の知識がない自分がGWを捧げてサービス・アプリケーションを勉強してみた！〜3日目VPC：構築ハンズオン②〜
tags:
  - AWS
  - vpc
  - ハンズオン
  - ルートテーブル
private: false
updated_at: '2022-05-02T22:06:25+09:00'
id: 7819a61ea4cb63532787
organization_url_name: null
slide: false
---
# はじめに

前回の記事の(https://qiita.com/hugo-crt/items/61ad9a60d599ee5d9b47)
の続きです。

# ルートテーブルとは


この記事からお借りします。

https://qiita.com/chro96/items/21863e0960ba4ac72470


>だいたいの記事に主語がないので補足すると、AWSのルートテーブルは、サブネット内にあるインスタンス等がどこに通信にいくかのルールを定めたものです。

>つまり、ルートテーブルはパケットの宛先(IPアドレス)を見て、どこに通信を流すかが書かれている表です。この表をみてパケットを運ぶので、表にない宛先のものはパケットを送らないので、通信できません。

>サブネット毎にどこに通信ができるかを定めたものだというところがポイントです。

ということで、ルートテーブルを作成して、Public / Private Subnetごとに通信の経路を設定する必要があるとわかりました。

言葉通りPublicはインターネットとの通信をしそうですね。
反対にPrivateはVPC内のみでの通信をしそうです。

下の画像のターゲットのLocalはVPC内の通信を意味します。
今回もこの図通りにルートテーブルを作成するので、これが設計図となります。
![スクリーンショット 2022-05-02 18.05.37.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/d9d794ff-2cb7-df83-d75c-f455d7806586.png)


# ルートテーブルの作成
　
- VPCのダッシュボードから「ルートテーブル」をクリックします。
![スクリーンショット 2022-05-02 17.57.03.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/4bfb6b31-2a2a-612d-29f7-fbbe5c246866.png)

- 前回VPC、サブネット、IGWを作成・アタッチした際に作成されたルートテーブルと、アカウント作成の時点で作成されていたデフォルトのルートテーブルが存在するので、VPCの列からどちらがどちらなのか確認しておきましょう（後で編集するため）。
![スクリーンショット 2022-05-02 17.57.33.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/fa6bd2f1-2460-069a-8095-71da67519e2f.png)

- 「ルートテーブルを作成する」をクリックします。
![スクリーンショット 2022-05-02 17.58.05.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/f4d343d6-e267-df1f-6a5a-e71035b403be.png)


- ルートテーブル名を入力します。今回はPublicのルートテーブルとして入力しました。
- このルートテーブルで使用するVPCを選択します。ハンズオンで作成したVPCを指定します。
- 「ルートテーブルを作成」をクリックします。
![スクリーンショット 2022-05-02 17.58.45.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/8f319f40-bc80-c572-f6c4-ef38372d094b.png)

作成すると以下のような画面になります。
編集していて気づいてしまった。PublicのLがRになってる・・・（お恥ずかしい）
後で直しておきましょう。ルートテーブルの一覧からリネームできるんです。

- インターネットからのルートを追加するため、画面右下の「ルートを編集」をクリックします。
![スクリーンショット 2022-05-02 17.58.54.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/762032ad-6d64-a8b4-dd6e-64c994022fcd.png)

- 画面左下の「ルートを追加」をクリックします。
- 送信先には「」を入力し、ターゲットはインターネットゲートウェイを選択し先ほど作成した「igw-handson」を選択、画面右下の「変更を保存」をクリックします。
![スクリーンショット 2022-05-02 17.59.50.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/980657b2-3fd9-e430-c2f4-4a6cd5b296c2.png)
![スクリーンショット 2022-05-02 17.59.59.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/6fb12458-dfbf-640e-bdbd-dc48f8e0f653.png)


- ルートテーブルの一覧に戻り、最初に確認したサブネットを作成したときに自動生成されたルートテーブルの名前を編集します。
サブネット作成時点ではデフォルトでプライベートに設定してあるため、今回は「Private Root Table」と入力します。
![スクリーンショット 2022-05-02 18.00.40.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/ed5f8a0d-20a6-a605-c6fd-23f798fdb4b3.png)


次に各ルートテーブルにサブネットをそれぞれ関連付けさせます。Publicで作成したルートテーブルにはaとcのPublic Subnetを、Privateで作成したルートテーブルには同じくaとcのPrivate Subnetを関連付けます。

- Public Root Tableにチェックを入れ、サブネットの関連付けを選択し、「サブネットの関連付けを編集」をクリックします。
![スクリーンショット 2022-05-02 18.01.08.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/4e6feaac-0fbb-2f8a-1a52-667202cf4d69.png)

- 今回はPublicのルートテーブルのため、Public Subnet-aと-cにチェックを入れて画面右下の「関連付けを保存」をクリックします。
![スクリーンショット 2022-05-02 18.01.27.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/5b565111-ebb3-abf0-8911-7a7500a5442a.png)
![スクリーンショット 2022-05-02 18.01.44.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/690a62c2-27eb-768e-67c2-2c0939c0b9a8.png)

ここで注意。先ほどリネームしたPrivate Root Tableは自動作成時にサブネットに紐付けされているものの、明示的に関連付けがないとの表示があります。

関連付けなくても支障はないですが、他のルートテーブルに合わせて関連付けさせる癖をつけておきましょう。

- Private Root Table にチェックをつけ、サブネットの関連付けを選択し、「サブネットの関連付けを編集」をクリックします。
![スクリーンショット 2022-05-02 18.02.17.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/02b8b56d-65cd-fadc-e989-7290c7a2ac97.png)

- 今回はPrivateのルートテーブルのため、 Private Subnet-aと-cにチェックを入れて画面右下の「関連付けを保存」をクリックします。
![スクリーンショット 2022-05-02 18.02.52.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/d69f0497-0ffc-430d-2b13-94ca7583a773.png)
![スクリーンショット 2022-05-02 18.03.04.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/37134601-5b30-d698-d554-bb57a474604c.png)

以上で終了となります。
次回は作成したVPCがインターネットからアクセスできるかをテストします。

# 最後に

今日でGW三日目ですが、あと1週間休めるのに次の出勤までの日数をカウントしたらちょっとゲンナリしました。
多少予定は入っていますが、無駄な時間だけは過ごさないよう、意識的に行動しなければ・・・。
