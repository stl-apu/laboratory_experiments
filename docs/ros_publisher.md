# ROSのパブリッシャー
第1週で使ったトーカーを自作してみます。今回は10回だけトピックを出版するトーカーを作成します。

practice_publisher_node.pyを作成します。コンテナーに入り、ROSの環境を設定した状態で進めてください。

```
$ cd ~/colcon_ws/src/laboratory_experiments_2025/practice_package/practice_package
$ nano practice_publisher_node.py
```

## モジュールのインポート
3つのモジュールをインポートします。リスナーと同様ですので、リスナーのプログラムをコピー＆ペーストしてください。

## クラスの定義
下記の通り、クラス「PracticePublisher」を定義します。関数「create_publisher」は「トピックメッセージ型」「トピック名」「通信品質」を指定します。「トピックメッセージ型」と「トピック名」は、リスナーの関数「create_subscription」で指定したものに合わせます。関数「create_timer」は「タイマーの間隔」と「コールバック関数名」を指定します。なお、「タイマーの間隔」の単位は秒（s）です。

```
class PracticePublisher(Node):
    def __init__(self):
        super().__init__('practice_publisher_node')
        self.pub = self.create_publisher(String, 'practice_topic', 10)
        self.timer = self.create_timer(1, self.timer_callback)
        self.i = 10
    def timer_callback(self):
        msg = String()
        if self.i > 0:
            msg.data = f'i = {self.i}'
        else:
            msg.data = f'Over and out!'
            self.destroy_timer(self.timer)
        self.pub.publish(msg)
        self.get_logger().info(f'Publish: {msg.data}')
        self.i -= 1
```

## メイン関数の定義
リスナーと同様ですので、リスナーのプログラムをコピー＆ペーストしてください。ただし、インスタンスを作成するクラス名が異なる点に注意してください（PracticeSubscriberでなく、PracticePublisherです）。

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
