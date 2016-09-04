# たくさんの責任
一つの要素にたくさんの責任を押し付けると壊れます。


## 例 (仮)
```html
<div class="post-group">
  <article class="post-item">
    <h1>夏休み最後の日</h1>
    <p><!-- ... --></p>
  </article>
  <article class="post-item"><!-- ... --></article>
  <article class="post-item"><!-- ... --></article>
  <article class="post-item"><!-- ... --></article>
  <article class="post-item"><!-- ... --></article>
</div>
```

この投稿のリストを横に三つづつ並べます。

```css
.post-group:before, .post-group:after {
  content: "";
  clear: both;
  content: block;
}

.post-item {
  float: left;
  width: 33%;
}
```

まあ、`float` なのでこのままでは表示がおかしくなるのですが、今日は無視して下さい。

この投稿一つをカードを並べたような感じに見せるために、枠線をつけようと思います。


```css
.post-item {
  float: left;
  width: 33%;
  border: solid gray 1px;
  margin: 0.2rem; /* 枠線が引っ付かないように... */
}
```

おかしくなりましたね? (多分、二個づつになるはず)

これは責任が同居しているせいです。今回の場合、「投稿をどういう風に並べるか?」と「投稿一つをどう表示するか?」の二つの責任が混じっています。

分かりやすく分割してみます。

```css
// 投稿をどういう風に並べるかの責任
.post-item {
  float: left;
  width: 33%
}

// 投稿一つをどう表示するかの責任
.post-item {
  border: solid gray 1px;
  margin: 0.2rem; /* 枠線が引っ付かないように... */
}
```

これらを分割すると、自由度がまします。

```html
<div class="post-group">
  <div class="post-item">
    <article class="post-item-content">
      <h1>夏休み最後の日</h1>
      <p><!-- ... --></p>
    </article>
  </div>
  <div class="post-item"><!-- ... --></div>
  <div class="post-item"><!-- ... --></div>
  <div class="post-item"><!-- ... --></div>
  <div class="post-item"><!-- ... --></div>
</div>
```

```css
.post-group:before, .post-group:after {
  content: "";
  clear: both;
  content: block;
}

.post-item {
  float: left;
  width: 33%;
}

.post-item-content {
  border: solid gray 1px;
  margin: 0.2rem; /* 枠線が引っ付かないように... */
}
```

かなりすっきりしました。

今の CSS では `clac` が使えますので、計算でなんとかすれば? と思われるかもしれません。ですが、「投稿一つをどう表示するか?」が分離していると、他の場所でも使えるというメリットがあります。これは計算では解決できません。

```html
<!-- 投稿一つを表示するウェブページ -->
<div class="post-detail-item">
  <article class="post-item-content">
    <h1>夏休み最後の日</h1>
    <p><!-- ... --></p>
  </article>
</div>
```

結論として、「投稿をどういう風に並べるか?」と「投稿一つをどう表示するか?」が分離されていない場合、再利用性が損なわれ、結果として CSS が壊れます。
