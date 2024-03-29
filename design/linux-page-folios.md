# メモリフォリオ

Linux 5.16から導入されたメモリ管理方式([「Linux 5.16」が公開 | OSDN Magazine](https://mag.osdn.jp/22/01/11/194000))。

[Memory folios [LWN.net]](https://lwn.net/Articles/856016/)より抜粋して勝手訳。

4KiBページでメモリを管理することは、深刻なオーバーヘッドとなります。

多くのベンチマークは、より大きな「ページサイズ」の恩恵を受けることができます。 例えば、このアイデアの初期のイテレーションでは、複合ページを使って（特にチューニングしていなかった）、カーネルをコンパイルする際に7%の性能向上が得られました。

複合ページやTHPを使用すると、我々の型システムの弱点が露呈します。関数はしばしば複合ページが渡されることを予期しておらず、PAGE_SIZEチャンクに対してのみ動作することがあります。 複合ページを認識している関数でさえ、ヘッドページを期待し、テールページを渡されると間違ったことをする可能性があります。

また、テールページを見ていないことを確認するために、多くの命令を浪費しています。

`PageFoo()`のほぼすべての呼び出しは`compound_head()`への 1 つ以上の隠れた呼び出しを含んでいます。これは`get_page()`,`put_page()`やその他多くの関数でも起こります。また、`compound_head()`がべき等であることを伝える方法もありません。

このパッチシリーズはメモリを管理するために新しい型である`struct folio`を使用します。

これは、それ自体で価値のあるいくつかの基本的なインフラストラクチャを提供し、カーネルを約5kBのテキストで縮小しています。
