---
title: 入社3年目、業務以外の知識がない自分がGWを捧げてサービス・アプリケーションを勉強してみた！〜3日目VPC：構築ハンズオン③〜
tags:
  - Apache
  - AWS
  - EC2
  - vpc
  - ハンズオン
private: false
updated_at: '2022-05-03T01:49:08+09:00'
id: aa128860f7b0f494e6e3
organization_url_name: null
slide: false
---
# はじめに

前回の記事(https://qiita.com/hugo-crt/items/7819a61ea4cb63532787)
の続きとなります。

今回は、前回作成したVPCのルートテーブルが実際に機能しているか、またPublicとPrivateではインターネットからの接続に対してどのような挙動を見せるのかをハンズオン形式で学習していきます。


# ハンズオン

- コンソール画面を開きます。
![スクリーンショット 2022-05-03 1.17.46.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/63622134-908a-aadf-72fc-3ff36e465826.png)


IAMユーザ作成編学習しましたが、作業ユーザに適切な権限を与えて作業するんでしたね。
ということは今回、Public Subnet内でwebサーバを立てるので、権限が必要です。

- IAMを検索し、クリックします。
- ダッシュボードのロールをクリックします。
![スクリーンショット 2022-05-02 23.39.59.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/ebdc79ab-bd75-9bf9-26cb-da7e1d189bc0.png)

現在作業しているユーザは画像の2つのロールしか持っておらず、
今回のハンズオンには関係がないため、新しいロールを作成しましょう。

- 「ロールを作成」をクリックします
![スクリーンショット 2022-05-02 23.40.12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/c255762d-288b-0dcb-162a-4a8ec5fc083b.png)


今回webサーバを立てるにあたって、EC2というサービスを使用します。
一連のハンズオン学習ではタイトルとしては学習していませんが、
今回の目的はルートテーブルの通信テストなため、説明は割愛します。

- 信頼されたエンティティタイプはAWSのサービスを選択します。
- ユースケースはEC2を選択し、「次へ」をクリックします。
![スクリーンショット 2022-05-02 23.40.27.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/d9e47d75-d4f9-6573-b8df-47725f73216f.png)

- ポリシーの検索窓でssmと入力します。
- 「AmazonSSMFullAccess」にチェックを入れて画面右上の「ポリシーを作成」をクリックします。
![スクリーンショット 2022-05-02 23.41.18.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/0e885647-6fe9-7109-c5e3-3251218b5821.png)

※ssmとはsystems managerの略で、以下の説明が参考になります。

https://docs.aws.amazon.com/ja_jp/systems-manager/latest/userguide/what-is-systems-manager.html

- 次にロールの名前を入力します。今回は「handson」と入力しました。
- 「ロールを作成」をクリックします。
![スクリーンショット 2022-05-02 23.41.45.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/773279b4-e488-6e66-f4e0-6c1da35f7ffe.png)

作成が成功すると以下のような画面になります。この後EC2を利用してサーバを立ち上げるための設定を行うときに、このロールを使用します。
- サービス検索窓からEC2を検索し、クリックします。
![スクリーンショット 2022-05-02 23.42.09.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/95e2cc23-e5f1-5b02-c2e6-c28ced69429f.png)


- ダッシュボードの「インスタンス」をクリックします。
![スクリーンショット 2022-05-02 23.42.29.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/031c41c2-8920-73c6-72d7-3b24ff13a184.png)


- 「インスタンスを起動」をクリックします。
![スクリーンショット 2022-05-02 23.42.52.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/ef387060-3148-b4f4-a4ee-f04b28ed883e.png)


ここからEC2の設定項目が多く混乱しがちですが、上から順に設定していきます。

- 名前とタグは、Nameと入力します。
![スクリーンショット 2022-05-02 23.48.15.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/0a14f779-9305-7222-433f-39247bfc14bc.png)


- 次にOSイメージを設定します。
今回はデフォルトで選択されていたAmazon Linuxのままにします。
- インスタンスタイプ、キーペアもデフォルトで問題ありません。
![スクリーンショット 2022-05-02 23.43.45.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/e501bee5-d973-f82d-cc93-7b12a073800a.png)
![スクリーンショット 2022-05-03 0.27.35.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/a03a7e68-d807-f9c9-8f76-f2caaee9de9a.png)



- 次にネットワーク設定です。デフォルトではネットワークがデフォルトVPCとなっているため、右上の「編集」をクリックします。
![スクリーンショット 2022-05-02 23.44.25.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/358a90df-bb17-daf1-1918-873c3c8541ae.png)


- 前回作成した「hands on」というVPCを選択し、サブネットはPublic Subnet-aを選択しましょう。
- パブリックIPの自動割り当ては有効化とし、ファイアウォールのセキュリティグループは新規に作成します。
![スクリーンショット 2022-05-02 23.49.52.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/119e1ec9-0ed8-0c2b-e84b-a79c1e675612.png)


- セキュリティグループ名はwebとします。
- ルールの部分のタイプはデフォルトではSSHとなっていますが、今回はブラウザから接続のテストを行うため、HTTPを選択します。
- ソースタイプはカスタムを選択し、ルートテーブル作成時同様、「0.0.0.0/0」を入力します。
![スクリーンショット 2022-05-02 23.50.02.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/24c9c251-8aef-6c3b-127f-d731b95744ad.png)


ストレージの設定はデフォルトのままで問題ありません。
![スクリーンショット 2022-05-03 0.36.37.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/e02165e5-d633-f2db-6f90-3a2cd5b81f95.png)

- 高度な詳細では、IAMインスタンスプロフィールを、先ほどSSMのフルアクセス権限を与えたロール「handson」を選択します。
- 下にスクロールするとユーザーデータという項目があります。この部分は起動時にEC2に対してコマンドを渡して実行させることができます。
そこで、起動時に起動時にwebサーバを立ち上げられるよう、以下のコマンドを入力します。

`#!/bin/bash`
`yum -y update`
`yum -y install httpd`
`systemctl enable httpd.service`
`systemctl start httpd.service`

![スクリーンショット 2022-05-02 23.46.32.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/c56d5230-2abe-0b68-d160-501f1cbc33ad.png)
![スクリーンショット 2022-05-02 23.47.10.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/21240e2d-2e4e-b0b3-e3fd-8f7986ef6927.png)


- この状態で「インスタンスを起動」をクリックします。
- すると以下のキーペア作成の確認画面が出てくるため、キーペアなしで続行を選択して、「キーペアなしで続行」をクリックします。
![スクリーンショット 2022-05-02 23.50.12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/13623a02-9558-4644-71d4-50ef79a47b03.png)
![スクリーンショット 2022-05-02 23.50.33.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/46368c97-4fdf-41fc-87fb-9c827482e020.png)



- インスタンスの起動が成功すると、以下のように灰色のステータスが2箇所あるため、画面真ん中の項目の保留中と、ステータスチェックが緑色の文字に変わるまで数分待ちます。
- 画面左上の更新ボタンを押すと、最新のステータスが確認できます。
![スクリーンショット 2022-05-02 23.50.51.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/6b5cabb8-d6e5-44f5-f232-8200f96a639b.png)


- インスタンスが立ち上がったら、チェックを入れて詳細情報を確認してみましょう。
![スクリーンショット 2022-05-02 23.53.53.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/0b8aa275-6d35-f1c3-4961-8caa48351c20.png)


- 詳細のパブリックIPv4アドレスの左側にあるコピーアイコンをクリックしてください。
これをブラウザの検索窓に入力して接続テストを行います。
![スクリーンショット 2022-05-02 23.54.23.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/c861feee-455f-d908-05e0-f043782863f2.png)


前回のルートテーブルの設定で「Public Root Table」に「Public Subnet-a」を関連付け、そのサブネット内にEC2インスタンスを立ち上げたので、接続ができていることが確認できますね。
![スクリーンショット 2022-05-03 1.22.51.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/ab6d60cc-57c7-88bc-74e2-b0f1a162ca09.png)


- 次にSSMが設定できるか確認しましょう。EC2インスタンス一覧の画面に戻ってインスタンスにチェックを入れ、「接続」をクリックします。
![スクリーンショット 2022-05-02 23.54.23.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/c861feee-455f-d908-05e0-f043782863f2.png)

- セッションマネージャーを選択し、「接続」をクリックします。
![スクリーンショット 2022-05-02 23.55.28.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/f9fe71ad-2124-6965-5622-8707d958504b.png)


- コマンドの画面が出ているのであれば、接続ができています。
![スクリーンショット 2022-05-02 23.55.53.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/fa3da224-07a7-a1fa-df1e-7bd2f7a5dc66.png)


では、VPCのPublic Subnet-aをPublic Root TableからPrivate Root Tableに関連付け直したらどうなるでしょうか。
設定方法は前回の記事で確認してください。

先ほどコピーしたパブリックIPv4アドレスを再度ブラウザの検索窓に入力してエンターを押すと、実行待ちの末、エラーが出ると思います。

これで接続テストは完了となります。

# 最後に

自分に必要ないスペックの高いガジェットって、不要ってわかってても欲しくなっちゃいますよね。
ことMacに関してはiMacとMac Bookだけなんですけど、
M1 Mac MiniとiPad Airで十分なんだよなあと。
ボーナス近づいてきたし欲しいけど我慢。他の有益なものを買います。
