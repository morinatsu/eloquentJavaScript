---
layout: default
title: 5. エラー・ハンドリング
---

エラー・ハンドリング
=====================================================

全てが期待通りであるときに動くプログラムを書くのは良いスタートだ。期待していない条件に遭遇したときに行儀良く振る舞うプログラムにすることは本当のチャレンジである。

プログラムが遭遇しうる問題のある状況は2つのカテゴリに分けられる：プログラマーのミスと本来の問題だ。もし誰かが必要な引数を関数に渡すのを忘れたら、それが1つめの種類の問題の例だ。他方、もしプログラムがユーザーに名前を入力するようにと質問し、そして空の文字列が帰ってきたとしたら、それはプログラマーが防止できる問題ではない。

一般的に、プログラマーの誤りというのは見つけ修正することができ、本来の誤りはに対しては、コードをチェックし回復するためにふさわしいアクションを取るか（例えば、名前を聞き直すとか）、さもなくば最後には想定された、きれいなやりかたで失敗する。

* * *

重要なのはプログラムが陥ったのはこれらのうちどちらのカテゴリーのものであるか確実に判断することだ。例えば、我々の古い`power`関数について考えよう:

```javascript
function power(base, exponent) {
  var result = 1;
  for (var count = 0; count < exponent; count++)
    result *= base;
  return result;
}
```

ギークが`power("Rabbit", 4)`と呼び出してみたとき、それは明らかにプログラマーの誤りだが、しかし`power(9, 0.5)`についてはどうだろうか？この関数は小数の累乗を扱うことができない、しかし、数学的な話としては、小数の累乗というのは完全に理屈に合っている（`Math.pow`は扱える）。完全にクリアではない種類の入力を関数が受けうる状況において、コメントに受け入れ可能な引数の種類書いておくのはしばしば良いアイデアである。

* * *

もし関数がそれ自体では解決不能な問題に遭遇したら、どのようにすべきか？[4章]( {{ "/Data structures.html" | pretend:site.baseurl }})で書いた`between`関数では:

```javascript
function between(string, start, end) {
  var startAt = string.indexOf(start) + start.length;
  var endAt = string.indexOf(end, startAt);
  return string.slice(startAt, endAt);
}
```

もし、文字列に存在しない`start`と`end`が与えられたら、`indexOf`は`-1`を返しこのバージョンの`between`はナンセンスに満ちた値を返すだろう：`between("Your mother!", "{-", "-}")`は`"our mother"`を返す。

このプログラムの実行中、期待された通りに文字列の値が設定されて、関数が呼ばれたため、幸運にも処理を続けることができる。しかし値は誤っていて、なんであろうと終了し、結果も誤っているだろう。
もし不幸にも、この誤りが20の他の関数に渡されるだけだったら1つの問題しか起こさないだろう。このような場合、問題が発生を見つけるのは非常に困難になる。

ある場合において、正しくない入力が与えられたときに行儀悪くふるまう関数に注意しないと、問題は不確かなものになる。例えば、関数が2, 3のカ所から呼ばれているだけなのが確実なら、これらの場所が正しい入力を与えているかどうか確認することができる。問題のあるケースを扱うために関数が大きく醜いものになるのは、一般的には悪いことではない。

しかし多くの場合、'静かに'失敗する関数は使うのが難しく、危険である。もし`between`を呼び出すコードが、全てがどこにあるか知ろうとしたらどうなるだろうか？ここで、全ての仕事をやり直すと言うことはできない。`between`を実行し、`between`自身の結果がそれ自体に含まれているかチェックする。それはよくない。1つの解決としては`between`が失敗したとき、`false`とか`undefined`のような特別な値を返すようにすることだ。:

```javascript
function between(string, start, end) {
  var startAt = string.indexOf(start);
  if (startAt == -1)
    return undefined;
  startAt += start.length;
  var endAt = string.indexOf(end, startAt);
  if (endAt == -1)
    return undefined;

  return string.slice(startAt, endAt);
}
```

