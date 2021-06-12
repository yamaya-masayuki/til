# Discover geometry-aware audio with the Physical Audio Spatialization Engine (PHASE)

<https://developer.apple.com/wwdc21/10079>

PHASEは新しいオーディオフレームワーク。

## 特徴

- オーディオエンジンにジオメトリ情報を提供し
- サウンドデザインに適したイベント駆動型のオーディオ再生システムを構築し、
- サポートされているすべてのデバイスで一貫した空間的なオーディオ体験を自動的に提供できるアプリケーションを書くことを可能にし
- 既存のオーサリングソリューションやパイプラインとの統合を可能にする

## 一般的なゲームオーディオのワークフロー

ここでは、リスナー、音源である小川の流れ、オクルーダーである納屋がある屋外シーンの例を紹介します。

オクルーダーとは、音源とリスナーの間の音を減衰させる可能性のあるシーン内のオブジェクトです。一般的には、エリアに沿って複数の点音源を配置します。リスナーの動きに合わせて、レイトレーシングなどの様々な技術を使って、点音源間の適切なフィルタリングとミックス比を決定し、良いオーディオ体験を提供するために手動でそれらの間をブレンドしなければなりません。

ゲーム開発の過程で、例えば例示したシーンの納屋のように、視覚的なシーンが変化した場合には、それに合わせてオーディオ体験を更新し、手作業で調整する必要があります。

## PHASEでは

オーディオソースが、シーンに応じて管理・ミックスするポイントではなく、オーディオシステムが自動的に管理するエリアやボリュームから発せられる音としてアプリケーションを構築できることを想像してみてください。PHASEはまさにそれを実現します。

### ボリューメトリック (Volumetric ~ 容積測定) ソースの導入

新しいフレームワークでは、音源を幾何学的な形状としてオーディオエンジンに渡すことができるAPIを提供しています。

ボリューメトリックな音源に加え、シーン内のオクルーダーも幾何学的な形状として渡すことができます。

また、オーディオ材料のプロパティをプリセットから選択して、オクルーダーに割り当てることもできます。

また、PHASEでは、用途に応じて点音源の媒質伝搬や音源指向性を設定することができます。

ここでは屋外のシーンを例に挙げていますが、屋内のシーンであれば、初期反射や後期残響の特性をプリセットのライブラリから選ぶことができます。

様々な音源、オクルーダー、リスナーの位置をフレームワークに伝えると、PHASEがシーン内の様々な音源のオクルージョンや拡散効果をモデル化してくれます。アプリケーションのオーディオシステムがジオメトリを認識するようになったことで、ゲーム開発の進展に伴うビジュアルシーンの変化に、より迅速に対応できるようになります。

### イベント駆動型の再生システム

サウンドイベントは、PHASEのオーディオ再生イベントを記述するための基本単位です。オーディオアセットの選択、ブレンド、および再生をカプセル化します。サウンドイベントには、ワンショット再生やループ再生のような単純なものから、再生イベントのサブツリーをブレンドしたり切り替えたりできるツリーとして構成された複雑なシーケンスまであります。

### レンダリングシステム

PHASEでは、シンプルなチャンネル構成でサウンドを再生することも、方向性と位置を持つ3D空間でサウンドを再生することも、方向性は持つが位置は持たないアンビエントベッド(ambient beds)としてサウンドを再生することもできます。

このエンジンは、すでにサポートされているiOSデバイスやmacOSデバイス、そしてAir Podsシリーズのヘッドフォンに搭載されている空間オーディオレンダリング機能をベースにしています。

これにより、サポートされているすべてのデバイスで一貫した空間オーディオ体験を提供するアプリケーションを自動的に構築することができます。

## ユースケースとAPI

### 一般的なコンセプト

PHASE APIは、3つの主要なコンセプトに分けることができます。

- エンジンは、アセットを管理します。
- ノードは、再生をコントロールします。
- ミキサーは、空間化を制御します。

PHASEエンジンは、3つの主要セクションに分けられます。

