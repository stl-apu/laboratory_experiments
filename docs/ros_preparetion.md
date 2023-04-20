# ROSのビルド
- ROSのプログラムをビルドする方法を学びつつ、ソースコードを作成するための環境を整備します。

## 環境変数の確認
- ROSがインストールされていることを確認します。
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
  - 正常にインストールされていれば、下記のように出力されます。
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

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
