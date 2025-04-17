# ROSの準備
ROSが使用できることを確認し、ソースコードを作成するための準備を行います。

## 環境変数の確認
コンテナーに入っていない場合は入ります。
```
$ docker container exec -it ros-cui /bin/bash
```

ROS 2（Humble）がインストールされていることを確認します。
```
$ printenv | grep ROS
ROS_DISTRO=humble
```

ROSの環境を設定します。
```
$ source /ros_entrypoint.sh
```

ROSに関する環境変数が設定されたかどうかを確認します。 正しく設定されていれば、下記のように出力されます。 
```
$ printenv | grep ROS
ROS_VERSION=2
ROS_PYTHON_VERSION=3
ROS_LOCALHOST_ONLY=0
ROS_DISTRO=humble
```

## ディレクトリーの作成
研究開発用ディレクトリーとしてcolcon_wsおよびsrcを作成しておきます。
```
$ mkdir -p ~/colcon_ws/src
```

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
