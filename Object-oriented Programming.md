---
layout: default
title: 8. オブジェクト指向プログラミング
---

オブジェクト指向プログラミング
=====================================================

90年代の初め、オブジェクト指向プログラミングと呼ばれるものがソフトウェア産業をかき立てた。その背景にあるアイデアはその時点でも本当に新しいものではなかったが、転がり始めるとついに十分な勢いを得て、ファッショナブルなものとなった。本が書かれ、コースが与えられ、プログラミング言語が開発された。全ては突然で、皆がオブジェクト指向の美点を賞賛し、熱狂的に全ての問題に適用し、彼ら自身は**プログラムを正しく書く方法**をついに見つけたのだと信じていた。

多くのことが起こった。プロセスが困難で混乱しているとき、人々は常に魔法的な解決に用心する。それ自身が与える解決が、そのようなものに見えるとき、彼らは献身的な花になる準備をする。多くのプログラマーにとって、今日でも、オブジェクト指向（または彼らがそのようにみなすもの）は福音であった。プログラムが'真のオブジェクト指向でない'とき、それは劣ったものであると判断されることも意味していた。

おかしな流行もこれと同じだけ続いた、にもかかわらず、オブジェクト指向の寿命は、その核心のアイデアがとても充実していて有益だったという事実によって大いに説明できる。この章では、JavaScriptの（むしろエキセントリックな）受容に沿って、これらのアイデアについて論じよう。上の段にはその信用を傷つけようという意図はない。私が望むのはそれらに無分別な傾倒を抱くことに対しての注意喚起だ。

* * *

その名が示すとおり、オブジェクト指向プログラミングはオブジェクトに関係がある。これまでは、我々はオブジェクトをデータのゆるい集まりとして使ってきており、それがふさわしいと思えるときにプロパティを付けたり変更したりしてきた。オブジェクト指向アプローチにおいては、オブジェクトはそれ自身の小さな世界とみなされ、外の世界からは限定的で予め定義されたインターフェース、個別のメソッドやプロパティのみを通じてそれに触れるようになる。[7章]({{ "/Searching.html" | prepend:site.baseurl }})で使った'リーチド・リスト'はこれの1つの例だ： `makeReachedList`、`storeReached`、`findReached`の3つの関数だけを使ってそれを操作できる。これらの3つの関数がそれらのオブジェクトのインターフェースを形作っている。

`Date`、`Error`、`BinaryHeap`オブジェクトもこれと同じように働く。そのオブジェクトで動く正規の関数を提供する代わりに、`new`キーワードでそれらのオブジェクトを作り、メソッドやプロパティで残りのインターフェースを提供する。

* * *

オブジェクトにメソッドを与える1つの方法は単に関数値をくっつけることだ。

```javascript
var rabbit = {};
rabbit.speak = function(line) {
  print("The rabbit says '", line, "'");
};

rabbit.speak("Well, now you're asking me.");
```

多くの場合、このメソッドは**何者が**動いているのかを知るために必要となるだろう。例えば、もし複数の異なるウサギがいたら、`speak`メソッドは話しているウサギが誰かを示す。この目的のために、`this`という特別な変数があり、これは関数がメソッドとして呼ばれたときに、関係するオブジェクトを常に示す。関数がメソッドとして呼ばれるというのは、プロパティであるかのように探された時で、`object.method()`というのが、明示的な呼び出しである。

```javascript
function speak(line) {
  print("The ", this.adjective, " rabbit says '", line, "'");
}
var whiteRabbit = {adjective: "white", speak: speak};
var fatRabbit = {adjective: "fat", speak: speak};

whiteRabbit.speak("Oh my ears and whiskers, how late it's getting!");
fatRabbit.speak("I could sure use a carrot right now.");
```

* * *

[6章]({{ "/Functional Programming.html" | prepend:site.baseurl }})では常に`null`にしておいた、`apply`メソッドの1つめの引数の謎を今明らかにしよう。この引数は適用する関数にオブジェクトを指定するために使える。メソッドでない関数にはこれは関係がなく、そのため`null`としていた。

```javascript
speak.apply(fatRabbit, ["Yum."]);
```

関数は`apply`と同じような`call`メソッドも持っていて、しかしこれには、1つの配列の代わりに、引数を別々に分けて関数に与えることができる。：

```javascript
speak.call(fatRabbit, "Burp.");
```

* * *

`new`キーワードは新しいオブジェクトを作る便利な方法を提供する。関数が`new`という語を先頭に付けて呼ばれたとき、その`this`変数は、（明示的に他の何かを返さないときは）自動的に返された**新しい**オブジェクトを指す。このような新しいオブジェクトを作るための関数はコンストラクターと呼ばれる。ウサギのためのコンストラクタはここ。：

```javascript
function Rabbit(adjective) {
  this.adjective = adjective;
  this.speak = function(line) {
    print("The ", this.adjective, " rabbit says '", line, "'");
  };
}

var killerRabbit = new Rabbit("killer");
killerRabbit.speak("GRAAAAAAAAAH!");
```

便宜上、JavaScriptのプログラマーの間ではコンストラクターの名前は大文字で始めることになっている。これで他の関数と簡単に区別できるようになる。

なぜ`new`キーワードが必要なのだろうか？結局のところ、単純にこのように書くこともできる。：

```javascript
function makeRabbit(adjective) {
  return {
    adjective: adjective,
    speak: function(line) {/*etc*/}
  };
}

var blackRabbit = makeRabbit("black");
```

しかし全く同じというわけではない。`new`は裏で他のことも行っている。その1つには、`killerRabit`はそれを作った`Rabit`関数を示す`constructor`というプロパティを持つ。`blackRabbit`もそのようなプロパティを持っているが、それは`Object`関数を示している。

```javascript
show(killerRabbit.constructor);
show(blackRabbit.constructor);
```

* * *

`constructor`プロパティはどこからくるのか？それはrabbitのプロトタイプの一部である。プロトタイプは、もし何かの混乱があったとき、JavaScriptのオブジェクトを動かすパワフルな方法だ。プロトタイプが元になっている全てのオブジェクトには固有のプロパティのセットが与えられている。これまで使ってきた単純なオブジェクトはもっとも基本的なプロトタイプを元にしていて、それは`Object`コンストラクターに結びついている。実際、`{}`とタイプするのは`new Object()`とタイプするのに等しい。

```javascript
var simpleObject = {};
show(simpleObject.constructor);
show(simpleObject.toString);
```

`toString`メソッドは`Object`プロトタイプの一部だ。これは、全ての単純なオブジェクトはそれを文字列に変換する`toString`メソッドを持つ、ということを意味する。rabbitオブジェクトは`Rabbit`コンストラクターに結びついたプロトタイプに基づいている。コンストラクターの`prototype`プロパティをそれ、それらのプロトタイプにアクセスするのに使える。：

```javascript
show(Rabbit.prototype);
show(Rabbit.prototype.constructor);
```

全ての関数は自動的に`prototype`プロパティを得て、`constructor`プロパティがその関数を指す返す。なぜならrabbitプロトタイプはそれ自体が`Object`プロトタイプをもとにしたオブジェクトであるからで、その`toString`メソッドを共有している。

```javascript
show(killerRabbit.toString == simpleObject.toString);
```

* * *

オブジェクトがそのプロトタイプのプロパティを共有しているように見えたとしても、その共有は一方通行のものだ。プロトタイプのプロパティはもとになっているオブジェクトに影響するが、しかしこのオブジェクトのプロパティがプロトタイプを変更することはない。

正確なルールはこうだ：プロパティの値を探すとき、JavaScriptは、まずオブジェクト**それ自身**が持っているプロパティを探す。もし探している名前のプロパティが見つかれば、その値を得る。もし、そのようなプロパティがなければ、オブジェクトのプロトタイプにさかのぼって探し続け、さらにそのプロトタイプのプロトタイプ、へと続く。もしプロパティが見つからなかったら、`undefined`値が与えられる。他方、プロパティに値を**設定するとき**は、JavaScriptはプロトタイプを見ることなく、常にそのオブジェクト自身のプロパティを設定する。

```javascript
Rabbit.prototype.teeth = "small";
show(killerRabbit.teeth);
killerRabbit.teeth = "long, sharp, and bloody";
show(killerRabbit.teeth);
show(Rabbit.prototype.teeth);
```

これは、プロトタイプはそれをもとにしているオブジェクトにいつでも新しいプロパティとメソッドを追加することができる、ということを意味する。例えば、ウサギたちにダンスをさせる必要がでてくるかもしれない。

```javascript
Rabbit.prototype.dance = function() {
  print("The ", this.adjective, " rabbit dances a jig.");
};

killerRabbit.dance();
```

そして、お察しの通り、プロトタイプ的なウサギは、`speak`メソッドのように全てのウサギが共通に持っている値を完全に置き換える。これは`Rabbit`コンストラクターへの新しいアプローチである。：

```javascript
function Rabbit(adjective) {
  this.adjective = adjective;
}
Rabbit.prototype.speak = function(line) {
  print("The ", this.adjective, " rabbit says '", line, "'");
};

var hazelRabbit = new Rabbit("hazel");
hazelRabbit.speak("Good Frith!");
```

* * *

全てのオブジェクトがプロトタイプを持ち、そしてプロトタイプ由来のプロパティを受け継ぐという事実はトリッキーになり得る。これは、[4章]({{ "/Data structures.html" | prepend:site.baseurl }})のように物事の集合をオブジェクトに格納するのは間違いを起こすかもしれないということを意味する。もし、例えば、`"constructor"`という猫に出会ったかどうかをこのようにチェックしたらどうなるか。：

