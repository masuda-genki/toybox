# このページについて
増田の設計方針についてまとめています。

# 設計ルール

設計ルールのベースには`MindBEMding`を採用しています。  
[MindBEMding公式ドキュメント（翻訳版）](https://github.com/juno/bem-methodology-ja/blob/master/definitions.md)

# 命名の基本形

## class

classの命名は`MindBEMding`をベースに下記の種別を基本形としています。  


```
.Block
.Block__Element
.Block--modifire
```

命名は必ずBlockから始まり、接頭辞にBlock名を持たずに`Element,Modifier`が命名されることはありません。  
また、同一の要素に複数の`Block`名があてられることはありません。  
`Modifier`は追加でユニークなスタイルをあてる場合にのみ使用し、Modifierが単独で使用されることはありません。  
`Modifier`付与によるスタイルの差分が大きい場合は、別モジュールとします。  
`Modifier`に付与によるスタイルの差分が小さい場合は、スタイルの変更が行われる`Element`に追加で`Modifier`を命名します。

### まちがった命名パターン

```
・Blockを複数付与している
<div class="box box-2">

・Elementを単独で使用している
<div class="__inner">

・Modifierを単独で使用している
<div class="box--shadow">
``` 

### ただしい命名パターン

```
・Blockが単一で使用されている
<div class="box">

・Elementの命名にBlock__が存在している
<div class="box">
<div class="box__inner">

・ModifierとBlockを同時に使用している
<div class="box box--shadow">
```

## id

idはJS機能やアンカーリンク、またはその他管理用で必要になった場合のみ付与します。  

### アンカーリンクの場合

`anc-[連番]`を基本形とし、ページの上から順番に命名します。

## 命名の注意点

汎用性の観点から極力内容と目的は切り離して命名します。  
尚、ユニークパーツはその限りではありません。

## 命名パターンの例

### Block

| 分類 | 命名 | modifire系のサンプル |
| ------ | ------ | ------ |
| 見出し | hdg | hdg--type02 |
| リード文 | lede | lede--type02 |
| リンク | link | link--type02 |
| リンクリスト | linklist | linklist--type02 |
| リスト | list | list--type02 |
| テーブル | tbl | tbl--type02 |
| 説明リスト | list-desc | list-desc--type02 |
| ボタン | btn | btn--type02 |

### Element

| 分類 | 使用例 |
| ------ | ------ |
| inner | Blockの内側を囲む際に使用<br>原則としてBlockの直下の子要素としてのみ許容する |
| content | 内部にその他の汎用モジュールが実装可能な場合の囲いに使用 |
| head | パネル等文脈の順序がある場合に使用<br>`head`が使用される場合は必ず`body`が存在する |
| body | パネル等文脈の順序がある場合に使用<br>`body`が使用される場合は必ず`head`が存在する |
| foot | パネル等文脈の順序がある場合に使用<br>`foot`が使用される場合は必ず`head`と`body`が存在する |
| col | 主に横ならび系レイアウトの子要素で使用 |
| item | 主にリストなど並列に複数の要素が必要な際に使用 |
| hdg | モジュール内の見出しにあたる要素に使用<br>`hdg`と`lede`の使い分けに厳密性はありません |
| lede | モジュール内のリード文にあたる要素に使用<br>`hdg`と`lede`の使い分けに厳密性はありません |
| txt | モジュール内のテキストにあたる要素に使用 |

### Modifire

| 分類 | 使用例 |
| ------ | ------ |
| bg-hoge | 背景色を変更する際に使用 |
| type-hoge | バージョン違いにする際に使用 |
| color-[色] | 色変化する際に使用 |

### その他

| 分類 | 使用例 |
| ------ | ------ |
| icn | アイコン（icon）の省略系として使用 |
| btn | ボタン（button）の省略系として使用 |

## Modifireなのか、別のBlockなのか

Modifierは機能拡張の意図として使用することが原則であるため、Modifierのスタイルが一定以上（感覚値ですが）のBlockのスタイルを打消している場合は、Modifierではなく新規のモジュールの作成を検討ください。

# 設計思想

## モジュールの構造

モジュールは基本的に[単一責任の原則](https://www.ogis-ri.co.jp/otc/hiroba/others/OOcolumn/single-responsibility-principle.html#:~:text=1%E3%81%A4%E3%81%AE%E3%82%AF%E3%83%A9%E3%82%B9%E3%81%AF1,%E4%B8%80%E3%81%A4%E3%81%A7%E3%81%82%E3%82%8B%E3%81%B9%E3%81%8D%E3%80%82)に則って設計する。

例
```
<div class="grid">
<!-- ↑gridの上下余白をもつだけ -->

<div class="grid__inner">
<!-- ↑grid__colを横並びにするだけ -->

<div class="grid__col">
<!-- ↑grid__col間の余白を持つだけ -->

<div class="grid__content">
<!-- ↑内包する要素の余白を制御するだけ -->

その他のモジュールが入る

</div>

</div>

</div>
</div>
```

# ダミーリンク

ダミーリンクについてはエディター上の範囲選択の観点から

```
___dummy___
```

を使用する

# SCSS

## リセット系

ベースはressでカスタム済のものを使用する。

## ベンタープレフィックス

autoprefixerを使用するため原則として手書きはしない。

## ファイル分類

| フォルダ | 用途 |
| ------ | ------ |
| core | サイト全体に影響する原則的なscssファイルを格納する |
| layout | ヘッダーやフッター、サイドナビや大枠等の全体に共通するパーツのscssファイルを格納する |
| mixin | mixin系のscssファイルを格納する |
| modules | モジュールごとのscssファイルを格納する |
| others | ライブラリやプラグイン系、その他調整用のscssファイルを格納する |
| unique | 固有ページ用のscssファイルを格納する |

### フォルダツリー

例
```
/assets/styles/
├ core/
│ ├ _index.scss
│ ├ _base.scss
│ ├ _setting.scss
│ └ _ress.scss
├ layout/
│ ├ _index.scss
│ ├ _header.scss
│ ├ _footer.scss
│ └ _content.scss
├ mixin/
│ ├ _index.scss
│ ├ _hoge.scss
│ └ _piyo.scss
├ modules/
│ ├ _index.scss
│ ├ _hdg.scss
│ │ ├ _index.scss
│ │ └ _hdg-l1.scss
│ └ _layout.scss
│   ├ _index.scss
│   └ _grid.scss
├ other
│ ├ hoge.css
│ └ hoge.scss
└ main.scss
```

## フォーマット

stylelintにてルールを設定済。

## 疑似要素

疑似要素の使用意図を明確にするため付近にコメントを残す。

例
```
.hoge{
    display: flex;
    color: #333;

    // arrow

    &::after {
        hoge
    }
}
```

# 画像管理
### 命名ルール

実装時の解釈によるブレを避けるためにパターン数は少なくします。  
`pc/sp`は出し分けの際のみ付与します。

```
[ページ名]-[分類]-[説明(必要に応じて付与)]-[連番]_[pc/sp].拡張子
```

### 分類
| 分類 | 命名 |
| --- | --- |
| 画像/図版 | img |
| ロゴ | logo |
| アイコン | icn |



----
