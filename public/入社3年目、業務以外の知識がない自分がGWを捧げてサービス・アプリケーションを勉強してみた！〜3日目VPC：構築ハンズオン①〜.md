---
title: 入社3年目、業務以外の知識がない自分がGWを捧げてサービス・アプリケーションを勉強してみた！〜3日目VPC：構築ハンズオン①〜
tags:
  - AWS
  - vpc
  - ハンズオン
private: false
updated_at: '2022-05-02T17:04:48+09:00'
id: 61ad9a60d599ee5d9b47
organization_url_name: null
slide: false
---
# はじめに

前記事(https://qiita.com/hugo-crt/items/662eb5726d7d12b83bfe)
の続きです。

今回はAWS Cloud Questで学習したときに苦手意識を持ってしまったネットワーク系、Amazon VPC　作成をハンズオンで学習していきます。


# Amazon VPC（Virtual Private cloud）とは

まずはこちらの画像をば。
今回はこの画像通りに作成していくので、いわば設計図です。
![スクリーンショット 2022-05-02 15.10.07.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/ecd42b37-4f58-7df5-d848-a3eda154e045.png)

AWS Cloud上に個別のクラウド環境（VPC）を持てるサービスになります。これから作成する環境に、Subnetと呼ばれるより細かい区画を作成して、それぞれに役割を持たせます。

そこにAWS外部のインターネットとの通信を出し入れするためのインターネットゲートウェイを作成・アタッチして、インターネットとVPCを繋ぐための環境づくりが今回のハンズオンのゴールです。

今回も自分なりに理解するためのイメージとなりますが、
AWS Cloudという国（例えば日本）の中で土地（VPC）を借ります。
そしてその土地内にSubnetという学生寮を建てます。

Public Subnetは配達員や友人など、関係者は入ることができますが、学生寮であるため、Private Subnet（各学生の部屋）には入室禁止です。

荷物や手紙など、渡したい物は管理人が代行して学生へ届けるみたいなイメージ。もちろん借りた土地の入り口には文字通り入場ゲートを用意して、関係のない一般人の入場は禁止します。

こんな塩梅でVPCを作成していきます。

# VPCハンズオン

- コンソール画面を開き、検索窓からVPCを検索し、クリックします。ちなみに今回はリージョンを東京に設定しています。
![スクリーンショット 2022-05-02 14.44.11.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/f7e6bb9b-b844-ca33-ba71-9c8e712fe350.png)

- まずはVPCから作成しましょう。画面右側のVPCをクリックします。
![スクリーンショット 2022-05-02 14.48.45.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/89966415-197d-f35c-5c97-30c7cc89751b.png)


すると既に作成されているVPCがありますが、これはテンプレートみたいなもので、簡単に検証が行いたいなど簡易的な作業を行うために存在する物です。
なので無視で構いません。

- 「VPCを作成」をクリックします。
![スクリーンショット 2022-05-02 14.49.04.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/63e8d590-6b23-902e-94b7-f90f6d83d17d.png)


- 各設定項目がありますが、リソースはVPCのみ、名前は今回「Hands on」としました。
- CIDRブロックは最初の画像に合わせるので、「10.0.0.0/16」とします。
- 後続の項目はデフォルトで大丈夫なので、画面下までスクロールし、「VPCを作成」をクリックします。
![スクリーンショット 2022-05-02 14.50.34.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/8ff6a33b-f927-a1bc-876a-46d93c4e6306.png)

- 作成に成功すると、以下のようになります。IPv4 CIDRのアドレスや作成したVPCの名前がが間違っていないか確認してください。
![スクリーンショット 2022-05-02 14.50.44.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/04027fb3-508e-aca3-f54e-ead33668292b.png)
![スクリーンショット 2022-05-02 14.51.00.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/1e299a27-9890-a01f-77d8-87f1f99728e2.png)

これでAWS Cloud上にVPCを作成することができました。
ここから、VPCの中にPublic・PrivateのSubnetを作成していきます。


- ダッシュボードから「サブネット」をクリックします。
![スクリーンショット 2022-05-02 14.51.19.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/9930a7fb-1f21-4787-c70b-e8e9f706006a.png)


VPCの作成前と同じく、デフォルトでサブネットも作成されていますが、今回は使用しないので無視します。

- 画面右上の「サブネットを作成」をクリックします。
![スクリーンショット 2022-05-02 14.51.32.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/59029d36-b707-1c23-9afd-04333839510e.png)


- サブネット作成にあたって、紐付けるVPCを選択します。先ほど作成した「Hands on」というVPCを選択してください。
![スクリーンショット 2022-05-02 14.51.43.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/de98c0f3-0ba8-80fd-f23c-842949d0c69c.png)

サブネットはaとcでそれぞれ作成します。そのため、サブネット名は「Public / Private Subnet-a / c」とします。
画像はPublic Subnet-aを作成しているところです。
- アベイラビリティーゾーンはaとcごとに合わせてください。
aでは「アジアパシフィック（東京）/ap-northeast-1a」なので、
aのPublic / Private Subnetは同じリージョンにします。
cだったら「〜-1c」です。
- CIDRブロックは記事最初の画面を見ながら作成します。
今回はPublic Subnet-aのため「10.0.1.0/24」と入力します。
- 上記各種項目を入力が完了したら、画面右上の「サブネットを作成」をクリックします。
![スクリーンショット 2022-05-02 14.53.54.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/82befa8e-41a8-afc1-2da0-c552036847a1.png)

作成に成功すると以下のような画面になります。作成したサブネットの名前やCIDRブロックを確認してください。
![スクリーンショット 2022-05-02 14.54.02.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/223b5445-3c09-7314-89bc-3f8860baa0a2.png)

Public/Privateのa/cそれぞれのサブネットを作成した状態が以下です。これではVPC、サブネットを作成しただけで、外部との通信するための経路がありません。

通信を振り分けるためのインターネットゲートウェイを作成しましょう。
- ダッシュボードの「インターネットゲートウェイ（以下、IGW）」をクリックします。
![スクリーンショット 2022-05-02 14.57.52.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/3bc90c49-f1cd-6202-31de-b5cf98b73fc6.png)


- 例に漏れずデフォルトのIGWが存在していますが無視です。
というかこれを使ったところで作り方の勉強にはなりません。
- 「インターネットゲートウェイを作成」をクリックします。
![スクリーンショット 2022-05-02 14.58.05.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/4b208139-3f4f-3cde-5b39-7cac6b047572.png)

- ここは簡単です。部品としてのIGWの名前をつけましょう。
つけ終わったら画面右下の「インターネットゲートウェイを作成」をクリックします。
![スクリーンショット 2022-05-02 14.58.33.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/cf80f389-fc02-1b67-bc96-b678ef45dcd8.png)

- 作成に成功すると以下の画面になります。このままではIGWの部品を作成しただけなので、VPCにアタッチするため、インターネットゲートウェイの画面へ戻ります。
![スクリーンショット 2022-05-02 14.58.43.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/f9005d59-36e3-6e01-33cf-bc0d6f74aa85.png)


下の画像ではまだ部品としてアタッチされていないので、状態が「Detached」となっています。
- 先ほど作成したIGWにチェックを入れ、「アクション」→「VPCにアタッチ」をクリックします。
![スクリーンショット 2022-05-02 14.59.27.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/77d1e57f-5562-6f8a-b95f-5cd5e524f9af.png)



- 作成したVPCを選択し、「インターネットゲートウェイのアタッチ」をクリックし、アタッチが成功していると2、3枚目の画面になります。
![スクリーンショット 2022-05-02 14.59.41.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/3332b5f8-68ac-03c0-b71b-609497623c0b.png)
![スクリーンショット 2022-05-02 14.59.50.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/8462ea1d-b0a6-b6e8-8a25-9e50e04d41a1.png)
![スクリーンショット 2022-05-02 14.59.59.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/d18be9c7-fc5e-ee22-b913-fe16a382ea0f.png)

これにてVPC、サブネット、IGWの作成およびアタッチは完了です。
ガワは完成しました。

ですが、今回作成した一覧のものではルールが足りていないんです。
サブネットって、PrivateとPublicで作成しましたよね？

そう、名前だけで、通信の制御に関してはまだ設定していないんです。次回、その設定から先をやっていきます。


# 最後に

AWS Cloud Questではそもそも英語で作業を指示されるので理解のハードルが日本語の場合よりも高く難しく感じましたが、
今回公式のハンズオン動画は日本語で説明している、かつ、講師の方の説明が簡潔でやることがスッと頭に入ってきました。

やっぱり母国語って大事ですね。
言語仕様のドキュメントとか英語だったりして読めるようにしておくことは大事ですが、日本語の解説はとても助かりました。

概要の説明では、TCP/IPやルーティングなどのネットワークの基礎知識があるとより理解しやすいとありました。
ここら辺は業務で触らないところなので別途勉強する必要がありますね。