```javascript
var noCatsAtAll = {};
if ("constructor" in noCatsAtAll)
  print("Yes, there definitely is a cat called 'constructor'.");
```

これは問題をはらんでいる。これに関する問題は、`Object`や`Array`のような標準コンストラクターのプロトタイプを新しい有用な関数で拡張するのはしばしば実用的になりうる、ということだ。例えば、オブジェクトの持っている全ての（隠されていない）プロパティの名前の配列を返す`properties`というメソッドを全てのオブジェクトに与えることもできる。：

```javascript
Object.prototype.properties = function() {
  var result = [];
  for (var property in this)
    result.push(property);
  return result;
};

var test = {x: 10, y: 3};
show(test.properties());
```

そしてこれは明確に問題を見せる。今や`Object`プロトタイプは`proerties`というプロパティをもち、`for`と`in`を使って任意のオブジェクトのプロパティをループし、我々が一般的には望んでいない共有されたプロパティまでも与えてくれる。我々はオブジェクトそれ自体が持っているプロパティにしか興味はない。

幸運にも、プロパティがそのオブジェクト自身のものか、そのプロトタイプの中の1つのものかを知る手段がある。残念ながら、それは1つのオブジェクトのプロパティを少々かっこう悪くループする。すべてのオブジェクトは`hasOwnPropertiy`というメソッドを持ち、それは与えられた名前のプロパティをオブジェクトが持っているかどうかを教えてくれる。これを使って、`properties`メソッドをこう書き換えよう。：

```javascript
Object.prototype.properties = function() {
  var result = [];
  for (var property in this) {
    if (this.hasOwnProperty(property))
      result.push(property);
  }
  return result;
};

var test = {"Fat Igor": true, "Fireball": true};
show(test.properties());
```

そしてもちろん、高階関数にそれを抽象化することもできる。`action`関数がオブジェクトの中でプロパティの名前とその値の両方で呼ばれていることに注意しよう。

```javascript
function forEachIn(object, action) {
  for (var property in object) {
    if (object.hasOwnProperty(property))
      action(property, object[property]);
  }
}

var chimera = {head: "lion", body: "goat", tail: "snake"};
forEachIn(chimera, function(name, value) {
  print("The ", name, " of a ", value, ".");
});
```

しかし、もし`hasOwnProperty`という名前の猫がいたらどうなるだろう？（あなたが知ることはないだろう。）それはオブジェクトに格納され、そして次に猫のコレクションを見ようとしたときに、そのプロパティが関数の値を指していないために、`object.hasOwnProperty`は失敗する。かっこう悪いことをしてこれを解決できる。：

```javascript
function forEachIn(object, action) {
  for (var property in object) {
    if (Object.prototype.hasOwnProperty.call(object, property))
      action(property, object[property]);
  }
}

var test = {name: "Mordecai", hasOwnProperty: "Uh-oh"};
forEachIn(test, function(name, value) {
  print("Property ", name, " = ", value);
});
```

(注: この例は現在のところInternet Explorer 8では正しく動かない、どうやら組み込みのプロトタイプのプロパティに問題があるらしい。)

ここで、オブジェクト自身で見つけたメソッドの代わりに、`Object`プロトタイプ由来のメソッドを得て、それから正しいオブジェクトに適用するために`call`してみよう。誰かが実際に`Object.prototype`のメソッドで何かヘマをすることがなければ（しないように）、これは正しく動くだろう。

* * *

`hasOwnProperty`はオブジェクトが特定のプロパティを持っているかを見たいときに`in`演算子を使うような状況でも使うことができる。しかしながら、もう一つ問題がある。[4章]({{ "/Data structures.html" | prepend:site.baseurl }})で見たいくつかのプロパティ、`toString`のような、は'隠されて'いて、`for/in`でプロパティを一通り見ても出てこない。Geckoファミリーのブラウザ（Firefox、最も重要な）は全てのオブジェクトに`__proto__`という隠されたプロパティを与えており、それはそのオブジェクトのプロトタイプを示しているということが分る。プログラムが明示的に加えてなくても、`hasOwnProperty`はこれについて真を返す。オブジェクトのプロトタイプへのアクセスを持つことはとても便利であり得るが、しかしそのように、それをプロパティにするのは良いアイデアではない。まだ、Firefoxは広く使われているブラウザなので、ウェブへのプログラムを書くときはこれに注意する必要がある。`propertyIsEnumerable`というメソッドがあり、それは隠されたプロパティについて`false`を返し、`__proto__`のようなおかしなものを除外するのにつかうことができる。このような1つの式で信頼できるものとして働くようになる。：

```javascript
var object = {foo: "bar"};
show(Object.prototype.hasOwnProperty.call(object, "foo") &&
     Object.prototype.propertyIsEnumerable.call(object, "foo"));
```

上手くて単純だろうか。違う？これはJavaScriptでうまくデザインされなかった点の1つだ。オブジェクトは2つの役割を演じるが、'メソッドを持つ値'の役割においてはプロトタイプは偉大な働きをし、'プロパティの集合'という役割においてはプロトタイプがただの邪魔になる。

* * *

渡されたオブジェクトのプロパティをチェックすることが必要になる度に上記の式を毎回書くのはうまくない。関数に押し込むこともできるが、しかし、オブジェクトをプロパティの集合として扱いたいときのような特別な状況のためにコンストラクターとプロトタイプを書くのがより良いアプローチだろう。これを使うことで、名前でプロパティを見つけ出すことができるようになるから、これを`Dictionary`（辞書）と呼ぼう。

```javascript
function Dictionary(startValues) {
  this.values = startValues || {};
}
Dictionary.prototype.store = function(name, value) {
  this.values[name] = value;
};
Dictionary.prototype.lookup = function(name) {
  return this.values[name];
};
Dictionary.prototype.contains = function(name) {
  return Object.prototype.hasOwnProperty.call(this.values, name) &&
    Object.prototype.propertyIsEnumerable.call(this.values, name);
};
Dictionary.prototype.each = function(action) {
  forEachIn(this.values, action);
};

var colours = new Dictionary({Grover: "blue",
                              Elmo: "orange",
                              Bert: "yellow"});
show(colours.contains("Grover"));
show(colours.contains("constructor"));
colours.each(function(name, colour) {
  print(name, " is ", colour);
});
```

オブジェクトをプロパティの集合として扱うアプローチに関する上手くない点が、便利なインターフェースにより、今や完全に'カプセル化'された：1つのコンストラクターと4つのメソッドである。`Dictionary`オブジェクトの`values`プロパティはこのインターフェースの部分ではないことに注意しよう。`Dictionary`オブジェクト使うにあたり、その内側の詳細を直接扱う必要はない。

インターフェースを書くのがいつであろうと、それが何を行いどのように使うものかを手早くコメントに書いておくのは良いアイデアである。このインターフェースを使いたいと思った、それを書いた3ヶ月後のあなた自身を含む誰かが、これによって、プログラムを完全に見なくても、その使い方を早く知ることができる。

インターフェースを設計する時の多くの時間、何を思いついたにせよ、すぐにその限界や問題を見つけ、変更するだろう。時間を浪費しないようにするには、2,3の本当の状況でそれら自身を試してからインターフェースを文書化することだ。―もちろん、これは文書化のことを一時的に忘れるようにということでもある。個人的には、私は文書を書くことをシステムの'仕上げ'を加えることとして見ている。その準備ができたように感じたときが、それについて書くときで、英語（かなんかの言語）が、JavaScript（かなんかのプログラミング言語）であるかのように書くのである。

* * *

オブジェクトの外部インターフェースと内部の詳細を区別するのは2つの理由により重要だ。1つめは、小さく、明確に書かれたインターフェースはオブジェクトを使いやすくするということだ。インターフェースを守ることだけを意識していれば、オブジェクトそれ自体の残りの部分を変更することに悩まなくて済む。

2つめは、結局はオブジェクト型[^1]の内部の実装を変更することがしばしば必要になったり実用的であったりするということだ。例えば、より効率的にすること、あるいは問題を修正すること。外側のコードが単一のプロパティ毎にそのオブジェクトの詳細にアクセスしていたら、オブジェクト以外のコードをたくさん修正することなしにはオブジェクトを何も修正できなくなる。もし外側のコードが小さなインターフェースしか使っていなければ、インターフェースを変更するまでは、やりたいだけのことができる。

  [^1] これらのタイプは他のプログラミング言語では通常'クラス'と呼ばれる。

ある人々はここからさらに先に行く。彼らは、例えば、オブジェクトのインターフェースにプロパティを含めず、メソッドだけを含めようとする -- もし彼らのオブジェクトが`length`を持っていたら、それにはlengthプロパティではなく、`getLength`メソッドでアクセスする。この方法で、もし彼らのオブジェクトを変更したいとき、`length`プロパティがない限り、例えば、今、内部的な配列の長さを返さなければならないなら、彼らはインターフェースを変えずに関数を変更できる。

私自身は多くの場合これにはそれほどの価値はないと考えている。`return this.length;`のみの`getLength`メソッドを加えることは；ほとんど意味のないコードの追加でしかなく、多くの状況で、自分のオブジェクトのインターフェースを時々変更しなければならないリスクより、意味のないコードが増えることの方が問題が大きいと考えている。

* * *

既存のプロトタイプに新しいメソッドを追加することはとても便利だ。特にJavaScriptでの`Array`や`String`プロトタイプは2, 3のより基本的なメソッドを使ってきた。例えば、`forEach`と`map`を配列のメソッドに代えて、4章で書いた`startsWith`関数を文字列のメソッドにしたり。