- アセットレジストリ

  エンジンのライフサイクルを通じて、アセットの登録と解除を行います。
  現在のところPHASEはサウンドアセットとサウンドイベントアセットの登録をサポートしています。

  サウンドアセットは、オーディオファイルから直接ロードすることも、生のオーディオデータとしてパックして独自のアセットに埋め込み、エンジンに直接ロードすることもできます。

  サウンドイベントアセットは、サウンドの再生を制御する1つまたは複数の階層ノードと、空間化を制御するダウンストリームミキサーの集合体です。

- シーングラフ

  シーングラフとは、シミュレーションに参加するオブジェクトの階層のことです。これにはリスナー、ソース、オクルーダーが含まれます。

  リスナーとは、シミュレーションを聞く空間上の位置を表すオブジェクトです。ソースとは、音が発生する場所を表すオブジェクトです。PHASEは点音源と体積音源の両方をサポートしています。

  オクルーダー(Occluder)とは、環境内を移動する音の伝達に影響を与えるシミュレーション内の形状を表すオブジェクトのことです。オクルーダーには、音の吸収や伝達に影響を与えるマテリアルが割り当てられます。PHASEには、オクルーダーに割り当てられるマテリアルのプリセットが用意されており、ダンボール箱、ガラス窓、レンガの壁など、あらゆるものをシミュレートできます。

  シーンにオブジェクトを追加する際には、オブジェクトを階層化して、エンジンのルートオブジェクトに直接または間接的にアタッチします。そうすることで、オブジェクトがフレームごとにシミュレーションに参加することができます。

- レンダリングステート

  レンダリングステートは、サウンドイベントやオーディオI/Oの再生を管理します。

  エンジンを最初に作成したときは、オーディオI/Oは無効になっています。これにより、アセットの登録、シーングラフの構築、サウンドイベントの構築、その他のエンジン操作を、オーディオI/Oを実行することなく行うことができます。サウンドイベントを再生する準備ができたら、エンジンを起動すれば、内部でオーディオI/Oが開始されます。

  同様に、サウンドイベントの再生が終了したら、エンジンを停止することができます。そうすると、オーディオI/Oが停止し、再生中のサウンドイベントも停止します。

  PHASEのノードは、オーディオコンテンツの再生を制御します。ノードは、オーディオ再生を生成または制御するオブジェクトの階層的な集合体です。ジェネレーター・ノードは、オーディオを生成します。ノードは、ノード階層の中で常にリーフノードです。コントロール・ノードは、空間化の前にジェネレーターを選択、ミックス、パラメータ化する方法のロジックを設定します。コントロール・ノードは常に親ノードであり、複雑なサウンド・デザイン・シナリオのために階層化することができます。サンプラー・ノードは、ジェネレーターノードの一種です。サンプラーは、登録されたサウンドアセットを再生します。サンプラー・ノードを構築したら、いくつかの基本的なプロパティを設定して、正しく再生させることができます。

  再生モードは、オーディオファイルをどのように再生するかを決定します。再生モードをワンショットに設定すると、オーディオファイルは一度再生された後、自動的に停止します。この機能は、サウンドエフェクトのトリガーなど、「撮って終わり」のシナリオに使用できます。再生モードをループ再生に設定すると、サンプラーを明示的に停止するまで、オーディオファイルは無限に再生されます。

  cullオプションは、サウンドが聞き取れなくなったときの処理をPHASEに指示します。cullオプションを`terminate`に設定すると、音が聞こえなくなったときに自動的に停止します。cullオプションを`sleep`に設定すると、音が聞こえなくなった時点でレンダリングを停止し、音が聞こえるようになった時点で再びレンダリングを開始します。これにより、エンジンによってカリングされたサウンドを手動で開始したり停止したりする必要がありません。

  キャリブレーション・レベルは、サウンドの実世界でのレベルをデシベルSPLで設定します。

  また、PHASEは4種類のコントロール・ノードをサポートしています。ランダム・ノード、スイッチ・ノード、ブレンド・ノード、コンテナ・ノードです。

  ランダム・ノードは、重み付けされたランダムな選択に従って、子の1つを選択します。例えば、このケースでは、次にサウンドイベントがトリガーされたときに、左のサンプラーが右のサンプラーよりも4：1の確率で選択されます。

  スイッチ・ノードは、パラメータ名に基づいて子プロセスを切り替えます。例えば、地形のスイッチを「きしむ木」から「柔らかい砂利」に変更することができます。次にサウンドイベントがトリガーされたときに、パラメータ名に一致するサンプラーが選択されます。

  ブレンド・ノードは、パラメータの値に基づいて子ノード間をブレンドします。例えば、blendノードにWetnessパラメータを割り当てると、ドライエンドでは大きな足音と静かな水しぶき、ウェットエンドでは静かな足音と大きな水しぶきの間でブレンドすることができます。

  コンテナ・ノードは、すべての子音を一度に再生します。例えば、足音を再生するサンプラーと、衣服の音（ゴアテックス製のジャケットのフリルの音など）を再生するサンプラーを用意することができます。コンテナがトリガーされるたびに、両方のサンプラーが同時に再生されます。

