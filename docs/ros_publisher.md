# ROSのパブリッシャー
第1週で学んだトーカーを自作してみます。

今回は10回だけトピックを出版するトーカーを作成します。

practice_publisher_node.pyを作成します。

```
$ cd ~/colcon_ws/src/laboratory_experiments_2023/practice_package/practice_package
$ nano practice_publisher_node.py
```

## モジュールのインポート
リスナーと同様です。

## クラスの定義
下記の通り、クラスPracticePublisherを定義します。

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

関数`create_publisher()`は「トピックメッセージ型」「トピック名」「通信品質」を指定します。リスナーの関数`create_subscription()`で指定した「トピックメッセージ型」と「トピック名」に合わせます。

関数`create_timer()`は「タイマーの間隔」と「コールバック」を指定します。「タイマーの間隔」の単位は秒（s）です。

## メイン関数の定義
リスナーと同様ですので、リスナーのプログラムをコピー＆ペーストしましょう。ただし、インスタンスを作成するクラス名が異なる点に注意してください。

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