エラーチェックが全般的に関数を美しくなくしたように見えるかもしれない。しかし今では`between`を呼び出すコードはこのように書ける。:

```javascript
var input = prompt("Tell me something", "");
var parenthesized = between(input, "(", ")");
if (parenthesized != undefined)
  print("You parenthesized '", parenthesized, "'.");
```

* * *

多くの場合、特別な値を返すのはエラーが起こっていることを示すのに完全で良い方法だ。それは、しかしながら、欠点もある。1つには、関数が既に可能な全て種類の値を返すものである場合はどうだろうか？例えば、配列の最後の要素を返す、この関数について考えてみよう。:

```javascript
function lastElement(array) {
  if (array.length > 0)
    return array[array.length - 1];
  else
    return undefined;
}

show(lastElement([1, 2, undefined]));
```

この配列は最後の要素を持っているか？`lastElement`の戻り値を見る限り、そうとは言いがたい。

2つめの問題は、特別な値を返すことは、しばしば別な問題を引き起こす元となるということだ。もし同じコードの部品が`between`10回呼び出すとき、`undefined`が帰ってきたか10回チェックする必要がある。また、もし関数が`between`を失敗からの回復の戦略なく呼び出したら、関数は`between`の戻り値をチェックせねばならず、もしそれが`undefined`だったときには、この関数は`undefined`か他の特別な値を呼び元に返し、そこでもまたこの値をチェックすることになる。

しばしば、おかしなことが起こったとき、実用的なのは、実行を止めて問題の扱い方を知っている所に明示的にジャンプすることだろう。

よし、我々はついている。多くのプログラミング言語がそのようなものを提供している。通常、それは例外ハンドリングと呼ばれる。

* * *

例外ハンドリングの理屈はこのようなものだ：コードが例外という値を起こす（または投げる）ことを可能にする。例外を起こすというのは関数からの強制的な飛び出しに似ている -- 現在の関数から飛び出すだけでなく、その呼び元の外、現在実行中の場所からトップレベルまでの全ての道を登り上がる。これはスタック解放と呼ばれる。[3章]({{ "/Functions.html" | pretend:site.baseurl }})の関数呼び出しのスタックを思い出そう。例外はこのスタックを飛び降り、遭遇した呼び出しの背景を全て捨てる。

それらが常に元のスタックを飛び降りるなら、例外は使いづらい物になるだろう、それらはプログラムをいいものにする別の手段も提供する。幸運にも、それは例外の障害物をスタックに沿ってセットすることを可能にする。これらは飛び降りてきた例外を'捕まえ'、それに何かをすることができ、例外が捕まった場所からプログラムは実行を続ける。

例:

```javascript
function lastElement(array) {
  if (array.length > 0)
    return array[array.length - 1];
  else
    throw "Can not take the last element of an empty array.";
}

function lastElementPlusTen(array) {
  return lastElement(array) + 10;
}

try {
  print(lastElementPlusTen([]));
}
catch (error) {
  print("Something went wrong: ", error);
}
```

`throw`は例外を起こすのに使うキーワードだ。`try`キーワードは例外への障害をセットアップする：その後ろのブロックの中のコードで、例外が起こったら、`catch`ブロックが実行される。`catch`キーワードの後の括弧の中の変数名はブロックの中で、例外の値に名前を与えるものだ。

`lastElementPlusTen`関数は`lastElement`の中で起こった誤りを完全に無視することに注意。これは例外のもつ大きな長所だ -- エラーハンドリングのコードはエラーが起こるかもしれない場所、そしてエラーを扱う場所にだけ必要なのだ。その間の関数ではすべて忘れて良い。

そう、ほとんど。

* * *

下記のコードについて考えよう：`processThing`関数は、実行中の実体が何であるかを示す`curentThing`というトップレベルの変数の設定を必要とし、他の関数もこれにアクセスする。通常は引数として渡すだけだろうが、しかしこれでは実用的ではない。関数が終了したとき、`currentThing`も`null`を返す。