## ミキサー

PHASEのミキサーは、オーディオコンテンツの空間化を制御します。PHASEは現在、チャンネルミキサー、アンビエントミキサー、空間ミキサーをサポートしています。

チャンネルミキサーは、空間化や環境効果なしにオーディオをレンダリングします。ステレオ音楽やセンターチャンネルのナレーションなど、出力デバイスに直接レンダリングする必要のある通常のステムベースのコンテンツには、チャンネルミキサーを使用します。

アンビエントミキサーは、オーディオを外部化してレンダリングしますが、距離モデルや環境エフェクトはありません。リスナーが頭を回転させても、サウンドは空間内の同じ相対的な位置から聞こえてきます。例えば、広大な森の中でコオロギの鳴き声を背景にするなど、環境をシミュレートしていないにもかかわらず、どこか宇宙から聞こえてくるようなマルチチャンネルコンテンツには、アンビエントミキサーを使用します。

空間ミキサーは、完全な空間化を行います。音源がリスナーに対して相対的に移動すると、パンニング、距離モデル、指向性モデルのアルゴリズムに基づいて、知覚される位置、レベル、周波数特性が変化して聞こえます。これに加えて、ジオメトリを考慮した環境効果が音源とリスナーの間の経路に適用されます。ヘッドフォンを装着している場合は、バイノーラルフィルターの適用による外在化も得られます。完全な環境シミュレーションに参加する必要があるサウンドには、空間ミキサーを使用します。空間ミキサーは、2つのユニークな距離モデリングアルゴリズムをサポートしています。標準的な幾何学的拡散損失を設定して、距離による自然な減衰を得ることができます。また、お好みで効果を増減させることもできます。例えば、値を下げることで、離れた場所での会話をブームマイクで録音したい場合に有効です。

もう一方では、距離による減衰を完全なピースワイズカーブで表現することもできます。例えば、最初と最後は自然な距離感で減衰させ、中間部では減衰量を減らして、重要な台詞をより遠くまで聞こえるようにしたセグメントのセットを構築することができます。

ポイントソースの場合、空間ミキサーは2種類の指向性モデリングアルゴリズムに対応しています。カーディオイドの指向性モデリングをスペースミックスに追加することができます。簡単な変更を加えるだけで、人間のスピーカーをカーディオイド指向性パターンでモデリングしたり、アコースティックな弦楽器の音をハイパーカーディオイド指向性パターンでモデリングすることができます。また、コーンの指向性モデリングを追加することもできます。このクラシックなモードでは、指向性フィルタリングを特定の回転範囲内に制限することができます。空間ミキサーは、空間パイプラインに基づいたジオメトリ対応の環境エフェクトにも対応しています。空間パイプラインは、有効または無効にする環境エフェクトと、それぞれへの送信レベルを選択します。

PHASEは現在、ダイレクトパス・トランスミッション、アーリー・リフレクション、レイト・リバーブをサポートしています。