しかしながら、もしプログラムが同じウェブページで、他のプログラム（あなたが書くものでも、他の誰かのものでも）と同じように実行されるなら、それは`for/in`を素朴に -- 以前我々もそうしてきたように -- プロトタイプ、特に`Object`や`Array`プロトタイプに何かを加えることは、これらのループが突然に新しいプロパティを見始めるため、間違いなく何かを壊すことになる。この理由により、これらのプロトタイプに絶対に触らないようにしている。もちろん、もし注意深ければ、かつ酷い書かれ方をしたコードと共存しなければならなくなるようなことはないと想定できれば、標準プロトタイプにメソッドを加えることは完全に良いテクニックである。

* * *

この章では、バーチャルな飼育器(terrarium)、タンクとその中を動き回る昆虫を作ることにする。いくつかのオブジェクトが入り組むことになるだろう（結局、この章はオブジェクト指向プログラミングなので）。むしろ単純なアプローチを取って、飼育器は、[7章]({{ "/Searching.html" | prepend:site.baseurl }})の2つめの地図のように二次元のマス目とする。このマス目の上に虫の数を持つ。飼育器が活動中のとき、全ての虫は移動のような行動を行うチャンスを半秒ごとに得る。

そういうわけで、時間と空間を固定されたサイズを持つ単位に分割しよう -- 空間には四角を、時間には半秒を。これは、通常、プログラム内で物事をモデル化するのを簡単にするが、しかしもちろん非常に不正確であるという欠点も持つ。幸運にも、この飼育器シミュレータはどのような正確さも必要としておらず、そのことは捨て置くことができる。

* * *

飼育器は文字列の配列である'計画'によって定義される。単一の文字列を使うこともできるが、JavaScriptは文字列を単一の行でしか書けないため、タイプするのがたいへんだ。

```javascript
var thePlan =
  ["############################",
   "#      #    #      o      ##",
   "#                          #",
   "#          #####           #",
   "##         #   #    ##     #",
   "###           ##     #     #",
   "#           ###      #     #",
   "#   ####                   #",
   "#   ##       o             #",
   "# o  #         o       ### #",
   "#    #                     #",
   "############################"];
```

`"#"`文字は飼育器の壁（そして飼育器に置かれた装飾用の岩）を表現し、`"o"`は虫を、そして空白はお察しの通り何もない空間を表現する。

このようなplan配列を飼育器オブジェクトを作るのに使う。このオブジェクトは飼育器の形と内容を追跡し、その中の虫を動かせる。4つのメソッドを持つ：1つめは`toString`で、飼育器をもとの文字列と同様な文字列に変換し、その中で何が起こっているか分るようにする。それから、`step`で、飼育器の中の全ての虫がもし彼らが望めば1ステップだけ動けるようにする。最後に`start`と`stop`、飼育器が動いているかどうかを制御し、動いているなら`step`が半秒ごとに呼び出されて、虫が動き続ける。

* * *

### <a name="Ex8-1">[演習 8.1]</a>

マス目上のポイントをオブジェクトにより再度表そう。[7章]({{ "/Searching.html" | prepend:site.baseurl }})ではpointsとともに働く`point`、`addPoints`、`samePoints`の3つの関数を使った。今回、コンストラクターと2つのメソッドを使う。地点を表わすxとyの2つの引数の組を取り、`x`と`y`をプロパティに持つオブジェクトを作る`Point`コンストラクタを書け。このコンストラクターのプロトタイプに、他の地点を引数に取り、2つの地点の`x`と`y`の合計した**新しい**地点を返す`add`メソッドを加えよ。1つの地点を引数に取り、`this`（この）ポイントと与えられたポイントが同じ地点を指しているかどうかを真偽値として返す`isEqualTo`メソッドも加えよ。

2つのメソッドとは別に、`x`と`y`プロパティはこの型のオブジェクトのインターフェースの一部でもある：pointオブジェクトを使うコードは自由に`x`と`y`を取り出し変更できる。

[解答を見る]

```javascript
function Point(x, y) {
  this.x = x;
  this.y = y;
}
Point.prototype.add = function(other) {
  return new Point(this.x + other.x, this.y + other.y);
};
Point.prototype.isEqualTo = function(other) {
  return this.x == other.x && this.y == other.y;
};

show((new Point(3, 1)).add(new Point(2, 4)));
```

あなたの版の`add`が`this`ポイントを壊すことなく、確実に新しいpointオブジェクトを作るようにすること。現在のpointを変更するメソッドはむしろ`+=`演算子のようなものであるから、そいうわけでこちらは`+`演算子のようなものだ。

* * *

決まったプログラムに実装するためにオブジェクトを書くとき、その機能がその後どちらに向かうかは常に明らかというわけでない。あることをオブジェクトのメソッドとして書くのが最善だとしても、他のものは分離した関数に書いた方が良く、またあるものは別の型のオブジェクトとして実装するのが最善である。物事をクリアに組織的にしておくには、オブジェクトのメソッドと応答をできる限り小さい量に抑えておくことが重要だ。1つのオブジェクトが大きすぎるとき、機能は大きなゴミの山となり、恐ろしく混乱したソースになるだろう。

上記で、飼育器のオブジェクトはその中身を格納し、その中の虫を動かさせる責任を負うだろうと言った。最初に、飼育器が虫を**動かす**、ではなく飼育器が虫を**動かさせる**、であることに注意して欲しい。虫たち自身もまたオブジェクトであり、そしてこれらのオブジェクトは彼らが何をしたいか判断する責任を負う。飼育器は半秒ごとに彼らに何をするか尋ねるインフラのみを提供し、そしてもし虫たちが動くことを決断したら、それでこの移動が実際に起こる。

飼育器の中身をマス目に格納するのはとても複雑になり得る。表現の種類と、この表現へのアクセス手段、マス目を'計画'配列で初期化する方法、`toString`メソッドのためにマス目の内容を文字列に各方法、マス目上の虫の動きを定義しよう。もしこれの部分を他のオブジェクトに移動できるとしたら、飼育器のオブジェクト自体が大きすぎたり複雑になったりしないので、その方が良いだろう。

* * *

1つのオブジェクトに混乱したデータ表現と問題のあるコードを見つける度に、データ表現のコードを他の型のオブジェクトに分離しようとするのは良いアイデアだ。この場合、値のマス目の表現が必要であり、そこで飼育器に必要な操作をサポートする`Grid`型を書く。

マス目上の値を格納するには、2つのオプションがある。1つは配列の配列を使う。このようになる。：

```javascript
var grid = [["0,0", "1,0", "2,0"],
            ["0,1", "1,1", "2,1"]];
show(grid[1][2]);
```

また、単一の配列に値を入れることもできる。この場合、`x`、`y`の要素は配列の中の`x + y * width`の位置の要素として探すことができ、この`width`というのはマス目の幅である。

```javascript
var grid = ["0,0", "1,0", "2,0",
            "0,1", "1,1", "2,1"];
show(grid[2 + 1 * 3]);
```

私は2つめの表現を選んだ、なぜなら、こちらの方が配列の初期化が楽だからだ。`new Array(x)`は`undefined`値で満たされた長さ`x`の新しい配列を作る。

```javascript
function Grid(width, height) {
  this.width = width;
  this.height = height;
  this.cells = new Array(width * height);
}
Grid.prototype.valueAt = function(point) {
  return this.cells[point.y * this.width + point.x];
};
Grid.prototype.setValueAt = function(point, value) {
  this.cells[point.y * this.width + point.x] = value;
};
Grid.prototype.isInside = function(point) {
  return point.x >= 0 && point.y >= 0 &&
         point.x < this.width && point.y < this.height;
};
Grid.prototype.moveValue = function(from, to) {
  this.setValueAt(to, this.valueAt(from));
  this.setValueAt(from, undefined);
};
```

* * *

### <a name="Ex8-2">[演習 8.2]</a>

マス目の全ての要素に渡って、移動が必要な虫を探したり、全てを文字列に変換することも必要だ。これを簡単に作るには、アクションをその引数として取る高階関数を作るのだ。`Grid`プロトタイプに`each`メソッドを追加し、それは引数が2つの関数を引数として取る。それはこの関数を全てのマス目について呼び出し、その地点のpointオブジェクトを1つめの引数に、そしてマス目上のその地点の値を2つめの引数として与える。

`0,0`の地点から始めて、1行を一時に、`1,0`は`0,1`の前に扱われる。これで後で飼育器の`toString`関数を書くのが楽になる。（ヒント：`x`の`for`ループの組を`y`のforループの組の中に入れよう）

gridオブジェクトに`cells`プロパティを直接ぶら下げず、その場所の値を取るのに`valueAt`を使うほうが賢明だろう。この方法は、もし（何らかの理由により）値の格納に別な方法を使うことに決めたとき、`valueAt`と`setValueAt`を書き換えるだけで済み、他のメソッドには触らないことができる。

[解答を見る]

```javascript
Grid.prototype.each = function(action) {
  for (var y = 0; y < this.height; y++) {
    for (var x = 0; x < this.width; x++) {
      var point = new Point(x, y);
      action(point, this.valueAt(point));
    }
  }
};
```

* * *

最後に、このgridをテストしよう。：

```javascript
var testGrid = new Grid(3, 2);
testGrid.setValueAt(new Point(1, 0), "#");
testGrid.setValueAt(new Point(1, 1), "o");
testGrid.each(function(point, value) {
  print(point.x, ",", point.y, ": ", value);
});
```

* * *

