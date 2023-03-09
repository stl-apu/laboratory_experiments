# ROSのサブスクライバー
- 第1週で学んだリスナーを自作してみます。
- test_subscriber_node.pyを作成します。
    ```
    $ cd ~/colcon_ws/src/test_package/test_package
    $ nano test_subscriber_node.py
    ```

## モジュールのインポート
- rclpyモジュールをインポートする。rclpyはROS Client Library for Pythonで、必ず必要なモジュールです。
    ```
    import rclpy
    from rclpy.node import Node
    ```
- また、今回は標準的なトピックメッセージ型を使用するため、std_msgsモジュールもインポートします。
    ```
    from std_msgs.msg import String
    ```

## クラスの定義
- ROSノードはクラスNodeを継承したクラスを用いて作成するのが一般的です。
- 下記の通り、クラスNodeを継承したクラスTestSubscriberを定義します。
    ```
    class TestSubscriber(Node):
        def __init__(self):
            super().__init__('test_subscriber_node')
            self.sub = self.create_subscription(String, 'test_topic', self.callback, 10)
        def callback(self, msg):
            self.get_logger().info(f'Subscribe: {msg.data}')
    ```
    - __init__()はコンストラクターで、クラスのインスタンスを生成する時に呼び出される。
      - super()でクラスNodeのコンストラクターを呼び出し、ノード名を指定する。
      - 関数`create_subscription()`では「トピックメッセージ型」「トピック名」「コールバックメソッド名」「通信品質」を指定する。
    - callback()はコールバックメソッドで、トピックを受け取った時に呼び出される。
      - get_loggerはターミナルに表示する。

## メイン関数の定義
- Pythonは逐次型の言語なので、main関数が無くても問題ありませんが、エントリーポイントとして記述します。
    ```
    def main():
        print('プログラム開始')
        rclpy.init()
        node = TestSubscriber()
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        print('Ctrl＋cが押されました。')
    finally:
        node.destroy_node()
        rclpy.shutdown()
        print('プログラム終了')
    ```
- 関数`main()`を定義します。
    - 関数`init()`でROS2の通信を初期化します。
    - そして、インスタンスを生成します。
    - 関数`spin()`でトピックを待ちます。
    - キーボードで強制終了した時に安全に終了できるようにします。

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
