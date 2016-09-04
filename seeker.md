# 正解を求めて
設計が正しいと、デザインやコンテンツの自由度が増します。その分デザインを詰める回数を増やせたり、ウェブサイトにさまざまな機能を追加する回数を増やせたりします。もちろん、お客さんをまたいだ再利用も十分可能になります。

ここでいうお客さんをまたいだ再利用というのは、お客さん A のために作ったものをお客さん B にほぼそのまま使って色合いだけ変えるとかではなく、見た目やウェブサイトの構成は大きく違うが、再利用するところは再利用するというものです。

そうするためにどうすればいいのでしょうか?

といっても自分は本職ではないのでこれだ! ってのはないですが...

* フレームワークの設計思想を解読しましょう
* 壊してみましょう


## フレームワークの設計思想を読み解く
今回のお話でカンのいい人は、Bootstrap の話をしていることに気がついたかもしれません。フレームワークは再利用性と拡張性の両方を備えていて、そのためのノウハウがたくさんつまっています。

Bootstrap でウェブサイトを作った方なら分かると思うのですが、とにかく要素が多くなりがちでした。

最初は自分はそれが嫌だったのですが、いろいろとウェブサイトを作っていくうちに、Bootstrap の設計を参考にしたほうがうまくいくことを何度も経験しました。

いいフレームワークの設計思想を読み解くことはかなりいい勉強になります。


## 壊してみる
現状が正解かどうかを調べるために壊してみるとということをよくやります。

実はその瞬間だけ動く物を作るのはとても簡単です。以下のウェブサイトはこの瞬間では正しく動作します。

```html
<html><head><title>...</title></head>
<body>
  <input id="input" type="text">
  <button type="button">Click Me!</button>
  <script>
    $('button').on('click', function () { alert('Hello!,' + $('#input').text()); });
  </script>
</body>
</html>
```

これに、テキストのクリアボタンを追加すると、このプログラムは壊れます。

```html
<html><head><title>...</title></head>
<body>
  <input id="input" type="text">
  <button type="button">Click Me!</button>
  <button id="clear"type="button">Clear</button>
  <script>
    $('button').on('click', function () { alert('Hello!,' + $('#input').text()); });
    $('#clear').on('click', function () { $('#input').text(''); });
  </script>
</body>
</html>
```

もちろん、`Click Me!` ボタンに ID をつけて回避します。

```html
<html><head><title>...</title></head>
<body>
  <input id="input" type="text">
  <button id="hello" type="button">Click Me!</button>
  <button id="clear"type="button">Clear</button>
  <script>
    $('#hello').on('click', function () { alert('Hello!,' + $('#input').text()); });
    $('#clear').on('click', function () { $('#input').text(''); });
  </script>
</body>
</html>
```

この状態からクリアボタンを削除します。

```html
<html><head><title>...</title></head>
<body>
  <input id="input" type="text">
  <button id="hello" type="button">Click Me!</button>
  <script>
    $('#hello').on('click', function () { alert('Hello!,' + $('#input').text()); });
  </script>
</body>
</html>
```

この状態はボタンの追加に耐えうる状態です。ですので、これが正解です。

1. 意地悪な仕様変更を考える
2. ウェブサイトに 1 の仕様変更を加える
3. 動くように修正する
4. 1 の仕様変更がなかったことにして、不要部分を削除する

このように、わざと壊してみて、壊れなかったら正解とする方法があります。もし壊れたとしても、4 で作ったものは正解となります。

結論として、正解かどうかは壊してみれば分かるということです。耐えられたら正解です、耐えられなかったら不正解です。現状のものをいくら見つめててもなかなか分かりません。あらゆる仕様変更に耐えられるように壊す必要はありませんが、設計書に書かれていないからと切り捨てた仕様変更にそって壊さない行動は必ず後で痛い目をみます。