`Terrarium`（飼育器）のコンストラクターを書く前に、その中に住む'虫のオブジェクト'をもう少し詳細にしよう。初めは、飼育器は彼らが取りたいアクションを尋ねると書いた。これはこのように働く：それぞれの虫のオブジェクトは`act`メソッドを持ち、それは呼び出されたら、'アクション'を返す。アクションは`type`プロパティを伴うオブジェクトで、その名前は虫が取りたいアクションのタイプ、例えば`"move"`（移動）になる。多くのアクションにおいて、アクションは虫が動きたい方向のような拡張の情報も持つ。

虫たちは恐るべき近眼で、すぐ隣のマス目しか見ることができない。しかし、彼らはそれをベースに行動する。`act`メソッドが呼ばれたとき、虫の周囲の情報を持つオブジェクトを与えられる。8つの方向のそれぞれについて、その中にプロパティを持つ。プロパティは虫の上なら北の`"n"`、左上なら北東の`"ne"`というようなことを示し、残りも同様とする。その名前で参照している方向を探すには、以下の辞書オブジェクトが有用である。：

```javascript
var directions = new Dictionary(
  {"n":  new Point( 0, -1),
   "ne": new Point( 1, -1),
   "e":  new Point( 1,  0),
   "se": new Point( 1,  1),
   "s":  new Point( 0,  1),
   "sw": new Point(-1,  1),
   "w":  new Point(-1,  0),
   "nw": new Point(-1, -1)});

show(new Point(4, 4).add(directions.lookup("se")));
```

虫が移動することを決断したとき、彼は、これらの方向の一つの名前が入った`direction`プロパティを持つactionオブジェクトを結果として与えることで、動きたい方向を示す。'光に向かって'、常に南にしか行かない、単純な、頭の悪い虫はこのようになる。：

```javascript
function StupidBug() {};
StupidBug.prototype.act = function(surroundings) {
  return {type: "move", direction: "s"};
};
```

* * *

これで`Terrarium`オブジェクト型それ自体に取り組めるようになった。最初に、計画（文字列の配列）を引数に取り、そのマス目を初期化する、そのコンストラクターだ。

```javascript
var wall = {};

function Terrarium(plan) {
  var grid = new Grid(plan[0].length, plan.length);
  for (var y = 0; y < plan.length; y++) {
    var line = plan[y];
    for (var x = 0; x < line.length; x++) {
      grid.setValueAt(new Point(x, y),
                      elementFromCharacter(line.charAt(x)));
    }
  }
  this.grid = grid;
}

function elementFromCharacter(character) {
  if (character == " ")
    return undefined;
  else if (character == "#")
    return wall;
  else if (character == "o")
    return new StupidBug();
}
```

`wall`はマス目の中で壁の位置を示すオブジェクトだ。本物の壁のように、何もせず、ただそこにあってスペースを埋める。

* * *

一番わかりやすい飼育器オブジェクトのメソッドは`toString`で、これは飼育器を文字列に変換する。これを簡単に作るために、`wall`と`StupidBug`のプロトタイプに、それを表現する文字を持つ`character`プロパティを付けてマークする。

```javascript
wall.character = "#";
StupidBug.prototype.character = "o";

function characterFromElement(element) {
  if (element == undefined)
    return " ";
  else
    return element.character;
}

show(characterFromElement(wall));
```

* * *

### <a name="Ex8-3">[演習 8.3]</a>

これで、`Grid`オブジェクトの`each`メソッドを文字列を組み立てるのに使うことができる。しかし読みやすい結果を作るためには、行の終わり毎に改行を入れるのがいいだろう。grid上の位置の`x`は行の終わりに着いたかどうかの判定に使える。引数を取らず、飼育器をうまく2次元の視点で`print`するための文字列を返す`toString`メソッドを追加しよう。

[解答を見る]

```javascript
Terrarium.prototype.toString = function() {
  var characters = [];
  var endOfLine = this.grid.width - 1;
  this.grid.each(function(point, value) {
    characters.push(characterFromElement(value));
    if (point.x == endOfLine)
      characters.push("\n");
  });
  return characters.join("");
};
```

そしてこれを試そう...

```javascript
var terrarium = new Terrarium(thePlan);
print(terrarium.toString());
```

* * *

可能なら、上記の演習を解くときに、gridの`each`に渡される引数である関数の中から`this.grid`へのアクセスを試みてみよう。これは動かないだろう。関数の呼び出しは、それがメソッドとして呼ばれたものでなくても、常に新しい`this`の中の関数の中で定義されたものを返す。それゆえ、関数の外側の`this`変数は見ることができない。

`endOfLine`のように、内側の関数から**参照できる**変数の中に必要な情報を格納することによって、`this`の代替にするのがしばしばわかりやすい手段である。もし完全な`this`オブジェクトにアクセスする必要があるなら、それも変数に入れてしまえる。`self`(または`that`)という名前がしばしばそのような変数のために使われる。

しかしこれら全ての余分な変数はかっこう悪いかもしれない。他の良い解決法は、[6章]({{ "/Functional Programming.html" | prepend:site.baseurl }})の`partial`と同じような関数を使うことだ。これは、関数に引数を追加する代わりに`this`オブジェクトを追加し、最初の引数を関数の`apply`メソッドに使う：

```javascript
function bind(func, object) {
  return function(){
    return func.apply(object, arguments);
  };
}

var testArray = [];
var pushTest = bind(testArray.push, testArray);
pushTest("A");
pushTest("B");
show(testArray);
```

この手で、内側の関数を`this`に`bind`して、それが外側の関数のものであるかのように同じ`this`を得られる。

* * *

### <a name="Ex8-4">[演習 8.4]</a>

`bind(testArray.push, testArray)`の式のtestArrayの名前が2回出てくる。オブジェクトの名前を2回も**使うことなく**、オブジェクトとその中の1つのメソッドを結びつける関数`method`を設計できるだろうか？

[解答を見る]

メソッドの名前を文字列で与えれば可能だ。こうすれば、`method`関数は正しい関数値をそれ自身から見つけることができる。

```javascript
function method(object, name) {
  return function() {
    object[name].apply(object, arguments);
  };
}

var pushTest = method(testArray, "push");
```

* * *

飼育器の`step`メソッドを実装するには`bind`（または`method`）が必要だ。このメソッドはマス目上の全てのバグに、そのアクションを尋ね、与えられたアクションを実行する。gridの`each`を使って、出会った虫をそのまま操作したいと思うかもしれない。しかし、そうしてしまうと、南か東に動いた虫が動いたとき、同じターンなのに、その虫が2回動けてしまう。

その代わりに、まず1つの配列に全ての虫を集め、それから後にその虫たちを処理する。この虫を捕まえるメソッド、または`act`メソッドを持つ他のものは、虫をその現在の位置とともに1つのオブジェクトに格納する。：

```javascript
Terrarium.prototype.listActingCreatures = function() {
  var found = [];
  this.grid.each(function(point, value) {
    if (value != undefined && value.act)
      found.push({object: value, point: point});
  });
  return found;
};
```

* * *

### <a name="Ex8-5">[演習 8.5]</a>

虫にどう行動するか聞くとき、その周囲いついての情報のオブジェクトを渡さなければならない。このオブジェクトは先ほどの名前（`"n"`、`"ne"`、etc.）をプロパティ名に使う。それぞれのプロパティは、`characterFromElement`によって、その方向で虫が出会うことになるものを示す、1文字の文字列を持つ。

`listSurroundings`メソッドを`Terrarium`プロトタイプに追加せよ。虫が今いる場所のpointを引数として取り、その地点の周囲の情報のオブジェクトを返す。その地点がマス目の端だったときは、`"#"`をマス目の外側の方向として使い、虫がマス目から抜け出そうとしないようにせよ。

[ヒント] 全ての方向について書き出さず、`directions`辞書の`each`メソッドを使おう。

[解答を見る]

```javascript
Terrarium.prototype.listSurroundings = function(center) {
  var result = {};
  var grid = this.grid;
  directions.each(function(name, direction) {
    var place = center.add(direction);
    if (grid.isInside(place))
      result[name] = characterFromElement(grid.valueAt(place));
    else
      result[name] = "#";
  });
  return result;
};
```

`grid`変数を使うに当たっては`this`周りの問題に注意しよう。

* * *

上記の両方のメソッドはいずれも`Terrarium`オブジェクトの外部インターフェースではなく、内側の詳細だ。いくつかの言語では明示的に決まったメソッドとプロパティを'プライベートな'ものとし、オブジェクトの外側からの呼び出しをエラーにする。JavaScriptはそうではなく、オブジェクトのインターフェースの記述はコメントに頼ることになる。外側と内側のプロパティを識別するのに、例えば、全ての内部プロパティの接頭辞の下線(`'_'`)など、名前のスキーマを使うことはしばしば有益である。これにより、たまたまオブジェクトのインターフェースに含まれない部分をスポット的に使うことができる。

* * *

次はもう一つの内部メソッドで、虫たちの次のアクションを聞いて、それを実行する。引数として、`object`と`listActingCreatures`の返り値である`point`プロパティを持つオブジェクトを取る。今のところ、わかっているのは`"move"`アクションのみである。：

```javascript
Terrarium.prototype.processCreature = function(creature) {
  var surroundings = this.listSurroundings(creature.point);
  var action = creature.object.act(surroundings);
  if (action.type == "move" && directions.contains(action.direction)) {
    var to = creature.point.add(directions.lookup(action.direction));
    if (this.grid.isInside(to) && this.grid.valueAt(to) == undefined)
      this.grid.moveValue(creature.point, to);
  }
  else {
    throw new Error("Unsupported action: " + action.type);
  }
};
```

