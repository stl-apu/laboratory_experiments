# GitHub
- GitHubを利用して、情報科学実験で使用するサンプルプログラムをダウンロードします。
- GitHubを利用して、情報科学実験のレポートを提出できるように設定します。

## 準備
- ターミナルを開きます。
    - Windowsではターミナルのことをコマンドプロンプトと言います。読み替えてください。
- Dockerサービスを起動します。
- Dockerコンテナーに入ります。
```
$ docker container exec -it ros-cui /bin/bash
```

## Gitの設定
- gitをインストールします。
```
$ sudo apt update
$ sudo apt install git -y
```
- gitがインストールされたことを確認します。
```
$ git version
```
- ユーザーの名前とメールアドレスを設定します。
```
《記法》
$ git config --global user.name "名前"
$ git config --global user.email "メールアドレス"
《実例》
$ git config --global user.name "Takuo Suzuki"
$ git config --global user.email "takuo.suzuki@ist.aichi-pu.ac.jp"
```
- 名前とメールアドレスが設定されていることを確認します。
```
$ git config -l
```
    - Gitの設定を変更したい時は再度`$ git config`を実行し、上書きします。

## GitHubの設定
- `clone`、`pull`、`push`などのコマンドを実行できるよう、SSH接続（鍵認証）の設定を行います。

- 下記のディレクトリーにid_rsaとid_rsa.pubが存在することを確認します。
```
$ ls ~/.ssh
```
    - 存在しない場合は作成します。パスフレーズは無しで大丈夫です。
```
$ cd ~/.ssh
$ ssh-keygen -t rsa
```
        - そもそもディレクトリーが存在しない場合は作成します。
```
$ mkdir ~/.ssh
```
        - コマンド`ssh-keygen`が無い場合はパッケージをインストールします。
```
$ sudo apt update
$ sudo apt install openssh-server -y
```

- 下記のコマンドで公開鍵（id_rsa.pub）の内容をクリップボードにコピーします。
```
$ cat ~/.ssh/id_rsa.pub | xsel -bi
```
    - コマンド`xsel`が無い場合はパッケージをインストールします。
```
$ sudo apt update
$ sudo apt install xsel -y
```
    - 「Can't open display」というエラーが出てしまう人は無理をせず、コマンド`cat`で公開鍵の内容をターミナルに表示し、マウスやキーボードを用いてコピーしてください。
```
$ cat ~/.ssh/id_rsa.pub
```

- GitHubのウェブサイトを開きます。
    - https://github.com/

- ［Settings］→［SSH and GPG keys］へと進み、［SSH keys］の所にあるボタン「New SSH key」を押します。

- 公開鍵の内容を記入欄「Key」の中にペーストし、登録します。
    - 記入欄「Title」にはコンピューター名など（例：MyComputer-Docker）を記入します。

- 正常に接続できるかを確認します。
```
$ ssh -T git@github.com
```
    - 「You've successfully authenticated, but GitHub does not provide shell access.」と表示されればOKです！
    - 「Are you sure you want to continue connecting (yes/no/[fingerprint])?」と聞かれた場合は「yes」と回答します。

## 全体ダウンロード
- ROSではプログラムをワークスペースのディレクトリーsrcに置くので、コマンド`cd`で移動します。
```
$ cd ~/colcon_ws/src/
```
- コマンド`clone`でプログラムをダウンロード（初回ダウンロード）します。
```
$ git clone git@github.com:stl-apu/laboratory_experiments_2023.git
```
- コマンド`ls`でディレクトリー「laboratory_experiments_2023」が存在することを確認します。
```
$ ls
```

## 差分アップロード
- ディレクトリー「laboratory_experiments_2023」に移動します。
```
$ cd laboratory_experiments_2023
```
- ブランチの一覧を確認します。mainのみが存在します。
```
$ git branch
```

- ブランチ「develop」に移動します。
```
$ git checkout develop
```
- ブランチ「develop」に移動したことを確認します。
```
$ git status
```
- 再度、ブランチの一覧を確認します。mainとdevelopが存在します。
```
$ git branch
```

- 自分用のブランチ（feature/学籍番号）を作成しながら移動します。
```
《記法》
$ git checkout -b ブランチ名
《実例》
$ git checkout -b feature/2021311000
```
    - オプションbはブランチを作成しながら切り替えます。普通に切り替える場合はオプションを付けません。
- テキストエディター（nanoなど）でtest.txtを開き、「Local 1」と追記し、保存します。
```
$ nano test.txt
```
- 編集内容をcommitの対象に含めます。
```
$ git add test.txt
```
- commitします。
```
《記法》
$ git commit -m "コミット文"
《実例》
$ git commit -m "Update test.txt"
```
- pushします。
```
《記法》
$ git push origin ブランチ名
《実例》
$ git push origin feature/2021311000
```
- GitHubのウェブサイトを開き、自分用のブランチを確認します。test.txtに「Local 1」が反映されていたらOKです！

## 競合の解消
- 他のメンバーによってtxtファイルが編集された状態を再現するため、GitHubのウェブサイト上でリモート側のファイルを編集します。

- test.txtを選択し、ペンの形のアイコン（Edit file）を押し、「Remote 1」と追記し、commit（保存）します。

- 再度、ローカル側での作業に戻ります。

- テキストエディターでtest.txtを開き、「Local 2」と追記し、保存します。
```
$ nano test.txt
```
- 編集内容をcommitの対象に含めます。
```
$ git add test.txt
```
- commitします。
```
$ git commit -m "Update test.txt"
```
- pushすると、エラーが発生します。pullしてからでないとpushできない状態となります。
```
$ git push origin feature/2021311000
```

- このように複数人が同時に同じファイルを編集すると、競合（conflict）が発生します。

- pullすると、別のエラーが発生します。
```
$ git pull origin feature/2021311000
```

- テキストエディターでtest.txtを開き、競合を解消し、保存します。
```
$ nano test.txt
```
    - 今回は「Local 2」の方を残すことにします。
        - 実際には研究開発グループ内で修正方針を検討する必要があります。
    - 「\<\<\<」や「\>\>\>」などの記号（競合マーカー／コンフリクトマーカー）は削除します。
    - 最終的に、ファイルの内容は下記の通りとなります。
```
Test
Local 1
Local 2
```
- 改めてpushします。今度はエラーが発生しません。
```
$ git add test.txt
$ git commit -m "Update test.txt"
$ git push origin feature/2021311000
```

- 現在のブランチを「develop」に戻しておきます。
```
$ git checkout develop
```

## 補足1：ブランチの結合
- ブランチmain（master）がロボットで実験する際の本番環境となります。
- 実験する前にはfeature → develop → mainと、プログラムをコマンド`merge`で結合する必要があります。
- 研究開発リーダー（責任者）が行う作業なので、情報科学実験では割愛しますが、将来、研究開発リーダーになりたい人は自分で学習してください。

## 補足2：Gitクライアント
- GitHub Desktopを使えば、ブランチなどをGUIで管理できます。
- Git機能を搭載したソフトウェアも多いです。例えば、MicrosoftのVisual Studio Codeなど、多くの総合開発環境（IDE）がGitと連携することができます。

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
