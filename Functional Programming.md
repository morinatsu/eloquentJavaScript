---
layout: default
title: 6. 関数型プログラミング
---

関数型プログラミング
=====================================================

プログラムが大きくなるにつれて、それらはより複雑にそして理解しにくくもなっていく。我々全員が我々自身をかなり利口であると考えているが、もちろん、我々はただの人間にすぎず、そして度を超した混沌が我々を挫折させようともしているのである。それで全ては下り坂だ。本当には良く理解できていないものについての仕事は、いつも映画でやっている時限爆弾の配線を切ることに少し似ている。もし幸運なら、正しい方―もしあなたが映画のヒーローであればそれっぽくドラマチックなポーズを決めるところだ -- しかし全てを吹き飛ばすことになる可能性が常にある。

確かに、多くの場合において、プログラムを壊すことは大きな爆発を引き起こしはしない。しかし、プログラムが、誰かの馬鹿げた修正によって、がたがたのエラーの集まりにに悪化した時、それを何か適当なものに作り直すことは恐ろしい労働だ -- しばしば、あなたはただ最初から作り直すだろう。

こうして、プログラムの複雑さを可能な限り低く抑える道をプログラマーは常に探すようになる。これをなす重要な道は、コードをより抽象的なものにしようとすることだ。プログラムを書くとき、それが全ての点において小さな枝葉に脱線してしまうのは容易だ。小さな課題に出会い、それに取り組み、そして次の小さな問題、その他に進む。これはコードをお婆ちゃんの昔話を読むようかなのようなものにする。

**ええ、あなた、豆のスープを作るには豆を、乾燥した種を取り出さないと。そして、夜の内にそれらを水に浸すか、あるいは何時間もかけてそれを調理しなければならないの。私はある時、さえない息子が豆のスープに挑戦していたことを思い出すの。彼が豆を水に浸さなかったということを信じられる？私たちの歯はほとんど折れてしまった、私たち全員のよ。何にせよ、豆を水に浸す時、そして一人あたり1カップの豆が欲しいときは水に浸した豆が大きく膨らむことに注意しないと、気をつけていないと、器からあふれ出てしまうわ、水はたっぷり用意して、だって、私が言ったとおり、1カップの豆、それが乾燥していたら、水に浸した後に豆1カップにつき4カップの水で煮てちょうだい。2時間ほどフタをしてぐつぐつ煮たら、それから角切りにしたタマネギと、スライスしたセロリの茎、あればニンジンを1本か2本とハムを加えて。それら全部をさらに2分間ほど煮たら、もう食べられるわよ。**

このレシピを他の方法で記述する：

```
材料1人あたり：乾燥豆1カップ、刻みタマネギ1/2個、にんじん1/2本、セロリの茎1本、あればハム。

豆を1晩水に漬け、1人あたり4カップの水で2時間煮て、野菜とハムを加えて、さらに10分以上煮る。
```

これは短い、しかし、もし豆を浸すやり方を知らなければ、水が少なすぎて台無しにしてしまうだろう。しかし、豆を水に浸すやり方は探すことができ、それが秘訣である。もし聴衆が基本的な知識を確かに持っていることを仮定できれば、大きな概念を言葉数少なく語ることができ、物事を短く明瞭にすることができる。これが、多かれ少なかれ、抽象化であると言える。

どのように、このありそうもないレシピの話がプログラミングに結びつくのか？いや、明らかに、レシピはプログラムなのである。その上、料理の基本的な知識はプログラマーにとっての関数や他の構築物に符合すると仮定することができる。もし、この本の導入において、`while`のようなものでループを作るのが簡単になったこと、[4章]( {{ "/Data structures.html" | prepend:site.baseurl }})で書いた単純な関数が他の関数を短く分りやすいものにしたことを覚えていれば。そのような道具は、言語それ自体によってももたらされるが、他のものはプログラマーによって作られ、プログラムの残りから退屈な些細なことの量を減らすために使われる、そのようにしてプログラムはより簡単に動くようになる。

* * *

この章の表題である関数的プログラミングでは、関数を連結するための冴えたやり方を抽象化する。プログラマーが、基礎的な関数および、より重要なそれらの関数の使い方のレパートリーを身につけることは、それらを一から作り始めるより効果的だ。不幸にも、標準JavaScript環境は嘆かわしいことに本質的な関数を少ししか持たず、それらは我々自身で書くか、より望ましいのは、他の誰かが書いたコードを使うようにしなければならない。（それについては[9章]({{ "/Modularity.html" | prepend:site.baseurl }})で）

抽象化するための、他の人気のあるアプローチとして、特筆すべきはオブジェクト指向プログラミングがあり、これは[8章]({{ "/Object-oriented Programming.html" | prepend:site.baseurl }})の課題だ。

* * *

醜く細々としているそれが、もしあなたには全くいいものであるとしたら、終わりのなく繰り返される`for`ループが配列を飛び越えてしまわないかが、あなたを悩ませ始めるにちがいない：`for (var i = 0; i < something.length; i++)`.... これは抽象化可能だろうか？

問題は、ループが必ず実行されるコードの断片を含んでいるように、多くの関数が何らかの値を取り、それを連結させ、何を返すことにある。配列の全ての要素をプリントする関数を書くのは簡単だ。：

```javascript
function printArray(array) {
  for (var i = 0; i < array.length; i++)
    print(array[i]);
}
```

しかし、もしこれにプリント以外のことをやらせたいのだとしたら？'何かする'を関数として書けるため、かつ関数は値でもあるため、アクションを関数の値のように渡すことができる：

```javascript
function forEach(array, action) {
  for (var i = 0; i < array.length; i++)
    action(array[i]);
}

forEach(["Wampeter", "Foma", "Granfalloon"], print);
```

そして無名関数を使うことで、`for`ループのようなものを無意味な詳細なしに書くことができる：

