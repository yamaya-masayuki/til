# From OpenGL to Metal - The Projection Matrix Problem

[From OpenGL to Metal - The Projection Matrix Problem - metashapes.com](https://metashapes.com/blog/opengl-metal-projection-matrix-problem/)の勝手訳。

AppleのMetal APIは、OpenGL/OpenGLESアプリケーションの移植を容易にするために設計されています。
ほとんどの場合、OpenGLの各コールには1:1の等価性があり、唯一の大きな変化は、ステートエンジンの代わりにRenderStateオブジェクトを使う方法などです。
行列も両APIで列ベースです。

しかし、今日私が発見したように、悪魔は細部に宿るのです。

直交投影(Orthogonal projection)ビューを実装する際に、奇妙な問題が発生しました。
透視投影では、OpenGLのレンダラーと同じマトリックスコードを使用していて、すべてうまくいっていたのですが、直交投影マトリックスを使用すると、オブジェクトがあっけなく消えてしまったのです!
透視投影の行列はうまくいったのに、OpenGLではうまくいっていたのに、なぜ直交投影の行列では問題が起こるのでしょうか？
私は困り果て、シェーダーコード、フレームキャプチャ、行列の乗算などを通してデバッグを行いました。

結局のところ、私はOpenGLの行列コードをやみくもに使っていて、それがうまくいっているように見えたので、Metalの行列計算全体はOpenGLと同じだと思い込んでいたのです。
これは、Metal Programming Guideのp.51の"Workinig with Viewport and Pixel Space Coordinates"にある、1つの小さな詳細を除いて、ほとんどの部分で真実です。

> Metalは、その正規化装置座標(NDC)を、中心を(0, 0, 0.5)とする**2x2x1**立方体として定義しています。  
> NDCのxとyの左と下はそれぞれ-1として指定されます。  
> NDCのxとyの右と上はそれぞれ+1として指定されます。

では、どこに問題があるのでしょうか？

さて、**OpenGLではNDCを(0, 0, 0)を中心とする2x2x2の立方体として定義しています**。

NDCが何だかわからないという方のために簡単に説明すると、各頂点は4種類の変換を経ています:

1. モデル行列: オブジェクト空間からワールド空間への変換。
2. ビュー行列: ワールド空間からカメラ空間への変換。
3. 正射投影行列: カメラ空間からNDC空間への変換。
4. ビューポート: NDC空間は、ビューポートを介して、UIView上の2Dピクセルにマッピングされます。

MetalでOpenGLの行列を使うと、基本的にビューのNDC立方体が半分になり、前半に入ったフラグメントはすべて自動的に破棄されるんです。

透視(Perspective)行列では、Perspective shorteningが働き、あまり問題になりませんが、直交(Orthogonal)行列では、私のオブジェクトはすべてNDCキューブの前半部分に入ってしまい、消えてしまいました。

AppleのMetalBasic3Dのサンプルコードで確認すると、彼らが構築する行列は、実は**OpenGLの正射投影ではない**ことが確認できます!
WWDCのビデオでもそのことには触れていませんし、OpenGLの投影行列を使うという間違いを犯しているWebサイトも多数あります。

では、正しい行列は何なのでしょうか？

## 数学

参考までに、[透視図法と直交行列の導き方について解説したサイト](http://www.songho.ca/opengl/gl_transform.html)を紹介します。

MetalのNDCの場合、同様の方法で行列を導き出すか、あるいは簡単な方法として、OpenGLのNDCをMetalのNDCに変換することができます。

これは、まず2x2x2の立方体を2x2x1にスケールし、正しい中心を持つように0.5だけシフトする後乗算を行うことによって行われます。

$$
M_{pers_{metal}} = M_{adjust} \cdot M_{pers_{OpenGL}} =
\begin{bmatrix}
1 & 0 &   0 &   0 \cr
1 & 1 &   0 &   0 \cr
0 & 0 & 0.5 & 0.5 \cr
0 & 0 &   0 &   1
\end{bmatrix}
\cdot
\begin{bmatrix}
\frac{2n}{r-l} &              0 & \frac{r+l}{r-l}    &                0 \cr
             0 & \frac{2n}{t-b} & \frac{t+b}{t-b}    &                0 \cr
             0 &              0 & \frac{-(f+n)}{f-n} & \frac{-2fn}{f-n} \cr
             0 &              0 &                 -1 &                0
\end{bmatrix}
=
\begin{bmatrix}
\frac{2n}{r-l} &              0 & \frac{r+l}{r-l} &               0 \cr
             0 & \frac{2n}{t-b} & \frac{t+b}{t-b} &               0 \cr
             0 &              0 & \frac{-f}{f-n}  & \frac{-fn}{f-n} \cr
             0 &              0 &              -1 &               0
\end{bmatrix}
$$

$$
M_{ortho_{metal}} = M_{adjust} \cdot M_{ortho_{OpenGL}} =
\begin{bmatrix}
1 & 0 &   0 &   0 \cr
1 & 1 &   0 &   0 \cr
0 & 0 & 0.5 & 0.5 \cr
0 & 0 &   0 &   1
\end{bmatrix}
\cdot
\begin{bmatrix}
\frac{2}{r-l} &             0 &              0 & -\frac{r+l}{r-l} \cr
            0 & \frac{2}{t-b} &              0 & -\frac{t+b}{t-b} \cr
            0 &             0 & \frac{-2}{f-n} & -\frac{f+n}{f-n} \cr
            0 &             0 &              0 &                1
\end{bmatrix}
=
\begin{bmatrix}
\frac{2}{r-l} &             0 &              0 & -\frac{r+l}{r-l} \cr
            0 & \frac{2}{t-b} &              0 & -\frac{t+b}{t-b} \cr
            0 &             0 & \frac{-1}{f-n} &   -\frac{n}{f-n} \cr
            0 &             0 &              0 &                1
\end{bmatrix}
$$
