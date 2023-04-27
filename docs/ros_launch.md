# ROS launch（ローンチ）
- 毎回、複数のターミナルを開いて実行するのは面倒なので、普通はlaunchファイルを作成します。
- コマンドlaunchでlaunchファイルを実行すると、複数のROSノードを1つのターミナルで起動することができます。
- launchディレクトリーを作成し、その中にlaunchファイルを置きます。
    ```
    ~/colcon_ws/src/パッケージ名/launch/launchファイル名.py
    ```
    - launchファイル名は「sample_launch.py」のように最後に「_launch」を付けることが多いです。

## launchファイルの試用
- 試しにlaunchファイルを実行してみましょう。1つのコマンドで3つのノードを起動することができます。
    ```
    $ ros2 launch sample_package listener_talker_launch.py
    ```
    - listenerノードが端末と共に起動します。
    - talkerノードが端末と共に起動します。
    - rqt_graphノードがGUIツールとして起動します。

## launchファイルの作成
- 拡張子から分かると思いますが、launchファイルはPythonで記述します。
- まず、パッケージpractice_packageにlaunchディレクトリーを作成します。
    ```
    $ cd ~/colcon_ws/src/practice_package/
    $ mkdir launch
    $ cd launch
    ```
- 次に、ファイルpractice_launch.pyを作成します。
    ```
    $ nano practice_launch.py
    ```

## launchファイルの記述
- ここから具体的にlaunchファイルの中身を記述していきます。
- 2つのモジュールをimportします。
    ```
    import launch
    import launch_ros.actions
    ```

- 関数`generate_launch_description`でオブジェクトLaunchDescriptionを返します。
    ```
    def generate_launch_description():
        return launch.LaunchDescription([
        ])
    ```

- 角括弧の間に起動するROSノードに関する情報を記述します。
- 1つ目のノードとしてsubscriberを起動します。
    ```
    launch_ros.actions.Node(
        package='practice_package',
        executable='subscriber',
        output='screen',
        prefix='xterm -e',
        on_exit=launch.actions.Shutdown())
    ```
    - 必須項目はpackageとexecutableのみです。他の項目については自分で調査してみましょう。
- 2つ目のノードとしてはpublisherを起動します。
    ```
    launch_ros.actions.Node(
        package='practice_package',
        executable='publisher',
        output='screen',
        prefix='xterm -e',
        on_exit=launch.actions.Shutdown())
    ```
    - 複数のROSノードを起動する時はコンマで区切ります。
- 3つ目はrqt_graphを起動します。
    ```
    launch_ros.actions.Node(
        package='rqt_graph',
        executable='rqt_graph',
        output='screen',
        on_exit=launch.actions.Shutdown())
    ```
    - rqt_graphはGUIツールなので、xtermを起動する必要はありません。

## launchファイルの設定
- setup.pyを編集する必要があります。
- osモジュールとglobモジュールをインポートします。
    ```
    import os
    from glob import glob
    ```
- data_filesにlaunchファイルの参照先を追加します。
    ```
    ：
    data_files=[
        ：
        (os.path.join('share', package_name), glob('launch/*_launch.py')),
    ],
    ：
    ```
    - package_nameを「practice_package」などと書き換える必要はありません。上記のまま記述してください。

## launchファイルの実行
- ビルドします。
    ```
    $ cd ~/colcon_ws && colcon build
    ```

- 一応、事前にxtermが使用できるかどうかを確認しておきます。
    ```
    $ xterm
    ```

- 実行します。
    ```
    $ ros2 launch practice_package practice_launch
    ```

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