```javascript
var currentThing = null;

function processThing(thing) {
  if (currentThing != null)
    throw "Oh no! We are already processing a thing!";

  currentThing = thing;
  /* よく分らない処理の実行中... */
  currentThing = null;
}
```

しかし、もしよく分らない処理が例外を投げたらどうなるだろうか？その場合`processThing`の呼び出しは例外のためスタックから放り出され、`currentThing`は`null`にリセットされなくなるだろう。

`try`文には`finally`キーワードも続けて書くことができ、それは'**何が**起ころうと、このコードを`try`ブロックのコードの実行後に実行する'という意味である。もし関数がクリアする必要がある何かを持っていたら、それをクリーンアップするコードは通常`finally`ブロックに書かれるべきだ。:

```javascript
function processThing(thing) {
  if (currentThing != null)
    throw "Oh no! We are already processing a thing!";

  currentThing = thing;
  try {
    /* do complicated processing... */
  }
  finally {
    currentThing = null;
  }
}
```

* * *

プログラムに含まれる多くの誤りはJavaScript実行環境に例外を起こさせる。例えば:

```javascript
try {
  print(Sasquatch);
}
catch (error) {
  print("Caught: " + error.message);
}
```

このような場合、特別なエラーオブジェクトが起こる。これらは常に問題の説明を含む`message`プロパティを持つ。同様なオブジェクトを`new`キーワードと`Error`コンストラクタで起こすことができる。:

```javascript
throw new Error("Fire!");
```

* * *

例外が捕まることなくスタックの底から出て来たとき、それは実行環境で扱われる。これは異なるブラウザ間では違いが起こることを意味し、それはしばしばログに書かれるエラーの説明であったり、エラーを説明するポップアップであったりする。

このページのコンソールに入力されたコードが作ったエラーは常にコンソールに捕まって表示され、他には出力されない。

* * *

多くのプログラマーは例外は純粋にエラーハンドリングの仕掛けであると考える。本質的には、しかしながら、プログラムの制御フローに影響を与えるもう一つの手段であるというだけだ。例えば、再帰関数の`break`文のように使うこともできる。下記はオブジェクト、ないしさらにその中のオブジェクトが、その中に少なくとも7つの`true`の値を含んでいるかを調べる相当奇妙な関数だ。:

```javascript
var FoundSeven = {};

function hasSevenTruths(object) {
  var counted = 0;

  function count(object) {
    for (var name in object) {
      if (object[name] === true) {
        counted++;
        if (counted == 7)
          throw FoundSeven;
      }
      else if (typeof object[name] == "object") {
        count(object[name]);
      }
    }
  }

  try {
    count(object);
    return false;
  }
  catch (exception) {
    if (exception != FoundSeven)
      throw exception;
    return true;
  }
}
```

内側の`count`関数は、引数のオブジェクト毎に再帰的に呼び出される。
変数`counted`が7に達したときは、数え続ける意味はない、しかしその下に他の呼び出しがあるかもしれないので、現在の`count`の呼び出しから戻っても、数を数えるのを停止する必要はない。制御が`count`の呼び出しから飛び抜けて、`cactch`ブロックに着地するように値を投げるだけだ。

しかし、例外の場合に`true`をただ返すだけというのは正しくない。何か他の間違いが起こっていないか、例外が`FoundSeven`オブジェクトかどうかを最初にチェックした上で、この目的のための個別のものを作る。もしそうでなければ、この`catch`ブロックはそれの扱い方が分らず、エラーがもう一度起こることになる。

これはエラー条件を扱う際の共通のパターンでもある -- `catch`ブロックは扱いが分る例外だけを確実に扱うようにしなければならない。この章のいくつかの例のように、文字列の値を投げるのは、例外のタイプを認識するのを難しくするために、良いアイデアであることは希だ。`FoundSeven`のようなユニークな値を使うか、[8章]({{ "/Object-oriented Programming.html" | pretend:site.baseurl }})で説明するような新しいタイプのオブジェクトを導入するのがベターだ。
