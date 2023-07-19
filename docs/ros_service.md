# ROS service（サービス）
- ROSではトピック通信だけでなくサービス通信を使うこともできます。
    - トピック通信は片方向・非同期のパブリッシュサブスクライブ通信（出版購読通信）
    - サービス通信は双方向・同期のリクエストレスポンス通信（要求応答通信）

- サービス名とサービスメッセージ型を決めて通信します。トピック通信はトピックメッセージ型のみでしたが、サービスメッセージ型はクライアントメッセージ型とサーバーメッセージ型の組になります。

- トピック通信はセンサーのデータを送信する時などに使用し、サービス通信はセンサーの設定を変更する時などに使用します。トピック通信では設定を正常に変更できたかどうかを確認できませんが、サービス通信では結果（正常or異常）を受け取ることができます。

- 本演習ではTurtlesimを用いてROSサービスを体験してみます。

## 一覧の確認
- まず、TurtleSimでは、どのようなサービスが使用されているのかを確認してみます。「/clear」や「/spawn」、「/turtle1/set_pen」などが存在していると思います。
```
$ ros2 service list
```

- オプションtを付けるとサービスメッセージ型も確認することができます。
```
$ ros2 service list -t
```

### 軌跡の消去
- サービス/clearで全ての線を消せます。
- サービスはコマンド`call`で実行できます。
```
《記法》
$ ros2 service call サービス名 サービスメッセージ型
《実例》
$ ros2 service call /clear std_srvs/srv/Empty
```

### 亀の追加
- サービスの多くは引数（arguments）で値を指定しながら実行します。「引数は省略できる場合がある」という理解の方が正しいです。「name」を空欄にすると自動的に名前が与えられます（恐らくturtle2です）。
```
《記法》
$ ros2 service call サービス名 サービスメッセージ型 引数
《実例》
$ ros2 service call /spawn turtlesim/srv/Spawn "{x: 2, y: 2, theta: 0.2, name: ''}"
```

- 「name」を指定する時はシングルクォーテーションマークで囲む必要がある点に注意してください。
```
$ ros2 service call /spawn turtlesim/srv/Spawn "{x: 2, y: 2, theta: 0.2, name: 'bowser'}"
```

### 軌跡の色の変更
- 亀ロボットを動かすと線が引かれますが、亀ごとに色を変更することができます。サービス名［/亀の名前/set_pen」で軌跡の色（r、g、b）や太さ（width）を変更することができます。

- 色を確認しながら調整できるよう、rqtを使用しましょう。Plugins→Services→Service callerからサービスを選択します。値を変更し、ボタン「Call」を押してみましょう。
```
$ rqt
```

### 亀の削除
- 指定した亀がその軌跡と共に消えます。
```
《記法》
$ ros2 service call /kill turtlesim/srv/Kill "{name: '亀の名前'}"
《実例》
$ ros2 service call /kill turtlesim/srv/Kill "{name: 'bowser'}"
```

- 実験を最初から遣り直すこともできます。
```
$ ros2 service call /reset std_srvs/srv/Empty
```

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
