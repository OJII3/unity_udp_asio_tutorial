# Chapter 6 - Unity の基礎知識

Unity の環境構築に関しては [環境構築](/chapter_2) で行いました。Unity では C# プログラミング以外の要素も多く、一から教えるのは大変なため、以下を実践することをオススメします。

## Unity 入門

Unity 初心者であれば私の知人が書いた [Unity 講習会2024入門編](
https://tuatmcc.com/blog/UnityLec2024Step1/) を一通り追うことをおすすめします。

## Unity 応用

ある程度分かる人や、上記入門編を終えた人は、[Unity 講習会2024 応用編](https://tuatmcc.com/blog/UnityLec2024Step2/) から、InputSystem と TextMeshPro を扱えるようになりましょう。

(ただし、この記事とは異なり、InputSystem は InputSystem は C# のコード生成をしてイベント駆動にして使うほうがプログラマーには分かりやすいと思います。)

## Unity 発展

[Unity 講習会2024 発展編](https://tuatmcc.com/blog/UnityLec2024Step3/) より、Zenject, UniTask, R3 に関する知識を身につけましょう。UniTask は C++ の ‘std::future‘ のシングルスレッド版と考えれば分かりやすいと思います。Zenject の理解が難しい場合は、オブジェクト指向やデザインパターンについて調べると良いと思われます。


## まとめ

ここまでできれば、今後知らないことがUnity内で出てきても行き詰まることは少なくなると思います。次回は早速 C# で UDP 通信を書いて行きたいと思います。