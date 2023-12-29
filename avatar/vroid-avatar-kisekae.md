# やりたいこと
VRoid Studioで作成したアバターを着せ替えたい

# 道具立て
- アバター
  - [VRoid Studio](https://vroid.com/studio)からエクスポートしたVRMファイル
- 服
  - [DOBUITA STYLE](https://booth.pm/ja/items/5203007)
    - 一旦はFor Shinra
    - [lilToon](https://lilxyzw.github.io/lilToon/#/)
- Unity拡張
  - [VRM Converter for VRChat](https://pokemori.booth.pm/items/1025226)
  - [Modular Avatar](https://modular-avatar.nadena.dev/ja)
    - DOBUITA STYLEの説明にも、Modular Avatar前提と書かれている

# お困りごと
服がアバターの動きに連動しない

# 手順
- 基本は[Modular Avatarの手順](https://modular-avatar.nadena.dev/ja/docs/tutorials/clothing)そのまま
  - VRoid Studioでアバターを作る
    - 服はインナーだけの素体にする
    - VRM0.x形式でエクスポートする
  - アバターのVRMファイルをUnityにインポートする
    - VRMをAssetsへドラッグ&ドロップでも、VRMがprefabに変換される模様
  - VRM Converter for VRChatで、アバターのprefabをVRChat向けに変換する
    - アバターのprefabを選択した状態で、ウィンドウメニューのVRM0->Duplicate and Convert for VRChatを選択
  - 服のunitypackageをImportする
  - 服をHierarchyに設置する
    - 服の親がアバター、というHierarchyになっていることが、Modular Avatarの前提の模様
      - この実現のため、アバターのPrefabをUnpackする
  - 服の位置を調整する
  - 服のオブジェクトに対して、右クリックでModular Avatar -> Setup Outfitを押し、Modular AvatarのComponentを導入する

# わかったこと
- 2023/11
  - UnityをPlayすると、服が連動するか、VRChatへのアップロード前に確かめられる
  - 同じ手順でも、VRoid Studioのアバターではなく薄荷ちゃんなら、連動する
  - VRoid Studioのアバターは、VRChatにアップロードして動作する
    - 着せ替えしたものは、服の腕が連動しない
  - DOBUITA STYLEのシェーダーは、lilToonが設定されている
  - Modular Avatarのサイズを合わせる機能は、VRoid Studioのアバターだと、ずれる?
    - 薄荷ちゃんだと、ダボダボがぴったりに合う
- 2023/12/5
  - 服のPrefabをUnpackすると、Modular AvatarでSetup Outfitした際に、統合先の自動選択が正しく動作する
    - Unpack前だと、アバターではなく服のボーンが指定されて赤文字で表示されたりする
  - 服のボーンをアバターのボーンに入れ込む方式だと、Modular Avatar無しでも、連動する
    - キセテネなどの拡張の方式が効果ありの可能性あり
  - Modular Avatarだと、Play時に、服のボーンがアバターに移動するように見える
    - 名前にUUIDの付いたボーンを動かすと、服が動く
  - VRMのインポートは、VRM0のメニューからでも、VRMのドラッグドロップでも、同じprefabができる
    - VRMのドラッグドロップだと、Unity内にvrmが残る
- 2023/12/22
  - Unity 2022だと
    - Hierarchyでアバターの下に服を持ってくる際に、Prefabを分解する必要が無い
      - アイコンに緑にプラスマークが付く
    - Playしてアバターの手を動かしても、手すら動かない
- 2023/12/26
  - Unity 2022では、着せ替え関係なく、そもそも手が動かない
  - アバターのボーン名を服のボーン名に手動で合わせたら、Play時のボーン名に128bit UUIDが付くものが増えた
    - ボーン名を変えない場合、Modular Avatarがアバターと服のボーン名を一致させられていないと思われる
  - ボーンの構造が、アバターと服で異なっている
    - VRoid Studioのアバターは、Hips->Spine->Chest->UpperChest->Shoulder(左右)という構成
    - Shinraちゃん用の服は、Hips->Spine->Chest->Shoulder(左右)という構成
  - 薄荷ちゃんでうまく動くのは、服の対象アバターが、同じ作者の別アバター用だったため、ボーン名と構造が一致させやすいと思われる
