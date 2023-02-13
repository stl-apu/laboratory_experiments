# ROSのビルド

## 環境変数の確認
- ROSがインストールされていることを確認します。  
  ```
  # printenv | grep ROS
  ```
  - 正常にインストールされていれば、下記のように出力されます。
  ```
  ROS_ETC_DIR=/opt/ros/melodic/etc/ros
  ROS_ROOT=/opt/ros/melodic/share/ros
  ROS_MASTER_URI=http://localhost:11311
  ROS_VERSION=1
  ROS_PYTHON_VERSION=2
  ROS_PACKAGE_PATH=/opt/ros/melodic/share
  ROSLISP_PACKAGE_DIRECTORIES=
  ROS_DISTRO=melodic
  ```

## ディレクトリーの作成
- 研究開発用ディレクトリーとしてcatkin_wsおよびsrcを作成します。
  ```
  # mkdir -p ~/catkin_ws/src
  ```

## プログラムのビルド
- コマンドcatkin_makeを用いてビルドします。この時点では何もプログラムが無いので、すぐに100％になり、ビルドが完了します。
  ```
  # cd ~/catkin_ws
  # catkin_make
  # echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
  # source ~/.bashrc
  ```
- 正常にビルドできていれば、コマンドroscdで「~/catkin_ws/devel」に移動します。
  ```
  # roscd
  ```

[このページのトップへ](#)

[応用実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
