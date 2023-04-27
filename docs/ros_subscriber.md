# ROSのサブスクライバー
- 第1週で学んだリスナーを自作してみます。
- practice_subscriber_node.pyを作成します。
    ```
    $ cd ~/colcon_ws/src/practice_package/practice_package
    $ nano practice_subscriber_node.py
    ```

## モジュールのインポート
- モジュールrclpyをインポートします。rclpyはROS Client Library for Pythonで、ROSノードをPythonで記述する際に必ず必要となるモジュールです。
    ```
    import rclpy
    from rclpy.node import Node
    ```
- また、今回は標準的なトピックメッセージ型を使用するため、モジュールstd_msgsもインポートします。
    ```
    from std_msgs.msg import String
    ```

## クラスの定義
- ROSノードはクラスNodeを継承して作成するのが一般的です。
- 下記の通り、クラスNodeを継承したクラスPracticeSubscriberを定義します。
    ```
    class PracticeSubscriber(Node):
        def __init__(self):
            super().__init__('practice_subscriber_node')
            self.sub = self.create_subscription(String, 'practice_topic', self.callback, 10)
        def callback(self, msg):
            self.get_logger().info(f'Subscribe: {msg.data}')
    ```
    - __init__()はコンストラクターで、クラスのインスタンスを生成する時に呼び出されます。
        - super()でクラスNodeのコンストラクターを呼び出し、ノード名を指定します。
        - 関数`create_subscription()`では「トピックメッセージ型」「トピック名」「コールバックメソッド名」「通信品質」を指定します。
    - callback()はコールバックメソッドで、トピックを受け取った時に呼び出されます。
        - get_loggerは情報をターミナルに表示します。

## メイン関数の定義
- Pythonは逐次型の言語なので、main関数が無くても問題ありませんが、エントリーポイントとして記述します。
    ```
    def main():
        print('==========プログラム開始==========')
        rclpy.init()
        node = PracticeSubscriber()
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        print('Ctrl＋cが押されました。')
    finally:
        node.destroy_node()
        rclpy.shutdown()
        print('==========プログラム終了==========')
    ```
    - 関数`main()`を定義します。
        - 関数`init()`でROS2の通信を初期化します。
        - 次に、インスタンスを生成します。
        - 関数`spin()`でトピックを待ちます。
        - 例外処理により、キーボードで強制終了した場合でも安全にROSノードを終了できるようにします。

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
