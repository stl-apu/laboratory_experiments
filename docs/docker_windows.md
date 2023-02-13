# Docker (Windows)

## コンピューターの設定の確認
- 「タスクマネージャー」を起動し、［パフォーマンス］→［CPU］から［仮想化：有効］となっていることを確認する。
  - 有効にする方法はコンピューターのメーカーや機種によって異なるので、自分で調べる。
- 「コントロールパネル」を起動し、［プログラム］→［Windowsの機能の有効化または無効化］を開き、［Hyper-V］と［Linux用Windowsサブシステム（Windows Subsystem for Linux）］にチェックが入っていることを確認する。

## ソフトウェアのインストール
- DockerのアカウントでDocker Hubにアクセスする。
  - https://hub.docker.com/
- Docker Desktop for Windowsをインストールする。
  - https://docs.docker.com/get-docker/
- Docker Desktop.exeを起動し、IDとPasswordを入力する。
- コマンドプロンプトを起動し、コマンドdockerが実行できることを確認する。
  ```
  > docker version
  ```

## GUIの設定
- GUIを使用するためにVcXsrvをインストールする。
  - https://sourceforge.net/projects/vcxsrv/
- ホストのIPアドレスを確認する。Wi-FiなどのIPv4アドレスを確認する。
  ```
  > ipconfig
  ```
- ホストの設定を変更する。Xmingのインストール先（/Program Files (x86)/Xming）にあるX0.hostsファイルに、X Window Systemの転送を許可するホストのIPアドレスを記入する。
  ```
  localhost
  ホストのIPアドレス（←追記！）
  ```
- コンピューターを再起動する。
- VcXsrvをソフトウェア「XLanuch」で起動する。
  - 「Native opengl」のチェックを外す。
  - 「Disable access control」にチェックを入れる。

## イメージのダウンロード
- ROSがインストールされたUbuntu 20.04のイメージをダウンロードする。
  ```
  > docker image pull ros:foxy-ros-base-focal
  ```
- 上記のイメージが存在することを確認する。
  ```
  > docker image ls
  ```

## コンテナーの作成→テスト
- ホストのIPアドレスを指定しつつ、イメージを用いてコンテナーを作成し、起動する。オプションnameでコンテナーに対して名前（例：ubuntu）を付けておく。
  ```
  《記法》
  > docker container run -itd -e DISPLAY=IPアドレス:0.0 -v ~/.Xauthority:/root/.Xauthority --name コンテナー名 イメージ名 /bin/bash
  《実例》
  > docker container run -itd -e DISPLAY=host.docker.internal:0 --name ros-cui ros:foxy-ros-base-focal /bin/bash
  ```
- コンテナー「ros-cui」が存在することを確認する。
  ```
  > docker container ls -a
  ```
- コンテナー「ros-cui」に入る。
  ```
  > docker container exec -it ros-cui /bin/bash
  ```
- コマンドsudoをインストールする。
  ```
  # su
  # apt update
  # apt -y install sudo
  # exit
  ```
- nanoをインストールし、実行してみる。
  ```
  # sudo apt -y install nano
  # nano
  ```
- x11-appsをインストールし、実行してみる。
  ```
  # sudo apt -y install x11-apps
  # xeyes
  ```
  - マウスカーソルを見続ける目が表示される。
  - ctrl＋cで終了する。

[このページのトップへ](#)

[Dockerページへ](https://stl-apu.github.io/laboratory_experiments/docker)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