選んだ方向がマス目の中かつ空であることをチェックし、他は無視していることに注意。これにより、虫たちがどのようなアクションを取ろうとしても -- もしそれが実際に可能なときのみ実行される。これが虫と飼育器の間の絶縁体の層として振る舞い、虫の`act`メソッドを書くときに精密さを低くすることができる -- 例えば`StupidBug`（愚かな虫）は、その道に壁があろうと無かろうと常に南にしか進まない。

* * *

これら3つの内部メソッドにより、ついに`step`メソッドを書けるようになった。全ての虫に何か（`act`メソッドを持つ全ての要素 -- もし望むなら`wall`オブジェクトにそれを与えれば、歩く壁を作れる）する機会を与える。

```javascript
Terrarium.prototype.step = function() {
  forEach(this.listActingCreatures(),
          bind(this.processCreature, this));
};
```

今こそ、飼育器を造り、虫が動くところを見よう...

```javascript
var terrarium = new Terrarium(thePlan);
print(terrarium);
terrarium.step();
print(terrarium);
```

* * *

待った。上記の`print(terrarium)`の呼び出しと`toString`メソッドの出力の表示の終了をどのようにしようか？`print`はその引数を`String`関数を使って文字列に変える。オブジェクトはその`toString`メソッドを呼び出されることで文字列に変わるので、オブジェクトをプリントアウトするときに読みやすいようにするには、オブジェクト型に意味のある`toString`を与えるのが良い手である。

```javascript
Point.prototype.toString = function() {
  return "(" + this.x + "," + this.y + ")";
};
print(new Point(5, 5));
```

* * *

約束したように、`Terrarium`オブジェクトはシミュレーションの開始と終了の`start`と`stop`メソッドも持つ。このために、ブラウザが提供する、`setInterval`と`clearInterval`の2つの関数を使う。1つめの関数はその1つめの引数（関数、またはJavaScriptのコードを含む文字列）を定期的に実行する。2つめの引数には発動の間隔をミリ秒（1/1000秒）で与える。その効果を止めるために`clearInterval`に与えるための値が戻り値として返る。

```javascript
var annoy = setInterval(function() {print("What?");}, 400);
```

そして...

```javascript
clearInterval(annoy);
```

時間ベースのワンショットのアクションのために同様の関数がある。`setTimeout`は関数か文字列をミリ秒で与えられた時間の後で実行し、`clearTimeout`はそのアクションをキャンセルする。

* * *

```javascript
Terrarium.prototype.start = function() {
  if (!this.running)
    this.running = setInterval(bind(this.step, this), 500);
};

Terrarium.prototype.stop = function() {
  if (this.running) {
    clearInterval(this.running);
    this.running = null;
  }
};
```

* * *

今我々は単細胞な虫がいる飼育器を持っていて、それを実行することができる。しかし、起こっていることを見続けようにも、`print(Terrarium)`を繰り返さなければそれを見ることができない。これはとても実用的でない。自動的にプリントされるようにしたほうがいいだろう。もしそれもできるなら、1000の飼育器をプリントする代わりに、1つの飼育器のプリントアウトを更新できたほうがいい。2つめの問題には、このページが便利なことに`inPlacePrinter`という関数を提供している。それは`print`のような関数だが、出力を追加する代わりに、前回の出力を置き換える。

```javascript
var printHere = inPlacePrinter();
printHere("Now you see it.");
setTimeout(partial(printHere, "Now you don't."), 1000);
```

飼育器を変更毎に再プリントするため、`step`メソッドをこのように書き換えよう：

```javascript
Terrarium.prototype.step = function() {
  forEach(this.listActingCreatures(),
          bind(this.processCreature, this));
  if (this.onStep)
    this.onStep();
};
```

今、飼育器に追加した`onStep`プロパティは、ステップ毎に呼び出される。

```javascript
var terrarium = new Terrarium(thePlan);
terrarium.onStep = partial(inPlacePrinter(), terrarium);
terrarium.start();
```

`partial`の使用に注意 -- これは飼育器に適用するためのin-placeプリンターを作る。このプリンターは1つだけ引数を取り、それが部分的に適用されたら残りの引数はなく、引数がゼロの関数となる。まさにそうなることが`onStep`プロパティに必要だ。

面白いことが起こらなくなったら（それはあっという間だろう）飼育器を止めるのを忘れないように。コンピュータの資源を浪費することはない：

```javascript
terrarium.stop();
```

* * *

しかし1種類の虫、しかも単細胞な虫しかいない飼育器を欲しい人がいるだろうか？私は嫌だ。もし別の種類の虫を加えることができたら、それがいいだろう。幸運にも、やることは`elementFromCharacter`をより一般的にすることだけだ。今ここには直接タイプされ、または'ハードコード'された3つのケースが含まれている。：

```javascript
function elementFromCharacter(character) {
  if (character == " ")
    return undefined;
  else if (character == "#")
    return wall;
  else if (character == "o")
    return new StupidBug();
}
```

最初の2つはそのままにしておけるが、最後の1つは個別的すぎるやり方だ。よりよいアプローチは文字と対応するコンストラクターを辞書に格納することで、このようになる。：

```javascript
var creatureTypes = new Dictionary();
creatureTypes.register = function(constructor) {
  this.store(constructor.prototype.character, constructor);
};

function elementFromCharacter(character) {
  if (character == " ")
    return undefined;
  else if (character == "#")
    return wall;
  else if (creatureTypes.contains(character))
    return new (creatureTypes.lookup(character))();
  else
    throw new Error("Unknown character: " + character);
}
```

`creatureTypes`に追加された`register`メソッドに注意 -- これは辞書オブジェクトだが、しかし追加のメソッドをサポートしていけない理由はない。このメソッドはコンストラクターに結びついた文字を探し出し、辞書に格納する。そのプロトタイプが実際に`character`プロパティを持っているコンストラクターについてだけ呼び出すことができる。

`elementFromCharacter`は今、`creatureTypes`から与えられた文字を探しだし、未知の文字に遭えば例外を起こす。

* * *

これが新しい虫のタイプだ。そしてその文字を`creatureTypes`に登録するためにregisterを呼び出す。

```javascript
function BouncingBug() {
  this.direction = "ne";
}
BouncingBug.prototype.act = function(surroundings) {
  if (surroundings[this.direction] != " ")
    this.direction = (this.direction == "ne" ? "sw" : "ne");
  return {type: "move", direction: this.direction};
};
BouncingBug.prototype.character = "%";

creatureTypes.register(BouncingBug);
```

どんな虫か解釈できるだろうか？

* * *

### <a name="Ex8-6">[演習 8.6]</a>

壁があろうと無かろうと、毎ターンランダムな方向に進もうとする`DrunkBug`という虫の型を作れ。7章の`Math.random`のトリックを忘れないこと。

[解答を見る]

ランダムな方向を取るために、方向の名前の配列が必要になる。もちろん、`["n", "ne", ...]`とタイプするだけでも可能だが、それでは情報が重複し、重複した情報は我々をナーバスにする。配列を組み立てるために`direction`の`each`メソッドを使うこともでき、既にあるものよりその方がいいだろう。

しかし、明らかに一般性のある方法がここにある。プロパティ名のリストを辞書に得るというのは有益なツールに思えるので、`Dictionary`プロトタイプにそれを加えよう。

```javascript
Dictionary.prototype.names = function() {
  var names = [];
  this.each(function(name, value) {names.push(name);});
  return names;
};

show(directions.names());
```

