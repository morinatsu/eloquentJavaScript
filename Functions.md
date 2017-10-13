---
layout: default
title: 3. 関数
---

関数
=====================================================

プログラムはしばしば異なる場所で同じ事をすることを必要とする。必要な全ての文を毎回繰り返すのは退屈で失敗しがちだ。これらを1カ所にまとめておけて、必要なときにプログラムから呼び出せればもっと良いだろう。このために関数が発明された：これによりプログラムがひつようとするたびにコードを呼び出せるようになる。スクリーンに文字列を出力するのには少々の文しか必要としないが、しかし`print`関数があることで、我々は`print("Aleph")`と書くだけでよくなり、そのようにしている。

関数を単なるコードの塊と見るのは正しくない。必要なら、これらは純粋な関数、アルゴリズム、回り道、抽象、判断、加群、継続そしてデータ構築その他といった役割を果たす。関数を効果的に使えるようになるにはプログラミングのまじめな種類の技術が必要となる。この章はこの課題への導入を提供し、[6.関数型プログラミング](/Functional Programming)でより深いことを論じよう。

* * *

純粋な関数、あなたが人生のどこかで学んでいると期待して数学的クラスの関数から始めよう。コサインや数の絶対値といったものは1つの引数を取る純粋な関数である。加算は2つの引数を取る純粋な関数だ。

純粋な関数の定義は同じ引数が与えられたら常に同じ結果を返すことで、side effectを持たない。引数を取り、これらの引数を元に値を返し、それ以外の余計なことはしない。

JavaScriptにおいて、加算は演算子であるが、しかしこれに似たものを関数にラップすることができる。（無意味に見えるが、これは様々な状況で実に便利なのだ）:

```javascript
function add(a, b) {
  return a + b;
}

show(add(2, 2));
```

`add`はこの関数の名前である。`a`と`b`は2つの引数の名前である。`return a + b;`はこの関数の実体である。

`function`キーワードは新しい関数の作成で常に使われる。その後に変数名が続いたとき、この名前のもとに結果の関数が格納される。この名前の次には引数名のリストが続き、それから最後に関数の実体が続く。`while`ループや`if`文のものと異なり、関数の実体の周りの中括弧は必須である[^1]。  
  [^1] 技術的には、これは必要でない。しかし私はJavaScriptの設計者は関数本体が常に括弧を持つようにすれば物事がクリアになったと考える。

`return`キーワードには、式が続き、関数の戻り値を決める。制御が`return`文に来たら、現在の関数から抜け出して関数を呼び出したコードに値が返される。`return`文に式が無ければ関数は`undefined`を返す。

本体にはもちろん1つ以上の文を含めることができる。これは累乗を計算する関数である（正の整数の指数による）:

```javascript
function power(base, exponent) {
  var result = 1;
  for (var count = 0; count < exponent; count++)
    result *= base;
  return result;
}

show(power(2, 10));
```

