# Dockerの使い方

## イメージのダウンロード
- ROS2が予めインストールされたUbuntu 20.04のイメージをダウンロードします。
```
$ docker image pull ros:foxy-ros-base-focal
```
- ダウンロード後、上記のイメージが存在することを確認します。
```
$ docker image ls
```

## コンテナーの作成
- イメージに基づいてコンテナーを作成し、起動します。この時、オプションnameでコンテナーに対して名前（例：ros-cui）を付けておきます。
```
《Ubuntuの場合》
$ docker container run -itd --net host -e DISPLAY=$DISPLAY --name ros-cui ros:foxy-ros-base-focal /bin/bash
《MacやWindowsの場合》
$ docker container run -itd -e DISPLAY=host.docker.internal:0 --name ros-cui ros:foxy-ros-base-focal /bin/bash
```
- 起動後、コンテナー「ros-cui」が存在することを確認します。
```
$ docker container ls -a
```
- コンテナー「ros-cui」の中に入ってみます。
```
$ docker container exec -it ros-cui /bin/bash
```

## コンテナーの試用
- 今後のためにコマンドsudoをインストールしておきます。
```
$ su
$ apt update
$ apt install sudo -y
$ exit
```
- エディター「nano」をインストールし、実行してみます。ctrl＋xで終了できます。
```
$ sudo apt install nano -y
$ nano
```
- X Window Systemの動作を確認するため、アプリ「x11-apps」をインストールし、実行してみます。正常に実行できた場合は「マウスカーソルを見続ける目」が表示されます。ctrl＋cで終了できます。
```
$ sudo apt install x11-apps -y
$ xeyes
```

## コンテナーの操作
- 今後のためにコンテナーに関するコマンドを整理しておきます。

### コンテナーの起動
- コンテナーを起動するコマンドです。コンテナーに入る前にコンテナーを起動しておく必要があります。
```
《記法》
$ docker container start コンテナー名
《実例》
$ docker container start ros-cui
```

### コンテナーの停止
- コンテナーを停止するコマンドです。コンテナーをコマンド`exit`で抜けてから実行します。
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

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
