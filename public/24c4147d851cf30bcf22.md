---
title: >-
  入社3年目、業務以外の知識がない自分がGWを捧げてサービス・アプリケーションを勉強してみた！〜4日目Git：リポジトリの作成からPull
  Requestまで〜
tags:
  - Git
  - GitHub
  - ハンズオン
private: false
updated_at: '2022-05-04T00:52:03+09:00'
id: 24c4147d851cf30bcf22
organization_url_name: null
slide: false
---
# はじめに

前記事(https://qiita.com/hugo-crt/items/aa128860f7b0f494e6e3)
の続きです。

一旦AWSからは離れてGitのハンズオンを行います。
以下の記事を見てハンズオン学習します。

https://qiita.com/TakumaKurosawa/items/79a75026327d8deb9c04

## 前提知識

リポジトリ：ソースコードなどのファイルをプロジェクト単位で区切った単位のこと。
リモートリポジトリ：GitHub上に作成されたリポジトリのこと。
ローカルリポジトリ：リモートリポジトリから作業端末に複製されたリポジトリのこと。

ブランチ：ある時点以降における、リポジトリの異なる世界線。例えば最終成果物であるmasterブランチがメインであるのに対して、メインのブランチをきれいに保てるよう、機能改善などで修正する際に、世界線を変えて編集すること。
（あくまで現時点で抱いているイメージです）
プルリクエスト：切り分けたブランチから最終的に積み上げる際に、積み上げてよいか確認するためのメッセージのこと。
マージ：切り分けたブランチをメインのブランチにまとめること。


CLONE：リモートからローカルにリポジトリを複製すること。
ADD:ローカルで編集したファイルをコミット対象にするため、インデックスあるいはステージングエリアという領域に移動させること。
COMMIT：編集した結果を（ローカル）リポジトリに反映させること。
PUSH：ローカルリポジトリにコミットした編集結果を、リモートリポジトリに反映させること。
DIFF：ローカルリポジトリにあるファイルと編集した同一ファイルの差分を表示させること。


## 必要なもの・こと
お好みのテキストエディタ（私はVS Codeを使用します）
GitHubのアカウント（事前に作成していたのでアカウントは作成済み）
自端末へのGitのインストール

今回は既に環境は整っている前提で、Gitの基本的な使い方を勉強します。
なお、Macでの作業を前提にしています。


# リポジトリの作成

- まずは[GitHub](https://github.com/)にログインします。
- 次にRepositoriesを選択し、画面右側のNewをクリックしリポジトリを作成します。
![スクリーンショット 2022-05-03 20.01.59.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/1a4ef56e-badd-e3cf-4449-e6055b594232.png)

- Repository Nameにプロジェクト名、Descriptionには任意の説明（なくてもOK）、作成するリポジトリを公開するか否かを選択、READMEを追加するか否かを選択します。
- 入力後、画面下までスクロールし、リポジトリを作成します。
![スクリーンショット 2022-05-03 20.02.25.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/59b1e40d-4870-0da0-2b75-97a1a554d937.png)

- 作成が完了すると、リポジトリ一覧に作成したリポジトリが表示されます。
![スクリーンショット 2022-05-03 22.06.31.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/a3205a2d-66a3-d72c-1e4b-5ba56f8b7bb2.png)



# 作成したリポジトリをCloneする

作成したリポジトリを自端末で編集できるようにします。
具体的にはClone（リポジトリを自端末に複製する）して、テキストエディタで編集できる状態を作ります。

- 作成したリポジトリを選択して、<>Codeを選択し、Code▼をクリックします。
- SSHを選択し、コピーアイコンをクリックします。
![スクリーンショット 2022-05-03 22.07.54.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/61bd8d20-ae39-c61c-5480-6d3a8acd6fa7.png)


- ターミナルを開き、CDコマンドでダウンロードしたいディレクトリまで移動します。
```bash
cd ~/git
```

- `ls`コマンドでクローン前の状態を確認してみましょう。
新規作成したディレクトリだと、実行結果として何も返ってこず、次のコマンドが打てる状態になっていると思います。
```bash
***@*** ~ % ls
***@*** ~ %
```


- `git clone`コマンドを打って自端末にリポジトリをクローンします。
```bash
git clone <先ほどコピーしたものをペースト>
```

以下のようにコマンド結果が返ってきます。
```bash
Cloning into 'handson'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
Receiving objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
```

- 親ディレクトリまで戻って、ローカルにリポジトリがクローンできているか`ls`コマンドで確認しましょう。
```bash
***@*** git % ls
handson
```

これでローカルで編集できる環境が整いましたね。

# 追加・編集したテキストファイルをADD・COMMIT・PUSHする

- 先ほどリモートリポジトリを作成した際にREADMEを追加したので、編集して保存します。

![スクリーンショット 2022-05-03 22.12.29.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/7aeff668-5ba6-933c-59f8-da3a4d649fb9.png)

- コミット対象にするため、`add`コマンドを実行します。
```bash
git add handson.txt（指定ファイルのみ）
もしくは
git add .(全件)
```
- 次にコミットして変更内容をローカルリポジトリに反映させます。
ダブルクオーテーションで囲っている部分に修正内容のコメントが入力できます。
業務では大切な部分なので、コミットする際は -m　を付与してコメントする癖をつけましょう。
```bash
***@*** handson % git commit -m "1回目の編集"
[main *****] 1回目の編集
 1 file changed, 2 insertions(+), 1 deletion(-)
```
- ローカルリポジトリにコミットした差分をリモートリポジトリに反映させます。
```bash
***@*** handson % git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 287 bytes | 287.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:******/handson.git
   *****..*****  main -> main
```

これで基本的な差分管理が完了しました。
リモートリポジトリに反映されているか見に行きましょう。
![スクリーンショット 2022-05-03 22.27.36.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/12c715cc-426b-3f60-1b03-3295513c88fb.png)

反映されていますね。これで基本的な使い方を学習できました。

# Pull Request〜マージ

業務では複数人で開発することが大半です。そのため、誰がどのような修正をしたか、ちょっとした前提の違いでデグレードが発生してもおかしくないですよね。

それもあってか、ローカルで修正したものをPullいいか、もっというならばレビューしてもらうためにPull Requestを作ります。

もちろん最終成果物的立ち位置のmasterブランチやmainブランチは頻繁に更新するものではなく、編集が完了して初めて積み上げるべきものです。

したがって、編集用の世界線であるブランチを切ることで余計コストをかけずに編集することができます。

- そこで、まずは作業用ブランチを __新たに__ 切ってみます。
```bash
***@*** handson % git checkout -b develop
Switched to a new branch 'develop'
```

ちなみにブランチを __既存の__ ブランチに戻したい場合、`git checkout <ブランチ名>`コマンドを実行します。
新たにブランチを切った時のコマンドオプション`-b`は新規にブランチを切るオプションです。

- この状態で前回同様READMEファイルを編集します。
- `git diff`コマンドで編集内容を確認してみましょう。
```bash
***@*** handson % git diff
diff --git a/README.md b/README.md
index ***..*** 100644
--- a/README.md
+++ b/README.md
@@ -1,2 +1,3 @@
 # handson
 Hallo World!
+Whats up!
```
+の部分は編集で追加されたもの、-は削除されたものです。


- このままadd、commit、pushします。
```bash
***@*** handson % git add README.md
***@*** handson % git commit -m "developブランチで編集"
[develop 6580611] developブランチで編集
 1 file changed, 1 insertion(+)
***@*** handson % git push
fatal: The current branch develop has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin develop
```

__あれ。。。なんか最初にpushした時と挙動が違う。__

そうなんです。これはなぜかというと、最初のpushした時は既にファイルが存在していたから。
なので、お返事に書いてあるコマンドを実行することで解決します。

```bash
***@*** handson %  git push --set-upstream origin develop
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 311 bytes | 311.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote: 
remote: Create a pull request for 'develop' on GitHub by visiting:
remote:      https://github.com/*******/handson/pull/new/develop
remote: 
To github.com:******/handson.git
 * [new branch]      develop -> develop
Branch 'develop' set up to track remote branch 'develop' from 'origin'.
```
リモートリポジトリで確認してみましょう。
![スクリーンショット 2022-05-03 23.54.50.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/3102e9fc-0629-affc-8c36-9c1880f1cbe9.png)
mainには今回のコミット履歴がありません。

![スクリーンショット 2022-05-03 23.53.14.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/61fded54-3f74-5d00-a3ea-2d65ee0140b9.png)

ブランチをmainからdevelopに変更すると、コミットされていることがわかりますね。


それでは次はPull Requestを作成します。
- Pull Requestを選択し、New pull requestをクリックします。
![スクリーンショット 2022-05-03 23.58.23.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/a6a26e3a-83f9-a86e-4258-1361ac6249c2.png)

developからmainにマージするよう変更します。
![スクリーンショット 2022-05-03 23.58.46.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/ae296df4-7d7c-3326-77d5-b5952cea85ed.png)

この状態で初めてマージ可能ですよ、とメッセージが表示されます。
- Create pull requestをクリックします。
![スクリーンショット 2022-05-03 23.58.57.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/59257760-01d4-4209-bb55-08a12b39175f.png)

- 必要なコメントを書いて、Create pull requestをクリックします。
![スクリーンショット 2022-05-03 23.59.17.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/9ae25f68-0cde-b7c3-7c24-56e3fab0478f.png)

これでPull Requestの完成です！
![スクリーンショット 2022-05-04 0.07.28.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/9c82dd09-3661-79d7-866b-5296ee665a03.png)



では次はレビューをしてみましょう。
- File changedを選択します。すると変更箇所が表示されます。
![スクリーンショット 2022-05-04 0.09.58.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/0526b66e-86c1-8259-1519-6429972d0a0a.png)

- コメントする場合は指摘箇所にマウスカーソルを持っていくと+ボタンが出てくるのでクリックしてコメントを入力してadd single commentをクリックします。
![スクリーンショット 2022-05-04 0.13.08.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/4acf293a-ae4e-bf17-d328-a68155d21461.png)

するとレビューコメントが見れます。
![スクリーンショット 2022-05-04 0.45.06.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/8a558a22-7ce0-9948-ffb6-52cbf95f6cdf.png)


- 肝心のマージするためにはMerge pull request、Confirm Mergeをクリックするとマージが完了します。
![スクリーンショット 2022-05-04 0.16.38.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/b292512f-c4bc-a8df-ed27-d4372f2a774c.png)
![スクリーンショット 2022-05-04 0.17.30.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/a35cd288-c9e3-edf4-da55-4f15ef1a990d.png)
![スクリーンショット 2022-05-04 0.17.42.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/7623f8e9-6b18-4448-0a75-4bb1ba35d95d.png)


- mainブランチに、developブランチで編集した内容がマージされているか確認しましょう。
![スクリーンショット 2022-05-04 0.18.04.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1833566/63e2685b-121a-53ff-35e0-1b4651e62f25.png)


これにて終了です。
少なくとも個人で開発する際には最低限コミットをして、（セルフ）レビューできる状態になりました。

# 最後に
見返しても３０分くらいでできるような内容だったのが、とても時間かかってしまいました。記事作成にも時間は取られますが、新情報的なものには理解までに他人よりも時間がかかってしまいます。

それでも手を動かしてその手順を形にすると頭に入る割合が全然違いますね。

参考にした記事と書いていることがほぼ同じで、車輪の再発明的なことになっているので、この記事はあまり参考にはならないでしょうが、勉強した軌跡ということで。