```javascript
function sum(numbers) {
  var total = 0;
  forEach(numbers, function (number) {
    total += number;
  });
  return total;
}
show(sum([1, 10, 100]));
```

変数`total`は、レキシカル・スコーピングのルールのために無名関数の中で参照できることに注意。このバージョンは`for`ループよりほとんど短くなく、むしろ不細工な`});` で終わる -- 無名関数の本体を閉じる中括弧、`forEach`の関数呼び出しを閉じる小括弧、この呼び出しが文であるためのセミコロンだ、

変数に結びついた現在の要素を配列`number`から取り出し、そこでは`number[i]`を使う必要はもはやない。式の評価により配列が作られたとき、それらが`forEach`に直接渡されるため、変数にそれを格納する必要がないのだ。

[4章]({{ "/Data structures.html" | prepend:site.baseurl }})の猫のコードはこんな部分を含んでいた：

```javascript
var paragraphs = mailArchive[mail].split("\n");
for (var i = 0; i < paragraphs.length; i++)
  handleParagraph(paragraphs[i]);
```

これは今ではこのように書ける...

```javascript
forEach(mailArchive[mail].split("\n"), handleParagraph);
```

全てにおいて、より抽象的な（または'高いレベル'の）構造物はより多くの情報を持ちノイズの少ない結果を出せる：`sum`のコードは、**'ゼロから始まる変数があって、numbersという配列のlengthまでカウントアップされ、この変数の値毎に、対応する要素を配列から見つけ出してtotalにそれを加算する'**…という代わりに、**'numbersのnumber毎に、totalにそのnumberを加算する'**と読まれる。

* * *

`forEach`がどんなアルゴリズムを取っていようと、この場合'配列に渡って'処理が行われるし、それは抽象化される。アルゴリズムのこの'切れ目'は、この場合、これらの要素のそれぞれに何をするかはこのアルゴリズム関数に渡された関数によって埋められる。

他の関数を操作する関数は高階関数と呼ばれる。関数を操作することによって、それらは全く新しいレベルで動作を語ることができる。[3章]({{ "/Functions.html" | prepend:site.baseurl }})の`makeAddFunction`関数もまた高階関数だ。引数として関数値を取る代わりに、それらは新しい関数を作る。

高階関数は正規の関数では簡単に書けない多くのアルゴリズムを一般化するのに使える。これらの関数のレパートリーを自由に使えるとき、あなたのコードについて明瞭なやり方で考える助けになる：変数とループのごたごたしたセットの代わりに、アルゴリズムを名前で呼び出される少数の基本的なアルゴリズムの組み合わせに分解することができ、そして繰り返し繰り返しそれらをタイプする必要はなくなる。

**どうやって**それを行うかを書く代わりに、**何を**望んでいるかを書けるようになることは、我々が抽象化の高いレベルで働いていることを意味する。実用上においては、これは短く、明瞭で、より好ましいコードであることを意味する。

* * *

他の高階関数の便利なタイプは与えられた関数値を**変更**する：

```javascript
function negate(func) {
  return function(x) {
    return !func(x);
  };
}
var isNotNaN = negate(isNaN);
show(isNotNaN(NaN));
```

`negate`により返された関数は、元の関数`func`に対して与えられた引数を与え、それから結果を反転する。しかし反転させたい関数が1つより多い引数を持つときはどうなるだろうか？配列`arguments`で渡された関数の任意の引数にアクセスできるが、引数の数が分らない時はどうやって関数を呼び出すのだろうか？

関数は、このような状況のために使われる、`apply`というメソッドを持つ。これは2つの引数を取る。1つめの引数の役割については[8章]({{ "/Object-oriented Programming.html" | prepend:site.baseurl }})で論じるので、今のところはただ`null`としておこう。2つめの引数は、その関数を適用しなければならない引数を含む配列である。

```javascript
show(Math.min.apply(null, [5, 6]));

function negate(func) {
  return function() {
    return !func.apply(null, arguments);
  };
}
```

残念なことに、Internet Explorerブラウザでは、`alert`といったような、多くの組み込み関数は**本当の**関数ではない…他のものである。これらは`typeof`演算子に与えられたときには`"object"`という型であると報告され、かつ、それらは`apply`メソッドを持たない。あなた自身の関数にはこのようなことは起こらない、それらは常に本当の関数である。

* * *

配列に関する基本的なアルゴリズムをもう少し見てみよう。`sum`関数は本当は通常`reduce`や`fold`というアルゴリズムのバリエーションである：

```javascript
function reduce(combine, base, array) {
  forEach(array, function (element) {
    base = combine(base, element);
  });
  return base;
}

function add(a, b) {
  return a + b;
}

function sum(numbers) {
  return reduce(add, 0, numbers);
}
```

`reduce`は配列を、元の値に配列の1つの要素を連結する関数の使用を繰り返すことで単一の値に連結する。正確にはこれが`sum`がやっていることで、`reduce`を使うことで短くできる...JavaScriptにおいては演算子であり関数ではない加算を除いて、それで、我々は最初にそれを関数に押し込めなければならなかった。

`reduce`が、`forEach`のように最後ではなく、1つめの引数として関数を取る理由は、部分的にはこれが伝統である -- 他の言語ではそのようになっている -- ことにあり、部分的には、これにより、この章の最後で論じる特別なトリックを使うことができるということにある。それはこういう意味だ。`reduce`を呼ぶとき、変換される関数が無名関数のように書かれるのはちょっと奇妙だ。なぜなら、今、他の引数は関数の後に続き、そして普通の`for`ブロックに似ていることは全くなくなったからである。

* * *

### <a name="Ex6-1">[演習 6.1]

数値の配列を引数として取り、その中に出てくるゼロの数を返す`countZeroes`関数を書け。`reduce`を使うこと。

