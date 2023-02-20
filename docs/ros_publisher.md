# ROSのパブリッシャー
- 第1週で学んだトーカーを自作してみます。

## Pythonプログラムの作成
- test_publisher_node.pyを作成します。
    ```
    $ cd ~/colcon_ws/src/test_package/test_package
    $ nano test_publisher_node.py
    ```

### モジュールのインポート
- リスナーと同様です。

### クラスの定義
- 下記の通り、TestPublisherを定義します。
    ```
    class TestSubscriber(Node):
        def __init__(self):
            super().__init__('test_publisher_node')
            self.sub = self.create_subscription(String, 'test_topic', self.callback, 1)
        def callback(self, msg):
            self.get_logger().info(f'Subscribe: {msg.data}')
    ```


[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