ダイレクトパス・トランスミッションは、ソースとリスナーの間のダイレクトパスとオクルードパスをレンダリングします。遮蔽された音では、一部のエネルギーは物質に吸収され、他のエネルギーは物体の反対側に伝達されることに注意してください。初期反射は、直接音の経路に強度の修正と色付けを行います。これらは通常、壁や床からの鏡面反射で作られます。また、広い空間では、顕著な反響が得られます。

レイト・リバーブは、環境の音を提供します。これは、拡散したエネルギーが高密度に蓄積され、最終的に空間の音を表現するためのものです。部屋の大きさや形を示す手がかりとなるだけでなく、包み込まれるような感覚を与える役割も担っています。

## ユースケース例

このセクションでは、

- オーディオファイルの再生
- 空間的なオーディオ体験の構築
- ビヘイビアサウンドイベントの構築

について説明します。

まず初めに、オーディオファイルを再生する方法を紹介します。

```swift
// 自動更新モードでエンジンを生成する
let engine = PHASEEngine(updateMode: .automatic)

// アプリケーションバンドル内のオーディオファイルへのURLを得る
let audioFileUrl = Bundle.main.url(forResource: "DrumLoop_24_48_Mono", withExtension: "wav")!

// オーディオファイルを登録する
// 名前は `drums` とし、メモリにロードして、再生時に動的ノーマライズを適用する
let soundAsset = try engine.assetRegistry.registerSoundAsset(url: audioFileUrl,
                                                             identifier: "drums",
                                                             assetType: .resident,
                                                             channelLayout: nil,
                                                             normalizationMode: .dynamic)
```

ここでは、サウンドイベントアセットを登録するコードを紹介します。

```swift
// Monoレイアウトタグからチャンネルレイアウトを生成する
let channelLayout = AVAudioChannelLayout(layoutTag: kAudioChannelLayoutTag_Mono)!
     
// チャンネルネイレウアウトからチャンネルミキサーを生成する
let channelMixerDefinition = PHASEChannelMixerDefinition(channelLayout: channelLayout)

// `drums`からサンプラーノードを生成して、ダウンストリームなチャンネルミキサーをフックする
let samplerNodeDefinition = PHASESamplerNodeDefinition(soundAssetIdentifier: "drums",
                                                       mixerDefinition: channelMixerDefinition)

// サンプラーノードの再生モードをループにする
samplerNodeDefinition.playbackMode = .looping;

// サンプラーノードのキャリブレーションモードをSPL相対にして、レベルを0dBにする
samplerNodeDefinition.setCalibrationMode(.relativeSpl, level: 0)

// エンジン名 `drumEvent` でサウンドイベントアセットを登録する
let soundEventAsset = try engine.assetRegistry.registerSoundEventAsset(rootNode:samplerNodeDefinition,
                                             identifier: "drumEvent")
```

これにより、快適なリスニングレベルが確保されます。

最後に、soundEventAssetをエンジンに登録し、後で再生用のサウンドイベントを作成する際に参照できるよう、drumEventという名前を渡します。

```swift
// サウンドイベントアセット `drumEvent` からサウンドイベントを生成する
let soundEvent = try PHASESoundEvent(engine: engine, assetIdentifier: "drumEvent")

// エンジンを開始する。これは内部的にオーディオI/Oスレッドを開始する
try engine.start()

// サウンドイベントを開始する
try soundEvent.start()
```

サウンドイベントの再生が終わったら、エンジンのクリーンアップを行います。

```swift
// サウンドイベントを無効化し停止します
soundEvent.stopAndInvalidate()

// エンジンを停止します。内部的にはオーディオI/Oスレッドを止めます。
engine.stop()

// サウンドイベントアセットを登録解除します
engine.assetRegistry.unregisterAsset(identifier: "drumEvent", completionHandler:nil)

// オーディオファイルを登録解除します
engine.assetRegistry.unregisterAsset(identifier: "drums", completionHandler:nil)

// エンジンを破棄します
engine = nil
```

## 空間オーディオ体験の構築

### サウンドイベントアセットの生成