[演習2.2](/Basic%20JavaScript.html#Ex2-2)を解いていれば、累乗の計算のためのこのテクニックは見慣れているだろう。

変数（`result`）を作り更新するのはside effectである。純粋な関数にはside effectはないと言わなかっただろうか？

関数の中で作られた変数は関数の中でしか存在しない。これは幸運なことに、プログラマーがプログラム全体を通じて必要なだけの全ての変数に違う名前を設定する必要は無い。`result`は`power`の中にしか存在しないので、この変更は関数から戻ってくるまでのものとなり、それを呼び出すコードの見通しにはside effectはない。

* * *

[演習 3.1](#Ex3-1)

引数として与えられた数値の絶対値を返す、`absolute`関数を書け。負の数の絶対値は同じ数値を正の数にしたもので、正の数（または0）の絶対値はその数そのものである。

[解答を見る]

```javascript
function absolute(number) {
  if (number < 0)
    return -number;
  else
    return number;
}

show(absolute(-144));
```

* * *

純粋な関数には2つのとてもいい特徴がある。考えるのに簡単で、再利用しやすいことだ。

関数が純粋なものなら、それを呼び出すときはそれ自身だけを見ればいい。正しく動いていないようであれば、コンソールから直接呼び出してテストすることができ、背景に依存しないために単純である [^2] 。テストを自動的に行うのも簡単だ―個々の関数をテストするプログラムを書くのだ。純粋で無い関数は全ての要素によって異なる値を返すかも知れず、テストするのは難しくしうるside effectを持つ。  
  [^2] 技術的には、純粋な関数は外部の変数の値を使うことができない。これらの値は変更されるかもしれず、それにより関数が同じ引数に対して異なる値を返すことになる。実用上は、プログラマーはある変数を'コンスタント'--それらは変更されないと期待される--と考えることができ、コンスタントしか使わない関数は純粋であると考える。関数の値を含む変数がしばしばコンスタント変数のいい例である。

純粋な関数は自己充足的であるため、純粋でないものより、便利で広い範囲の状況において適切だ。例として`show`を取り上げる。この関数の便利なところはプリント出力するスクリーンが特別な場所にあることによる。もしこのような場所になければ、この関数は不便だ。`format`という似たような関数のことを想像しよう、値を引数として取り、この値を返す表現する文字列を返す。この関数はより多くの状況で`show`より便利だ。

もちろん、`format`は`show`と同じ問題を解決するものではなく、純粋でない関数は問題を解くためにside effectを必要とする。多くの場合において、純粋でない関数はその必要に正確だ。そうでない場合には、問題は純粋な関数によって解かれ、純粋でないものはより便利で効果的な場合に使われるだろう。

だから、純粋な関数で表現することが簡単であるときは、そのやり方で書こう。しかし純粋でない関数を汚いもののように思わないこと。

* * *

side effectのある関数は`return`文を含まなくてもよい。もし、`return`文に出会わなければ、関数は`undefined`を返す。

```javascript
function yell(message) {
  alert(message + "!!");
}

yell("Yow");
```

* * *

関数の引数の名前はその中の変数と同じように使える。関数の引数の値はそれが呼び出されたときの値を参照し、関数の中で作られた通常の変数と同様に、関数の外には存在しない。トップレベルの環境の他に、より小さな、ローカルな環境が関数呼び出しによって作られる。

```javascript
function alertIsPrint(value) {
  var alert = print;
  alert(value);
}

alertIsPrint("Troglodites");
```

このローカルな環境の中の変数は関数の中のコードでしか見えない。もしこの関数が他の関数から呼び出されたら、新しく呼び出された関数は最初の関数の中の変数を見ない。:

```javascript
var variable = "top-level";

function printVariable() {
  print("inside printVariable, the variable holds '" +
        variable + "'.");
}

function test() {
  var variable = "local";
  print("inside test, the variable holds '" + variable + "'.");
  printVariable();
}

test();
```

しかしながら、これは巧妙だがとても便利な現象であり、関数が他の関数の**中で**定義された時、そのローカルな環境はトップレベルのものの代わりに、それを取り巻くローカルな環境がもとになっている。

```javascript
var variable = "top-level";
function parentFunction() {
  var variable = "local";
  function childFunction() {
    print(variable);
  }
  childFunction();
}
parentFunction();
```

これがもたらすのは、関数の中で見える変数はプログラムテキストの中で関数が書かれている場所によって決まるということだ。関数の定義の'上'で定義された全ての変数は見ることができ、これは関数の実体の中に入れられたものと、トップレベルのものと両方を意味する。この変数の参照へのアプローチを語彙的なスコーピングと呼ばれる。

* * *

他のプログラミング言語の経験がある人々はブロックの中のコード（中括弧の中）もまた新しくローカルな環境を作ることを期待するかもしれない。JavaScriptでは異なる。関数のみが新しいスコープを作る。このような自由なブロックを使うことは許されている...。

```javascript
var something = 1;
{
  var something = 2;
  print("Inside: " + something);
}
print("Outside: " + something);
```

…しかしブロックの中の`something`はブロックの外側のものと同じものを参照している。実際、このようなブロックは許されてはいるが、まったく使い途がない。多くの人々はJavaScriptの設計上の些細な失敗だと同意しており、ECMAScript Harmonyではブロックの中での変数定義を行う手段が追加される。（`let`キーワード）

* * *

これはあなたを驚かせるかもしれない:

```javascript
var variable = "top-level";
function parentFunction() {
  var variable = "local";
  function childFunction() {
    print(variable);
  }
  return childFunction;
}

var child = parentFunction();
child();
```

`paretntFunction`はその内部の関数を**返し**、下のコードがこの関数を呼び出す。`parentFunction`はここでは既に実行が終了しているにもかかわらず、`variable`が`"local"`の値を持っているローカル環境は未だ存在し、そして`childFunction`はそれを使うことができる。この現象はクロージャと呼ばれる。

* * *

プログラムのその部分を早く見るのがそれによって簡単になることを差し置いても、変数がプログラムテキストの形を見ることで、レキシカルスコーピングもまた関数の'同期'を許す。囲まれた関数の中の変数を使うことにより、内側の関数も異なる働きをする。少しだけ異なる同様な関数が必要なとき、引数に2を足す関数と5を足す関数が必要な時を想像しよう。

```javascript
function makeAddFunction(amount) {
  function add(number) {
    return number + amount;
  }
  return add;
}

var addTwo = makeAddFunction(2);
var addFive = makeAddFunction(5);
show(addTwo(1) + addFive(1));
```

* * *

異なる関数が同じ名前の変数をこんがらがることなしに含むことができるときという事実の上では、スコーピングのルールもまた実行の問題なしに呼び出されうる関数を許す。**それ自身**を呼び出す関数は再帰と呼ばれる。再帰は面白い定義を許す。`power`の以下の実装を見てみよう。:

```javascript
function power(base, exponent) {
  if (exponent == 0)
    return 1;
  else
    return base * power(base, exponent - 1);
}
```

これはむしろ数学者による定義の解説に近く、私にとっては今までのものよりとてもよく見える。ループを整理し、しかし`while`、`for`はなく、ローカルなside effectがあるようにも見えない。自分自身を呼び出すことで、この関数は同じ効果を作り出す。

1つ重要な問題がある：多くのブラウザで、この2つめのバージョンは最初のものより10倍近く遅い。JavaScriptでは、単純なループを走らせる方が、関数を何度も呼び出すよりとても安価なのだ。

* * *

スピード対エレガンスさのジレンマはとても面白いものの一つだ。forと再帰の判断に留まらない。多くの状況で、エレガントであること、直感的であること、そして時には短い解決が、入り組んだしかし速い解決に置き換えられうる。

`power`関数の場合はエレガントでないバージョンもまだとても単純で読みやすいものであった。再帰のバージョンでこれを入れ替えるのはセンスが良いとは言えない。しばしば、にもかかわらず、プログラムをまっすぐなものにするために、プログラムに複雑さをもたらす効率性をあきらめることは魅力的な選択になる。

多くのプログラマーによって繰り返され、完全に同意されてきた基本的なルールは、プログラムが遅すぎると証明されるまで効率のことは気にすべきでないということだ。その時、遅すぎると分った部分についてだけ、エレガンスさを効率性に置き換え始めれば良いのだ。

もちろん、上記のルールはパフォーマンスのことを全く無視して良いということを意味していない。多くの場合において、`power`関数のようには、'エレガントな'アプローチによっては十分な単純さが得られない。他の場合、経験のあるプログラマーは単純なアプローチは十分に速くならないと考えることもある。

このような大きな話をする理由は、驚くほど多くのプログラマーが細かいところにおいてもファンタジックに効率性にフォーカスしていることにある。その結果は大きく、複雑で、しばしば正しさに欠けるプログラムであり、よりまっすぐで等価値のものよりも書くのに長くかかり、かつしばしばほんの少ししか速くなっていない。

* * *

しかし再帰の話に戻ろう。再帰の概念はスタックというものに密接に関係している。関数が呼ばれたとき、制御は関数の実体に移る。実体が帰ってきたとき、関数を呼んだところから再開する。実体が実行されているとき、コンピューターはその関数が呼ばれたときの背景を記憶していなければならない、後でどこから続きを行うべきか知らなければならない。この背景を格納する場所がスタックである。

これがスタックと呼ばれるものが、見てきたように、関数の実体が関数を再度呼び出したときに実際にやっていることだ。関数が呼ばれる度に、別の背景が格納される。背景の積み重ねとしてこれを見ることができる。関数が呼ばれる度に、この重なりの一番上に現在の背景が投げ込まれる。関数から戻ったとき、重なりの一番上から背景が取られて再開が始まる。

このスタックを格納するためにコンピューターのメモリ上のスペースを必要とする。スタックは大きくなりすぎたとき、コンピュータは"スタックのスペースが尽きた"とか"再帰しすぎ"のようなメッセージとともにギブアップするだろう。

```javascript
function chicken() {
  return egg();
}
function egg() {
  return chicken();
}
print(chicken() + " came first.");
```

壊れたプログラムを書くとても面白い方法をこのデモンストレーションに付け加えてあり、この例では関数が直接自分自身を再帰的に呼び出すのではない。もしある関数が1つめの関数を再び呼ぶ他の関数を（直接にせよ間接にせよ）呼び出すようにしたら、それもまた再帰である。

* * *

再帰は常にループのより効率的でない代替であるというわけではない。幾つかの問題はループより再帰の方が簡単に解決できる。よくあるのは探索や幾つかに別れる'枝'を処理する必要のある問題であり、枝のそれぞれがまたさらに枝分かれしているというものである。

このようなパズルを考えてみよう：1という数から始めて5を足すか3をかけることを繰り返せば、無限に新しい値が作られ続ける。数値が与えられたときに、その数値を作るのにどのような順番で足し算とかけ算が行われたのか探してみる関数はどう書けば良いだろうか。

例えば、13という数は最初に1に3をかけ、5を2回足す。15という数には到達不可能である。

これが解答である:

```javascript
function findSequence(goal) {
  function find(start, history) {
    if (start == goal)
      return history;
    else if (start > goal)
      return null;
    else
      return find(start + 5, "(" + history + " + 5)") ||
             find(start * 3, "(" + history + " * 3)");
  }
  return find(1, "1");
}

print(findSequence(24));
```

もっとも短い演算を探す必要はなく、任意のものが見つかれば十分であることに注意。

`find`関数の中で、自分自身を2つのことなるやり方で呼び出し、5の加算と3のかけ算の両方の探索を行っている。もし数値が見つかれば、`history`の文字列を返し、それにはこの数値を選るまでに行った全ての演算が記録されている。現在の値が`goal`より大きいかのチェックも行われており、それは与えた数に至らない枝である場合に探索を打ち切るためにある。

この例における`||`演算子の用法は'`start`に5を加算する解決を探して返し、もしそれに失敗したら、`start`に3を積算する解決を探してそれを返す'である。このように、より自然言葉のように書くこともできる。

```javascript
    else {
      var found = find(start + 5, "(" + history + " + 5)");
      if (found == null)
        found = find(start * 3, history + " * 3");
      return found;
    }
```

* * *

関数定義がプログラムの残りの部分にあるにも係わらず、これらは同じタイムラインにはない:

```javascript
print("The future says: ", future());

function future() {
  return "We STILL have no flying cars.";
}
```

何が起こっているかというと、コンピューターが関数定義を探し、結びついた関数を格納するのは、プログラムの残りの部分の実行を開始する**前に**行われる。関数が他の関数の中で定義されているときも同じようなことが起こる。外側の関数が呼び出されたとき、最初に起こるのは全ての内側の関数が新しい環境の中に付け加えられることである。

* * *

関数の値を定義する別の方法は、他の値を作るときの方法にもっと似ている。`function`キーワードが式が必要な場所で使われたとき、それは関数の値を作り出す一つの式として扱われる。この方法で作られた関数は名前が与えられていなくてもよい（与えても良い）。

```javascript
var add = function(a, b) {
  return a + b;
};
show(add(5, 5));
```

`add`の定義の後のセミコロンに注意。通常の関数定義では必要ないが、この文は`var add = 22;`と同様の全体構造を持ち、セミコロンを必要とする。

この種類の関数値は名前を持たないため、無名関数と呼ばれる。先ほどの`makeAddFunction`のように関数名を与えても使い途がないこともある。:

```javascript
function makeAddFunction(amount) {
  return function (number) {
    return number + amount;
  };
}
```

最初のバージョンの`makeAddFunction`で`add`と名付けられた関数は一度しか参照されておらず、その名前はなんの目的も果たしていなかった。だから、その関数値を直接返すこともできた。

* * *

[演習 3.2](#Ex3-2)

1つの引数を取り、その引数より大きいことをテストする関数を返す`greaterThan`関数を書け。返された関数が1つ数値の引数として呼び出されたとき、それは真偽値を返す：与えられた数値がそのテスト関数が作られたときに使われた数値より大きければ`true`を、そうでなければ`false`を返す。

[解答を見る]

```javascript
function greaterThan(x) {
  return function(y) {
    return y > x;
  };
}

var greaterThanTen = greaterThan(10);
show(greaterThanTen(9));
```

* * *

下記を試してみよう。:

```javascript
alert("Hello", "Good Evening", "How do you do?", "Goodbye");
```

`alert`関数は公式には1つだけの引数を取る。このように呼び出されたとき、コンピュータは全く文句を言わずに、しかし他の引数をただ無視する。

```javascript
show();
```

見たとおり、少なすぎる引数を与えることもできる。引数が与えられなかったとき、関数の中でその値は`undefined`となる。

次の章では、関数の実体が渡された引数のリストをどのように得るのか正確に見てみよう。これは多くの数の引数を受け取ることのできる関数を作るときに便利だ。`print`はこのようにも使える:

```javascript
print("R", 2, "D", 2);
```

もちろん、`alert`のように、間違って期待されていたものと異なる誤った数の引数が関数に渡されるというよろしくない面もあるが、それについては語らない。
