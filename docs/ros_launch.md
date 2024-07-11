# ROS launch（ローンチ）
毎回、複数のターミナルを開いて実行するのは面倒なので、普通はlaunchファイルを作成します。

コマンド`launch`でlaunchファイルを実行すると、複数のROSノードを1つのターミナルで起動することができます。

ディレクトリー「launch」を作成し、その中にlaunchファイルを置きます。launchファイル名は「sample_launch.py」のように最後に「_launch」を付けることが多いです。

```
~/colcon_ws/src/パッケージ名/launch/ローンチファイル名.py
や
~/colcon_ws/src/メタパッケージ名/パッケージ名/launch/ローンチファイル名.py
```

## xtermのインストール
予め端末エミュレーター「XTerm」をインストールしておきます。

```
$ sudo apt update
$ sudo apt install xterm -y
```


## launchファイルの試用
試しにlaunchファイルを実行してみましょう。1つのコマンドで3つのノードを起動することができます。

```
$ ros2 launch sample_package listener_talker_launch.py
```

3つのノードは、
- listenerノードが端末と共に起動します。
- talkerノードが端末と共に起動します。
- rqt_graphノードがGUIツールとして起動します。


## launchファイルの作成
拡張子から予想できると思いますが、launchファイルはPythonで記述します。

まず、パッケージ「practice_package」にディレクトリー「launch」を作成します。

```
$ cd ~/colcon_ws/src/laboratory_experiments_2024/practice_package/
$ mkdir launch
$ cd launch
```

次に、ファイルpractice_launch.pyを作成します。

```
$ nano practice_launch.py
```

## launchファイルの記述
ここから具体的にlaunchファイルの中身を記述していきます。

2つのモジュールをimportします。

```
import launch
import launch_ros.actions
```

関数「generate_launch_description」でオブジェクト「LaunchDescription」を返します。

```
def generate_launch_description():
    return launch.LaunchDescription([
    ])
```

角括弧の間に起動するROSノードに関する情報を記述していきます。

1つ目のノードとしてsubscriberを起動します。必須項目は「package」と「executable」の2つのみです。他の項目については自分で学習しましょう。

```
launch_ros.actions.Node(
    package='practice_package',
    executable='practice_subscriber_node',
    output='screen',
    prefix='xterm -e',
    on_exit=launch.actions.Shutdown())
```

2つ目のノードとしてpublisherを起動します。<span style="color: #CC0066;">複数のROSノードを起動する時はコンマで区切ります。</span>

```
launch_ros.actions.Node(
    package='practice_package',
    executable='practice_publisher_node',
    output='screen',
    prefix='xterm -e',
    on_exit=launch.actions.Shutdown())
```

3つ目のノードとしてrqt_graphを起動します。rqt_graphはGUIツールなので、xtermを起動する必要はありません。

```
launch_ros.actions.Node(
    package='rqt_graph',
    executable='rqt_graph',
    output='screen',
    on_exit=launch.actions.Shutdown())
```

## launchファイルの設定
setup.pyを<span style="color: #CC0066;">編集する</span>必要があるので、テキストエディターで開きます。ROSパッケージを作成した時に必要なファイルの1つとして作成されているので、新たに作成する必要はありません。

```
$ nano setup.py
```

まず、osモジュールとglobモジュールをインポートします。

```
import os
from glob import glob
```

次に、data_filesにlaunchファイルの参照先を追加します。関数joinの中の「package_name」を「practice_package」などと書き換える必要はありません。下記のまま記述してください。関数globの中のアステリスク（*）は正規表現です。意味が分からない人は学習してください。なお、「：」は省略記号です。

```
：
data_files=[
    ：
    (os.path.join('share', package_name), glob('launch/*_launch.py')),
],
：
```

## launchファイルの実行
ビルドします。

```
$ cd ~/colcon_ws && colcon build
```

一応、xtermが正常に起動するかを事前に確認しておきます。

```
$ xterm
```

実行します。

```
$ ros2 launch practice_package practice_launch.py
```

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
