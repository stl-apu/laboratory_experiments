# ROS launch（ローンチ）
毎回、複数のターミナルを開いて実行するのは面倒なので、普通はlaunchファイルを作成しておき、launchファイルを実行します。コマンド`launch`でlaunchファイルを実行すると、複数のROSノードを1つのターミナルで起動することができます。

ディレクトリー「launch」を作成し、その中にlaunchファイルを置きます。launchファイル名は「sample_launch.py」のように最後に「_launch」を付けることが多いです。

```
《記法》
~/colcon_ws/src/パッケージ名/launch/ローンチファイル名.py
や
~/colcon_ws/src/メタパッケージ名/パッケージ名/launch/ローンチファイル名.py
```

## xtermのインストール
予め端末エミュレーター「XTerm」をインストールしておきます。
```
$ sudo apt install xterm -y
```

一応、xtermが正常に起動するかを確認しておきます。起動した場合はCtrl＋dで閉じておきます。
```
$ xterm
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
$ cd ~/colcon_ws/src/laboratory_experiments_2025/practice_package/
$ mkdir launch && cd $_
```

次に、ファイルpractice_launch.pyを作成します。
```
$ nano practice_launch.py
```

## launchファイルの記述
ここから具体的にlaunchファイルの中身を記述していきます。

まず、2つのモジュールをimportします。
```
import launch
import launch_ros.actions
```

関数「generate_launch_description」でオブジェクト「LaunchDescription」を返します。この角括弧の間に起動するROSノードに関する情報を記述していきます。
```
def generate_launch_description():
    return launch.LaunchDescription([
    ])
```

1つ目のノードとしてsubscriberを起動します。必須項目は「package」と「executable」の2つのみです。他の項目については自分で学習してみてください。
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

最終的には下記のような内容になっていると思います。なお、「…」は省略記号で、実際にはpackageやexecutableに関する設定が記述されています。
```
import launch
import launch_ros.actions
def generate_launch_description():
    return launch.LaunchDescription([
        launch_ros.actions.Node(…),
        launch_ros.actions.Node(…),
        launch_ros.actions.Node(…)
    ])
```

「Ctrl＋x」で保存しながら終了します。


## 設定ファイルの更新
setup.pyをテキストエディターで開き、編集します。setup.pyはROSパッケージを作成した時に必要なファイルの1つとして作成されている設定ファイルで、新たに作成する必要はありません。
```
$ cd ~/colcon_ws/src/laboratory_experiments_2025/practice_package/
$ nano setup.py
```

まず、osモジュールとglobモジュールをインポートするために、2行追記します。
```
import os
from glob import glob
```

次に、data_filesにlaunchファイルの参照先を追加します。注意点として、関数「join」の中の「package_name」を「practice_package」と書き換える必要はありません。下記のまま記述してください。関数「glob」の中のアステリスク（*）は正規表現です。なお、「：」は省略記号です。
```
setup(
    ：
    data_files=[
        ：
        (os.path.join('share', package_name), glob('launch/*_launch.py')),
    ],
    ：
)
```

## launchファイルの実行
ビルドします。
```
$ cd ~/colcon_ws && colcon build
```

実行します。ターミナル（xterm）2つとrqt_graphが表示されれば成功です。
```
$ ros2 launch practice_package practice_launch.py
```

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
