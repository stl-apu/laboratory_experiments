# Dockerの使い方

## イメージのダウンロード
- ROS2がインストールされたUbuntu 20.04のイメージをダウンロードします。
    ```
    $ docker image pull ros:foxy-ros-base-focal
    ```
- 上記のイメージが存在することを確認します。
    ```
    $ docker image ls
    ```

## コンテナーの作成
- イメージを用いてコンテナーを作成し、起動します。この時、オプションnameでコンテナーに対して名前（例：ros-cui）を付けておきます。
    ```
    《UbuntuやWindows（WSL2）の場合》
    $ docker container run -itd --net host -e DISPLAY=$DISPLAY --name ros-cui ros:foxy-ros-base-focal /bin/bash
    《MacやWindows（WSL1）の場合》
    $ docker container run -itd -e DISPLAY=host.docker.internal:0 --name ros-cui ros:foxy-ros-base-focal /bin/bash
    ```
- コンテナー「ros-cui」が存在することを確認します。
    ```
    $ docker container ls -a
    ```
- コンテナー「ros-cui」の中に入ります。
    ```
    $ docker container exec -it ros-cui /bin/bash
    ```

## コンテナーの試用
- コマンドsudoをインストールしておきます。
    ```
    $ su
    $ apt update
    $ apt install sudo -y
    $ exit
    ```
- エディター「nano」をインストールし、実行してみます。
    ```
    $ sudo apt install nano -y
    $ nano
    ```
    - ctrl＋xで終了できます。
- X Window Systemの動作を確認するため、アプリ「x11-apps」をインストールし、実行してみます。
    ```
    $ sudo apt install x11-apps -y
    $ xeyes
    ```
    - マウスカーソルを見続ける目が表示されます。
    - ctrl＋cで終了できます。

## コンテナーの操作
- 今後のためにコンテナーに関するコマンドを整理しておきます。

### コンテナーの起動
- コンテナーを起動します。
    ```
    《記法》
    $ docker container start コンテナー名
    《実例》
    $ docker container start ros-cui
    ```

### コンテナーの停止
- コンテナーをコマンド`exit`で抜けてから、コンテナーを停止します。
    ```
    《記法》
    $ docker container stop コンテナー名
    《実例》
    $ docker container stop ros-cui
    ```

### コンテナーの削除
- コンテナーを作り直す場合は、まず、コマンド`container ls`でコンテナーIDやコンテナー名を確認します。そして、コンテナーを停止し、コマンド`container rm`で削除します。
    ```
    $ docker container ls -a --no-trunc
    ```
    ```
    《記法》
    $ docker container rm コンテナーID
    または
    $ docker container rm コンテナー名
    《実例》
    $ docker container rm 81ae65b0ac4296538f66d4037b18d36a2cc530b926eb0c9d3bce4a5a622ef003
    や
    $ docker container rm ros-cui
    ```

[このページのトップへ](#)

[Dockerページへ](https://stl-apu.github.io/laboratory_experiments/docker)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