本物の神経症のプログラマーは、辞書の中に格納された値のリストを返す、`values`メソッドも加えて、明示的に対称性を戻したいと思うだろう。しかし、[それが必要になるまで](http://www.c2.com/cgi/wiki?YouArentGonnaNeedIt)待とうというほうに賛成だ。

配列からランダムな要素を取り出す方法をここに示す。：

```javascript
function randomElement(array) {
  if (array.length == 0)
    throw new Error("The array is empty.");
  return array[Math.floor(Math.random() * array.length)];
}

show(randomElement(["heads", "tails"]));
```

そして虫そのもの。：

```javascript
function DrunkBug() {};
DrunkBug.prototype.act = function(surroundings) {
  return {type: "move",
          direction: randomElement(directions.names())};
};
DrunkBug.prototype.character = "~";

creatureTypes.register(DrunkBug);
```

* * *

さて、新しい虫をテストしよう。：

```javascript
var newPlan =
  ["############################",
   "#                      #####",
   "#    ##                 ####",
   "#   ####     ~ ~          ##",
   "#    ##       ~            #",
   "#                          #",
   "#                ###       #",
   "#               #####      #",
   "#                ###       #",
   "# %        ###        %    #",
   "#        #######           #",
   "############################"];

var terrarium = new Terrarium(newPlan);
terrarium.onStep = partial(inPlacePrinter(), terrarium);
terrarium.start();
```

跳ねる虫が酔っ払った奴にぶつかって跳ね返ったら？ホンモノのドラマだ。いずれにせよ、この魅惑的なショーを見終わったら、それを閉じよう。：

```javascript
terrarium.stop();
```

* * *

今の2種類のオブジェクトはともに1つの`act`メソッドと`character`プロパティを持っている。なぜなら、これらの特徴を共有することで、飼育器は同じ手段でそれらにアプローチできるからだ。これにより、飼育器についてのコードを全く変えることなしに、全ての種類の虫を持つことができる。このテクニックはポリモーフィズムとよばれ、ほぼ間違いなく、オブジェクト指向プログラミングの最もパワフルな面である。

ポリモーフィズムの基本的なアイデアは、コードの部分が決まったインターフェースを持つオブジェクトとともに動くよう書かれていれば、このインターフェースをサポートする任意の種類のオブジェクトがコードに接続でき、そしてそれがそのまま動くということだ。これの単純な例は、既に見てきたオブジェクトの`toString`メソッドである。意味のある`toString`メソッドを持つ全てのオブジェクトは`print`、および文字列に値を変換する必要のある他の関数に与えられることができ、そして文字列を組み立てるのに選ばれたそれらの`toString`メソッドがどのようなものであろうと、正しい文字列が作られる。

同様に、`forEach`も変数`arguments`の中に見つかった本物の配列と偽物の配列の両方で動き、なぜならそれが必要としているものは`length`プロパティと、配列の要素を示す、`0`、`1``とかそのような、プロパティで全部だからである。

* * *

飼育器の中の生命をより生命らしくするために、食物と繁殖の概念を追加しよう。飼育器の中のそれぞれの生物は、新しいプロパティ、`energy`を得て、それは行動することで減り、物を食べることで増える。energyが十分なとき、生物は繁殖する[^2]ことができ、同じ種類の新しい生物が生成される。

  [^2] 物事を合理的に単純に保つため、飼育器の中の生物は全て無性生殖であることとする。

もし、虫しかいなければ、移動によるエネルギーの消費と共食いにより、飼育器はエントロピーの力に屈してしまい、エネルギーは枯渇し、生命の無い荒れ地になるだろう。これの（少なくとも、早すぎる）発生を防ぐために、飼育器に地衣類を生やそう。地衣類は動かず、光合成でエネルギーを集め、繁殖する。

この働きをつくるために、異なる`processCreature`メソッドを持つ飼育器が必要だ。`Terrarium`プロトタイプのメソッドをただ置き換えることもできるが、しかし飛び跳ねる虫や依った虫のシミュレーションに合わせたいし、古い飼育器を壊すのは嫌だ。

私たちがやるのは、`Terrarium`プロトタイプをベースとしつつ異なる`processCreature`メソッドを持つ、`LifeLikeTerrarium`新しいコンストラクターを作ることだ。

* * *

これを行うには2つの方法がある。`Terratium.prototype`を一通り見て、1つ1つ`LifeLikeTerrarium.prototype`に加えることができる。これは簡単で、いくつかの場合には最も良い解決だ。しかし、この場合はより明らかな手段がある。もし古いprototypeオブジェクトを新しいprototypeオブジェクトのプロトタイプにしたら（あなたはここを2, 3度読み返す必要があるだろう）、それは自動的にその全てのプロパティを持つ。

残念ながら、JavaScriptは決まった他のオブジェクトをプロトタイプとして、オブジェクトを作るわかりやすい手段を持っていない。これを行う関数を書くのは可能であるが、それでも、以下のトリックが必要だ。：

```javascript
function clone(object) {
  function OneShotConstructor(){}
  OneShotConstructor.prototype = object;
  return new OneShotConstructor();
}
```

この関数は、与えられたオブジェクトをプロトタイプとする、空のワンショットのコンストラクターを使う。このコンストラクターで`new`を使ったとき、与えられたオブジェクトをもとにした新しいオブジェクトが作られる。

```javascript
function LifeLikeTerrarium(plan) {
  Terrarium.call(this, plan);
}
LifeLikeTerrarium.prototype = clone(Terrarium.prototype);
LifeLikeTerrarium.prototype.constructor = LifeLikeTerrarium;
```

新しいコンストラクターは古いものから異なることをする必要が無く、`this`オブジェクトの、古いものを呼ぶだけだ。新しいプロトタイプに`constructor`プロパティを戻すか、またはそのコンストラクターが`Terrarium`であると主張しなければならない（もちろん、このプロパティを使う時だけ実際に問題になり、我々はそうしない）。

* * *

今、LifeLikeTerrariumオブジェクトのメソッドのいくつかを置き換えたり、あるいは新しいものを加えることができる。新しいオブジェクトは古い物をベースにしているので、TerrariumとLifeLikeTerrariumで同じになるメソッドを再度書く手間は省ける。このテクニックは'継承（インヘリタンス）'と呼ばれる。新しい型は古い型のプロパティを継承する。多くの場合、これは新しい型は古い型のインターフェースをまだサポートした上で、古い型が持たないメソッドもサポートするということである。これにより、新しい型のオブジェクトは（ポリモーフィズム的に）古い型が使われていた全ての場所で使われることができる。

明示的にオブジェクト指向プログラミングをサポートしている、多くのプログラミング言語では、継承はとてもわかいやすいものだ。JavaScriptでは、言語は本当にそれを単純に行う手段を個別には持っていない。このため、JavaScriptプログラマーは'継承'のための多くの異なるアプローチを発明してきた。残念ながら、完璧なものはない。幸運にも、アプローチの範囲は幅広く、プログラマーは解決したい問題に最も適したもの選択でき、他の言語では全く不可能なトリックでさえ使える。

この章の終わりで、継承を行う他の手段と、それらの問題を見せよう。

これが新しい`processCreature`メソッドである。大きくなった。

```javascript
LifeLikeTerrarium.prototype.processCreature = function(creature) {
  var surroundings = this.listSurroundings(creature.point);
  var action = creature.object.act(surroundings);

  var target = undefined;
  var valueAtTarget = undefined;
  if (action.direction && directions.contains(action.direction)) {
    var direction = directions.lookup(action.direction);
    var maybe = creature.point.add(direction);
    if (this.grid.isInside(maybe)) {
      target = maybe;
      valueAtTarget = this.grid.valueAt(target);
    }
  }

  if (action.type == "move") {
    if (target && !valueAtTarget) {
      this.grid.moveValue(creature.point, target);
      creature.point = target;
      creature.object.energy -= 1;
    }
  }
  else if (action.type == "eat") {
    if (valueAtTarget && valueAtTarget.energy) {
      this.grid.setValueAt(target, undefined);
      creature.object.energy += valueAtTarget.energy;
    }
  }
  else if (action.type == "photosynthese") {
    creature.object.energy += 1;
  }
  else if (action.type == "reproduce") {
    if (target && !valueAtTarget) {
      var species = characterFromElement(creature.object);
      var baby = elementFromCharacter(species);
      creature.object.energy -= baby.energy * 2;
      if (creature.object.energy > 0)
        this.grid.setValueAt(target, baby);
    }
  }
  else if (action.type == "wait") {
    creature.object.energy -= 0.2;
  }
  else {
    throw new Error("Unsupported action: " + action.type);
  }

  if (creature.object.energy <= 0)
    this.grid.setValueAt(creature.point, undefined);
};
```

この関数はまだ生物に行動を聞くところから始まる。それから、もし行動が`direction`プロパティを持っていたら、この方向のポイントと現在いるところの値をマス目上のポイントで明示的に計算する。サポートされている行動5つのうち3つについてはこれを知る必要があり、もしそれら全てを分離して計算していたらもっとコードが汚くなっただろう。`direction`プロパティが無かったり、間違っていたら、`target`と`valueAtTarget`変数は未定義となる。

この後は、全ての行動に渡る。いくつかの行動は実行する前に追加のチェックを必要とし、これは別々の`if`で行われる。例えば、もし生物だったら、壁の中を進もうとしたりというように。我々は`"Unsupported action"`例外を生成しない。

`"reproduce"`アクションに注意、親の生物は新しい生物の得るエネルギーの2倍のエネルギーを失い（出産は楽じゃ無い）、新しい生物は親が十分なエネルギーを持っているときのみ生まれることができる。

行動が実行された後、生物がエネルギー切れになっていないかチェックし、もしそうであれば、それは死んだので、取り除く。

* * *

地衣類はあまり複雑な生命ではない。それを表現するのに"*"文字を使う。[演習8.6](#Ex8-6)の`randomElement`関数を確実に定義すること。

```javascript
function Lichen() {
  this.energy = 5;
}
Lichen.prototype.act = function(surroundings) {
  var emptySpace = findDirections(surroundings, " ");
  if (this.energy >= 13 && emptySpace.length > 0)
    return {type: "reproduce", direction: randomElement(emptySpace)};
  else if (this.energy < 20)
    return {type: "photosynthese"};
  else
    return {type: "wait"};
};
Lichen.prototype.character = "*";

creatureTypes.register(Lichen);

function findDirections(surroundings, wanted) {
  var found = [];
  directions.each(function(name) {
    if (surroundings[name] == wanted)
      found.push(name);
  });
  return found;
}
```

地衣類は20エネルギーより大きく成長することは無く、または他の地衣類に周りを囲まれ繁殖するスペースが無いときだけ**大きくなる**。

* * *

### <a name="Ex8-7">[演習 8.7]</a>

`LichenEater`（苔を食べる生物）を作れ。`10`のエネルギーから始めて、下記のように振る舞う。：

* 30以上のエネルギーがあり、周りに空いている場所があれば、繁殖する。
* そうでなければ、もし地衣類が近くにいれば、その中の1つをランダムに食べる。
* そうでなければ、動く場所があれば、ランダムに空いている隣の四角に移動する。
* そうでなければ、待つ。

`findDirections`と`randomElement`を周囲のチェックと方向の選択に使う。`"c"`（パックマン）をLichenEaterの文字とする、

[解答を見る]

```javascript
function LichenEater() {
  this.energy = 10;
}
LichenEater.prototype.act = function(surroundings) {
  var emptySpace = findDirections(surroundings, " ");
  var lichen = findDirections(surroundings, "*");

  if (this.energy >= 30 && emptySpace.length > 0)
    return {type: "reproduce", direction: randomElement(emptySpace)};
  else if (lichen.length > 0)
    return {type: "eat", direction: randomElement(lichen)};
  else if (emptySpace.length > 0)
    return {type: "move", direction: randomElement(emptySpace)};
  else
    return {type: "wait"};
};
LichenEater.prototype.character = "c";

creatureTypes.register(LichenEater);
```

* * *

試してみよう。

```javascript
var lichenPlan =
  ["############################",
   "#                     ######",
   "#    ***                **##",
   "#   *##**         **  c  *##",
   "#    ***     c    ##**    *#",
   "#       c         ##***   *#",
   "#                 ##**    *#",
   "#   c       #*            *#",
   "#*          #**       c   *#",
   "#***        ##**    c    **#",
   "#*****     ###***       *###",
   "############################"];

var terrarium = new LifeLikeTerrarium(lichenPlan);
terrarium.onStep = partial(inPlacePrinter(), terrarium);
terrarium.start();
```

おそらく、地衣類が飼育器の中の大きな部分を占めるように早く成長するのを見て、その後、大量の食料がLichenEaterを増やし、全ての地衣類を食べ尽くし、彼ら自身もいなくなる。ああ、自然の悲劇よ。

```javascript
terrarium.stop();
```

* * *

飼育器の居住者が2, 3分後に死滅するのでは気が滅入る。これに対処するために、LichenEaterに長期間持続可能な農業経営を教えよう。周りに最低2つの地衣類が無ければ、腹が減っていても食べないことにして、地衣類が根絶されないようにする。これにはしつけが必要だが、しかしその結果は自滅しない生活圏になるだろう。これが新しい`act`メソッドだ -- `lichen.length`が最低2あるときだけ食べるようにするところだけ変更した。

```javascript
LichenEater.prototype.act = function(surroundings) {
  var emptySpace = findDirections(surroundings, " ");
  var lichen = findDirections(surroundings, "*");

  if (this.energy >= 30 && emptySpace.length > 0)
    return {type: "reproduce", direction: randomElement(emptySpace)};
  else if (lichen.length > 1)
    return {type: "eat", direction: randomElement(lichen)};
  else if (emptySpace.length > 0)
    return {type: "move", direction: randomElement(emptySpace)};
  else
    return {type: "wait"};
};
```

上記の`lichenPlan`飼育器を再び動かし、どうなるか見よう。とても幸運なことがない限り、LichenEaterはしばらく後に絶滅してしまうだろう。なぜなら、多くの時間、飢えたまま、隅に生えている地衣類を見つける代わりに、空の空間を目的無く四方に這うだけになるからである。

* * *

### <a name="Ex8-8">[演習 8.8]</a>

`LichenEater`が生き残りやすくなるよう変更する手段を見つけよう。ズルはしないこと -- `this.energy += 100`はズルだ。もしコンストラクターを書き直すなら、`creatureType`辞書に再登録するのを忘れないようにするか、飼育器は古いコンストラクターのものを使い続けよう。

[解答を見る]

1つのアプローチは移動のランダムさを減らすことだ。常にランダムな方向をとり続けることで、しばしばどこにも向かわずに行ったり来たりすることになる。進んだ最後の方向を覚えておいて、その方向を好んで選ぶことにより、時間の浪費を減らし、早く食物を見つけられるようにしよう。

```javascript
function CleverLichenEater() {
  this.energy = 10;
  this.direction = "ne";
}
CleverLichenEater.prototype.act = function(surroundings) {
  var emptySpace = findDirections(surroundings, " ");
  var lichen = findDirections(surroundings, "*");

  if (this.energy >= 30 && emptySpace.length > 0) {
    return {type: "reproduce",
            direction: randomElement(emptySpace)};
  }
  else if (lichen.length > 1) {
    return {type: "eat",
            direction: randomElement(lichen)};
  }
  else if (emptySpace.length > 0) {
    if (surroundings[this.direction] != " ")
      this.direction = randomElement(emptySpace);
    return {type: "move",
            direction: this.direction};
  }
  else {
    return {type: "wait"};
  }
};
CleverLichenEater.prototype.character = "c";

creatureTypes.register(CleverLichenEater);
```

前回の飼育器プランを使って試してみよう。

* * *

### <a name="Ex8-9">[演習 8.9]</a>

1つにリンクした食物連鎖はまだちょっと初歩的だ。LichenEaterを食べて生きる`LichenEaterEater`（`"@"`文字）という新しい生物を書くことができるだろうか？早すぎる滅亡のないエコシステムに合うような手を見つけてみよう。`lichenPlan`配列をこれらを2, 3含むようにして、それを試そう。

あなた自身で書いてみて欲しい。私は、これらの生物が根絶したり、LichenEaterを食べ尽くして根絶させたりすることを防ぐ本当に良い手段を見つけるのに失敗した。食料が2つあるときだけ食べるというトリックはうまくいかなかった。なぜなら、食物の方も動くので2つが1つの場所で出会うことが希だったからである。LichenEaterEaterを太らせる（高いenergy）のは良さそうに思われ、LichenEaterが乏しいときは長く生き残り、繁殖だけが遅くなり、その食物が早く根絶することは防がれた。

地衣類と捕食者達は周期的な動きをするようになる -- 地衣類が豊作になると、捕食者達が大量に生まれ、それにより地衣類が少なくなり、捕食者達が餓死し、それにより地衣類が豊富になる、そのように続く。LichenEaterEaterが、2, 3ターン食物を見つけられないよう、冬眠するように作ることもできる（`"wait"`行動をしばらく続くようにして）。もしこの冬眠のターン数を正しく選ぶか、あるいはたくさんの食物の匂いをかぎつけたときに自動的に起きるようにしたら、これは良い戦略になるだろう。

* * *

飼育器に関する議論はここで結びとしよう。章の残りは継承と、JavaScriptにおける継承にまつわる問題をより深く見ることに費やそう。

* * *

最初に、いくつかの理論では、オブジェクト指向プログラミングの生徒はしばしば、継承の使い方の正しい、正しくないという議論に酷く退屈することがある。継承を覚えることは重要である、結局は、怠惰な[^3]プログラマーが書くコードを減らすためのただのトリックとして。すなわち、継承を正しく使えているかどうかという疑問は結局、結果のコードが正しく、無益な繰り返しを避けられているかということになる。それでも、これらの生徒が使っている原則は継承について考え始めるための良い手段を提供する。

  [^3] 怠惰はプログラマーにとって必ずしも悪ではない。せっせと同じ事を何度も繰り返す種類の人々は偉大な組み立てライン工やひどいプログラマーになる。

継承は、既存の型'スーパータイプ'をもとにした新しい型のオブジェクト'サブタイプ'を作ることである。サブタイプはスーパータイプの全てのプロパティをメソッドを初めから持ち、それらを継承し、それからこれらをいくつか変更したり、任意に新しい物を追加する。サブタイプによってモデル化されたものがスーパータイプのオブジェクト**である**ときに継承はよく使われる。

このように、`Piano`（ピアノ）型は`Instrument`（楽器）型のサブタイプであり得て、なぜならそれはピアノは楽器**である**からだ。ピアノが完全な鍵盤の配列を持っているからといって、`Piano`を配列のサブタイプにしようとするかもしれないが、ピアノは配列**ではなく**、そのように実装することは全ての種類の愚かさへの手綱に縛り付けるようなものだ。例えば、ピアノはペダルも持っている。なぜ`piano[0]`が最初の鍵盤であって、最初のペダルでは無いのか？この状況においては、もちろん、ピアノは鍵盤も**持っている**ので、`keys`プロパティと`pedals`プロパティの両方を配列として与えるほうがいいだろう。

あるサブタイプを他のサブタイプのスーパータイプとすることは可能だ。いくつかの問題が複雑な型の家系図を組み立てることによって最も良く解決される。それでも、継承の使いすぎには注意しよう。継承の過剰使用はプログラムを大きな醜い失敗作にするための大きな道だ。

* * *

`new`キーワードとコンストラクターの`prototype`プロパティの働きはオブジェクトの使用の確かな道を提案する。飼育器の生物のような単純なオブジェクトには、この方法がむしろいいだろう。残念ながら、継承の厳格な使用を始める時、オブジェクトへのこのアプローチは早く不細工になる。共通の操作の世話をするいくつかの関数を加えることが物事を少しスムーズにする。多くの人々が、例えば、オブジェクトに`inherit`と`method`メソッドを定義する。

```javascript
Object.prototype.inherit = function(baseConstructor) {
  this.prototype = clone(baseConstructor.prototype);
  this.prototype.constructor = this;
};
Object.prototype.method = function(name, func) {
  this.prototype[name] = func;
};

function StrangeArray(){}
StrangeArray.inherit(Array);
StrangeArray.method("push", function(value) {
  Array.prototype.push.call(this, value);
  Array.prototype.push.call(this, value);
});

var strange = new StrangeArray();
strange.push(4);
show(strange);
```

もし'JavaScript'と'inheritance'の語でウェブを検索しても、これの多くの異なるバリエーションにしか出会わないだろう。そのうちのいくつかは上記よりもっと複雑でできがいい。

ここで書かれている`push`メソッドは親の型のプロトタイプの`push`メソッドをどのように使っているかに注意しよう。これは継承を使う時に -- サブタイプの中で内部的にスーパータイプのメソッドを、なにも拡張すること無く使う時にしばしば使われる。

* * *

この基本的なアプローチの、大きな問題はコンストラクターとプロトタイプの間の二重性だ。コンストラクターはとても中心的な役割で、オブジェクト型それ自身の名前を与えられたものであり、そしてプロトタイプを得ることが必要なとき、コンストラクターに行きその`prototype`プロパティを得なければならない。

タイプする文字数が**多い**（`"prototype"`は9文字）だけではなく、混乱してもいる。上記の例では、空の、使い途の無いコンストラクターを`StrangeArray`のために書かなければならない。何度か、間違ってコンストラクターの代わりにそのプロトタイプにメソッドを追加したり、本当は`Array.prototype.slice`のつもりで`Array.slice`を呼び出そうとしたりしたことがある。私に関する限り、プロトタイプそれ自体がオブジェクト型の重要な面であり、コンストラクターはただその拡張であり、特別な種類のメソッドでしかない。

* * *

少数の単純なヘルパーメソッドを`Object.prototype`に加えることで、オブジェクトと継承に代わりのアプローチを作ることが可能になる。このアプローチでは、型はそのプロトタイプで表現され、それらのプロトタイプを格納するのに先頭を大文字にした変数を使うことになる。それが何か'constructing'の作業が必要なときは、`construct`というメソッドを実行する。`new`キーワードの代わりに使う`create`というメソッドを`Object`プロトタイプに加えよう。オブジェクトを複製し、そのようなメソッドがあれば、`create`に渡された引数をそれに与えて、その`construct`メソッドを呼び出す。

```javascript
Object.prototype.create = function() {
  var object = clone(this);
  if (typeof object.construct == "function")
    object.construct.apply(object, arguments);
  return object;
};
```

継承はプロトタイプ・オブジェクトの複製とそのプロパティのいくつかの追加またはで置き換えで行える。これの便利な短縮形、`extend`メソッドも提供する。これは適用されたオブジェクトを複製し、この複製に、引数として与えられたオブジェクトのプロパティを加える。

```javascript
Object.prototype.extend = function(properties) {
  var result = clone(this);
  forEachIn(properties, function(name, value) {
    result[name] = value;
  });
  return result;
};
```

`Object`プロトタイプを壊す危険がないとはいえない場合は、これらはもちろん通常の（メソッドで無い）関数として実装できる。

* * *

1つの例として、もしあなたがそれなりの歳なら、かつて1度は'テキスト・アドベンチャー'ゲームを遊んだことがあるかもしれない。バーチャルな世界をコマンドをタイプすることで動き回り、周囲の物事と行った行動について、テキストによる説明を得るのだ。そのようなゲームだ！

そんなゲームのアイテムのプロトタイプはこのように書く。

```javascript
var Item = {
  construct: function(name) {
    this.name = name;
  },
  inspect: function() {
    print("it is ", this.name, ".");
  },
  kick: function() {
    print("klunk!");
  },
  take: function() {
    print("you can not lift ", this.name, ".");
  }
};

var lantern = Item.create("the brass lantern");
lantern.kick();
```

これをこのように継承する...

```javascript
var DetailedItem = Item.extend({
  construct: function(name, details) {
    Item.construct.call(this, name);
    this.details = details;
  },
  inspect: function() {
    print("you see ", this.name, ", ", this.details, ".");
  }
});

var giantSloth = DetailedItem.create(
  "the giant sloth",
  "it is quietly hanging from a tree, munching leaves");
giantSloth.inspect();
```

強制的な`prototype`プロトタイプの部分から抜け出すのは、`DetailedItem`のコンストラクターから`Item.construct`を呼び出すような事をちょっと単純にする。`DetailedItem.construct`の中で`this.name = name`とするのはあまり良いアイデアでは無いことに注意しよう。これは行を重複させる。確かに、行を重複させると`Item.construct`関数を呼び出すより短くなる、しかし、もしこのコンストラクターに後から何か付け加えようとしたときに、2カ所でそれを加えなければならなくなる。

* * *

多くの場合、サブタイプのコンストラクターはスーパータイプのコンストラクターを呼び出すことから始まる。この手段により、スーパータイプとして正しい型のオブジェクトとして始めて、それから拡張することができる。このプロトタイプへの新しいアプローチでは、コンストラクターを必要としない型はそのままでよい。自動的にそのスーパータイプのコンストラクターを継承する。

```javascript
var SmallItem = Item.extend({
  kick: function() {
    print(this.name, " flies across the room.");
  },
  take: function() {
    // (imagine some code that moves the item to your pocket here)
    print("you take ", this.name, ".");
  }
});

var pencil = SmallItem.create("the red pencil");
pencil.take();
```

`SmallItem`はそれ自体のコンストラクターが定義されていないにもかかわらず、`name`引数が働くように作られ、なぜならそれは`Item`プロトタイプからコンストラクターを継承しているからである。

* * *

JavaScriptは`instanceof`という演算子を持ち、オブジェクトがどのプロトタイプをもとに作られたか求めることができる。左辺にオブジェクト、右辺にコンストラクターを与えれば、真偽値が返ってきて、`true`であればコンストラクターの`prototype`プロパティは直接あるいは間接的にそのオブジェクトのプロトタイプであり、`false`であればそうではない。

正規のコンストラクターを使わないとき、この演算子はむしろややこしい -- コンストラクター関数がその2つめの引数として期待されているが、しかし我々はプロトタイプしか持っていない。`clone`関数と同様のトリックをそのために使うことができる。：'でっち上げのコンストラクター'を使って、それに`instanceof`を適用しよう。

```javascript
Object.prototype.hasPrototype = function(prototype) {
  function DummyConstructor() {}
  DummyConstructor.prototype = prototype;
  return this instanceof DummyConstructor;
};

show(pencil.hasPrototype(Item));
show(pencil.hasPrototype(DetailedItem));
```

* * *

次に、細かい説明を持った小さなアイテムを作ることにしよう。それは`DetailedItem`と`SmallItem`の両方を継承したように見える。JavaScriptはオブジェクトが複数のプロトタイプを持つことを、実際にそうであろうと許していない、問題を解決するのは簡単ではない。例えば、`SmallItem`が、何らかの理由で、`inspect`メソッドも定義し、その`inspect`メソッドを新しいプロトタイプが使うとしたら？

複数の親の型からオブジェクト型を引き出すことを多重継承という。ある言語はそれに怯えて完全に禁止し、他のものは、よく定義され実用的な手段で、それが動くように複雑な計画を定義している。JavaScriptでまともな多重継承を実装することは可能だ。実際には、いつも通り、これに対する複数のアプローチが存在する。しかしこれら全てはここで論じるには複雑すぎる。代わりに、多くの場合に十分であろう、ごく単純なアプローチを見せよう。

* * *

mix-inは他のプロトタイプを混ぜ合わせることのできる特別な種類のプロトタイプだ。`SmallItem`はそのようなプロトタイプであるように見ることができる。その`kick`と`take`メソッドを他のプロトタイプにコピーすることで、小ささをこのプロトタイプに混ぜよう。

```javascript
function mixInto(object, mixIn) {
  forEachIn(mixIn, function(name, value) {
    object[name] = value;
  });
};

var SmallDetailedItem = clone(DetailedItem);
mixInto(SmallDetailedItem, SmallItem);

var deadMouse = SmallDetailedItem.create(
  "Fred the mouse",
  "he is dead");
deadMouse.inspect();
deadMouse.kick();
```

`forEachIn`はオブジェクト**それ自体**が持つプロパティだけに対して動き、`kick`と`take`だけがコピーされ、`SmallItem`が`Item`から継承したコンストラクターはコピーされないことを忘れないように。

* * *

プロトタイプの混ぜ合わせはmix-inがコンストラクターを持つとき、またはそのメソッドのあるものが混ぜ合わされるプロトタイプのメソッドを'壊す'ときはより複雑になる。時折、'手動で混ぜ合わせる'ことで動く。それ自体のコンストラクターを持つ、`Monster`プロトタイプを持っていて、それを`DetailedItem`に混ぜたいとする。

```javascript
var Monster = Item.extend({
  construct: function(name, dangerous) {
    Item.construct.call(this, name);
    this.dangerous = dangerous;
  },
  kick: function() {
    if (this.dangerous)
      print(this.name, " bites your head off.");
    else
      print(this.name, " runs away, weeping.");
  }
});

var DetailedMonster = DetailedItem.extend({
  construct: function(name, description, dangerous) {
    DetailedItem.construct.call(this, name, description);
    Monster.construct.call(this, name, dangerous);
  },
  kick: Monster.kick
});

var giantSloth = DetailedMonster.create(
  "the giant sloth",
  "it is quietly hanging from a tree, munching leaves",
  true);
giantSloth.kick();
```

しかし、これは、`DetailedMonster`を作るにあたり`Item`コンストラクターの2回の呼び出しをもたらすことに注意しよう -- 1回目は`DetailedItem`コンストラクター、もう1回は`Monster`コンストラクター。この場合はあまり害にはならないが、問題を引き起こすような状況もある。

* * *

しかし、そのような複雑さは、あなたに継承の使用を思いとどまらせるようなものではない。多重継承は、ある状況では大いに有用であるが、多くの場合においては無視するのが安全だ。Javaのような言語で多重継承を禁止しているのはこの理由による。そしてもし、いくつかの点で、それが本当に必要になったとき、ウェブを検索し、研究し、あなたの抱える状況で動くアプローチを考えだそう。

今考えているのは、JavaScriptはテキスト・アドベンチャーを組み立てるのにおそらくいい環境だろうということだ。プロトタイプの継承が我々にもたらした、オブジェクトの振る舞いを意のままに変更する能力は、これによく合致している。もし`hedgehog`のオブジェクトを持っていたら、その`kick`メソッドを変更するだけで、蹴られたら丸まるというユニークな癖をそれに持たせることができる。、

残念ながら、テキスト・アドベンチャーはビニール製のレコードとともに去ってしまい、一度はとても人気があったものの、現在では少数の[ファン](http://groups.google.com/group/rec.arts.int-fiction/topics)のみによってプレイされている。
