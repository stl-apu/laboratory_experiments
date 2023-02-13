# Docker（Mac）

## ソフトウェアのインストール
- DockerのアカウントでDocker Hubにアクセスする。
  - https://hub.docker.com/
- Docker Desktop for Macをインストールする。
  - https://docs.docker.com/get-docker/
- Docker.appを起動し、IDとPasswordを入力する。
- ターミナルを起動し、コマンドdockerが実行できることを確認する。
  ```
  % docker version
  ```

## GUIの設定
- XQuartzをインストールする。
  - https://www.xquartz.org/
- XQuartzを起動し、［環境設定］→［セキュリティ］から［ネットワーク・クライアントからの接続を許可］にチェックを入れる。
- コンピューターを再起動する。
- アクセス制限を確認する。localhostが含まれているかを確認する。
  ```
  % xhost
  INET:localhost
  INET6:localhost
  ```
  - 含まれていなかった場合はlocalhostを許可する。
    ```
    % xhost +localhost
    ```
  - localhostが許可されていることを確認する。
    ```
    % xhost
    ```
  - 再度、XQuartzを起動する。

## イメージのダウンロード
- ROSがインストールされたUbuntu 20.04のイメージをダウンロードする。
  ```
  % docker image pull ros:foxy-ros-base-focal
  ```
- 上記のイメージが存在することを確認する。
  ```
  % docker image ls
  ```

## コンテナーの作成→テスト
- イメージを用いてコンテナーを作成し、起動する。オプションnameでコンテナーに対して名前（例：ubuntu）を付けておく。
  ```
  % docker container run -itd -e DISPLAY=host.docker.internal:0 --name ros-cui ros:foxy-ros-base-focal /bin/bash
  ```
- コンテナー「ros-cui」が存在することを確認する。
  ```
  % docker container ls -a
  ```
- コンテナー「ros-cui」に入る。
  ```
  % docker container exec -it ros-cui /bin/bash
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


[このページのトップへ](#)

[Dockerページへ](https://stl-apu.github.io/laboratory_experiments/docker)

[応用実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
