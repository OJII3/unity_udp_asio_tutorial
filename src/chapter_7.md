# Chapter 7 - Unity 補足

前回の記事で足りていないUnityテクニックを紹介します。

## .gitignore ファイルについて

[gitignore.io](https://www.toptal.com/developers/gitignore) に Unity と Rider を指定して生成すると良いでしょう。間違ってもVisual Studio を選択しないようにしてください。Unity で必須な多くのファイルが無視されてしまいます。

## Android SDK のインストール

OSのパッケージマネージャーで入れる必要はありません。Unity Editor のインストール時に、Andorid Build Support を選択すると、Android SDK も一緒にインストールされます。既にエディタをインストール済みの場合、Unity Hub のInstalls タブから、エディタの詳細設定を変更して、Android Build Support を追加します。

## Project Settings について

Edit → Project Settings でプロジェクトの設定を変更できます。以下の項目が重要です。

### ビルド設定

- Player → Other Settings → Configuration
    - Scripting Backend を IL2CPP に変更: LLVMを経由してネイティブコードを生成するため、ビルド時間が増加する変わりにパフォーマンスが大きく向上します。
    - Android → Minimum API Level を 新しいバージョンに変更: 古いバージョンのAPIをサポートするためには、API Level を下げる必要がありますが、セキュリティリスクが高まるため、最新のAPI Level を選択することをお勧めします。
    - Use Incremental GC を有効にする: ガベージコレクションのパフォーマンスを向上させます。デフォルトで有効のはずですが、エディタバージョンによっては何故かデフォルト無効になっているバグがあります。
- Editor
    - Enter Play Mode Options にチェックを入れ、Reload Domain のみ選択。Play Mode に入るときに、シーンをリロードしないことで、開発効率が向上します。

ここで小話をすると、Unity では Static は好まれません。その一因として、Static な変数や関数はメモリから解放するためには、Play Mode を停止するだけでなく、シーンをリロードする必要があります。しかし、快適な開発のために、Enter Play Mode Options で Reload Scene を選択したくない、という事情があります。

とくに、Static なクラスが状態を持つ場合、状態がリセットされなくなるため、注意が必要です。私が今回、DIコンテナを採用した理由の一つが、この問題を解決するためです。(シングルトンパターンを使うと、Static なクラスが増えるため、この問題が発生しやすくなります。)

## 名前空間について

Unity Editor 上の Create ボタン等から生成するC#スクリプトには名前空間が書かれていません。一般的には、プロジェクト名を名前空間に使うことが多いです。Rider が推奨する名前空間はディレクトリ構成しか見ないため、`ProjectName.Scripts` 等になってしまいますが、`Scripts/` を名前空間から除外するよう設定できます。
