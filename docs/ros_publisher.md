# ROSのパブリッシャー
- 第1週で学んだトーカーを自作してみます。
- 今回は10回だけトピックを出版するトーカーを作成します。
- test_publisher_node.pyを作成します。
    ```
    $ cd ~/colcon_ws/src/test_package/test_package
    $ nano test_publisher_node.py
    ```

## モジュールのインポート
- リスナーと同様です。

## クラスの定義
- 下記の通り、TestPublisherを定義します。
    ```
    class TestPublisher(Node):
        def __init__(self):
            super().__init__('test_publisher_node')
            self.pub = self.create_publisher(String, 'test_topic', 10)
            self.timer = self.create_timer(1, self.timer_callback)
            self.i = 10
        def timer_callback(self):
            msg = String()
            if self.i > 0:
                msg.data = f'Test {self.i}'
            else:
                msg.data = f'Test finished'
                self.destroy_timer(self.timer)
            self.pub.publish(msg)
            self.get_logger().info(f'Publish: {msg.data}')
            self.i -= 1
    ```
    - 関数`create_publisher()`は「メッセージ型」「トピック名」「通信品質」を指定します。
        - 関数`create_subscription()`で指定した「メッセージ型」と「トピック名」に合わせます。
    - 関数`create_timer()`は「タイマーの間隔」と「コールバック」を指定する。
        - 「タイマーの間隔」の単位は秒（s）です。

## メイン関数の定義
- リスナーと同様です。ただし、インスタンスを作成するクラス名が異なります。

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