それから、1つの配列とテスト関数を引数に取って、テスト関数が`true`の値を返す配列の要素の数を返す、高階関数`count`を書け。`countZeroes`をこの関数を使って再実装せよ。

[解答を見る]

```javascript
function countZeroes(array) {
  function counter(total, element) {
    return total + (element === 0 ? 1 : 0);
  }
  return reduce(counter, 0, array);
}
```

奇妙な部分、疑問符とセミコロンのあるところでは新しい演算子を使っている。[2章]({{ "/Basic JavaScript.html" | prepend:site.baseurl }})では単項演算子と二項演算子を見た。これは三項演算子 -- 3つの値を操作する。これは`if/else`を省略する効果を持ち、例外は、`if`は条件により文を実行し、これは条件により式を選ぶところにある。1つ目の部分、疑問符の前は、条件である。もしこの条件が`true``あれば、疑問符の後の式が選ばれ、この場合は`1`である。もし`false`であれば、コロンの後の部分、この場合は`0`が選ばれる。

この演算子を使うとコードのある部分を短くできる。コードの中の式が大きくなり、あるいは条件の部分により多くの判定をもつようなときは、ただの平坦な`if`と`else`の方が読みやすい。

これが`count`関数を使った解答であり、等価テストの関数を含むことにより、最後の`countZeroes`もまた短くなる：

```javascript
function count(test, array) {
  return reduce(function(total, element) {
    return total + (test(element) ? 1 : 0);
  }, 0, array);
}

function equals(x) {
  return function(element) {return x === element;};
}

function countZeroes(array) {
  return count(equals(0), array);
}
```

* * *

もう一つの一般的で便利な配列に関する'基礎的なアルゴリズム'は`map`と呼ばれる。配列について、`forEach`のように全ての要素に関数を適用し、しかし関数に返された値を破棄する代わりに、これらの値から新しい配列を作る。

```javascript
function map(func, array) {
  var result = [];
  forEach(array, function (element) {
    result.push(func(element));
  });
  return result;
}

show(map(Math.round, [0.01, 2, 9.89, Math.PI]));
```

最初の引数を`function`ではなく`func`としているのは、これは`function`はキーワードであり正しい変数名ではないからであることに注意。

* * *

かつて、世捨て人がトランシルバニア山の深い森に住んでいた。ほとんどの時間は、山の周囲をさまよい、木々と話し、鳥と笑いあうだけであった。しかし今後、流れる雨が彼を小屋の中に閉じ込めたとき、吠える風が彼を耐えがたく小さいものと感じさせ、世捨て人は何か書かねばという焦りを感じ、流れゆく考えを紙に書き出したい、彼が彼自身がそうであるより大きなものに見せかけようとするようになった。

みじめに詩作、フィクション、哲学に陥った後、世捨て人はついに技術の本を書こうと決めた。若い時、彼はコンピュータープログラミングをしていて、それについて良い本を書くことができ、名声と認知はそれについてくると確かに考えていた。

彼は書いた。最初は樹皮の断片で、しかしとても実用的ではないと思い直した。彼は一番近い村に降りてラップトップコンピュータを買った。2, 3の章を書いた後、HTML形式で本を出したいと思うようになった。彼のウェブページに載せるために…。

* * *

あなたはHTMLに親しんでいるだろうか？ウェブ上のページにマークアップを追加するための方法で、我々はこの本でも何度か使っていて、もしあなたが、最低でも一般的にそれがどう動くか知っていたら良いと思う。もし良い生徒であれば、今すぐ、HTMLへの良い導入を得るためウェブを検索するだろう、そして読み終わってから戻ってくるだろう。あなた方の多くはおそらくひどい生徒だろうから、私が短い説明を与え、それで十分だと期待するとしよう。

HTMLとは'HyperText Mark-up Language'のことだ。HTML文書はすべてテキストである。なぜなら、このテキストはヘッディングだとか、このテキストは紫色だとか、その他の情報、このテキストの構造を表現できなければならないからで、少数の文字がJavaScriptの文字列におけるバックスラッシュのような特別な意味を持っている。'より小さい(<)'と'より大きい(>)'は'タグ'を作るために使われる。タグは文書の中のテキストに拡張された情報を与える。それ自身で成り立ち、例えば、ページの中で画像を見せる場所をマークしたりする。あるいはテキストや他のタグを中に入れたり、例えば段落の最初と最後をマークしたりする。

いくつかのタグは必須であり、完全なHTML文書は常に`html`タグの間に含まれなければならない。ここにHTML文書の例を挙げる：

```html
<html>
  <head>
    <title>A quote</title>
  </head>
  <body>
    <h1>A quote</h1>
    <blockquote>
      <p>The connection between the language in which we
      think/program and the problems and solutions we can imagine
      is very close.  For this reason restricting language
      features with the intent of eliminating programmer errors is
      at best dangerous.</p>
      <p>-- Bjarne Stroustrup</p>
    </blockquote>
    <p>Mr. Stroustrup is the inventor of the C++ programming
    language, but quite an insightful person nevertheless.</p>
    <p>Also, here is a picture of an ostrich:</p>
    <img src="img/ostrich.png"/>
  </body>
