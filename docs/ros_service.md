# ROS service（サービス）
ROSでは、トピック通信だけでなくサービス通信を使うこともできます。TurtleSimを用いてROSサービスを体験してみます。
- トピック通信：片方向・非同期のパブリッシュサブスクライブ通信（出版購読通信）
- サービス通信：双方向・同期のリクエストレスポンス通信（要求応答通信）

サービス名とサービスメッセージ型を決めて通信します。

トピック通信のトピックメッセージ型は1つの情報のみでしたが、サービス通信のサービスメッセージ型は2つの情報の組（要求と応答）になります。

トピック通信はセンサーのデータを送受信する時などに使用し、サービス通信はセンサーの設定を変更する時などに使用します。トピック通信では設定を正常に変更できたかどうかを確認できませんが、サービス通信では結果（正常終了／異常終了）を受け取ることができます。

## サービス名の確認
まず、TurtleSimでは、どのようなサービスが使用されているのかを確認してみます。シミュレーター実行中に下記のコマンドを実行してみてください。「/clear」や「/spawn」、「/turtle1/set_pen」などが存在していると思います。
```
$ ros2 service list
```

オプションtを付けるとサービスメッセージ型を確認することができます。
```
$ ros2 service list -t
```

## サービスメッセージ型の確認
サービスメッセージ型の構造は下記のコマンドで確認できます。記号「---」は区切り線で、線の上は要求に関する内容、線の下は応答に関する内容になります。
```
《記法》
$ ros2 interface show サービスメッセージ型
《実例》
$ ros2 interface show turtlesim/srv/Spawn
```

## サービス通信の試用
サービス通信はコマンド`ros2 service call`で実行できます。サービスの多くは引数（arguments）で値を指定しながら実行します。また、引数は省略できる場合があります。
```
《記法》
$ ros2 service call サービス名 サービスメッセージ型 引数
```

### 移動軌跡の消去
サービス「/clear」で全ての線（移動軌跡）を消せます。引数はありません。
```
$ ros2 service call /clear std_srvs/srv/Empty
```

### 亀ロボットの追加
引数「name」を空欄にし、サービス「/spawn」を実行してみます。すると、自動的にロボット名が与えられます（恐らくturtle2という名前が与えられます）。ロボット名を指定する場合はシングルクォーテーションマークで囲む必要がある点に注意してください。

```
$ ros2 service call /spawn turtlesim/srv/Spawn "{x: 2, y: 2, theta: 0.2, name: ''}"
や
$ ros2 service call /spawn turtlesim/srv/Spawn "{x: 2, y: 2, theta: 0.2, name: 'bowser'}"
```

複数のロボットを別々に動かしたい時は、「turtle_teleop_key」を追加します。その際、オプション「remap」でロボット名を変更することを忘れないでください。
```
$ ros2 run turtlesim turtle_teleop_key --ros-args -r turtle1/cmd_vel:=turtle2/cmd_vel -r turtle1/rotate_absolute:=turtle2/rotate_absolute
```

### 亀ロボットの削除
指定した亀ロボットがその軌跡と共に消えます。
```
《記法》
$ ros2 service call /kill turtlesim/srv/Kill "{name: '亀の名前'}"
《実例》
$ ros2 service call /kill turtlesim/srv/Kill "{name: 'bowser'}"
```

### リセット
シミュレーター実験を最初から遣り直すこともできます。
```
$ ros2 service call /reset std_srvs/srv/Empty
```

## RQtの活用
ROSサービスでもrqtを使うことができます。

### 移動軌跡の変更
亀ロボットを動かすと線（移動軌跡）が引かれますが、亀ごとに色を変更することができます。サービス名［/亀の名前/set_pen］で線の色（r、g、b）や線の太さ（width）を変更することができます。

色を確認しながら調整できるよう、rqtを使用しましょう。Plugins→Services→Service callerからサービス名を選択します。値を変更し、ボタン「Call」を押してみましょう。
```
$ rqt
```

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
