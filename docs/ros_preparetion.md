# ROSの準備
- ROSが使用できることを確認し、ソースコードを作成するための準備を行います。

## 環境変数の確認
- ROS2（foxy）がインストールされていることを確認します。
```
$ printenv | grep ROS
ROS_DISTRO=foxy
```
- ROSの環境を設定します。
```
$ source /ros_entrypoint.sh
```
- ROSに関する環境変数を確認します。  
```
$ printenv | grep ROS
```
    - 正常に設定できていれば、下記のように出力されます。
```
ROS_VERSION=2
ROS_PYTHON_VERSION=3
ROS_LOCALHOST_ONLY=0
ROS_DISTRO=foxy
```

## ディレクトリーの作成
- 研究開発用ディレクトリーとしてcolcon_wsおよびsrcを作成します。
```
$ mkdir -p ~/colcon_ws/src
```

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
