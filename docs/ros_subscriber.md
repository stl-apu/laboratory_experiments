# ROSのサブスクライバー
第1週で学んだリスナーを自作してみます。

practice_subscriber_node.pyを作成します。

```
$ cd ~/colcon_ws/src/laboratory_experiments_2024/practice_package/practice_package
$ nano practice_subscriber_node.py
```

## モジュールのインポート
モジュール「rclpy」をインポートします。rclpyはROS Client Library for Pythonのことで、ROSノードをPythonで記述する際に必ず必要となるモジュールです。

```
import rclpy
from rclpy.node import Node
```

また、今回は標準的なトピックメッセージ型を使用するため、モジュール「std_msgs」もインポートします。

```
from std_msgs.msg import String
```

## クラスの定義
ROSノードはクラスNodeを継承して作成するのが一般的です。下記の通り、クラス「Node」を継承したクラス「PracticeSubscriber」を定義します。

```
class PracticeSubscriber(Node):
    def __init__(self):
        super().__init__('practice_subscriber_node')
        self.sub = self.create_subscription(String, 'practice_topic', self.callback, 10)
    def callback(self, msg):
        self.get_logger().info(f'Subscribe: {msg.data}')
```

関数「\_\_init\_\_」はコンストラクターで、クラスのインスタンスを生成する時に呼び出されます。関数「super」でクラスNodeのコンストラクターを呼び出し、ノード名を指定します。関数「create_subscription」では「トピックメッセージ型」「トピック名」「コールバック関数名」「通信品質」を指定します。

関数「callback」はコールバック関数で、トピックを受け取った時に呼び出されます。関数「get_logger」は情報をターミナルに表示します。

## メイン関数の定義
Pythonは逐次型のプログラミング言語なので、main関数が無くても問題ありませんが、エントリーポイントとして記述します。下記の通り関数「main」を定義します。

```
def main():
    print('========== プログラム開始 ==========')
    rclpy.init()
    node = PracticeSubscriber()
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        print('Ctrl＋cが押されました。')
    finally:
        node.destroy_node()
        rclpy.shutdown()
        print('========== プログラム終了 ==========')
```

関数「init」でROS 2の通信を初期化します。

次に、インスタンスを生成します。

例外処理により、キーボードで強制終了した場合でも安全にROSノードを終了できるようにします。

あとは関数「spin」でトピックを待ち続けます。

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