```swift
// 空間パイプラインの生成
let spatialPipelineOptions: PHASESpatialPipeline.Options = [.directPathTransmission, .lateReverb]
let spatialPipeline = PHASESpatialPipeline(options: spatialPipelineOptions)!
spatialPipeline.entries[PHASESpatialCategory.lateReverb]!.sendLevel = 0.1;
engine.defaultReverbPreset = .mediumRoom

// 空間パイプラインを指定した空間ミキサーの生成
let spatialMixerDefinition = PHASESpatialMixerDefinition(spatialPipeline: spatialPipeline)

// 空間ミキサーの距離モードを設定する
let distanceModelParameters = PHASEGeometricSpreadingDistanceModelParameters()
distanceModelParameters.fadeOutParameters = PHASEDistanceModelFadeOutParameters(cullDistance: 10.0)
distanceModelParameters.rolloffFactor = 0.25
spatialMixerDefinition.distanceModelParameters = distanceModelParameters

// `drums` からサンプラーのーどを生成して、ダウンストリーム空間ミキサーをフックする
let samplerNodeDefinition = PHASESamplerNodeDefinition(soundAssetIdentifier: "drums", mixerDefinition:spatialMixerDefinition)

// サンプラーノードの再生モードをループに設定する
samplerNodeDefinition.playbackMode = .looping

// サンプラーノードのキャリブレーションモードをSPL相対にして、レベルを12dBに設定する
samplerNodeDefinition.setCalibrationMode(.relativeSpl, level: 12)

// サンプラーノードのカルオプションをスリープにする
samplerNodeDefinition.cullOption = .sleepWakeAtRealtimeOffset;

// サウンドイベントアセットをエンジン名`drumEvent`として生成する
let soundEventAsset = try engine.assetRegistry.registerSoundEventAsset(rootNode: samplerNodeDefinition, identifier: "drumEvent")
```

### リスナーの生成

```swift
// リスナーの生成
let listener = PHASEListener(engine: engine)

// リスナーの変形を回転なしに設定する
listener.transform = matrix_identity_float4x4;

// ルートオブジェクト経由でエンジンのシーングラフにリスナーをアタッチする
// これはそのシミュレーション内でリスナーをアクティブにする
try engine.rootObject.addChild(listener)
```


### ボリューメトリク・ソースの生成

```swift
:setl paste
// 20面体メッシュの生成
let mesh = MDLMesh.newIcosahedron(withRadius: 0.0142, inwardNormals: false, allocator:nil)

// 20面体メッシュから球体を生成する
let shape = PHASEShape(engine: engine, mesh: mesh)

// 球体からボリューメトリックソースを生成する
let source = PHASESource(engine: engine, shapes: [shape])

// リスナーの前面に2メートルのソースを移動して、リスナーに向けて回転させる
var sourceTransform: simd_float4x4
sourceTransform.columns.0 = simd_make_float4(-1.0, 0.0, 0.0, 0.0)
sourceTransform.columns.1 = simd_make_float4(0.0, 1.0, 0.0, 0.0)
sourceTransform.columns.2 = simd_make_float4(0.0, 0.0, -1.0, 0.0)
sourceTransform.columns.3 = simd_make_float4(0.0, 0.0, 2.0, 1.0)
source.transform = sourceTransform;

// エンジンのシーングラフにソースをアタッチする
// これはそのシミュレーション内でリスナーをアクティブにする
try engine.rootObject.addChild(source)
```

### オクルーダーの生成