</html>
```

テキストや他のタグを含む要素は、最初の`<タグ名>`により始まり、後の`</タグ名>`で終わる。`html`要素は常に2つの子供を含む：`head`と`body`だ。1つ目は文書**についての**情報を含み、2つ目は実際の文書を含んでいる。

多くのタグ名は暗号のように省略されている。`h1`は'heading 1'、最も大きな見出しである。引き続き`h2`から`h6`というより小さい見出しもある。`p`は'paragraph'の意味で、`img`は'image'だ。`img`要素はテキストや他のタグを含まず、拡張された情報を持つ、`src="img/ostrich.png"`のようなものは属性と呼ばれる。この場合、それはここに表示すべき画像ファイルの情報である。

`<`と`>`がHTML文書において特別な意味を持つため、文書中のテキストにそれらを直接書くことはできない。もし`'5 < 10'`とHTML文書内に書きたければ、`'5 &lt; 10'`と書かねばならず、`'lt'`は'less than（より小さい）'の意味で、`'&gt;'`は`'>'`を書くのに使われる。そしてこれらのコードの中のアンパサンド(&)文字にも特別な意味が持たされているため、ただの`'&'`は`'&amp;'`と書かれる。

今の、これらはHTMLのほんの基本的なことだけであるが、この章でやることに関しては十分であろうし、後の章で完全に混乱なしにHTML文書を扱う。

* * *

JavaScriptコンソールはHTML文書を見るための`viewHTML`関数を持っている。上記の例の文書を`stroustrupQuote`変数に格納してあるので、下記のコードを実行することでそれを見ることができる：

```javascript
viewHTML(stroustrupQuote);
```

もしある種のポップアップブロッカーがインストールされていたり、ブラウザに組み込まれていたら、それはおそらく`viewHTML`と連携し、HTML文書を新しいウインドウかタブで開こうとするだろう。このサイトからのポップアップを許可するようブロッカーを設定してみてほしい。

* * *

ストーリーに戻ると、世捨て人は彼の本をHTML形式することを望んだ。最初に彼は全てのタグを直接彼の原稿に書きこもうとしたが、全ての「より小さい」と「より大きい」記号をタイプすることは彼の指を傷つけ、かつ彼は、`&`が必要な時は`&amp;`を書くのだということはすっかり忘れていた。これで彼は頭痛になった。次に、彼はMicrsoft Wordでこの本を書き、HTMLとして保存しよう試みた。しかし、HTMLは15倍の大きさになり、彼が考えるより複雑なものになった。こうして、Microsoft Wordも彼を頭痛にした。

やっとたどり着いた解決はこのようなものだ：彼はこの本をプレインテキストで書く、段落と見出しに見えるものを単純なルールに従って分離する。それから、このテキストを彼の望みに合ったHTMLに変換するプログラムを書くのだ。

このようなルールだ：

1. 段落は空白行によって分離される。
2. `%`記号から始まる段落は見出しとする。より多くの`%`記号があれば、より小さい見出しとする。
3. 段落の中で、アスタリスクの間に挟まれたテキストは強調する。
4. 脚注は括弧の間に書く。

* * *

彼の本との痛々しい奮闘の後から6ヶ月が過ぎたが、世捨て人はまだ少数の段落しか終わっていなかった。このポイントで、彼の小屋は雷に打たれ、彼は殺され、残りを書こうという野望は永遠に叶わなくなった。彼の黒焦げのラップトップの残骸から、下記のファイルを復元することができた。

```
% The Book of Programming

%% The Two Aspects

Below the surface of the machine, the program moves. Without effort,
it expands and contracts. In great harmony, electrons scatter and
regroup. The forms on the monitor are but ripples on the water. The
essence stays invisibly below.

When the creators built the machine, they put in the processor and the
memory. From these arise the two aspects of the program.

The aspect of the processor is the active substance. It is called
Control. The aspect of the memory is the passive substance. It is
called Data.

Data is made of merely bits, yet it takes complex forms. Control
consists only of simple instructions, yet it performs difficult
tasks. From the small and trivial, the large and complex arise.

The program source is Data. Control arises from it. The Control
proceeds to create new Data. The one is born from the other, the
other is useless without the one. This is the harmonious cycle of
Data and Control.

Of themselves, Data and Control are without structure. The programmers
of old moulded their programs out of this raw substance. Over time,
the amorphous Data has crystallised into data types, and the chaotic
Control was restricted into control structures and functions.

%% Short Sayings

When a student asked Fu-Tzu about the nature of the cycle of Data and
Control, Fu-Tzu replied 'Think of a compiler, compiling itself.'

A student asked 'The programmers of old used only simple machines and
no programming languages, yet they made beautiful programs. Why do we
use complicated machines and programming languages?'. Fu-Tzu replied
'The builders of old used only sticks and clay, yet they made
beautiful huts.'

A hermit spent ten years writing a program. 'My program can compute
the motion of the stars on a 286-computer running MS DOS', he proudly
announced. 'Nobody owns a 286-computer or uses MS DOS anymore.',
Fu-Tzu responded.

Fu-Tzu had written a small program that was full of global state and
dubious shortcuts. Reading it, a student asked 'You warned us against
these techniques, yet I find them in your program. How can this be?'
Fu-Tzu said 'There is no need to fetch a water hose when the house is
not on fire.'{This is not to be read as an encouragement of sloppy
programming, but rather as a warning against neurotic adherence to
rules of thumb.}

%% Wisdom

A student was complaining about digital numbers. 'When I take the root
of two and then square it again, the result is already inaccurate!'.
Overhearing him, Fu-Tzu laughed. 'Here is a sheet of paper. Write down
the precise value of the square root of two for me.'

Fu-Tzu said 'When you cut against the grain of the wood, much strength
is needed. When you program against the grain of a problem, much code
is needed.'

Tzu-li and Tzu-ssu were boasting about the size of their latest
programs. 'Two-hundred thousand lines', said Tzu-li, 'not counting
comments!'. 'Psah', said Tzu-ssu, 'mine is almost a *million* lines
already.' Fu-Tzu said 'My best program has five hundred lines.'
Hearing this, Tzu-li and Tzu-ssu were enlightened.

A student had been sitting motionless behind his computer for hours,
frowning darkly. He was trying to write a beautiful solution to a
difficult problem, but could not find the right approach. Fu-Tzu hit
him on the back of his head and shouted '*Type something!*' The student
started writing an ugly solution. After he had finished, he suddenly
understood the beautiful solution.

