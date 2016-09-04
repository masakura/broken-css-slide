# 分類なし
みなさん、`class` をどうやってつけてますか? 多分、デザインを反映させるために、`class` をつけていると思います。

例えば、以下のメニューに対してどういう風に CSS を書きますか?

```html
<nav>
  <ul>
    <li><a href="./">Home</a></li>
    <li><a href="cart.html">Cart</a></li>
    <li><a href="about.html">About</a></li>
    <li><a href="contact.html">Contact</a></li>
  </ul>
</nav>
```

これに対して、こういうふうにマークアップします。

```css
ul {
  padding-left: 0;
}

ul:before, ul:after {
  content: "";
  clear: both;
  display: block;
}

li {
  float: right;
}
```

こうすると、確かに見た目はメニューは意図通りに表示されますが、他の部分が壊れます。

```html
<section>

<!-- ... -->

<ul>
  <li>クレジットカード</li>
  <li>銀行振り込む<li>
  <li>代金引換</li>
</ul>

<!-- ... -->

</section>
```

どうして壊れるか? はみなさん感覚として分かっていると思いますが、なぜかを知る必要があります。

実際に CSS を読んで仕様にしてみます。

* `全ての ul 要素` は左パディングを 0 にする
* `全ての li 要素` は右に寄せる
* etc...

じゃあ、要件はどうでしょう?

* **ナビゲーションメニュー** にパディングはない
* **ナビゲーションメニューのメニュー項目** は右に寄せる
* etc...

実はこの逆算した仕様と要件の違いが、CSS が壊れる原因なんです。

要件に合わせて HTML を馬鹿正直に書いてみます。

```html
<nav>
  <ul class="nav-menu">
    <li class="nav-menu-item"><a href="./">Home</a></li>
    <li class="nav-menu-item"><a href="cart.html">Cart</a></li>
    <li class="nav-menu-item"><a href="about.html">About</a></li>
    <li class="nav-menu-item"><a href="contact.html">Contact</a></li>
  </ul>
</nav>
```

それにあわせて CSS も馬鹿正直に書きます。

```css
.nav-menu {
  padding-left: 0;
}

.nav-menu:before, .nav-menu:after {
  content: "";
  clear: both;
  display: block;
}

.nav-menu-item {
  float: right;
}
```

こうすると、思ってもみない要素へ反映されることはありません。

つまり...

1. デザインを大雑把に紙に書く
2. HTML を `class` なしで書く
3. CSS を書きながら、必要な `class` を追加していく

のではなく

1. デザインを大雑把に紙に書く
2. 紙の上でブロックに分割し、分類を決める
3. 2 で決めたものに対して、HTML と class を書いていく
4. CSS を書く

ということになります。

`class` は CSS でデザインを当てるために決めるものではなく、ブロックをどういう分類にするかを決め、その分類を利用して CSS でデザインを当てていく形がいいわけです。

余談です。以下のように、要素・子孫セレクタを使う方法もありまが、あまりよくないです。

```css
nav ul {
  padding-left: 0;
}

nav ul:before, ul:after {
  content: "";
  clear: both;
  display: block;
}

nav li {
  float: right;
}
```

子孫に `ul` 要素を持つ `nav` 要素が将来出てこないとも限りません。例として `article` 要素は単独で成立するため、その中に nav 要素が入ってくることもあります。

```html
<section><!-- 新しくサイトに追加されたセクション -->
  <article>
    <hader>
      <nav>
        <ul>
          <li>こっちは</li>
          <li>縦に</li>
          <li>並べたい</li>
        </ul>
      </nav>
    </header>

    <!-- ... -->
  </article>

  <!-- ... -->
</section>
```

要素・子孫セレクタだけで何とかする方法でも壊れにくくすることはできますが、新しい概念が出た時に崩れやすくなります。

```html
<section><!-- 新しくサイトに追加されたセクション -->
  <article>
    <hader>
      <nav>
        <ul class="post-nav-menu">
          <li class="post-nav-menu-item">こっちは</li>
          <li class="post-nav-menu-item">縦に</li>
          <li class="post-nav-menu-item">並べたい</li>
        </ul>
      </nav>
    </header>

    <!-- ... -->
  </article>

  <!-- ... -->
</section>
```

結論として、CSS を壊れにくくするためには、`class` を要件にある分類通りに書くのは非常に重要です。