```swift
// ボックスメッシュを生成する
let boxMesh = MDLMesh.newBox(withDimensions: simd_make_float3(0.6096, 0.3048, 0.1016),
                             segments: simd_uint3(repeating: 6),
                             geometryType: .triangles,
                             inwardNormals: false,
                             allocator: nil)

// ボックスメッシュからシェイプを生成する
let boxShape = PHASEShape(engine: engine, mesh:boxMesh)

// マテリアルを生成する
// このケースでは `Cardboard` を生成する
let material = PHASEMaterial(engine: engine, preset: .cardboard)

// そのシェイプ常にマテリアルを設定する
boxShape.elements[0].material = material

// そのシェイプからオクルーダーを生成する
let occluder = PHASEOccluder(engine: engine, shapes: [boxShape])
    
// リスナーの前面1mにオクルーダーを移動させて、リスナーに向ける
// これにより、オクルーダーはソースとリスナーの中間に位置することになります。
var occluderTransform: simd_float4x4
occluderTransform.columns.0 = simd_make_float4(-1.0, 0.0, 0.0, 0.0)
occluderTransform.columns.1 = simd_make_float4(0.0, 1.0, 0.0, 0.0)
occluderTransform.columns.2 = simd_make_float4(0.0, 0.0, -1.0, 0.0)
occluderTransform.columns.3 = simd_make_float4(0.0, 0.0, 1.0, 1.0)
occluder.transform = occluderTransform

// エンジンのシーングラフにソースをアタッチする
// これはそのシミュレーション内でリスナーをアクティブにする
try engine.rootObject.addChild(occluder)
```

### サウンドイベントの生成

```swift
// サウンドイベントの空間ミキサーでソースとリスナーを関連づける
let mixerParameters = PHASEMixerParameters()
mixerParameters.addSpatialMixerParameters(identifier: spatialMixerDefinition.identifier, source: source, listener: listener)

// 構築したサウンドイベントアセット`drumEvent`からサウンドイベントを生成する
let soundEvent = try PHASESoundEvent(engine: engine, assetIdentifier: "drumEvent", mixerParameters: mixerParameters)
```

## 例1

軋んだ木の上の足音。

```swift
// `footstep_wood_clip_1` からサンプルノードを生成して、チャンネルミキサーに接続する
let footstep_wood_sampler_1 = PHASESamplerNodeDefinition(soundAssetIdentifier: "footstep_wood_clip_1", mixerDefinition: channelMixerDefinition)
```

## 例2

軋んだ木の上を歩くランダムな足音。

```swift
// `footstep_wood_clip_1` からサンプルノードを生成して、チャンネルミキサーに接続する
let footstep_wood_sampler_1 = PHASESamplerNodeDefinition(soundAssetIdentifier: "footstep_wood_clip_1", mixerDefinition: channelMixerDefinition)

// `footstep_wood_clip_2` からサンプルノードを生成して、チャンネルミキサーに接続する
let footstep_wood_sampler_2 = PHASESamplerNodeDefinition(soundAssetIdentifier: "footstep_wood_clip_2", mixerDefinition: channelMixerDefinition)

// ランダムノードを生成する
// 'Footstep on Creaky Wood' サンプラーノードをランダムノードの子として生成する
// `weight`が高いほど、その子が選ばれる可能性が高くなることに注意してください。
let footstep_wood_random = PHASERandomNodeDefinition()
footstep_wood_random.addSubtree(footstep_wood_sampler_1, weight: 2)
footstep_wood_random.addSubtree(footstep_wood_sampler_2, weight: 1)
```

## 例3

軋んだ木や柔らかい砂利の上を歩くランダムな足音。

```swift
// `footstep_gravel_clip_1`からサンプルノードを作成し、チャンネルミキサーに接続します
let footstep_gravel_sampler_1 = PHASESamplerNodeDefinition(soundAssetIdentifier: "footstep_gravel_clip_1", mixerDefinition: channelMixerDefinition)

// `footstep_gravel_clip_2`からサンプルノードを作成し、チャンネルミキサーに接続します
let footstep_gravel_sampler_2 = PHASESamplerNodeDefinition(soundAssetIdentifier: "footstep_gravel_clip_2", mixerDefinition: channelMixerDefinition)

// ランダムノードを生成する
// ランダムノードの子として`Footstep on Soft Gravel`サンプラーノードを追加する
// `weight`が高いほど、その子が選ばれる可能性が高くなることに注意してください。
let footstep_gravel_random = PHASERandomNodeDefinition()
footstep_gravel_random.addSubtree(footstep_gravel_sampler_1, weight: 2)
footstep_gravel_random.addSubtree(footstep_gravel_sampler_2, weight: 1)

// 地形文字列メタパラメータを生成する
// デフォルト値として`creaky_wood`を設定する
let terrain = PHASEStringMetaParameterDefinition(value: "creaky_wood")

// 地域スイッチノードを生成する
// `Random Footstep on Creaky Wood` と `Random Footstep on Soft Gravel` を子として追加する
let terrain_switch = PHASESwitchNodeDefinition(switchMetaParameterDefinition: terrain)
terrain_switch.addSubtree(footstep_wood_random, switchValue: "creaky_wood")
terrain_switch.addSubtree(footstep_gravel_random, switchValue: "soft_gravel")
```

