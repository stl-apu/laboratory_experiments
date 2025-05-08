# ROS parameters（パラメーター）
トピック通信とサービス通信に加えて、パラメーター通信についても体験します。パラメーターはノードの設定値です。ノードを実行した後（実行中）に値を変更したい時に使用します。また、ノード実行する時の条件として設定することもできます。

パラメーター通信を理解すると、グローバル変数のように全てのノードで値を共有することができます。また、パラメーターを介して、他のノードの値を参照することもできます。

## ROSパラメーターの確認
とりあえず、ノード「turtlesim」に属しているパラメーターを確認してみましょう。シミュレーター実行中に下記のコマンドを実行してみてください。パラメーターの一覧が表示されると思います。全てのノードがパラメーター「use_sim_time」を持ちます。コンピューターシステムの時間（falseの場合）とROSシステムの時間（trueの場合）のどちらを使用するのかを設定するためのパラメーターです。
```
$ ros2 param list
```

パラメーターの種類（key）と値（value）を確認してみます。整数型、浮動小数点型、論理型、文字型などを使用することができます。
```
《記法》
$ ros2 param get ノード名 パラメーター名
《実例》
$ ros2 param get /turtlesim background_b
```

## ROSパラメーターの変更
パラメーターの値を変更してみます。例えば、シミュレーターの背景色を変更することができます。background_bだけでなく、background_gやbackground_rも変更してみましょう。
```
《記法》
$ ros2 param set ノード名 パラメーター名 値
《実例》
$ ros2 param set /turtlesim background_b 51
```

ノードを起動するたびにパラメーターを1つ1つ変更するのは大変なので、普通は設定値を外部ファイルに保存しておきます。現在のディレクトリーにノード名のyamlファイル（例：turtlesim.yaml）が作成されます。
```
《記法》
$ ros2 param dump ノード名 > パラメーターファイル名
《実例》
$ ros2 param dump /turtlesim > turtlesim.yaml
```

現在の設定値が保存されているかを確認してみましょう。
```
《記法》
$ cat パラメーターファイル名
《実例》
$ cat turtlesim.yaml
```

1度、ノード「turtlesim」を停止し、パラメーターの値を指定しながら再起動してみます。なお、「./」はカレントディレクトリー（現在のディレクトリー）を意味します。
```
《記法》
$ ros2 run パッケージ名 実行ファイル名 --ros-args --params-file パラメーターファイル名
《実例》
$ ros2 run turtlesim turtlesim_node --ros-args --params-file ./turtlesim.yaml
```

## 参考：ROSパラメーターの使い道
ロボットを動かしながら値を変更できるので、狭い道に入ったら最高速度の上限を下げて安全に移動するなど、ロボットの動きを柔軟に調整することができます。

起動時に設定する場合はlaunchファイル内でyamlファイルを読み込みます。詳細はページ「[ROS2 Documentation](https://docs.ros.org/en/humble/Tutorials/Intermediate/Launch/Using-ROS2-Launch-For-Large-Projects.html#loading-parameters-from-yaml-file)」などを参照してください。

その他、参考情報として、名前空間（パラメーター名前空間）を使用すると、同じパラメーター名を複数使用することができます。同じロボットが複数台いる状況などで重宝します。

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
