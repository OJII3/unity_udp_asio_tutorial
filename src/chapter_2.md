# Chapter 2 - 環境構築

この章では、本チュートリアルを進めるための環境構築を行います。

## OS

- Ubuntu 22.04 LTS
- Android

必ず実機で用意してください。WSL2, Docker, VM では動かせない部分が出てきます。Android のバージョンは未確認ですが、古すぎない方が良いでしょう。

(筆者の経験上、Arch Linux を用いることは避けるべきです。動作はしますが、簡単に壊れる上、サポートが不十分です。)

## ROS 2

すでにインストールできているものとして、ここでは扱いません。

ROS 2 及び C++ の最低限の知識があることを前提としていますが、丁寧に進めていく予定ですので、心配しなくても大丈夫でしょう。

バージョンは ROS 2 Humble の使用を想定しています。

## Unity

[Unity公式のインストールガイド](https://docs.unity3d.com/hub/manual/InstallHub.html)を参考にしてください。Unity の Linux 版サポートは遅かったり、不十分な場合がありますが、基本的には問題ないでしょう。

1. パッケージ認証用の公開鍵を追加します。

```bash
wget -qO - https://hub.unity3d.com/linux/keys/public | gpg --dearmor | sudo tee /usr/share/keyrings/Unity_Technologies_ApS.gpg > /dev/null
```

2. リポジトリを追加します。

```bash
sudo sh -c 'echo "deb [signed-by=/usr/share/keyrings/Unity_Technologies_ApS.gpg] https://hub.unity3d.com/linux/repos/deb stable main" > /etc/apt/sources.list.d/unityhub.list'
```

3. Unity Hub をインストールします。

```bash
sudo apt update
sudo apt-get install unityhub
```

4. Unity Hub を起動し、Unity Editor をインストールします。

任意のバージョンで問題ありません。Android ビルドサポートのチェックを忘れないでください。

5. .NET SDK をインストールします。

`apt` で最も新しいバージョンをインストールしておけば問題ないでしょう。

```bash
sudo apt install dotnet-sdk-8.0
```

6. JetBrains Rider をインストールします。(Optional)

JetBrains 社の Unity 用 IDE で、学生ライセンスの認証を入手することで、Rider だけでなく、他の JetBrains 製品も無料で使えるようになります。
ただし、この操作は任意です。一応、VSCode で代替することが出来ます(非推奨)。

JetBrains Rider を使用する利点としては、デバッグ機能が強力であること、フォーマッターやリファクタリング機能が優れていることが挙げられます。

[公式サイト](https://www.jetbrains.com/ja-jp/toolbox-app/) から Toolbox をダウンロードし、Toolbox から Rider をインストールしてください。JetBrains Toolbox を経由せずに Rider を入れると、後のトラブルの原因になります。

[このブログ](https://blog.jetbrains.com/ja/2019/08/22/2105/) を参考に、学生ライセンスを取得してください。

## Android の開発者モード

Android の設定アプリから、端末情報をタップし、ビルド番号を 10 回タップすることで、開発者モードを有効にしてください。その後、USB デバッグを有効にしてください。
