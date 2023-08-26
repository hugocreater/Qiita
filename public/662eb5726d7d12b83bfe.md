---
title: 入社3年目、業務以外の知識がない自分がGWを捧げてサービス・アプリケーションを勉強してみた！〜2日目AWS：IAMユーザ作成・後編〜
tags:
  - AWS
  - IAM
  - IAMグループ
  - IAMユーザー
  - IAMポリシー
private: false
updated_at: '2022-04-30T14:00:58+09:00'
id: 662eb5726d7d12b83bfe
organization_url_name: null
slide: false
---
# はじめに
前記事(https://qiita.com/hugo-crt/items/40c4bcb883c74085e539)
の続きです。

今回は実際の作業者ユーザである、IAMユーザを作成します。
一度作り方を覚えてしまえば、最低限責任範囲の切り分けができて
基本的なセキュリティ対策を講じていますといえますね。

どうやら巷ではIAMって最初の方に勉強するけど、細部をチューニングしていくにつれ沼にハマっていく、
実はかなり高度なサービスであると聞いたことがありますが、これはまた別のお話。
我々初学者は基本的なところから知っていけばいいんです。


# IAM：IAMグループの作成
グループ作成の方法を学びます。
なぜユーザ作成ではなくグループなのかというと、
端的に権限やポリシーの設定が楽になるからです。
下の画像を見てください。
この画像では管理者（Admins）・開発者（Developers）・テスター（Test）の3グループに分かれています。
役割によって必要な設定をグループに与えてしまえば、その役割に沿って必要な制限事項が勝手に付与されるのです。
大規模なプロジェクトの場合、数十、数百のユーザに対して各種設定を一人一人割り振るのは大変です。
なので、役割ごとに作業範囲を均一に設定することで、ユーザ作成の負担を軽減できます。
![iam-intro-users-and-groups.diagram.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/bfe198fd-7464-3cad-5419-a38d86ea92c6.png)

ではまずはグループを作成していきましょう。


- 毎度おなじみ、コンソールに __rootユーザー__ でログインしてIAMを検索します。
- ダッシュボードの画面左上、ユーザーグループをクリックします。
![スクリーンショット 2022-04-30 11.50.11.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/d889a4cb-cbf8-690e-a331-781791759b32.png)

　- 画面右上のグループの作成をクリックします。
![スクリーンショット 2022-04-30 11.53.28.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/1c7ba0e5-d267-460c-f444-7912aff6a7d0.png)

- ユーザグループ名を付けましょう。
![スクリーンショット 2022-04-30 11.54.12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/ee1a797d-da63-48e6-55e3-dd50e30e9a60.png)

- 設定画面を下にスクロールするとアクセス許可ポリシーが設定できる項目にたどり着くので、検索窓で「administrator」と入力して「AdministratorAccess」のチェックボックスにチェックを入れたら画面右下のグループの作成をクリックします。
ちなみにポリシー名をクリックすると、そのポリシーのアクセス権限の状態が確認できます。
今回は割愛します。是非見てみてください。
![スクリーンショット 2022-04-30 11.54.26.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/33facff8-4bdc-3288-7e72-c77ce99a0724.png)
![スクリーンショット 2022-04-30 11.54.58.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/b87c8855-f5d4-66b6-acb2-b51a836126e4.png)

- グループの作成が完了すると以下の画面に遷移します。
- 画面左の「ユーザー」をクリックして、ユーザー作成に移りましょう。
![スクリーンショット 2022-04-30 11.56.01.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/f531508c-fa21-eed6-0a5e-930b08b27832.png)

# IAM:IAMユーザーの作成

- 画面右側の「ユーザーを追加」をクリックします。
![スクリーンショット 2022-04-30 11.56.15.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/7f78c3dd-956d-4c5f-d1de-34f48966d38d.png)

- ユーザ名を入力します。
- アクセスの種類の選択は、今回は全てにチェックを入れます。
![スクリーンショット 2022-04-30 11.57.06.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/0a51e405-254a-b47b-f401-d11ab66ecc6d.png)

- 今回はカスタムパスワードを設定し、パスワードのリセットは不要としました。
- 画面右下の「次のステップ：アクセス権限」をクリックします。
![スクリーンショット 2022-04-30 11.58.44.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/3b5e666e-174e-5e02-4eba-ec9580f3a749.png)

- 先ほど設定したグループにユーザを追加します。
- グループ名右のチェックボックスにチェックを入れて、画面右下の「次のステップ：タグ」をクリックします。
![スクリーンショット 2022-04-30 11.58.54.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/251c2887-c75a-2e23-1d1e-bff5bad6d6ff.png)

- ここはユーザを特定するための情報をタグとして設定します。
- 今回はお試しでメールアドレスを入力しました。
- 入力が終わったら画面右下の「次のステップ：確認」をクリックします。
![スクリーンショット 2022-04-30 12.00.02.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/32f35663-0ff6-e7ea-3f19-23418f54c441.png)


- 入力情報が間違っていないか確認します。
ここまできたらユーザ作成はほぼ完了です。
- 問題がなければ画面右下の「ユーザーの作成」をクリックします。
![スクリーンショット 2022-04-30 12.00.16.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/8a74db51-7e2d-b8ae-e844-140774ec3fae.png)


ユーザーが作成されましたね。
しかしこのままではMFAデバイスの割り当てがされていないので設定しましょう。


- 作成したユーザをクリックします。
![スクリーンショット 2022-04-30 12.01.20.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/86760d6b-b64b-ad83-2c2f-d4ccb3f25289.png)

- 遷移された画面の中にある「認証情報」タブをクリックし、「MFAデバイスの割り当て」の「管理」をクリックして設定すると以下の画面になります。
設定方法は前回の記事で説明したので割愛します。
- __後ほどIAMユーザとしてログインする際に便利な裏技があるので、
コンソールのサインインリンクをコピーしておきます。
右側にあるコピーのアイコンをクリックします。__
![スクリーンショット 2022-04-30 12.03.18.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/0c8c5fe5-8fdb-50ed-1c38-ac15317459dc.png)

- 今回作成したユーザは管理者権限をポリシーで付与したので、請求情報へアクセスできるよう設定します。
- 画面右上のIAMユーザ→アカウントをクリックします。
![スクリーンショット 2022-04-30 12.03.55.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/66f46a2c-6186-415c-cb90-ba1fa62e68d0.png)

- 遷移した画面を下にスクロールし、IAMユーザー/ロールによる請求情報へのアクセスの項目で、右にある「編集」をクリックします。
- IAMアクセスのアクティブ化にチェックを入れ、更新します。
- これにてユーザ作成完了です。次は実際にIAMユーザとしてサインインしてみましょう。
![スクリーンショット 2022-04-30 12.04.32.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/de876f0f-44ac-a828-4004-1a8073c7bde9.png)
![スクリーンショット 2022-04-30 12.04.40.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/a40e2f64-c225-b897-afe4-d12be5648022.png)


- rootユーザをログアウトし、再度コンソールにサインインします。
![スクリーンショット 2022-04-30 12.05.05.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/d8b72fb1-53bd-4db4-67ef-bb540b86e784.png)

- 先ほどIAMユーザのサインイン時に便利な裏技のためコピーした文字列を入力窓にペーストします。
![スクリーンショット 2022-04-30 12.05.21.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/31a827bc-aec3-05af-64b2-ff295607a33b.png)

- すると既にユーザ名とパスワードが入力された状態で認証画面に飛びました。
- 各自確認してほしいのですが、先ほどの裏技を入力しないと、メールアドレスやパスワードなど、
一つずつ認証していくのでめんどくさそうでした。
- 未入力の項目を埋めてMFA認証して終了です。
![スクリーンショット 2022-04-30 13.49.31.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/223f34a3-613c-9400-cc20-a74e7756d25a.png)
![スクリーンショット 2022-04-30 12.10.46.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/5f0d6ca7-9321-6566-e3d9-35121708ae89.png)
![スクリーンショット 2022-04-30 12.11.19.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/43d711e8-aec7-2f3d-8e61-5e8b337be10f.png)

# 最後に

GW中は雨予報が多く、友人もそこまではしゃいでないから勉強に集中できるなと思った矢先、
本日快晴で外出したくなりました。

座りっぱなしも体に悪いことだし、散歩しながら勉強したことを思い返してリフレッシュしようかな。
