# ROSのビルド

## 環境変数の確認
- ROSがインストールされていることを確認します。  
  ```
  # printenv | grep ROS
  ROS_DISTRO=foxy
  ```
- 基本となる環境変数を設定する。
  ```
  # source /ros_entrypoint.sh
  ```
- ROSに関する環境変数を確認します。  
  ```
  # printenv | grep ROS
  ```
  - 正常にインストールされていれば、下記のように出力されます。
  ```
  ROS_VERSION=2
  ROS_PYTHON_VERSION=3
  ROS_LOCALHOST_ONLY=0
  ROS_DISTRO=foxy
  ```

## ディレクトリーの作成
- 研究開発用ディレクトリーとしてcatkin_wsおよびsrcを作成します。
  ```
  # mkdir -p ~/ros2_ws/src
  ```

## プログラムのビルド
- コマンドcatkin_makeを用いてビルドします。この時点では何もプログラムが無いので、すぐに100％になり、ビルドが完了します。
  ```
  # cd ~/ros2_ws
  # colcon build
  # ~/ros2_ws/install/setup.bash
  ```

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