%% Progression

A beginning programmer writes his programs like an ant builds her
hill, one piece at a time, without thought for the bigger structure.
His programs will be like loose sand. They may stand for a while, but
growing too big they fall apart{Referring to the danger of internal
inconsistency and duplicated structure in unorganised code.}.

Realising this problem, the programmer will start to spend a lot of
time thinking about structure. His programs will be rigidly
structured, like rock sculptures. They are solid, but when they must
change, violence must be done to them{Referring to the fact that
structure tends to put restrictions on the evolution of a program.}.

The master programmer knows when to apply structure and when to leave
things in their simple form. His programs are like clay, solid yet
malleable.

%% Language

When a programming language is created, it is given syntax and
semantics. The syntax describes the form of the program, the semantics
describe the function. When the syntax is beautiful and the semantics
are clear, the program will be like a stately tree. When the syntax is
clumsy and the semantics confusing, the program will be like a bramble
bush.

Tzu-ssu was asked to write a program in the language called Java,
which takes a very primitive approach to functions. Every morning, as
he sat down in front of his computer, he started complaining. All day
he cursed, blaming the language for all that went wrong. Fu-Tzu
listened for a while, and then reproached him, saying 'Every language
has its own way. Follow its form, do not try to program as if you
were using another language.'
```

* * *

良き世捨て人の記憶を称え、彼のHTML生成プログラムを完成させよう。この問題に対する良いアプローチはこのようになる：

1. 全ての空行毎に区切ることで、ファイルを段落に分ける。
2. `%`文字を見出し行から取り除き、それを見出しとしてマークアップする。
3. 段落自体のテキストを処理し、それを通常のパーツと、強調されたパーツと、脚注に分ける。
4. 全ての脚注を文書の末尾に移動し、そこに番号を残す。
5. それぞれの部分を正しいHTMLタグでラップする。
6. 全てを1つのHTML文書に連結する。

このアプローチでは強調されたテキストの中の脚注や、その逆のものが許されない。これは自由裁量の類だが、例のコードを単純なものにする助けになる。もし、この章の終わりに、拡張された挑戦をやろうかと思ったら、'ネストされたマークアップ'をサポートするようプログラムを書き換えてみることができる。

原稿全体は、文字列の値として、このページでは`recluseFile`関数を呼び出すことで使用可能になる。

* * *

アルゴリズムのステップ１はありふれたものだ。1つの行に2つの改行があったら、空行であるとして、もし、[4章]({{ "/Data structures.html" | prepend:site.baseurl }})で見た、文字列が持つ`split`メソッドを覚えていたら、このトリックを実現するのだ：

```javascript
var paragraphs = recluseFile().split("\n\n");
print("Found ", paragraphs.length, " paragraphs.");
```

* * *

### <a name="Ex6-2">[演習 6.2]

引数として段落の文字列を与えられたら、これが見出しであるか判定し、見出しであれば`'%'`文字を消すとともに数を数え、それから、段落の中のテキストを含む`content`、およびこの段落が何でラップされるべきか、通常の段落であれば`"p"`、1つの`'%'`の見出しであれば`"h1"`、`x`個の`'%'`文字の見出しであれば`"hX"`を含む`type`と2つのプロパティを持つオブジェクトを返す`processParagraph`関数を書け。

文字列が`charAt`メソッドを持ち、個別の文字が段落の中にあるかどうか探すことができることを忘れずに。

[解答を見る]

```javascript
function processParagraph(paragraph) {
  var header = 0;
  while (paragraph.charAt(0) == "%") {
    paragraph = paragraph.slice(1);
    header++;
  }

  return {type: (header == 0 ? "p" : "h" + header),
          content: paragraph};
}

show(processParagraph(paragraphs[0]));
```

* * *

先ほど見た`map`関数を試してみよう。

```javascript
var paragraphs = map(processParagraph,
                     recluseFile().split("\n\n"));
```

**良し**、段落オブジェクトがうまく分類された配列を得ることができた。うまくいったけれど、アルゴリズムのステップ3を忘れていた。

**段落のテキスト自体を処理して、通常のパーツと、強調されたパーツと、脚注に分割しよう。**

このように分解する：

1. もし段落がアスタリスクから始まっていれば、強調されたパーツとして格納する。
2. もし段落が左括弧"{"から始まっていたら、脚注として格納する。
3. どちらでもなく、最初に強調されたパーツや脚注として取り出されてなければ、または文字列の終わりでなければ、通常のテキストとして格納する。
4. もし段落の中に何か残っていたら、再び1から始める。

* * *

### <a name="Ex6-3">[演習 6.3]

与えられた段落の文字列を、段落の断片の配列として返す`splitParagraph`関数を作れ。断片を表現する良い方法を考えよう。

文字または文字列の一部を文字列から探しその位置を返す、あるいは見つからなかったら`-1`を返す、`indexOf`メソッドがおそらくここで便利に使えるだろう。

これはトリッキーなアルゴリズムで、多くは全く正しくなかったり、あるいはあまりにも遠回りなやり方になるだろう。問題に当たったら、考えるのは少しの時間だけにしよう。アルゴリズムを作り上げるための、より小さなアクションを実現する内側の関数を書いてみよう。

[解答を見る]

あり得る解の1つがこれだ：

```javascript
function splitParagraph(text) {
  function indexOrEnd(character) {
    var index = text.indexOf(character);
    return index == -1 ? text.length : index;
  }

  function takeNormal() {
    var end = reduce(Math.min, text.length,
                     map(indexOrEnd, ["*", "{"]));
    var part = text.slice(0, end);
    text = text.slice(end);
    return part;
  }

  function takeUpTo(character) {
    var end = text.indexOf(character, 1);
    if (end == -1)
      throw new Error("Missing closing '" + character + "'");
    var part = text.slice(1, end);
    text = text.slice(end + 1);
    return part;
  }

  var fragments = [];

  while (text != "") {
    if (text.charAt(0) == "*")
      fragments.push({type: "emphasised",
                      content: takeUpTo("*")});
    else if (text.charAt(0) == "{")
      fragments.push({type: "footnote",
                      content: takeUpTo("}")});
    else
      fragments.push({type: "normal",
                      content: takeNormal()});
  }
  return fragments;
}
```

`takeNormal`関数の中の`map`と`reduce`は大食いであることに注意。ここは関数的プログラミングについての章なので、プログラムが関数的になっているのは我々の意図したとおりだ！これがどう動くかわかるだろうか？`map`は与えられた文字が見つかった位置や、またはそれらの文字が見つからなくなったら文字列の終わり位置の配列を作りだし、そして`reduce`はそれらの最小のものを取り、次に探すべき文字列の開始位置とする。

もしmapとreduceを使わずに書いたらこのようになっただろう：

```javascript
var nextAsterisk = text.indexOf("*");
var nextBrace = text.indexOf("{");
var end = text.length;
if (nextAsterisk != -1)
  end = nextAsterisk;
