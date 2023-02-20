# Git
- Gitを利用して、情報科学実験で使用するプログラムをダウンロードする。
- Gitを利用して、情報科学実験のレポートを提出する。

## 準備
- ターミナルやコマンドプロンプトを開く。
- Dockerサービスを起動する。
- Dockerコンテナーに入る。
  ```
  $ docker container exec -it ros-cui /bin/bash
  ```

## Gitの設定
- gitをインストールする。
  ```
  $ sudo apt -y install git
  ```
- gitがインストールされたことを確認する。
  ```
  $ git version
  ```
- ユーザーの名前とメールアドレスを設定する。
  ```
  《記法》
  $ git config --global user.name "名前"
  $ git config --global user.email "メールアドレス"
  《実例》
  $ git config --global user.name "Takuo Suzuki"
  $ git config --global user.email "takuo.suzuki@ist.aichi-pu.ac.jp"
  ```
- 名前とメールアドレスが設定されていることを確認する。
  ```
  $ git config -l
  ```
  - Gitの設定を変更したい時は再度`$ git config`を実行し、上書きする。

## GitHubの設定
- clone、pull、pushなどのコマンドを実行できるよう、SSH接続（鍵認証）の設定を行います。
- 下記のディレクトリーにid_rsaとid_rsa.pubが存在することを確認します。
  ```
  $ ls ~/.ssh
  ```
  - 存在しない場合は作成します。パスフレーズは無しで大丈夫です。
    ```
    $ cd ~/.ssh
    $ ssh-keygen -t rsa
    ```
    - ディレクトリーも存在しない場合は作成します。
      ```
      $ mkdir ~/.ssh
      ```
    - コマンドssh-keygenが無い場合はパッケージをインストールします。
      ```
      $ sudo apt update
      $ sudo apt -y install openssh-server
      ```
- 下記のコマンドで公開鍵（id_rsa.pub）の内容をクリップボードにコピーします。
  ```
  $ cat ~/.ssh/id_rsa.pub | xsel -bi
  ```
  - コマンドxselが無い場合はパッケージをインストールします。
    ```
    $ sudo apt update
    $ sudo apt -y install xsel
    ```
- GitHubのウェブサイトを開きます。
  - https://github.com/settings/ssh
- ［Settings］→［SSH and GPG keys］へと進み、［SSH keys］の所にあるボタン「New SSH key」を押します。
- 公開鍵の内容を記入欄「Key」の中にペーストし、登録します。
  - 記入欄「Title」にはコンピューター名などを記入します。
- 正常に接続できるかを確認します。
  ```
  $ ssh -T git@github.com
  ```
  - 「You've successfully authenticated, but GitHub does not provide shell access.」と表示されればOKです！
  - 「Are you sure you want to continue connecting (yes/no/[fingerprint])?」と聞かれた場合は「yes」と回答します。

## 全体ダウンロード
- ROSでは、プログラムをワークスペースのsrcに置くので、ディレクトリーを移動します。
  ```
  $ cd ~/colcon_ws/src
  ```
- サブコマンドcloneでプログラムをコピー（初回ダウンロード）します。
  ```
  $ git clone https://github.com/stl-apu/laboratory_experiments_2023.git
  ```
- コマンドlsでディレクトリー「laboratory_experiments_2023」が存在することを確認します。
  ```
  $ ls
  ```

## 差分アップロード
- ディレクトリー「laboratory_experiments_2023」に移動します。
  ```
  $ cd laboratory_experiments_2023
  ```
- ブランチの一覧を確認します。
  ```
  $ git branch
  ```
  - mainのみが存在します。
- ブランチ「develop」に移動します。
  ```
  $ git checkout develop
  ```
- ブランチ「develop」に移動したことを確認します。
  ```
  $ git status
  ```
- 再度、ブランチの一覧を確認します。
  ```
  $ git branch
  ```
  - mainとdevelopが存在します。
- 自分用のブランチ（feature/学籍番号）を作成しながら移動します。
  ```
  《記法》
  $ git checkout -b ブランチ名
  《実例》
  $ git checkout -b feature/2021311000
  ```
  - オプションbはブランチを作成しながら切り替えます。普通に切り替える場合はオプション無しで大丈夫です。
  ```
  $ git checkout feature/2021311000
  ```
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
- GitHubのウェブサイトを開き、自分用のブランチを確認します。
  - 「Local 1」が反映されていたらOKです！

## 競合の解消
- 他のメンバーによってtxtファイルが編集されたことを再現するため、GitHubのウェブサイト上でリモート側のファイルを編集します。
- test.txtを選択し、ペンの形のアイコン（Edit file）を押し、「Remote 1」と追記し、commit（保存）します。
- 再度、ローカル側でテキストエディターでtest.txtを開き、「Local 2」と追記し、保存します。
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
- pushします。
  ```
  $ git push origin feature/2021311000
  ```
  - エラーが発生します。PullしてからでないとPushできません。
- Pullすると、別のエラーが発生します。
  ```
  $ git pull origin feature/2021311000
  ```
  - 複数人が同時に同じファイルを編集すると、競合（conflict）が発生します。
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
- pushします。
  ```
  $ git add test.txt
  $ git commit -m "Update test.txt"
  $ git push origin feature/2021311000
  ```
- ブランチ「main」に戻しておきます。
  ```
  $ git checkout main
  ```

## 補足1：ブランチの結合
- ブランチmain（master）が本番環境となります。
- feature → develop → masterと、プログラムをサブコマンドmergeで結合する必要があります。
- 研究開発リーダー（責任者）が行う作業なので、情報科学実験では割愛します。
  - 将来、研究開発リーダーになりたい人は学んでみてください。

## 補足2：Gitクライアント
- GitHub Desktopを使えば、GUIで管理できます。
- Git機能を搭載したソフトウェアも多いです。MicrosoftのVisual Studio Codeなど…。

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
