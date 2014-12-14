css_architecture
================

This is my css architecture idea. This is frequently updated.



# Categorize
1. Base
1. Component
1. Unit
	- layout
	- pack
1. Asset
	- utility
	- assist


## Base
SMACSSのBaseにあたるものです。

## Component

ページを構成するパーツのすべてです。
再利用しやすいよう、親に依存せず、Component単独でスタイルを維持できるように設計します。
Componentは自分自身の見た目のみを司り、位置関係に関しては、後述するUnitクラスがすべて受け持つようになります。

#### Naming convention
- ComponentName
- ComponentName--skinName_[n]
- ComponentName-descendentName_[n]

Componentはskeletonとskinに分けて記述します。

`ComponentName`がスケルトンになり、そのComponentに形作るための必要最低限のスタイルを司り、
`ComponentName--skinName`が、そのComponentのskin（modifier）のスタイル司っています。
また、Componentを作るモジュール群に関しては、`ComponentName-descendentName`で記述します。

Componentの子要素にはすべてクラス名でスタイル当てていきます。(構造とスタイルの分離のため)
親コンポーネントによって、子コンポーネントの形が変わる場合には親コンポーネントと同じskinNameを付与し対応します。

例を示します。

```html
<div class="Island Island--notify">
  <div class="Island-head">
    <h3 class="Heading Heading--h3"></h3>
  </div>

  <div class="Island-body">
    <ul class="LinkList LinkList--notify">
      <li class="LinkList-item"></li>
    </ul>
  </div>
</div>
```

セマンティックに名前を付けられない場合、またはパターンがあるものに関しては、コンポーネント名(または、skinName,descendentName)の最後に`_[value]`を追加し対応します。

```html
<a href="#" class="TextLink TextLink_1"></a>
```


## Unit

Unitにはページで一つだけしか使えないLayoutクラスと、汎用的に使えるPackクラスが存在します。

先述したように、
Componentは、そのComponentがどのような見た目をしているかだけを司り、
そして、その親のUnitが、そのComponent(またはComponent群)が、どのような位置関係にあるかを司ります。

これによって、Componentがどの位置にあるかについては全く関与しないで、自分自身の見た目だけを司ることができます。


#### Layout
ページに唯一のUnitとしてはプリフィックスとしてl-をつけます(Layout)
LayoutはSMACSSでいうThemeの役割も担っており、ページごとにオリジナルなComponentにしたい場合のフックに利用します。


```css
.l-header {}
.l-sidebarRight {}
.l-specialPage {}
```


#### Pack
汎用的に使えるUnitとしてプリフィックスとしてp-をつけます(Pack)

```css
.p-container {}
.p-streamCard {}
```

## Asset

Assetはグローバルに使えるUtilityクラスと、コンポーネントを補助するAssistクラスが存在します。

#### Utility
marginやpadding、そしてclearfixなどグローバルに使える、単一のスタイルが付与されたクラスがここに分類されます。
プリフィックスとして`u-`をつけます。
スタイルには明示的に`!important `を付与します。

```css
.u-mb10 {}
.u-clearFix {}
```

#### Assist
Assistクラスは必ずコンComponentとセットで利用します。
必ずCSSは**Componentとの結合セレクタで定義**します。

```css
.has-border {}
.has-header {}
```

タブがアクティブ、非アクティブやモーダルが、表示、非表示など**コンポーネントの状態を示すとき**は`is-`を使います

```css
.is-hidden {}
.is-error {}
.is-active {}

.Tab-button.is-active {}
```

ボーダーがある場合、ない場合やアイコンをある場合、ない場合など**コンポーネントのパーツを拡張したり、修飾する場合**は`has-`を使います

```css
.has-border {}
.has-header {}
.has-icon {}

.Panel.has-header {}
```

skinとほぼ同じ役割のように見えますが、クラス属性が冗長になるのを防ぐために、モジュールの追加(パネルのヘッダ)やグローバルなモジュール(枠線など)の場合は`has-`を使います。

```html
<div class="Panel Panel--primary has-header"></div>
```

