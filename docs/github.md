# GitHub
GitHubを利用して、実験で使用するサンプルプログラムをダウンロードします。また、実験のレポートを提出できるように設定します。

## 準備
ターミナルを開きます。なお、Windowsではターミナルのことをコマンドプロンプトと言います。以後、読み替えてください。

Dockerコンテナーに入っていないなら、DockerサービスとDockerコンテナーが起動していることを確認した上で、Dockerコンテナーに入ります。
```
$ docker container exec -it ros-cui /bin/bash
```

## Gitの設定
Gitがインストールされたことを確認します。
```
$ git version
```

ユーザーの名前とメールアドレスを設定します。
```
《記法》
$ git config --global user.name "名前"
$ git config --global user.email "メールアドレス"
《実例》
$ git config --global user.name "Takuo Suzuki"
$ git config --global user.email "takuo.suzuki@ist.aichi-pu.ac.jp"
```

ユーザーの名前とメールアドレスが設定されていることを確認します。ちなみに、Gitの設定を変更したい時は再度`$ git config`を実行し、上書きします。
```
$ git config -l
```

## GitHubの設定
Gitコマンド（`clone`、`pull`、`push`など）を実行できるよう、SSH接続（鍵認証）の設定を行います。

鍵を作成するために、コマンド`ssh-keygen`をインストールします。ついでに、コマンド`xsel`もインストールしておきます。
```
$ sudo apt update
$ sudo apt install openssh-server xsel -y
```

ディレクトリー「~/.ssh」にid_rsaとid_rsa.pubを作成します。パスフレーズは無しで大丈夫です。
```
$ mkdir ~/.ssh
$ cd ~/.ssh
$ ssh-keygen -t rsa
```

下記のコマンドで公開鍵（id_rsa.pub）の内容をクリップボードにコピーします。エラー「Can't open display」が出てしまう人は無理をせず、コマンド`cat`で公開鍵の内容をターミナルに直接表示し、マウスやキーボードを用いてコピーしてください。
```
$ cat ~/.ssh/id_rsa.pub | xsel -bi

↓エラーが発生してしまう人は…

$ cat ~/.ssh/id_rsa.pub
```

GitHubのウェブサイトを開きます。

→[https://github.com/](https://github.com/)

サインインしたら、［Settings］→［SSH and GPG keys］へと進み、［SSH keys］の所にあるボタン「New SSH key」を押します。そして、公開鍵の内容を記入欄「Key」の中にペーストし、登録します。なお、記入欄「Title」にはコンピューター名など（例：MyComputer-Docker）を記入します。

正常に接続できるかを確認します。「Are you sure you want to continue connecting (yes/no/[fingerprint])?」と聞かれた場合は「yes」と回答します。「You've successfully authenticated, but GitHub does not provide shell access.」と表示されればOKです！
```
$ ssh -T git@github.com
```

## 全体ダウンロード
ROSではプログラムをワークスペースのディレクトリー「src」に置くので、コマンド`cd`で移動します。
```
$ cd ~/colcon_ws/src/
```

コマンド`clone`でプログラムをダウンロード（初回ダウンロード）します。
```
$ git clone git@github.com:stl-apu/laboratory_experiments_2025.git
```

コマンド`ls`でディレクトリー「laboratory_experiments_2025」が存在することを確認します。
```
$ ls
```

## 差分アップロード
ディレクトリー「laboratory_experiments_2025」に移動します。
```
$ cd laboratory_experiments_2025
```

ブランチの一覧を確認します。この段階では「main」のみが存在すると思います。
```
$ git branch
```

ブランチ「develop」に移動してみます。
```
$ git checkout develop
```

ブランチ「develop」に移動したことを確認します。
```
$ git status
```

再度、ブランチの一覧を確認します。「main」と「develop」が存在すると思います。
```
$ git branch
```

自分用のブランチ（feature/学籍番号）を作成しながら移動します。オプションbを付けることで、ブランチを作成しながら切り替えることができます。
```
《記法》
$ git checkout -b ブランチ名
《実例》
$ git checkout -b feature/2023311000
```

テキストエディター（nanoなど）でtest.txtを開き、「Local 1」と追記し、保存します。
```
$ nano test.txt
```

編集内容をcommitの対象に含めます。
```
$ git add test.txt
```

commitします。
```
《記法》
$ git commit -m "コミット文"
《実例》
$ git commit -m "Update test.txt"
```

pushします。
```
《記法》
$ git push origin ブランチ名
《実例》
$ git push origin feature/2023311000
```

GitHubのウェブサイトを開き、自分用のブランチを切り替え、text.txtの中身を確認してみます。「Local 1」と追記したことが反映されていたらOKです！

## 競合の解消
他のメンバーによってファイルが編集された状況を再現するため、GitHubのウェブサイト上でリモート側のファイルを編集してみます。

GitHubのウェブサイト上でtest.txtを選択し、ペンの形のアイコン（Edit file）を押し、「Remote 1」と追記し、commit（保存）します。

再度、ローカル側での作業に戻ります。

テキストエディター（nanoなど）でtest.txtを開き、「Local 2」と追記し、保存します。
```
$ nano test.txt
```

編集内容をcommitの対象に含めます。
```
$ git add test.txt
```

commitします。
```
$ git commit -m "Update test.txt"
```

pushしようとすると、エラーが発生します。このように複数人が同時に同じファイルを編集すると、競合（conflict）が発生します。pullしてからでないとpushできない状態となります。
```
$ git push origin feature/2023311000
```

pullしようとすると、別のエラーが発生します。
```
$ git pull origin feature/2023311000
```

テキストエディター（nanoなど）でtest.txtを開きます。
```
$ nano test.txt
```

競合を解消してみます。今回は「Local 2」の方を残し、「Remote 1」を消します。「\<\<\<」や「\>\>\>」などの記号（競合マーカー／コンフリクトマーカー）は削除して大丈夫です。修正後のファイルの内容は下記の通りとなります。
```
Test
Local 1
Local 2
```

実際には、『「Local 2」と「Remote 1」のどちらを残すのか？』という修正方針を研究開発グループ内で検討する必要があります。もしかしたら第2週や第3週で競合が発生するかもしれませんので、覚えておいてください。

改めてpushしてみます。今度はエラーが発生しないはずです。
```
$ git add test.txt
$ git commit -m "Update test.txt"
$ git push origin feature/2023311000
```

現在のブランチを「develop」に戻しておきます。
```
$ git checkout develop
```

Dockerコンテナーから抜けて終了です。
```
$ exit
```

これ以降は参考情報になります。

## 補足1：ブランチの結合
ブランチmain（master）がロボットを用いて実験する際の本番環境となります。実験する前にはfeature → develop → mainと、プログラムをコマンド`merge`で結合する必要があります。

研究開発リーダー（責任者）が行う作業なので、情報科学実験では割愛しますが、将来、研究開発リーダーになりたい人は自分で調べてみてください。

## 補足2：Gitクライアント
GitHub Desktopを使えば、ブランチなどをGUIで管理できます。また、Git機能を搭載したソフトウェアも多いです。MicrosoftのVisual Studio Codeなど、多くの総合開発環境（IDE）がGitと連携することができます。

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