if (nextBrace != -1 && nextBrace < end)
  end = nextBrace;
```

これはもっと醜い。多くの時間を、物事の順番を基準に判定するときに費やしており、もしそれらが2つしかないにしても、配列操作として書く方が全ての値を別々の`if`文で処理するよりましだ。（幸運にも、'この、またはあの'文字が最初にどこに出現するかを判定する、より簡単な手段を[10章]({{ "/Regular Expressions.html" | prepend:site.baseurl }})で説明する）

もし上記と異なる方法で断片を格納する`splitParagraph`を書いていたら、それを修正したくなるかもしれない、なぜなら章の残りの関数は断片を`type`および`content`プロパティをもつものと仮定しているからである。

* * *

我々は今や`processParagraph`で段落の中のテキストも分割することができ、私のバージョンをこのように変更できる：

```javascript
function processParagraph(paragraph) {
  var header = 0;
  while (paragraph.charAt(0) == "%") {
    paragraph = paragraph.slice(1);
    header++;
  }

  return {type: (header == 0 ? "p" : "h" + header),
          content: splitParagraph(paragraph)};
}
```

段落の配列を、fragmentオブジェクトの配列を含むparagraphオブジェクトの配列にマッピングする。次にやることは脚注を取りだし、それへの参照を作ることだ。このようになる。：

```javascript
function extractFootnotes(paragraphs) {
  var footnotes = [];
  var currentNote = 0;

  function replaceFootnote(fragment) {
    if (fragment.type == "footnote") {
      currentNote++;
      footnotes.push(fragment);
      fragment.number = currentNote;
      return {type: "reference", number: currentNote};
    }
    else {
      return fragment;
    }
  }

  forEach(paragraphs, function(paragraph) {
    paragraph.content = map(replaceFootnote,
                            paragraph.content);
  });

  return footnotes;
}
```

`replaceFootnote` 関数は断片毎に呼び出される。それが断片であればその場にそのまま返すが、脚注であれば、この脚注を`footnote`配列に格納し、代わりに参照を返す。この処理の中で、脚注と参照には番号が付けられる。

* * *

これで必要な情報を展開する道具立ては揃った。残るはそれを正しいHTMLに生成することだ。

多くの人々が文字列を連結することがHTMLを作る王道だと考える。彼らは、例えば、囲碁で遊べるサイトへリンクすることを必要とするとき、このようにするだろう。:

```javascript
var url = "http://www.gokgs.com/";
var text = "Play Go!";
var linkText = "<a href=\"" + url + "\">" + text + "</a>";
print(linkText);
```

（このaはHTML文書の中にリンクを作るときに使われるタグである）...不細工なだけでなく、`text`文字列に不等号やアンパサンドが含まれていたときには、処理を誤る。あなたのウェブサイトに奇怪なことが起こり、とても素人っぽく見えるだろう。起こって欲しくないことだ。単純なHTMLを生成する関数は簡単に書ける。それを書こう。

* * *

HTMLの生成をうまくやる秘訣はHTML文書をテキストの平坦な部品として扱う代わりに、データ構造として扱うことにある。JavaScriptのオブジェクトはとても簡単にこれをモデル化する手段を提供している。：

```javascript
var linkObject = {name: "a",
                  attributes: {href: "http://www.gokgs.com/"},
                  content: ["Play Go!"]};
```

それぞれのHTML要素は表示されるタグ名を与える`name`プロパティを含んでいる。それが属性を持つときには、属性が格納されたオブジェクトを含む`attributes`プロパティも含まれる。それが中身を持っていれば、この要素に含まれる他の要素の配列を含む`content`プロパティもある。文字列はHTML文書においてテキストの部品の役割をし、配列`["Play Go!"]`はこのリンクはテキストの単純な部品である1つの要素だけを持つことを示している。

これらのオブジェクトをタイプするのはかっこ悪いが、その必要はない。これをやるためのショートカットになる関数を作ろう。：

```javascript
function tag(name, content, attributes) {
  return {name: name, attributes: attributes, content: content};
}
```

もしそれが適切でなければ、我々は要素の`attributes`や`content`を定義しないこともできるようにし、この関数の2番目か3番目の引数は必要でなければ無視されることに注意。

`tag`はまだむしろ原始的であるから、リンクのような共通のタイプの要素や単純な文書の外側についてはショートカットを書こう。：

```javascript
function link(target, text) {
  return tag("a", [text], {href: target});
}

