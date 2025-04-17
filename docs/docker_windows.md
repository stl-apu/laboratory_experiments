# Dockerの設定（Windows）

## コンピューターの設定
「タスクマネージャー」を起動し、［パフォーマンス］→［CPU］から［仮想化：有効］となっていることを確認します。［仮想化：無効］となっている場合は有効化してください。有効にする方法はコンピューターのメーカーや機種によって異なるので、自分で調べてください。

「コントロールパネル」を起動し、［プログラム］→［Windowsの機能の有効化または無効化］を開き、［Hyper-V］と［Linux用Windowsサブシステム（Windows Subsystem for Linux）］にチェックが入っていることを確認します。入っていない場合は入れてください（再起動が必要になると思います）。また、ページ「[事前準備](https://stl-apu.github.io/laboratory_experiments/preparetion)」に書きましたが、WindowsのエディションがHomeの場合は、そもそもプログラムが存在しない可能性が高いです。存在しない場合は各自でインストールしてください。

## ソフトウェアのインストール
DockerのアカウントでDocker Hubにサインインできることを確認します。

→[https://hub.docker.com/](https://hub.docker.com/)

Docker Desktop for Windowsをインストールします。

→[https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

Docker Desktop.exeを起動し、IDとPasswordを入力します。

コマンドプロンプトを起動し、コマンド`docker`が実行できることを確認します。インストールされたDockerの版が表示されたらOKです。
```
> docker version
```

## GUIの設定
GUIを使用するためにVcXsrvをインストールします。

→[https://sourceforge.net/projects/vcxsrv/](https://sourceforge.net/projects/vcxsrv/)

ホストOSのIPアドレスを確認しておきます。Wi-FiなどのIPv4アドレスを確認しましょう。
```
> ipconfig
```

GUIを使用するためにX11サーバーの設定を変更します。VcXsrvのインストール先（/Program Files/VcXsrv）にあるX0.hostsファイルに、X Window Systemの転送を許可するホストOSのIPアドレスを記入し、保存します。注意点として、ネットワーク環境によってIPアドレスは変化するので、大学以外で実験を実施する場合などは適宜、修正してください。
```
《記法》
localhost
ホストOSのIPアドレス　※追記
《実例》
localhost
172.31.1.10
```

コンピューターを再起動します。

再起動後、VcXsrv（X11サーバー）をソフトウェア「XLanuch」で起動します。下記の通り設定します。システムトレイ（通知領域）にXLanuchのアイコンが表示されます。
- 「Multiple windows」を選択します。
- 「Display number」は「-1」でOKです。
- 「Start no client」を選択します。
- 「Native opengl」のチェックを外します。

「Dockerの設定」に続いて「Dockerの使い方」を実施してもらいますが、<span style="color: #CC0066;">もし上記の設定でGUI（マウスカーソルを見続ける目など）が表示されない場合は実験中だけ「Disable access control」にチェックを入れるようにしましょう。</span>

[このページのトップへ](#)

[Dockerページへ](https://stl-apu.github.io/laboratory_experiments/docker)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
