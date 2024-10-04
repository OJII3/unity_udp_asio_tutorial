# Chapter 10 - ROS 2 でUDP通信

C++ で UDP 通信を行なう際は、`asio` を用いる。直接ソケットにさわる方法もあるが、型が効きずらくなる他、生ポインタを扱うことになるため、推奨しない。

## asio のインストール

apt で `libasio-dev` をインストールする。


## 実際のコード

- [GitHub](https://github.com/TUAT-RUR/ros2_joycon)
- [GitLab](https://rur.mech.tuat.ac.jp/internal/gitlab/RUR_Library/ros2/joycon)

## 使い方

README.md に記載の通り。

## 説明

- できるかぎり、Node 内では コンストラクタ、タイマー、サブスクリプションコールバック以外の処理を書かないようにした。特に、自分で非同期処理、スレッドを書くことは避ける。標準入出力がメインスレッドに帰ってこないため、`SIGINT` が一発で効かなくなる。同様の理由から、ログ出力はROSのロガーを使用するように。
- 最後に慌ててシーケンス送信を実装したため、`odometry_sender` のコードが汚い。
- `packet.hpp` は、Unity 側と同様のプロトコルを定義、実装してある。ただ、任意の構造体をシリアライズできるようにすれば良かったと思う。

さらなる詳細はCursorかCopilotChatにでも聞いてください。