## 例4

濡れた路面でのランダムな足音。

```swift
// `splash_clip_1`からサンプルノードを作成し、チャンネルミキサーに接続します
let splash_sampler_1 = PHASESamplerNodeDefinition(soundAssetIdentifier: "splash_clip_1", mixerDefinition: channelMixerDefinition)
    
// `splash_clip_2`からサンプルノードを作成し、チャンネルミキサーに接続します
let splash_sampler_2 = PHASESamplerNodeDefinition(soundAssetIdentifier: "splash_clip_2", mixerDefinition: channelMixerDefinition)

// ランダムノードを生成する
// ランダムノードの子として`Splash`サンプラーノードを追加する
// `weight`が高いほど、その子が選ばれる可能性が高くなることに注意してください。
let splash_random = PHASERandomNodeDefinition()
splash_random.addSubtree(splash_sampler_1, weight: 9)
splash_random.addSubtree(splash_sampler_2, weight: 7)

// 湿潤度数メタパラメータを作成する
// 範囲はドライからウェットの`[0, 1]`にする。デフォルト値は0.5。
let wetness = PHASENumberMetaParameterDefinition(value: 0.5, minimum: 0, maximum: 1)
    
// 土地がドライからウェットの間に混合するように`Wetness`ブレンドノードを生成する
// `Terrain`スイッチノードと`Splash`ランダムノードを子として追加する
// 湿り気を増していくと、乾いた足音と水しぶきの混ざり具合が変わっていきます。
let wetness_blend = PHASEBlendNodeDefinition(blendMetaParameterDefinition: wetness)
wetness_blend.addRangeForInputValues(belowValue: 1, fullGainAtValue: 0, fadeCurveType: .linear, subtree: terrain_switch)
wetness_blend.addRangeForInputValues(aboveValue: 0, fullGainAtValue: 1, fadeCurveType: .linear, subTree: splash_random)
```

## 例5

変幻自在に変化する路面でのランダムな足音と、ノイズの多いゴアテックス・ジャケット。

```swift
// `gortex_clip_1`からサンプルノードを作成し、チャンネルミキサーに接続します
let noisy_clothing_sampler_1 = PHASESamplerNodeDefinition(soundAssetIdentifier: "gortex_clip_1", mixerDefinition: channelMixerDefinition)

// `gortex_clip_2`からサンプルノードを作成し、チャンネルミキサーに接続します
let noisy_clothing_sampler_2 = PHASESamplerNodeDefinition(soundAssetIdentifier: "gortex_clip_2", mixerDefinition: channelMixerDefinition)

// ランダムノードを生成する
// ランダムノードの子として`Noisy Clothing`サンプラーノードを追加する
// `weight`が高いほど、その子が選ばれる可能性が高くなることに注意してください。
let noisy_clothing_random = PHASERandomNodeDefinition()
noisy_clothing_random.addSubtree(noisy_clothing_sampler_1, weight: 3)
noisy_clothing_random.addSubtree(noisy_clothing_sampler_2, weight: 5)

// コンテナノードを生成する
// 子として、`Wetness`ブレンドノードと`Noisy Clothing`ランダムノードを追加する
let actor_container = PHASEContainerNodeDefinition()
actor_container.addSubtree(wetness_blend)
actor_container.addSubtree(noisy_clothing_random)
```
