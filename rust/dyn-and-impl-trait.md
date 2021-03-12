# dynトレイトと implトレイト

[dyn Trait and impl Trait in Rust](https://www.ncameron.org/blog/dyn-trait-and-impl-trait-in-rust/)

## `impl`トレイト

関数引数や戻り値型で使用可能なジェネリクスの短縮文法。

以下のジェネリクスな関数宣言があるとする。

```rust
fn f<B: Bar>(b: B) -> usize
```

これを implトレイトを使用すると以下のように書ける:

```rust
fn f(b: impl Bar) -> usize
```

注意点としては、複数引数で以下のような宣言の場合:

```rust
fn f(b1: impl Bar, b2: impl Bar) -> usize
```

と同等な宣言は:

```rust
fn f<B1: Bar, B2: Bar>(b1: B1, b2: B2) -> usize
```

であって、次の宣言ではない。

```rust
fn f<B: Bar>(b1: B, b2: B) -> usize
```

## `dyn`トレイト

トレイトオブジェクはC++における仮想オブジェクトのようなもの。
トレイトオブジェクトは常にポインタ(ボロウ参照、`Box`または他のスマートポインタ)により参照される。

`dyn`トレイトを用いたトレイトオブジェクトの型としては、`&dyn Bar`, `Box<dyn Bar>`などがある。
`impl Trait`と異なり、wrapping pointer無しの `dyn Trait` は使用できない。
