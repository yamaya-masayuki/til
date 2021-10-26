# Nvim documentation: uiの勝手訳

## UIイベント

UIは、[RPC](https://neovim.io/doc/user/api.html#RPC) [API](https://neovim.io/doc/user/api.html#API)を介してNvimと通信する外部クライアントプロセスとして実装できます。デフォルトのUIモデルは、単一の等幅フォントを使用したターミナルのようなグリッドです。UIは、ウィンドウを別のグリッドに描画したり、いくつかの要素（ウィジェット）をNvimではなくUI自体が表示するように設定することができます（外部化）。

[nvim_ui_attach()](https://neovim.io/doc/user/api.html#nvim_ui_attach())を呼び出して、プログラムがNvimの画面グリッドを描画することをNvimに伝えます。画面のグリッドを、幅×高さのセルサイズで描きたいとNvimに伝えます。

これは通常、エンベッダーが起動時に行なうのが一般的ですが（[ui-startup](https://neovim.io/doc/user/ui.html#ui-startup)を参照）、UIは実行中のNvimインスタンスに接続してNvimインスタンスに接続して`nvim_ui_attach()`を呼び出すこともできます。

optionsパラメータは、以下のキーを持つマップで、以下のキーを持ちます:

キー           |意味
---------------|----
`rgb`          |色形式。`true`:(デフォルト)24ビットRGBカラー。`false`:端末カラー(8ビット、最大256色)
`override`     |UI機能をどのように解決するかを決める。`true`:接続されているすべてのUIでサポートされていない場合でも、要求されたUI機能を有効にします([TUI](https://neovim.io/doc/user/term.html#TUI)を含む)。`false`:(デフォルト)接続されているすべてのUIでサポートされていないUI機能を無効にする([TUI](https://neovim.io/doc/user/term.html#TUI)を含む)。
`ext_cmdline`  |コマンドラインの外部化。[ui-cmdline](https://neovim.io/doc/user/ui.html#ui-cmdline)
`ext_hlstate`  |詳細なハイライト状態[ui-hlstate](https://neovim.io/doc/user/ui.html#ui-hlstate)。暗黙のうちに`ext_linegrid`を設定します。
`ext_linegrid` |行指向グリッド[イベント](https://neovim.io/doc/user/autocmd.html#events)[ui-linegrid](https://neovim.io/doc/user/ui.html#ui-linegrid)。暗黙的に[ui-grid-old](https://neovim.io/doc/user/ui.html#ui-grid-old)を無効にします。
`ext_message`  |メッセージの外部化[ui-messages](https://neovim.io/doc/user/ui.html#ui-messages)。暗黙的に`ext_linegrid`と`ext_cmdline`を設定する。
`ext_multigrid`|ウィンドウ単位のグリッド[イベント](https://neovim.io/doc/user/autocmd.html#events)[ui-multigrid](https://neovim.io/doc/user/ui.html#ui-multigrid)。暗黙的に`ext_linegrid`を設定する。
`ext_popupmenu`|[ポップアップメニュー補完](https://neovim.io/doc/user/insert.html#popupmenu-completion)と[wiledmenu](https://neovim.io/doc/user/options.html#'wildmenu')の外部化[ui-popupmenu](https://neovim.io/doc/user/ui.html#ui-popupmenu)。
`ext_tabline`  |タブ行の外部化[ui-tabline](https://neovim.io/doc/user/ui.html#ui-tabline)。
`ext_termcolors`|外部のデフォルトカラーを使う。

未知のオプションを指定した場合はエラーになる；UIはサポートされているオプションについて、[api-metadata](https://neovim.io/doc/user/api.html#api-metadata)`ui_options`キーをチェックすることができます。

デフォルトでは、Nvimは接続されたすべてのUIが同じ機能をサポートすることを要求します。そのため、アクティブな能力は要求された能力の論理積となります。

UIは[ui-override](https://neovim.io/doc/user/ui.html#ui-override)を指定してこの動作を反転させることができます（デバッグに便利です）。

`option_set`イベントは、どの能力がアクティブであるかを通知します。

Nvimはすべての接続されたUIに`redraw`というメソッド名に、画面「更新イベント」の配列（バッチ）を引数に指定したRPC通知を送信します。

各更新イベントはそれ自体が配列で、最初の要素はイベント名、残りの要素はイベントパラメータのタプルです。したがって、同じ種類の複数のイベントをイベント名を繰り返すことなく連続して送信することができます。

以下に、単一のRPC通知における一般的な`redraw`バッチの例を示します:

```python
['notification', 'redraw',
  [
    ['grid_resize', [2, 77, 36]],
    ['grid_line',
      [2, 0, 0, [[' ' , 0, 77]]],
      [2, 1, 0, [['~', 7], [' ', 7, 76]]],
      [2, 9, 0, [['~', 7], [' ', 7, 76]]],
      ...
      [2, 35, 0, [['~', 7], [' ', 7, 76]]],
      [1, 36, 0, [['[', 9], ['N'], ['o'], [' '], ['N'], ['a'], ['m'], ['e'], [']']]],
      [1, 36, 9, [[' ', 9, 50]]],
      [1, 36, 59, [['0', 9], [','], ['0'], ['-' ], ['1'], [' ', 9, 10], ['A'], ['l', 9, 2]]]
    ],
    ['msg_showmode', [[]]],
    ['win_pos', [2, 1000, 0, 0, 77, 36]],
    ['grid_cursor_goto', [2, 0, 0]],
    ['flush', []]
  ]
]
```

イベントは順番に処理する必要があります。

Nvimは、以下の場合に`flush`イベントを送信します。Nvimは、画面全体の再描画が完了したときに`flush`イベントを送信します（これにより、すべてのウィンドウでバッファの状態やオプションなどの表示が統一されます）。画面全体の再描画が完了する前に、複数の`redraw`バッチを送信されることができ、`flush`は最後のバッチの後にのみ続きます。

ユーザーは、最終的な状態（`flush`が送信されたとき）のみを見るべきで、一部の処理中の中間状態は見てはいけません。バッチ配列の一部を処理している間の中間状態や、`flush`で終わらないバッチのあとではなく、また、`flush "で終わらないバッチの後も同様です。

デフォルトでは、Nvimは[ui-global](https://neovim.io/doc/user/ui.html#ui-global)と[ui-grid-old](https://neovim.io/doc/user/ui.html#ui-grid-old)[イベント](https://neovim.io/doc/user/autocmd.html#events)（後方互換性のため）を送信します；これらはターミナルのようなインターフェイスを実装するのに充分なものです。

しかし、 新しい[ui-linegrid](https://neovim.io/doc/user/ui.html#ui-linegrid)は、テキストをより効率的に表現し（特にハイライトされたテキスト）をより効率的に表現し、複数のグリッドを必要とするUI機能を可能にします。

新しいUIは、[ui-grid-old](https://neovim.io/doc/user/ui.html#ui-grid-old)の代わりに[ui-linegrid](https://neovim.io/doc/user/ui.html#ui-linegrid)を実装する必要があります。

Nvimはオプションで、さまざまな画面要素を、生のグリッドラインではなく、構造化されたイベントとして、[ui-ext-options](https://neovim.io/doc/user/ui.html#ui-ext-options)によって指定されたものとして、「意味的に」送信します。UIはそのような要素を自ら提示しなければならず、Nvimはそれらをグリッド上に描画しません。

Nvimの将来のバージョンでは、新しいアップデートの種類が追加されたり、既存のアップデートの種類に新しいパラメータが追加されたりすることがあります。

クライアントは、将来の互換性のために、そのような拡張を無視する準備をしなければなりません[api-contract](https://neovim.io/doc/user/api.html#api-contract)。

## UIスタートアップ

UIエンベデッダ（`--embed`でNvimを開始した、または、`--handlers`なしでNvimを開始した後`nvim_ui_attach()`をコールしたクライアント）。

Nvimはスタートアップファイルをロードし、バッファを読み込む前に一時停止します、そして、UIはリクエストを発行する機会を持ち、初期化の早いうちに、それをします。UIが`nvim_ui_attach()`を呼び出すとすぐに起動が開始されます。

1. クライアントライブラリのセットアップのためにサポートされているUI拡張のリストが必要な場合は`nvim_get_api_info()`を発行する。
2. ユーザーコンフィグレーションがロードされる前に、任意のコンフィグレーションを実施すべき。この時点でバッファとウィンドウは無効、しかし、`init.vim`で参照できる`g:`変数を使用することができる。
3. もしUIがユーザーコンフィグレーションがロードされる前に追加のセットアップが必要なら、`VimEnter`のautocmd `nvim_command("autocmd VimEnter * call rpcrequest(1, 'vimenter')")` を登録します。
4. `nvim_ui_attach()`を発行します。UIは`init.vim`をソースし、バッファをロードし、プロンプトをブロックして、ユーザー入力を処理しなければならない。
5. `3.`を使った場合、NvimはブロッキングしていたリクエストをUIに送信するでしょう。このリクエストハンドラの内部では、UIは安全にノーマルモードに入る前に任意の初期化を安全に実行できる。例えば、`init.vim`でセットした変数を読むために。

## グローバルイベント

以下のUIイベントは常に発行され、そして、エディタのグルーバル状態を描写する。

- `["set_title", title]` `["set_icon", icon]`

  ウィンドウタイトルとアイコン（最小化）ウィンドウタイトルをそれぞれ設定します。この2つを区別しないウィンドウシステムでは、`set_icon`は無視できます。

- `["mode_info_set", cursor_style_enabled, mode_info]`

  `cursor_style_enabled`は、UIがカーソルスタイルを設定するかどうかを示すブール値である。
  `mode_info`は、モードのプロパティマップのリストです。
  現在のモードは、`mode_change`イベントの`mode_idx`フィールドによって与えられます。

  各モードのプロパティマップには、これらのキーを含めることができます：

  キー | 説明
  -----|------------
  `cursor_shape`   |`block`, `horizontal`, `vertical`
  `cell_percentage`| カーソルのセルの占有率
  `blinkwait`, `blinkon`, `blinkoff` | [cursor-blinking](https://neovim.io/doc/user/options.html#cursor-blinking)
  `attr_id` | カーソル属性ID(`hl_attr_define`で定義される)。attr_idが0になった場合、前景色と背景色を入れ替えるべき。
  `attr_id_lm` | 'langmap'がアクティブになったときのカーソル属性ID
  `short_name` | モードのコード名。'guicursor'を参照。
  `name`|モードの記述名。
  `mouse_shape`|(実装すること)

  幾つかのモードで幾つかのキーが失われる。

  以下のキーは非推奨：

    - `hl_id`: 代わりに`attr_id`を使用する。
    - `hl_lm`: 代わりに`attr_id_lm`を使用する。

- `["option_set", name, value]`

  UIに関するオプションが変更した場合、以下のいずれかの`name`で発行される：

    - 'arabicshape'
    - 'ambiwidth'
    - 'emoji'
    - 'guifont'
    - 'guifontwide'
    - 'linespace'
    - 'mousefocus'
    - 'pumblend'
    - 'showtabline'
    - 'termguicolors'
    - "ext_*" ([ui-ext-options](https://neovim.io/doc/user/ui.html#ui-ext-options))

  UIが最初にNvimに接続したときと、ユーザーやプラグインがオプションを変更したときに通知します。ユーザーやプラグインによって変更されるたびに通知されます。

  オプションの効果が他のUIイベントで伝達される場合は、ここでは表現されません。例えば、`mouse`オプションの値を伝達する代わりに、`mouse_on`および`mouse_off`UIイベントがマウスサポートがアクティブかどうかを直接示します。`ambiwidth`のようないくつかのオプションは、グリッド上ではすでに効果があり、適切な空のセルが追加されますが、UIは[ui-cmdline](https://neovim.io/doc/user/ui.html#ui-cmdline)のように、Nvimから送られた生のテキストをレンダリングする際に、それらのオプションを使用することがあります。

- `["mode_change", mode, mode_idx]`

  エディタのモードが変更されたことを発信する。パラメータ`mode`は、現在のモードを表す文字列です。現在のモードを表す文字列です。

  `mode_idx`は `mode_info_set` イベントで出力される配列のインデックスです。

  UIは、対応するアイテムで指定されたプロパティに従って、カーソルスタイルを変更する必要があります。

  通知されるモードのセットは、Nvimの新しいバージョンで変更される予定です。例えば、より多くのサブモードや一時的な状態が、別のモードとして表現されるかもしれません。

- `["mouse_on"]` `["mouse_off"]`

  現在のエディタモードで 'mouse' を有効/無効にした場合に通知します。

  ターミナルUI や、Nvimのマウスが他のマウスの使い方と衝突するようなアプリケーションへの組み込みに便利です。他のUIはこのイベントを無視することができます。

- `["busy_start"]` `["busy_stop"]`

  UIにカーソルのレンダリングを停止する必要があることを示します。

  このイベントは名前が間違っていて、実際には忙しさとは何の関係もありません。

- `["suspend"]`

   [:suspend](https://neovim.io/doc/user/starting.html#:suspend)コマンド、または、 [CTRL-Zマッピング](https://neovim.io/doc/user/map.html#mapping)で使用される。

   端末クライアント（または、意味のある他のクライアント）は、自分自身をサスペンドすることができます。他のクライアントは安全にそれを無視することができます。

- `["update_menu"]`

  メニューマッピングが変化した時。

- `["bell"]` `["visual_bell"]`

  ベルとビジュアル・ベルをそれぞれ通知します。

- `["flush"]`

  Nvimが画面の再描画を終えた時に通知します。

  内部バッファにレンダリングする実装では、この時に再描画した部分をユーザーに表示することになります。

## グリッドイベント (行指向)

`ext_linegrid` [ui-option](https://neovim.io/doc/user/ui.html#ui-option) で有効にされます。すべての新しいUIはこれを使用することを推奨します。[ui-grid-old](https://neovim.io/doc/user/ui.html#ui-grid-old)を暗黙的に無効にします。

[ui-grid-old](https://neovim.io/doc/user/ui.html#ui-grid-old)と比べ最も大きな変更点は、スクリーン行のコンテンツを更新するために単一の`grid_line`イベントを使用することです(古いプロトコルでは、カーソル、ハイライト、テキストの各イベントを組み合わせて使用していました)。

これらのイベントのほとんどは、最初のパラメータとして `grid` インデックスを受け取ります。グリッド1は、エディタの画面全体の状態にデフォルトで使用されるグローバルグリッドです。ウィンドウ単位のグリッドを有効にするには、 [ui-multigrid](https://neovim.io/doc/user/ui.html#ui-multigrid)を有効にしてください。

ハイライト属性グループはあらかじめ定義されています。UIは数字のハイライトIDを実際の属性にマッピングするテーブルを維持する必要があります。

- `["grid_resize", grid, width, height]`

  グリッドをリサイズします。もし `grid` が以前にクライアントに表示されていなければ、このサイズで新しいグリッドが作成されます。

- `["default_colors_set", rgb_fg, rgb_bg, rgb_sp, cterm_fg, cterm_bg]`

  最初の3つの引数は、それぞれデフォルトの前景色、背景色、特殊色を設定する。`cterm_fg`と`cterm_bg`は、256色の端末で使用するデフォルトのカラーコードを指定します。

  RGB値は、デフォルトでは常に有効な色になります。色が設定されていない場合は、[background](https://neovim.io/doc/user/options.html#'background')に応じて、黒と白がデフォルトになります。`ext_termcolors`オプションを設定すると、未設定の色には-1が使われます。これは、[TUI](https://neovim.io/doc/user/term.html#TUI)の実装で、[端末](https://neovim.io/doc/user/nvim_terminal_emulator.html#terminal)組み込み（"ANSI"）のデフォルトを使用することが期待される場合に役立ちます。

  注意: [ui-grid-old](https://neovim.io/doc/user/ui.html#ui-grid-old)イベントと異なり、このイベントを送信した後に画面がクリアされるとは限りません。UIは、変更された背景色で画面を再描画する必要があります。

- `["hl_attr_define", id, rgb_attr, cterm_attr, info]`

  次の（すべてオプションの）キーを使用して、 `rgb_attr`および`cterm_attr`辞書で指定された属性を使用して、`id`のハイライトをハイライトテーブルに追加します。

  キー|意味
  -|-
  `foreground`|前景色。
  `background`|背景色。
  `special`|[下線](https://neovim.io/doc/user/syntax.html#underline)と[波下線](https://neovim.io/doc/user/syntax.html#undercurl)で使用される。
  `reverse`|ビデオ反転。前景色と背景色を入れ替える。
  `italic`|イタリック・テキスト。
  `bold`|ボールド・テキスト。
  `strikethrough`|取り消し線。
  `underline`|下線付きテキスト。下線の色は`special`を用いる。
  `undercurl`|波下線付きテキスト。波線の色は`special`を用いる。
  `blend`|ブレンドレベル(0-100)。UIではフローティングウィンドウの背景のブレンドや、透明なカーソルのシグナルをサポートするために使用できます。
  
  上記のキーがない場合は、デフォルトカラーを使用します。デフォルト値をテーブルに格納するのではなく、センチネル値を格納することで、変更されたデフォルトカラーが有効になるようにします。すべての真偽値キーのデフォルトはfalseであり、trueのときにのみ送信されます。
  
  ハイライトは、RGB形式と256色のターミナルコードの両方で、それぞれ`rgb_attr`と`cterm_attr`パラメータとして常に送信されます。[ui-rgb](https://neovim.io/doc/user/ui.html#ui-rgb)オプションは、もう何の効果もありません。ほとんどの外部UIは、`rgb_attr`属性を保存して使用するだけでよいでしょう。
  
  `id` 0 は、`default_colors_set`で定義された色で、スタイルが適用されていないデフォルトのハイライトとして常に使用されます。
  
  注意: Nvim は、内部のハイライトテーブルがいっぱいになると、`id`の値を再利用することがあります。この場合、Nvimは再定義されたIDの影響を受けるスクリーンセルの再描画を常に実行するので、UI がこれを追跡する必要はありません。
  
  `info`はデフォルトでは空の配列で、以下で説明する[ui-hlstate](https://neovim.io/doc/user/ui.html#ui-hlstate)拡張機能で使用されます。

- `["hl_group_set", name, hl_id]`

  ビルトインのハイライトグループ`name`が、以前の`hl_attr_define`呼び出しで定義された属性`hl_id`を使用するように設定されました。

  このイベントは、属性IDを直接使用するグリッドをレンダリングするためには必要ありませんが、一貫したハイライトで独自の要素をレンダリングしたいUIにとっては便利です。

  例えば[ui-popupmenu](https://neovim.io/doc/user/ui.html#ui-popupmenu)イベントを使用するUIは、内蔵のハイライトの[hl-Pmenu](https://neovim.io/doc/user/syntax.html#hl-Pmenu)ファミリーを使用するかもしれません。

- `["grid_line", grid, row, col_start, cells]`

  `col_start`列から始まる `row`の連続した部分を`grid`上に再描画します。
  
  `cells`は、1から3のアイテムを持つ配列の配列です: `[text(, hl_id, repeat)]`。
  
  `text` は、セルに入れるべきUTF-8テキストで、以前の`hl_attr_define`コールで定義されたハイライト`hl_id`を含みます。
  
  もし、`hl_id` が存在しない場合は、同じコールの中で最も最近に見られた`hl_id`を使用します(これは常にイベントの最初のセルに対して送信されます)。
  
  もし、`repeat` があれば、そのセルは`repeat`回(最初の回数を含む)繰り返されるべきで、そうでなければ1回だけです。
  
  全角文字の右セルは空文字列として表現されます。全角の文字は`repeat`を使用しません。
  
  セル変更の配列が行末まで届かない場合は、残りの部分は変更しないでください。行の残りが消去されるべきときには、その部分を充分に覆うように空白文字が繰り返し送信されます。

- `["grid_clear", grid]`

  グリッドをクリアする。

- `["grid_destroy", grid]`

  `grid`はもう誰にも使用されず、UIは関連するデータを解放することができる。

- `["grid_cursor_goto", grid, row, column]`

  `grid`をカレントグリッドにして、カーソルを`row, column`に配置します。

  このイベントは`redraw`バッチの中で最大1回だけ送信され、可視カーソル位置を示します。

- `["grid_scroll", grid, top, bot, left, right, rows, cols]`

  グリッドの区域をスクロールする。

  これは意味的にはエディタの[scrolling](https://neovim.io/doc/user/scroll.html#scrolling)とは無関係で、むしろこれは、「これらのスクリーンセルをコピーする」という最適化された方法です。

  以下の図は、スクロール方向毎に何が起きるかを示します。

  The following diagrams show what happens per scroll direction.
  `===` はSR(区域のスクロール)の範囲を示します。
  `---` は移動した矩形を指示します。
  なお、dstとsrcは共通の領域を持っています。

  もし`rows`が0より大きい場合、SR下部で矩形を移動します。これは下[スクロール](https://neovim.io/doc/user/scroll.html#scrolling)を発生させます。

  ```text
  +-------------------------+
  | (クリップされたSR上部)  |            ^
  |=========================| dst_top    |
  | dst (SR範囲内)          |            |
  +-------------------------+ src_top    |
  | src (上に移動した)とdst |            |
  |-------------------------| dst_bot    |
  | src (不正)              |            |
  +=========================+ src_bot
  ```

  もし`rows`が0未満なら、SR上部で矩形を移動させます。これは上スクロールを発生させます。

  ```text
  +=========================+ src_top
  | src (不正)              |            |
  |------------------------ | dst_top    |
  | src (下に移動した) とdst|            |
  +-------------------------+ src_bot    |
  | dst (SR範囲内)          |            |
  |=========================| dst_bot    |
  | (クリップされたSR下部)  |            v
  +-------------------------+
  ```

  Nvimの現バージョンに置いては`cols`は常に0で、将来的な利用のために予約されています。

  [ui-grid-old](https://neovim.io/doc/user/ui.html#ui-grid-old)イベントからアップデートする際の注意:
  範囲は終了を含まずAPIの慣習と一致していますが、終了を含んでいた`set_scroll_region`とは異なります。

  スクロールされた領域は、スクロールイベントの直後に[ui-event-grid_line](https://neovim.io/doc/user/ui.html#ui-event-grid_line)を使って埋められます。そのため、UIはスクロールイベントを処理する際にこの領域をクリアする必要はありません。

## グリッドイベント (セル指向)

これはスクリーングリッドのレガシー表現で、[ui-linegrid](https://neovim.io/doc/user/ui.html#ui-linegrid)が非アクティブの場合に出力されます。新しいUIでは、代わりに[ui-linegrid](https://neovim.io/doc/user/ui.html#ui-linegrid)を実装する必要があります。

<省略>

## ハイライト状態拡張の詳細

`ext_hlstate` [UIオプション](https://neovim.io/doc/user/ui.html#ui-option)で有効にされます。暗黙的に`ui-linegrid`を有効にします。

デフォルトでは、Nvimは[ui-event-highlight_set](https://neovim.io/doc/user/ui.html#ui-event-highlight_set)の辞書キーで記述された、最終的に計算されたハイライト属性だけを用いてグリッドセルを記述します。`ext_hlstate`という拡張機能を使うと、セルで有効なハイライトの意味的な説明をUIで受け取ることができます。このモードでは、[ui-event-hl_attr_define](https://neovim.io/doc/user/ui.html#ui-event-hl_attr_define)や[ui-event-grid_line](https://neovim.io/doc/user/ui.html#ui-event-grid_line)のように、ハイライトが1つのテーブルで事前に定義されます。

`hl_attr_define`の`info`パラメータには、ハイライトの意味的な説明が入ります。ハイライトグループは組み合わせることができるので、これはアイテムの配列となり、最も優先度の高いアイテムが最後になります。各項目は、以下のようなキーを持つ辞書となります:

- `kind`

  常に存在し、以下の値のいずれか。

    - `ui`

      内蔵UIハイライト。[highlight-group](https://neovim.io/doc/user/syntax.html#highlight-groups)

    - `syntax`

      ハイライトはシンタックス宣言、もしくは、[nvim_buf_add_highlight()](https://neovim.io/doc/user/api.html#nvim_buf_add_highlight())のような、他のランライム/プラグイン機能によって、バッファに適用される。

    - `terminal`

      [端末エミュレータ]()で稼働するプロセスからのハイライト。意味的情報を含まない。

- `ui_name`

  [highlight-group]()からのハイライト名。`kind`が`ui`の場合のみ。

- `hi_name`

  使用される属性が定義されている最終的な[:highlight]()グループの名前。

- `id`

  この項目に採番される一意の数値。

注意: `ui`アイテムには`ui_name`と`hi_name`の両方が現れます。

これらは、組み込みグループが別のグループにリンク[:hi-link](https://neovim.io/doc/user/syntax.html#:hi-link)されていたり、'winhighlight' が使用されていたりするため、異なる可能性があります。UIアイテムは、ハイライトグループがクリアされても送信されるので、属性が適用されていなくても、`ui_name`を使って常に確実に画面要素を識別することができます。

## 複数グリッドイベント

`ext_multigrid` [UIオプション](https://neovim.io/doc/user/ui.html#ui-option)で有効にされます。暗黙的に`ui-linegrid`を有効にします。

グリッドイベントについては[ui-linegrid](https://neovim.io/doc/user/ui.html#ui-linegrid)を参照してください。

グリッドサイズの変更リクエストについては[nvim_ui_try_resize_grid()](https://neovim.io/doc/user/api.html#nvim_ui_try_resize_grid())を参照してください。

Nvimへマウスイベントを送信するには[nvim_input_mouse()](https://neovim.io/doc/user/api.html#nvim_input_mouse())を参照してください。

複数グリッド拡張は、どのようにウィンドウを表示するかを超えて、多くの制御をUIに与えます:

- UIはウィンドウ毎に分割した一つのグリッドで更新を受信します。
- UIはグローバルレイアウト上でウィンドウが占めるスペースの大きさとは無関係に、グリッドサイズを設定することができます。もしくは、UIツールキットのスクロールバーなどの独自の要素のために、ウィンドウの境界部分にスペースを確保することもできます。
- メッセージには専用のグリッドが使用され、ウィンドウ領域を超えてスクロールすることがあります([ext_messages](https://neovim.io/doc/user/intro.html#ext_messages)を使用することもできます)。

デフォルトでは、グリッドサイズはNvimによって処理され、分割が作成されるたびに外側のグリッドサイズ（つまり、Nvimのウィンドウフレームのサイズ）に設定されます。UIがグリッドサイズを設定すると、Nvimはそのグリッドのサイズを処理せず、UIは外側のサイズが変更されるたびにグリッドサイズを変更する必要があります。グリッドサイズの処理をNvimに委譲するには、サイズ(0, 0)を要求します。

ウィンドウは、グリッドの割り当てを解除せずに非表示にして再表示できます。これは、たとえば、タブを切り替える時に、同じウィンドウで複数回発生する可能性があります

- `["win_pos", grid, win, start_row, start_col, width, height]`

  グリッドの位置とサイズをNvimで設定します（つまり、外側のグリッドサイズ）。以前にウィンドウが非表示になっていた場合は、再び表示されるはずです。

- `["win_float_pos", grid, win, anchor, anchor_grid, anchor_row, anchor_col, focusable]`

  フローティングウィンドウ `win` を表示または再構成します。

  ウィンドウは、別のグリッド `anchor_grid` の上に、指定した位置 `anchor_row` と `anchor_col` に表示されます。

  `anchor`の意味や位置関係の詳細については、[nvim_open_win()](https://neovim.io/doc/user/api.html#nvim_open_win())を参照してください。

- `["win_external_pos", grid, win]`

  外部ウィンドウ`win`を表示または再構成します。このウィンドウは、デスクトップ環境の別のトップレベルのウィンドウとして表示されるか、またはそれに類似したものでなければなりません。

- `["win_hide", grid]`

  ウィンドウの表示を停止します。ウィンドウは後で再び表示することができます。

- `["win_close", grid]`

  ウィンドウを閉じます。

- `["msg_set_pos", grid, row, scrolled, sep_char]`

  メッセージを`grid`に表示します。グリッドは、デフォルトのグリッド(grid=1)の`row`に、カラム幅いっぱいに表示するでしょう。

  `scrolled`はメッセージエリアが他のグリッドを覆うようにスクロールされたかどうかを示します。

  セパレータを表示すると便利です ('display' msgsep flag)。

  内蔵TUIでは`sep_char`を`hl-MsgSeparator`ハイライトで埋め尽くされたフルラインを描画するでしょう。

  `ext_messages`がアクティブな場合、メッセージグリッドは使用されず、このイベントは送信されません。

## 補足

`ext_linegrid`を使用しているUI実装。

- [equalsraf/neovim-qt](https://github.com/equalsraf/neovim-qt)

  `shell.cpp`を参照。

- [KillTheMule/nvim-rs](https://github.com/KillTheMule/nvim-rs)

  rust製UIプロトコルライブラリ。

- [sakhnik/nvim-gdb](https://github.com/sakhnik/nvim-gdb)

- [glacambre/firenvim](https://github.com/glacambre/firenvim)

- [smolck/uivonim](https://github.com/smolck/uivonim)

  javascript

- [jeanguyomarch/eovim](https://github.com/jeanguyomarch/eovim)

  [EFL](https://www.enlightenment.org/about-efl)を使用したCで書かれたGUIクライアント。