function htmlDoc(title, bodyContent) {
  return tag("html", [tag("head", [tag("title", [title])]),
                      tag("body", bodyContent)]);
}
```

* * *

### <a name="Ex6-4">[演習 6.4]

もし必要なら例のHTML文書を見返して、画像ファイルの場所を与えられたときにHTMLびimg要素を作る`image`関数を書け。

[解答を見る]

```javascript
function image(src) {
  return tag("img", [], {src: src});
}
```

* * *

文書を作ったとき、それは文字列になる。しかしこの文字列をデータ構造から組み立てるときにとてもまっすぐなやりかたを取った。文書のテキストの中の特殊な文字を変形することは覚えておくべき重要なことだ...

```javascript
function escapeHTML(text) {
  var replacements = [[/&/g, "&amp;"], [/"/g, "&quot;"],
                      [/</g, "&lt;"], [/>/g, "&gt;"]];
  forEach(replacements, function(replace) {
    text = text.replace(replace[0], replace[1]);
  });
  return text;
}
```

文字列の`replace`メソッドは1つめの引数のパターンの出現した場所全てを2つめの引数で置き換えた新しい文字列を作り、`"Borobudur".replace(/r/g, "k")`であれば`"Bokobuduk"`になる。パターンの書き方については悩まないように -- [10章]({{ "/Regular Expressions.html" | prepend:site.baseurl }})で分かる。`escapeHTML`関数は配列に入れられた複数の異なる置き換えを、ループしながら1つ1つ引数の文字列に適用していく。

二重引用符も置き換えられるのは、なぜならこの関数はHTMLタグの属性の中のテキストにも使うからである。二重引用符に囲まれたそれらは、それゆえに中に二重引用符を含むことができない。

replaceを4回呼び出しているのは、コンピューターは文字列全体のチェックと置換を4回行うという意味だ。これはとても効率的でない。もし十分な注意をしたら、先ほどの`splitParagraph`に1度だけやったのように、この関数のより複雑なバージョンを書けるだろう。今は、これをやるには我々は忙しすぎる。もう一度、[10章]({{ "/Regular Expressions.html" | prepend:site.baseurl }})でこれをやる良い方法を見せよう。

* * *

HTML要素のオブジェクトを文字列にするには、このような再帰関数が使える。：

```javascript
function renderHTML(element) {
  var pieces = [];

  function renderAttributes(attributes) {
    var result = [];
    if (attributes) {
      for (var name in attributes)
        result.push(" " + name + "=\"" +
                    escapeHTML(attributes[name]) + "\"");
    }
    return result.join("");
  }

  function render(element) {
    // Text node
    if (typeof element == "string") {
      pieces.push(escapeHTML(element));
    }
    // Empty tag
    else if (!element.content || element.content.length == 0) {
      pieces.push("<" + element.name +
                  renderAttributes(element.attributes) + "/>");
    }
    // Tag with content
    else {
      pieces.push("<" + element.name +
                  renderAttributes(element.attributes) + ">");
      forEach(element.content, render);
      pieces.push("</" + element.name + ">");
    }
  }

  render(element);
  return pieces.join("");
}
```

HTMLタグ属性を作るためにJavaScriptオブジェクトからプロパティを展開する`in`ループが外に出たことに注意。また、2つの場所で、配列が文字列を蓄積するのに使われ、それから1つの文字列に連結されることにも注意。なぜ空の文字列から始めてそれに中身を`+=`演算子で追加するのことをしないのか。

新しい文字列、特別に大きい文字列を作ること、実に多くの仕事になる。JavaScriptの文字列の値は変更不可能であることを覚えておくこと。もし何かをこれに連結し、新しい文字列が作られ、古い文字列もそのまま残る。もし、大きな文字列を多数の小さな文字列の連結によってつくったら、全てのステップで新しい文字列が作られ、次の部品をそれらに連結するためだけに放り出される。もし、その一方で、全ての小さな文字列を配列に格納してそれから連結するのであれば、大きな文字列が作られるのは1度だけで済む。

* * *

そう、このHTML生成システムを試してみよう...

```javascript
print(renderHTML(link("http://www.nedroid.com", "Drawings!")));
```

動くようだ。

```javascript
var body = [tag("h1", ["The Test"]),
            tag("p", ["Here is a paragraph, and an image..."]),
            image("img/sheep.png")];
var doc = htmlDoc("The Test", body);
viewHTML(renderHTML(doc));
```

今、私はこのアプローチがおそらく完全ではないことを警告しておくべきだろう。これが実際に作るものはXMLで、HTMLに近いものではあるが、より構造化されたものだ。上記のような単純なケースでは、これが問題を起こすことはない。しかしながら、正しいXMLであっても、妥当なHTMLではないことがあって、作った文書を表示しようとするブラウザを混乱させることがある。例えば、もし文書の中に空の`script`タグ（JavaScriptをページに入れるときに使われる）があったら、ブラウザは空とは考えずにその後の全てがJavaScriptであると考えるだろう。（この場合、問題は1つの空白をタグの中に含め、空のタグでなくすことで正しくタグを閉じ、解決できる）

* * *

### <a name="Ex6-5">[演習 6.5]

段落オブジェクト（脚注が既に取り除かれている）を取り、正しいHTML要素（段落か見出しかは、paragraphオブジェクトの`type`プロパティによって判断）を作る`renderFragment`関数を書き、それを使って`renderParagraph`の別の実装を行え。

この関数は脚注の参照の作成において便利なものになるだろう：

```javascript
function footnote(number) {
  return tag("sup", [link("#footnote" + number,
                          String(number))]);
}
```

`sup`タグは正しくは'superscript（上付きルビ）'であり、他のテキストより小さく少し高めの位置にする。リンクの先は`"#footnote1"`のような何かだ。`'#'`文字を含むリンクはページ内のアンカーを参照し、この場合は脚注をクリックすると、読者がページの末尾の脚注を読めるようにするリンクを作るのに使われる。

強調を行う断片を作るタグは`em`で、通常のテキストは拡張タグ無しで作られる。

[解答を見る]

```javascript
function renderParagraph(paragraph) {
  return tag(paragraph.type, map(renderFragment,
                                 paragraph.content));
}

function renderFragment(fragment) {
  if (fragment.type == "reference")
    return footnote(fragment.number);
  else if (fragment.type == "emphasised")
    return tag("em", [fragment.content]);
  else if (fragment.type == "normal")
    return fragment.content;
}
```

* * *

ほとんど終わりだ。脚注を作る関数だけが後に残っている。`"#footnote1"`のリンクを動かすには、その脚注を含むアンカーがなければならない。HTMLで、アンカーはリンクにも使われる1つの`a`要素である。この場合、`name`属性が`href`の代わりに必要となる。

```javascript
function renderFootnote(footnote) {
  var anchor = tag("a", [], {name: "footnote" + footnote.number});
  var number = "[" + footnote.number + "] ";
  return tag("p", [tag("small", [anchor, number,
                                 footnote.content])]);
}
```

それから、与えられた正しい形式のファイルと文書タイトルからHTML文書を返す関数がこれだ。：

```javascript
function renderFile(file, title) {
  var paragraphs = map(processParagraph, file.split("\n\n"));
  var footnotes = map(renderFootnote,
                      extractFootnotes(paragraphs));
  var body = map(renderParagraph, paragraphs).concat(footnotes);
  return renderHTML(htmlDoc(title, body));
}

viewHTML(renderFile(recluseFile(), "The Book of Programming"));
```

配列の`concat`メソッドは、文字列の`+`演算子と同様に他の配列を連結するのに使われる。

* * *

これ以降の章では、`map`や`reduce`のような基礎的な高階関数が常に利用可能で、コードの例で使われる。今後、新しい便利な道具をこれに加えよう。[9章]({{ "/Modularity.html" | prepend:site.baseurl }})にて、この'基本的'な関数のセットをより構造的なアプローチで発展させよう。

* * *

高階関数を使うとき、JavaScriptでの演算子は関数でないことがしばしば悩みの元となる。`add`や`eauqls`の関数がいくつかのポイントで必要になる。毎回これらを書き直すのは、あなたも同意するだろうが、痛みを伴うことだ。これからは、これらの関数を含む`op`オブジェクトが存在するものと仮定しよう。：

```javascript
var op = {
  "+": function(a, b){return a + b;},
  "==": function(a, b){return a == b;},
  "===": function(a, b){return a === b;},
  "!": function(a){return !a;}
  /* and so on */
};
```

そう、我々は配列を合計するのに、`reduce(op["+"], 0, [1, 2, 3, 4, 5])`と書くことができる。しかし`equals`や、引数の1つに既に値を持つ`makeAddFunction`のようなものが必要なのだとしたら？そのような場合新しい関数を書き直さなければならない。

そのような場合には、'部分的な適用(partial application)'というものが便利だ。その引数の幾つかが既に知られている新しい関数を作りたいなら、追加の引数はこれら固定の引数の後に渡されるように扱えば良い。これで関数の`apply`メソッドを創造的に使うことができる。：

```javascript
function asArray(quasiArray, start) {
  var result = [];
  for (var i = (start || 0); i < quasiArray.length; i++)
    result.push(quasiArray[i]);
  return result;
}

function partial(func) {
  var fixedArgs = asArray(arguments, 1);
  return function(){
    return func.apply(null, fixedArgs.concat(asArray(arguments)));
  };
}
```

複数の引数を同時に結びつけることができるようにしたい、`asArray`関数は通常の配列を`argments`オブジェクトの外に出すのに必要となる。`concat`メソッドが使えるように、これらの中身を本物の配列にコピーする。それはオプションの2つめの引数も取ることができ、スタートから幾つかの引数を外すのに使うことができる。

外側の関数(`partial`)の`arguments`を別な名前の変数に格納する必要があることにも注意。なぜなら、そうでなければ内側の関数はそれをみることができない -- それはそれ自身の`arguments`変数を持ちそれによって外側の関数の引数が隠れてしまうからである。

これで`equal(10)`は`partial(op["=="], 10)`と書けるようになった。

```javascript
show(map(partial(op["+"], 1), [0, 2, 4, 6, 8, 10]));
```

`map`が配列の引数の前に、関数の引数を取る理由は、それが関数として与えられた方がmapを部分的に適用するのにしばしば便利だからである。これは1つの値を操作する関数から値の配列を操作する関数に格上げする。例えば、数値の配列の配列があって、これらの全ての平方をとりたいときはこうする。：

```javascript
function square(x) {return x * x;}

show(map(partial(map, square), [[10, 100], [12, 16], [0, 1]]));
```

* * *

最後のトリックは関数を連結して合成関数にしたいときに便利だ。この章のスタートで、呼び出す関数の結果に真偽値の**not**演算子を適用する`negate`関数を見せた。：

```javascript
function negate(func) {
  return function() {
    return !func.apply(null, arguments);
  };
}
```

これは一般的なパターンの特殊な場合である：関数Aを呼び、それからその結果に関数Bを適用する。数学では合成は共通の概念だ。高階関数でこのように書ける。：

```javascript
function compose(func1, func2) {
  return function() {
    return func1(func2.apply(null, arguments));
  };
}

var isUndefined = partial(op["==="], undefined);
var isDefined = compose(op["!"], isUndefined);
show(isDefined(Math.PI));
show(isDefined(Math.PIE));
```

ここでは`function`キーワードを全く使わずに新しい関数を定義している。これは例えば、`map`や`reduce`に渡す単純な関数を作り出すことが必要なときに便利だ。しかしながら、関数がこれらの例より複雑になるときは、`function`としてそのまま書き出すよりも通常は短くなる（より効率的であるとは言えない）。
