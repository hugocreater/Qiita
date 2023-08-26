---
title: 入社3年目、業務以外の知識がない自分がGWを捧げてサービス・アプリケーションを勉強してみた！〜1日目AWS：ユーザ登録〜
tags:
  - AWS
  - ハンズオン
  - ユーザー登録
private: false
updated_at: '2022-04-29T15:24:39+09:00'
id: 3cebbbf16db1d2fde063
organization_url_name: null
slide: false
---
# はじめに

前記事(https://qiita.com/hugo-crt/items/ed194cb5426b70094651)
の続きです。

AWSの初歩もいいところ、ユーザ登録からハンズオン学習を始めます。

何だよ、ユーザ登録とか引っかかるところないだろ、と思いつつも、
初学者だしいつか人に教える時に再利用できればいいか、くらいのテンションでやっていきます！

# ユーザ登録に必要なもの
- メールアドレス
- 連絡の取れる電話番号
- クレジットカードもしくはデビットカード


# ユーザ登録までの道のり

- お使いのブラウザでAWSのWebページを検索し、クリックします。
![スクリーンショット 2022-04-29 14.48.24.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/50600e56-4093-91cb-b81d-ce1711e6b99f.png)


- 下画像右上のコンソールにサインインをクリックします。
![スクリーンショット 2022-04-29 14.49.04.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/6b9a3e79-6b77-9fbb-9512-5aa09de013c8.png)


- この画面は後ほど説明しますが、登録後のログイン画面でもあるため、今回は「新しいAWSアカウントの作成」をクリックします。
![スクリーンショット 2022-04-29 14.47.12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/540ee87f-49fb-0f7c-01b6-68d23fe0e78e.png)

- 任意のメールアドレス、作成したいユーザ名を入力して「Eメールアドレスを検証」をクリックします。
![スクリーンショット 2022-04-29 14.52.37.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/3827c538-de8b-8895-94cc-1b3c979b019a.png)

- すると入力したメールアドレスに検証コードが送られてくるので、Webページで入力します（スクショ撮るの忘れてた・・・）
![スクリーンショット 2022-04-29 14.54.37.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/0df95b7d-3aa4-61cf-418c-079e9d5df9cf.png)

- パスワードを設定します。
__バリバリAWS使ってる諸先輩方、passwordとかaiueo12345みたいな危険なパスワードは使ってないですよね・・・？__
![スクリーンショット 2022-04-29 14.56.19.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/31f9b270-ea3e-1388-67cc-2927f8029818.png)

- 個人情報を入力します。今回はハンズオン学習なので、利用用途は個人で問題ないです。
- __名前とか住所とかローマ字で入力しないといけないので注意。__
![スクリーンショット 2022-04-29 14.59.51.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/b14394e3-fd98-ee6b-ca60-b1b93afdb082.png)

- クレジット情報を入力します。
![スクリーンショット 2022-04-29 15.03.29.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/abe93a39-936b-cbaa-cacd-d9b1955f82e7.png)

- 続いて本人確認です。音声通話で認証ってやったことないけど、通話料かかるのかな・・・
![スクリーンショット 2022-04-29 15.04.33.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/61189c1f-60c3-f622-5141-ace7e7284156.png)
![スクリーンショット 2022-04-29 15.06.12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/10bbeb3f-f345-dd05-00e0-c9cfaf4916be.png)

- ハンズオンなので無料で（以下略）。
- これで登録完了です。この後AWSから数分で登録完了のメールが届きました。ひとまず登録は問題なさそうです。
![スクリーンショット 2022-04-29 15.06.38.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/9bae4a01-8099-4b5a-e149-483f4c07d392.png)
![スクリーンショット 2022-04-29 15.08.29.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/7e5fb6a7-b746-fb3b-a5d9-c7d1d46cd188.png)


# AWSにログインできるか確認

- アカウント作成画面に遷移する前の画面に戻ってきました。
- ここではルートユーザーを選択し、先ほど登録したメールアドレスを入力して次へ進みます。
- セキュリティチェックも入力して進んでください。
- ユーザ登録時に設定したパスワードを入力しちゃいましょう。
![スクリーンショット 2022-04-29 15.09.41.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/21347952-0b20-cfd7-fe0a-0b3266cc2a07.png)
![スクリーンショット 2022-04-29 15.11.59.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/7ef7c908-1830-5a35-87c0-20ae10274089.png)
![スクリーンショット 2022-04-29 15.14.06.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/f160ee5a-50af-e189-77de-090a25b30b6a.png)

- サインインができていれば、下のような画面に遷移します。画面右上に隠してはいますが、登録時のユーザ名が表示されているはずです。
![スクリーンショット 2022-04-29 15.14.58.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/c20ee4d9-9b97-9189-965a-a341ded140eb.png)

これにてユーザ登録完了です。
詰まったことといえば、目が悪くてセキュリティチェックの文字列を見間違えたくらいでした。


# 感想

__~~正直書くまでのことではない・・・~~__
記事書く練習にもなったのでまあよしです。

記事執筆がテストのエビデンス作りみたいな感じで、
休日も仕事みたいなことしてる・・・と思いました。



