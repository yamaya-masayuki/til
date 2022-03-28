# Normalized Device Coordinate Metal coming from OpenGL

[math - Normalized Device Coordinate Metal coming from OpenGL - Stack Overflow](https://stackoverflow.com/questions/54827687/normalized-device-coordinate-metal-coming-from-opengl)の意訳。

## Q

SOでは、正規化されたデバイス座標に言及した質問がたくさんありますが、私の特定の問題を扱っているものはありません。
つまり、私が描くものはすべて2Dスクリーン座標で指定され、左上、左下は(0,0）、右下は(screenWidth, screenHeight)です。

それから頂点シェーダーでこの計算を行ってNDCを算出します（基本的に、私はUI要素をレンダリングします）:

```glsl
float ndcX = (screenX - ScreenHalfWidth) / ScreenHalfWidth;
float ndcY = 1.0 - (screenY / ScreenHalfHeight);
```

ここで、ScreenX/ScreenYはピクセル座標(600,700)、`screenHalf*` は画面の幅/高さの半分です。

そして、ラスタライズ状態の頂点シェーダーから返す最終位置は:

```glsl
gl_Position = vec4(ndcX, ndcY, Depth, 1.0);
```

です。

これはOpenGL ESでは全く問題なく動作します。

さて、問題は、Metal 2でこのように試してみると、うまくいかないことです。

MetalのNDCが2x2x1でOpenglのNDCが2x2x2であることは知っていますが、私はこの方程式で頂点ごとに自己で渡しているので、ここの深さは重要な役割を演じないと思っていました。

私は今のところすべてを2Dでレンダリングしているので、頂点シェーダーでの行列計算を避けようとしているので、この[リンク](https://metashapes.com/blog/opengl-metal-projection-matrix-problem/)と[このSO質問](https://stackoverflow.com/questions/42751427/transformations-from-pixels-to-ndc)を試してみましたが、混乱し、リンクはそれほど役に立ちませんでした。

そこで質問なのですが、Metalでピクセル座標をNDCに変換する公式は何でしょうか？
正射投影行列を使わなくても可能でしょうか？
なぜ私の式はMetalでうまくいかないのでしょうか？

## A

もちろん、正射投影行列がなくても可能です。

行列は、変換を適用するための便利な道具に過ぎないのです。
しかし、このような状況が発生した場合、行列がどのように機能するかを理解することが重要です。
一般的な正射投影行列を使用すると、同じ結果を得るために不必要な演算を行うことになるからです。

そのために使うかもしれない数式を紹介します:

```msl
float xScale =  2.0f / drawableSize.x;
float yScale = -2.0f / drawableSize.y;
float xBias = -1.0f;
float yBias =  1.0f;

float clipX = position.x * xScale + xBias;
float clipY = position.y * yScale + yBias;
```

ここで、`drawableSize`はレンダーバッファの寸法（ピクセル単位）であり、頂点シェーダーにバッファとして渡すことができます。
GPUでの計算をいくらか節約するために、スケールファクタを事前に計算し、画面寸法の代わりにそれを渡すこともできます。

### Reply

シェーダーでの計算が少ないことを除けば、私の計算とほぼ同じですね。
なぜうまくいかないと思っていたのか、問題が見つかりました。
それは、iOSで起こるネイティブスケーリングと関係がありました。
計算は正しいのですが、ビューポートと描画可能なサーフェイスの設定が間違っていたのです。
