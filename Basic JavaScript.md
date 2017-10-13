---
layout: default
title: 2. 基本のJavaScript
---

基本のJavaScript: 値、変数、および制御の流れ
=====================================================

コンピューターの世界の中にはただデータしか存在しない。データでないものは存在しない。全てのデータは本質的にはただのビット [^1] の連続であり、そうして本来的に等しく、全てのデータはそれ自身の役割を演じている。JavaScriptのシステムでは多くのデータは値と呼ばれるものにきちんと分割されている。全ての値は型を持ち、それはそれが演じる役割を現わしている。基本的な型は6つある：数値、文字列、真偽値、オブジェクト、関数そして未定義の値である。  
  [^1] ビットとは2種類の値を持つもので、通常`0`と`1`とで表される。コンピュータの中では、電圧の高低、信号の強弱といったような形をとり、CDの面においては反射の善し悪しという形を取る。

値を作るには、それにただ名前を与えるだけでよい。これはとても便利だ。値のためにその構成要素を集めたり、何かを支払ったりする必要はなく、名前を呼んで口笛を吹くだけで手に入れることができる。もちろん、空中から生じるのではない。全ての値はどこかに格納されている必要があり、もし巨大な数の値を使用することを望むならコンピューターのメモリからはみ出ることになる。幸運にも、これは全てを同時に必要とした場合の問題である。値を使わなくなったら、それは分散し、わずかなビットを残して消える。これらのビットは新しい値を生成するために再利用される。


* * *

数値(number)型の値は、あなたの想像通り、数値の値だ。数値が普通に書かれるとおりに書かれる:

```javascript
144
```
コンソールにこれを入力すると、同じものが出力ウインドウにプリントされる。打ち込んだテキストは数値の値になり、コンソールはこの数値を取ってスクリーンに書き込む。このような場合、むしろこれは無意味な演習であり、我々はすぐに、あまり直裁的でない方法で値を作るだろうが、作られたものをコンソールで試してみるには便利である。

144をビットで表現するとこうなる [^2] :  
  [^2] ここで、もし`10010000`であることを期待していたのであれば、いい読みである。が、そのまま読み続けよう。JavaScriptの数はただの整数として格納されるのではないのだ。

```javascript
0100000001100010000000000000000000000000000000000000000000000000
```

上記の数値は64ビットである。JavaScriptの数値は常にそうなる。これには重要な影響がある：異なる数を表現するのに限度があるということだ。3桁の10進数では、0から999までの数しか書けず、それは10^3 = 1000の異なる数値となる。64桁の2進数では、2^64の異なる数を書くことができ、これは多くの、10^19（1の後に0が19個）より大きい。

10^19以下の自然数が全てJavaScriptで扱えるわけではない。負の数値というものもあり、その符号を格納するためにビットの1つが使われる。自然数でない数値も表現しなければならないというより大きな問題もある。このために、11ビットが数値の中のピリオドの位置を格納するために使われる。

残る52ビット [^3] 、2^52より小さい自然数、10^15を超えたところ、これがJavaScriptが安全に扱える数値である。多くの場合、我々が使う数値はこれより小さく、ビットを全て使い切ることを考える必要はない。これは良いことだ。物事をなすためにひどく多くのビットを**必要とした**としても、個々のビットに対して何かすることはない。全ての可能性としても、大きなものを扱うのに好都合だ。  
  [^3] 本当だ。53であるのは1ビットを未使用にしているからだ。詳細が気になるなら'IEEE 754'フォーマットを検索しよう。

小数を書くにはドットを使う。:

```javascript
9.81
```

とても大きいまたはとても小さい数には、`e`に続けて指数となる数値を加えることで'指数表現'を使うこともできる。:

```javascript
2.998e8
```

これは2.998 * 10^8 = 299800000である。

自然数（整数とも呼ばれる）の計算は52ビットまで正確に行えることが保証されている。不運にも、小数の計算は全般的には異なる。π（円周率）のような数は10進数で正確に表現することができず、多くの数は64ビットだけで格納しようとすると若干正確さが失われる。これは残念なことだが、とても特別な状況でしか実用上の問題とはならない。正確な値ではなく近似値を使うことで小数を扱うことができるようになるということが重要だ。

* * *

数値の主な使い途は計算だ。足し算やかけ算のような計算操作は2つの数値の値から新しい数値を作る。JavaScriptではこのようになる:

```javascript
100 + 4 * 11
```

`+`と`*`記号は演算子と呼ばれる。一つめは足し算、2つめはかけ算だ。2つの値の間に演算子を入れれば、それらの値に適用され、新しい値が作られる。

この例は'4と100を足して、その結果に11をかける'か、あるいはかけ算が足し算より先だろうか？お察しの通り、数学でのように、かけ算が最初である。しかし、追加の括弧で挟むことでこれを変更することができる。:

```javascript
(100 + 4) * 11
```

引き算には`-`演算子があり、そして割り算は`/`でできる。括弧無しで演算子が現われたとき、演算子の優先順位に従ってそれが適用される。最初の例ではかけ算が足し算より高い優先順位を持っていることが分った。割り算とかけ算は常に引き算や足し算より優先される。互いに同じ優先順位の演算子が複数あるとき(`1 - 1 + 1`)は左から右に適用される。

どんな値ができるか計算し、それから実行してあなたの計算が正しかったか確認してみてほしい。:

```javascript
115 * 4 - 4 + 88 / 2
```

優先順位のルールがあなたの考えたものと違っていたと嘆かなくてもいい。疑わしいときは括弧を追加するだけだ。

もう一つ数学的な演算子があり、それはおそらくあなたがよく知らないものだ。`%`記号はmodulo計算をするときに使われる。`X`modulo`Y`は`X`を`Y`で割ったときの余りである。例えば`314 % 100`は`14` 、そして`10 % 3` は`1`、`144 % 12`は`0`だ。moduloはかけ算や割り算と同じ優先順位を持つ。

* * *

次のデータ型は文字列(string)だ。その名前から想像される使い途は数値のそれより明白だとは言えないが、これもまたとても基本的な役割である。文字列はテキストを表現するのに使われ、その名前はおそらく、それが文字の束を繋いだものだという事実から来ている。文字列は引用符でその内容を囲んで書かれる。:

```javascript
"Patch my boat with chewing gum."
```

二重引用符の間はほとんど何でも良く、JavaScriptはそれを外して文字列の値を作る。しかし、トリッキーな文字が若干ある。引用符の中に引用符をどう入れるか想像するのは難しいだろう。新しい行、エンターキーを押したときのもの、あれを引用符の中に入れつつ、文字列は単一行のままにすることはできないのだろうか。

そのような文字を文字列に入れるには、以下のトリックを使う：引用されたテキストの中にバックスラッシュ('`\`')があれば、それは続くキャラクターが特別な意味を持つことを示す。バックスラッシュに続く引用符は文字列の終わりではなく、その一部となる。バックスラッシュの次に'`n`'があれば、それは新しい行と解釈される。同様に、バックスラッシュの次の'`t`'は4文字文のタブ [^4] を意味する。:  
  [#f4] 文字列の値をコンソールにタイプする時、引用符とバックスラッシュをタイプして戻るように注意しよう。特殊文字を明示的に表すため、\ ``print ('a\nb')``\ とすることもできる。―なぜこれが動くのか、後で説明しよう。

```javascript
"This is the first line\nAnd this is the second"
```

バックスラッシュを文字列の中で特殊なコードではなく、ただバックスラッシュとしたい状況ももちろんあるだろう。もし2つのバックスラッシュが互いに重なれば、互いの右を折りたたみ、文字列としての結果には一つだけが残る。:

```javascript
"A newline character is written like \"\\n\"."
```

文字列は割り算したり、かけ算したり、引き算したりできない。`+`演算子だけは使うことが**できる**。加算ではなく、連結になる。2つの文字列を互いに結びつける。:

```javascript
"con" + "cat" + "e" + "nate"
```

文字列を操作する方法はもっとあるが、それは後ほど。

* * *

全ての演算子が記号というわけではなく、単語として書かれるものもある。例えば、`typeof`演算子、これは値の持つ型の名前を文字列の値を作る。:

```javascript
typeof 4.5
```

我々が見てきた他の演算子は2つの値を演算するが、`typeof`は1つだけだ。2つの値を使う演算子は2項演算子と呼ばれ、1つのものは単項演算子と呼ばれる。マイナス演算子は二項演算子としても単項演算子としても使われる。:

```javascript
- (10 - 2)
```

* * *

それから真偽値(boolean)型の値。これは2つしかない:`true`と`false`だ。`true`の値の作りかたの1つはこう。:

```javascript
3 > 2
````

そして`false`はこのように。:

```javascript
3 < 2
````

あなたが`>`と`<`の記号を既に知っていることを期待している。これはそれぞれ'より大きい'と'より小さい'という意味だ。これらは二項演算子で、その結果は式を適用した結果の真偽値を持つ。

文字列も同様に比較できる。:

```javascript
"Aardvark" < "Zoroaster"
```

文字列はアルファベット順の大小で比較される。大小…大文字は常に小文字より小さく、`"Z" < "a"`（大文字のZは小文字のaより小さい）は`true`となり、アルファベットでない文字（'`!`', '`@`', etc）も順番に含まれる。この比較は実際にはユニコード標準をベースに行われる。この標準は必要とされた全ての文字に仮想の数値を割り当て、ギリシャ文字、アラビア文字、日本語、タミル文字などなど全ての文字が含まれる。このように数値を持つのはコンピュータに文字列を格納するのには実際的で―それらを数値のリストのように扱うことができる。文字列を比較するとき、JavaScriptは文字列の中の文字の数値を左から右へと比較するだけである。

他に同様の演算子`>=`（'大なりイコール'）、`<=`（'小なりイコール'）、`==`（'等しい'）、そして`!=`（'等しくない'）がある。:

```javascript
"Itchy" != "Scratchy"
```

```javascript
5e2 == 500
```

* * *

真偽値それ自体に適用することもできる便利なものもある。JavaScriptは3つの論理演算子をサポートしている：**論理積(and)** 、**論理和(or)** 、そして**否定(not)**。これらは真偽の'判断'に使うことができる。

`&&`演算子は**論理積**を示す。これは二項演算子で、両方の値がともに`true`のときだけ`true`となる。:

```javascript
true && false
```

`||`は**論理和**で、与えられた値のどちらかが`true`であれば`true`となる。:

```javascript
true || false
```

否定は`!`、感嘆符として書かれ、与えられた値を反転させる単項演算子であり、`!true`は`false`、そして`!false`は`true`となる。

* * *

[演習 2.1](#Ex2-1)

```javascript
((4 >= 6) || ("grass" != "green")) &&
    !(((12 * 2) == 144) && true)
```

これは真であろうか？読みやすくするため、不要な括弧も書いてある。下の単純な版も同じ意味だ。:

```javascript
(4 >= 6 || "grass" != "green") &&
   !(12 * 2 == 144 && true)
```

[解答を見る]

これは真である。このように1つ1つ確かめることができる。:

```javascript
(false || true) && !(false && true)
true && !false
true
```

`"grass" != "green"`であるということに気づいてくれることを期待する。grass（草）はgreen（緑色）かもしれないが、それは緑色と等しいということではない。

常に括弧が必要というわけではない。実践では、我々がこれまで見てきた演算子、`||`は`&&`より低い優先順位を持ち、それから比較演算子（`>`、`==`、他）はその後に残るといった知識は普通のこととなる。どのようなやり方を選ぼうと、単純なケースでは可能な限り括弧を少なくすることが必要だ。

* * *

全ての例で電卓のように言語を使うことができるだろう。値を設定し演算子を適用して新たな値を作る。このような値の作成は全てのJavaScriptの本質の1つであるが、ただ1つに留まるものでもない。値を作成するコードは式と呼ばれる。全ての値はそれが（`22`や`"psycoanalysis"`のように）直接書かれたものであっても1つの式である。括弧で囲われた1つの式もまた1つの式であり、2つの式に適用された2項演算子、1つの式に適用された単項演算子もまた1つの式である。

式を作る方法はもう2,3あるが、それは時が来たら見せよう。

式より大きな単位もある。それは文と呼ばれる、プログラムは文のリストとして作られる。多くの文はセミコロン(`;`)で終了する。最も単純な文は式の後にセミコロンを付けたものである。こんなものもプログラムだ。:

```javascript
1;
!false;
```

これは使い途のないプログラムだ。式ができるのは値を作ることだけだが、文は世界になにがしかの変化をもたらす。スクリーンに何かをプリントし―世界を変えた分だけカウントが変わる―またはプログラムの内部的な状態が文の後に影響を受けて変わるかもしれない。この変化は'side effect'と呼ばれる。例の文は`1`および`true`という値を作り、それをビットのバケツ [^5] の中に投げ込む。これは世界に何の印象も残さないので、side effectではない。  
  [^5] ビットのバケツはおそらく古いビットが溜まっている場所になっている。あるシステムでは、プログラマは手作業でそれを空にしなければならなかった。幸運にも、JavaScriptは全自動でビットをリサイクルするシステムを備えている。

* * *

プログラムが内部的な状態を持つとはどういうことだろうか？何かを覚えているのだろうか？我々は古い値から新しい値の作り方を見てきたが、それは古い値に何の変化も与えず、新しい値はその場で使われるかそのまま消えるかのどちらかであった。新しい値を捉え捕まえておくために、JavaScriptは変数と呼ばれるものを提供する。:

```javascript
var caught = 5 * 5;
```

変数は常に名前を持ち、それがその保持する値を示す。上記の文は`caught`という変数を作り、それを`5`かける`5`のかけ算によってできた数を保持しておくことに使う。

上記のプログラムの実行後、`caught`という語をコンソールにタイプし、`25`という値を取り出すことができる。変数の名前はその値を取り出すのに使われる。`caught + 1`もまた動作する。変数の名前は1つの式として使うことができ、それをより大きな式の一部とすることができる。

`var`という語は新しい変数を作るのに使われる。`var`の後、変数の名前が続く。変数名がほとんど全ての語を使えるが、中に空白を含めてはいけない。数字は中に入れても良く、`catch22`は正しい名前だが、数字を名前の最初に持ってくることはできない。'`$`'と'`_`'という文字は名前に使用できるので、`$_$`も正しい変数名である。

しばしばあるケースだが、もし新しい変数に値を入れたければ、`=`演算子を使うことで式の値を変数に与えることができる。

変数が値を示すとき、それは、それらの結びつきが永遠のものであるということを意味しない。いつでも、`=`演算子を使って既にある値を取り去って新しいものに差し替えることができる。:

```javascript
caught = 4 * 4;
```

* * *

変数に箱よりむしろタコの触手のようなイメージを持つかもしれない。それらは値を`容れる`のではなく、`捕まえる`のだ--2つの変数は同じ値を指すことができる。プログラムが保持する値はただアクセスできるだけだ。もし何かを覚えておく必要があるとき、新しい値を保持する触手を育て、あるいは既にある触手に再度捕まえさせる：ルイージに貸してる金を覚えておくには、こんなことができる...。:

```javascript
var luigiDebt = 140;
```

その後、ルイージがいくらか支払ってくる度に、変数に新しい数値を与えていくことによって負債を減らしていく。:

```javascript
luigiDebt = luigiDebt - 35;
```

一度に与えられた変数のコレクションとそれらの変数を環境と呼ぶ。プログラムがスタートしたとき、その環境は空ではない。幾つかの標準変数が常に含まれている。ブラウザがページをロードするとき、新しい環境が作りこれらの標準変数に触れる。そのページのプログラムにより作られ更新された変数はブラウザが新しいページに行くまで生き残る。

* * *

標準環境により'関数'型の多くの変数が提供される。関数は1つの値に包まれたプログラムの一片である。全般的に、便利な何らかのプログラムの一片は、その関数を含む値という形で使われる。ブラウザ環境では、`alert`変数が短いメッセージをダイアログウインドウに表示する関数を保持している。:

```javascript
alert("Also, your hair is on fire.");
```

関数の中のコードを実行することは呼び出しあるいは適用と呼ばれる。それを示すには括弧を使う。括弧の後に続く全ての式が関数の値を作るときに使われる。括弧の中の文字列がこの関数に与えられたら、それはダイアログウインドウの中に表示するテキストとして使われる。関数に与えられる値はパラメーターまたは引数と呼ばれる。`alert`が必要とするは1つだが、他の関数は異なる数だけ必要とすることもある。

* * *

ダイアログウインドウを表示するのはside effectである。多くの関数が便利なのはそれらの作るside effectによる。値を作るための関数というのも可能であり、この場合は便利なside effectを持つ必要はない。例えば、`Math.max`関数というものがあり、これは2つの値のうち大きい方を返す。:

```javascript
alert(Math.max(2, 4));
```

関数が値を作るとき、それを返却するという。なぜならJavaScriptでは作られた値は常に式だからであり、関数呼び出しはより大きな式の一部として使われうるからである。:

```javascript
alert(Math.min(2, 4) + 100);
```

[3. 関数](/Functions.html)であなた自身の関数を書く方法を議論する。

* * *

先の例で見たように、`alert`は式の結果を表示するのに便利だ。クリックして小さなウインドウは消してしまい、これからは`print`という同じような関数を使うことにしよう。これはウィンドウをポップアップさせず、コンソールの出力エリアに値を書き出す。`print`は標準JavaScript関数ではないので、ブラウザはそれを提供しないが、本書ではページの中で使えるようにしてあるので、これらのページの中で使用することはできる。:

```javascript
print("N");
```

これらのページで提供している同様の関数で、`show`というものもある。`print`はその引数を平坦なテキストとして表示するが、`show`はプログラムの中と同じやりかたで表示しようとし、値の型に関するより詳しい情報を与えてくれる。例えば、引用符の中の文字列型の値を`show`に与える。:

```javascript
show("N");
```

ブラウザから提供される標準環境はウインドウをポップアップするもう2,3の関数を提供する。ユーザーに確認をとるためにOK/Cancelを`confirm`を使って質問することができる。これは真偽値を返却し、'OK'が押されたら`true`、'Cancel'が押された`false`となる。

```javascript
show(confirm("Shall we, then?"));
```

`prompt`は'open'な質問をするときに使える。最初の引数が質問で、2つめがユーザーがそこから始めるテキストである。1行のテキストがウインドウにタイプされたら、関数はこれを文字列として返す。:

```javascript
show(prompt("Tell us everything you know.", "..."));
```

* * *

環境のほとんど全ての変数に新しい値を与えることも可能だ。これはとても便利だが、危険でもある。もし`8`という値を`print`に与えたら、それ以降何もプリントすることができなくなる。幸運なことに、コンソールには'Reset'という大きなボタンがあり、それで環境を元の状態にリセットすることができる。

* * *

1行のプログラムはあまり面白くない。より多くの文をプログラムに入れたら、それらの文は、お察しの通り上から下まで一気に実行される。

```javascript
var theNumber = Number(prompt("Pick a number", ""));
print("Your number is the square root of " +
  (theNumber * theNumber));
```

`Number`関数は値を数値に変換し、それは`prompt`の結果が文字列であるために必要となる。`String`や`Boolean`といった同様の関数がありそれらはそれぞれの型に値を変換する。

* * *

0から12までの偶数をプリントするプログラムを考える。1つのやり方としてはこのように書ける。:

```javascript
print(0);
print(2);
print(4);
print(6);
print(8);
print(10);
print(12);
```

これは動くが、プログラムを書くためのアイデアとして**よい仕事とは言えない**。もし1000以下の偶数全てが必要となったとき、上のやりかたではとても動かない。コードを自動的に繰り返してくれる方法が必要だ。

```javascript
var currentNumber = 0;
while (currentNumber <= 12) {
  print(currentNumber);
  currentNumber = currentNumber + 2;
}
```

おそらく`while`を[1.導入](/Introduction.html)の章で見ているだろう。`while`という語から始まる文はループを作る。ループは文の順列を実行し続け、複数回その文を繰り返す。この場合、`while`に続く括弧の中の式（括弧はここになければならない）、これはループがそのままループし続けるか終了するかの判定に使われる。この式が作る真偽値が`true`である間、ループの中のコードが繰り返され、`false`になったらすぐに、プログラムはループの下に行き通常のようにに実行を続ける。

`currentNumber`変数は変数によってプログラムの進行を追うことができることを見せてくれる。ループが繰り返される度に、`2`つずつ加算され、繰り返しの最初で、ループを続けるかどうかの判断のために`12`という数値と比較される。

`while`の3つめの部分は他の文である。これはループの本体であり、複数回繰り返される1つないし複数の動作である。もし数をプリントする必要がなければ、プログラムはこのようになっていた。:

```javascript
var currentNumber = 0;
while (currentNumber <= 12)
  currentNumber = currentNumber + 2;
```

ここでは、`currentNumber = currentNumber + 2;`がループの本体の形を取る文である。我々は数をプリントしなければならないので、ループの文は1行より多くなった。中括弧（`{`と`}`）がブロックの中に文のグループを入れるために使われる。ブロックの外側の世界にとっては、ブロックは単一の行として数えられる。先ほどの例では、ループに`print`の呼び出しと`currentNumber`の更新の両方を含めるためにこれが使われた。

[演習 2.2](#Ex2-2)

このテクニックを使って、`2^10`（2の10乗）を計算するプログラムを書け。当然、`2 * 2 * ...`みたいな安っぽいトリックを使って書いてはいけない。

もしこれでトラブルにあったら、偶数の例を見てみること。プログラムは複数回動作を繰り返さなければならない。カウンター変数が`while`のループとともに使われていた。カウンターをプリントする代わりに、このプログラムは何かを2倍しなければならない。この何かは結果を作り上げるのに使う別の変数になるだろう。

まだうまく動かなくても嘆かなくていい。もしこの章がカバーするテクニックを完璧に理解できていれば、難しいのはそれを個々のプログラムに適用することだけだ。コードの読み書きは感覚を育てる助けになり、解答を学習して、次の演習に挑もう。

[解答を見る]

```javascript
var result = 1;
var counter = 0;
while (counter < 10) {
  result = result * 2;
  counter = counter + 1;
}
show(result);
```

`counter`は`1`から始まり`<= 10`とチェックされるが、しかし、後に述べる理由により、0から始めるよりいいアイデアである。

当然、あなた自身の解答が私の示すものと全く同じである必要はない。それが動くなら。そしてもし全然違っていれば、わたしの解答も確実に理解すること。

* * *

[演習 2.3](#Ex2-3)

ちょっとだけ変更して、先ほどの演習の解答から三角形を書くプログラムを作ろう。私が言う'三角形を書く'というのは、ぱっと見三角形に見えるものをプリントアウトするという意味だ。

10行プリントアウトする。1行めでは'`#`'文字が1つ。2行目では2つ。そのように。

どうしたらX個の'`#`'を文字列として得られるだろうか？1つめの手では毎回文字列を作るために'inner loop'―ループの中のループ必要とする。もっと簡単な手はループの前回の繰り返しで使った値を再利用し、それに1つ文字をくっつけるというものだ。

[解答を見る]

```javascript
var line = "";
var counter = 0;
while (counter < 10) {
  line = line + "#";
  print(line);
  counter = counter + 1;
}
```

* * *

私が幾つかの文の前につけた空白に気づいただろう。これらは必要なものではない：コンピュータはそれらがなくてもプログラムを受け付ける。実際、プログラムの行毎の改行もオプショナルなものだ。もしそういうのが好みなら全てを単一の長い行に書くこともできた。ブロックの中のインデントの役割はコードの構造を読み手にとって明らかにすることだ。新しいブロックは他の中のブロックで始めることができるため、どこでブロックが始まるのか複雑なコードの中では読み取りにくくなるかもしれないのだ。行がインデントされていれば、プログラムのみかけがそのなかのブロックの形と同様になる。私はブロックを始める度に2つ空白を空けるのが好みだが、違う人もいる。

Opera以外のブラウザでは、プログラムをタイプするコンソールのフィールドには自動的に空白が追加されるだろう。最初は戸惑うかもしれないが、たくさんコードを書く時には大きな時間短縮になる。タブキーを押すと今カーソルがある場所から再度インデントするだろう。

いくつかの場合において、JavaScriptは文の終わりのセミコロンの省略を許す。他の場合は、何かおかしな事が起こる。安全に省略できるというルールは複雑で奇異である。本書では、私はセミコロンを付け忘れたくないし、あなた自身のプログラムにおいてもそうすべきと強くおすすめする。

* * *

`while`を使うにあたり同じようなパターンを見てきた。1つめは'counter'変数が作られるということだ。この変数はループの進行を追跡する。`while`自身に、通常は何らかの境界にまだ届いてないかのチェックが含まれている。それから、ループの本体の終わりで、カウンターが更新される。

多くのループがこのパターンになる。この理由で、JavaScriptや同様の言語はもっと短一括的な書き方も提供している。:

```javascript
for (var number = 0; number <= 12; number = number + 2)
  show(number);
```

このプログラムは正確にさきほどの偶数をプリントするサンプルと同じである。違いはループの状態に関する全ての文が今や1行になっていることだけだ。`for`の後の括弧には2つのセミコロンが含まれる。1つめのセミコロンの前の部分はループの`初期化`が、通常は変数を定義することで行われる。2つめの部分はループを継続すべきかどうかの条件を**チェック**する式である。最後の部分はループの状態を**更新**する。多くの場合これで`while`構造が短くクリアになる。

* * *

私は幾つかの変数名で大文字を使ってきた。変数名には空白を入れられないからである―コンピュータはそれらを2つに別れた変数として読むかもしれない―複数の語からなる名前の作り方については多かれ少なかれ次のように限定され選択となるだろう：`fuzzylittleturtle`、`fuzzy_little_turtle`、`FuzzyLittleTurtle`、または`fuzzyLittleTurtle`。最初のものは読みにくい。個人的に、私はアンダースコアを付けたものが好きだ、タイプするにはちょっと困るけど。しかしながら、標準JavaScript関数では、そして多くのJavaScriptプログラマーが最後のものに従っている。このようにするのは難しくない、大勢に従って1つめより後の語の先頭の文字を大文字にするだけだ。

わずかなケースでは、例えば`Number`関数のような、変数名の最初の文字もまた大文字にされている。これはこの関数がコンストラクタであることを示す。コンストラクタとは[8.オブジェクト志向プログラミング](/Object-oriented Programming.html)において明らかにする。今は、協調性を欠くことで失敗しないことが重要だ。

特別な意味を持つ名前も注記しよう、`var`、`while`そして`for`のようなものは変数名として使ってはいけない。これらはキーワードと呼ばれる。JavaScriptの将来のバージョンで'予約語'とされている幾つかの語もある。これらは公式に変数名として使うことが許されていないが、幾つかのブラウザでは許されてしまっている。全体のリストは長い:

```javascript
abstract boolean break byte case catch char class const continue
debugger default delete do double else enum export extends false
final finally float for function goto if implements import in
instanceof int interface long native new null package private
protected public return short static super switch synchronized
this throw throws transient true try typeof var void volatile
while with
```

今覚えきれないと嘆くことはないが、これによって何かが期待通りに動かない問題が起こるかもしれないということは覚えておこう。私の経験では、`char`（1文字の文字列を格納する）と`class`がもっとも誤って使われがちな一般名だ。

* * *

[演習 2.4](#Ex2-4)

先の2つの演習の解答を`while`の代わりに`for`を使って書き直せ。

[解答を見る]

```javascript
var result = 1;
for (var counter = 0; counter < 10; counter = counter + 1)
  result = result * 2;
show(result);
```

'`{`'から始まるブロックがなくても、上の行のループに含まれることを明示するため、ループの中の文に2つの空白のインデントを行うことを忘れずに。

```javascript
var line = "";
for (var counter = 0; counter < 10; counter = counter + 1) {
  line = line + "#";
  print(line);
}
```

* * *

プログラムは変数の以前の値をもとに変数の値を更新することをしばしば必要とする。例えば`counter = counter + 1`のように。JavaScriptはこれの短縮形を提供する：`counter += 1`。他の多くの演算子でもこれは動き、例えば`result *= 2`は`result`の値を2倍にし、あるいは`counter -= 1`はカウントダウンである。

`counter += 1`と`counter -= 1`にはさらに短い書き方がある：`counter++`と`counter--`

* * *

ループはプログラムの制御フローに影響すると言われる。それは文が実行される順序を変更する。多くの場合では、フローの別の種類が便利だ：文のスキップである。

20以下の数について、3と4の両方で割り切れるものを全て表示する。

```javascript
for (var counter = 0; counter < 20; counter++) {
  if (counter % 3 == 0 && counter % 4 == 0)
    show(counter);
}
```

キーワード`if`はキーワード`while`とそんなに大きく違わない：与えられた条件（括弧の中の）をチェックし、その条件に従って後の文を実行する。しかしこれは1回だけ行われので、文が実行されるのは0回もしくは1回である。

modulo(`%`)演算子のトリックが数値が他の数値で割り切れるかどうかテストする簡単な方法である。もし割り切れるなら、割り算の余りとしてmoduloは0を返す。

20以下の全ての数を、しかし4で割り切れない数は括弧付きでプリントしたいなら、このように書くことができる:

```javascript
for (var counter = 0; counter < 20; counter++) {
  if (counter % 4 == 0)
    print(counter);
  if (counter % 4 != 0)
    print("(" + counter + ")");
}
```

しかし今のプログラムは`counter`が`4`で割り切れるか否かの判定を2回行っている。`else`部を`if`文に追加することで同じ効果を得ることが可能だ。`else`文は`if`の条件がfalseであったときだけ実行される。

```javascript
for (var counter = 0; counter < 20; counter++) {
  if (counter % 4 == 0)
    print(counter);
  else
    print("(" + counter + ")");
}
```

このちょっとした例をもう少し発展させて、同じ数が15より大きければ星2つを、10より大きければ（しかし15より大きくなければ）星1つを、どちらでもなければ星はなしでプリントさせてみたいとする。

```javascript
for (var counter = 0; counter < 20; counter++) {
  if (counter > 15)
    print(counter + "**");
  else if (counter > 10)
    print(counter + "*");
  else
    print(counter);
}
```

これは`if`文は互いに連鎖できるということのデモンストレーションである。この場合、プログラムは`counter`が`15`より大きいかを最初に見る。もしそうであれば、星2つを印刷して他のテストはスキップする。もしそうでなければ、続けて`counter`が`10`より大きいかをチェックする。`counter`が`10`より大きくなければ最後の`print`文にたどり着く。

[演習 2.5](#Ex2-5)

`pronpt`を用いて、2 + 2がどんな値になるか、あなた自身に質問するプログラムを書け。もし答えが"4"であれば、`alert`で賞賛の言葉を。もし"3"か"5"であれば、"Almost!"を。他の場合は、何かそのような意味の言葉を表示させなさい。

[解答を見る]

```javascript
var answer = prompt("You! What is the value of 2 + 2?", "");
if (answer == "4")
  alert("You must be a genius or something.");
else if (answer == "3" || answer == "5")
  alert("Almost!");
else
  alert("You're an embarrassment.");
```

* * *

ループはその終了に続く道を必ずしも進まないとき、`break`キーワードが便利である。直ちに現在のループから脱出し、その後から実行を続ける。このプログラムは20より大きくて7で割り切れる最初の数を求める。

```javascript
for (var current = 20; ; current++) {
  if (current % 7 == 0)
    break;
}
print(current);
```

この`for`構造ではループの終了チェックの部分を持たない。これは中の`break`文にその終了をゆだねているということを意味する。同じプログラムをより単純に書くこともできた…。

```javascript
for (var current = 20; current % 7 != 0; current++)
  ;
print(current);
```

この場合、ループの本体は空である。単独のセミコロンは空文を作るのに使われる。ここでは、ループの効果として変数`current`を加算する事だけを望んでいる。しかし私は最初のバージョンの`break`文を使った例の方が注意を引くので、望ましいと思う。

* * *

[演習 2.6](#Ex2-6)

前の演習の解答にを`while`と`break`を加えて、正しい答えが得られるまで質問を繰り返すようにせよ。

`while(true)`について...それ自体では終了しないループを作るのに使える。`true`が`true`である限りループするというプログラムはちょっとバカバカしいが、しかし有用なトリックである。

[解答を見る]

```javascript
var answer;
while (true) {
  answer = prompt("You! What is the value of 2 + 2?", "");
  if (answer == "4") {
    alert("You must be a genius or something.");
    break;
  }
  else if (answer == "3" || answer == "5") {
    alert("Almost!");
  }
  else {
    alert("You're an embarrassment.");
  }
}
```

最初の`if`の本体が2つの文を持つため、本体の周りに中括弧を付け足した。これは好みの問題だ。1つの`if/else`連鎖の中である本体がブロックでありかつ他が単一行であるとちょっとバランスが悪いように私には思えるのだが、しかしあなたはあなた自身の好みで決めて良い。

もう一つの解答は、良さそうに見えるが、`break`を使ってない。:

```javascript
var value = null;
while (value != "4") {
  value = prompt("You! What is the value of 2 + 2?", "");
  if (value == "4")
    alert("You must be a genius or something.");
  else if (value == "3" || value == "5")
    alert("Almost!");
  else
    alert("You're an embarrassment.");
}
```

* * *

先の演習の解答の中に`var answer;`という文があった。これは`answer`という名の変数を作るが、しかしそれに値を与えていない。この変数の値を取り出したらどうなるだろうか？

```javascript
var mysteryVariable;
show(mysteryVariable);
```

タコにしてみれば、この変数は空気のようなもので、掴めるものは何もない。空である場所の値を聞いたとき、`undefined`という特別な値が得られる。`print`や`alert`のように、興味を引くような値を返さない関数もまた`undefined`という値を返す。

```javascript
show(alert("I am a side effect."));
```

同じような値として`null`というものがあり、これは'この変数は定義されているが、しかし値を持たない'という意味である。`undefined`と`null`の違いはもっとも学術的で、普通はあまり興味を引かない。実用的なプログラムでは、しばしば何かが'値を持っているか'をチェックするのに必要になる。これらの場合、`something == undefined`という式が使われ、それらが正確には同じ値ではないにも係わらず、`null == undefined`は`true`となる。

* * *

別のトリッキーな問題を持ち込むもの...

```javascript
show(false == 0);
show("" == 0);
show("5" == 5);
```

これらは全て`true`の値になる。型が異なる変数同士を比較したとき、JavaScriptは面倒で混乱したルールを使う。あまり精密な説明はしたくないが、多くの場合片方の値をもう片方の値の型に変換しようと試みられるのである。しかしながら、`null`や`undefined`があったとき、もし両方の値が`null`または`undefined`のときには`true`にしかならない。

もし変数が`false`という値を参照しているかをテストしたいときは？文字列や数値を真偽値に変換するルールは`0`または空の文字列は`false`とし、他の値はすべて`true`とする、である。このため、変数が`0`か`""`であれば`変数 == false`という式もまた`true`である。これに似た場合、自動的な型変換を**望まない**場合、2つの拡張演算子がある：`===`および`!==`。1つめのものは値が他方と精密に等しいか、2つめは値が精密に等しくないかをテストする。

```javascript
show(null === undefined);
show(false === 0);
show("" === 0);
show("5" === 5);
```

これら全て`false`である。

* * *

`if`、`while`、`for`文の条件として与えられる値は真偽値でないかもしれない。これらはチェックの前に真偽値に自動的に型変換されるだろう。これは数値の`0`や空の文字列`""`、`null`、`undefined`そしてもちろん`false`が、全て`false`として扱われることを意味する。

他の値が全て`true`として扱われることはこの場合、多くの状況で明示的な比較を省略することを可能にする。変数の内容がが文字列か`null`であると分っているとき、これをとても単純にチェックできる...。

```javascript
var maybeNull = null;
// ... maybeNullに文字列を入れるかもしれない謎のコード
if (maybeNull)
  print("maybeNull has a value");
```

このケースで期待されるのは謎のコードが`maybeNull`に`""`の値を入れることである。空の文字列は`false`であるから、何もプリントされない。何を試すかによっているが、これは**間違っているかもしれない**。しばしば明示的に`=== null`または`=== false`を加える良いアイデアが微妙なミスを防ぐ。数値に対する`0`についても同様である。

* * *

先の例の'謎のコード'と書かれた行がちょっと奇異に見えたかもしれない。プログラムに追加のテキストを入れられればしばしば便利だ。プログラムに人間の言葉で説明を加えることがもっとも一般的な使い途だ。

```javascript
// The variable counter, which is about to be defined, is going
// to start with a value of 0, which is zero.
var counter = 0;
// Now, we are going to loop, hold on to your hat.
while (counter < 100 /* counter is less than one hundred */)
/* Every time we loop, we INCREMENT the value of counter,
   Seriously, we just add one to it. */
  counter++;
// And then, we are done.
```

この種類のテキストをコメントと呼ぶ。このようなルールである：'`/*`'で始まり'`*/`'が見つかるまでがコメントとなる。'`//`'始まりは別の種類のコメントで、その行の終わりまで続く。

見ての通り、単純なプログラムにたくさんのコメントを単純に付け加えてしまうと、大きく、醜く、面倒になってしまうことがある。

* * *

他の状況でも自動的な型変換が起こることがある。もし文字列型でない値を文字型に付け加えたら、その値は前に連結された文字列に自動的に変換される。もし数値を文字列にかけ算したら、JavaScriptは文字列から数値を作り出そうとする。

```javascript
show("Apollo" + 5);
show(null + "ify");
show("5" * 5);
show("strawberry" * 5);
```

最後の文は`NaN`という特別な値をプリントする。これは'not a number'であり、数値型の値（少々矛盾して聞こえるかもしれないが）である。この場合、`strawberry`は実際に数値でない。`NaN`からの全ての数学的な演算の結果は`NaN`になり、それがこの例のように`5`をかけたらそれも`NaN`なったことの理由である。また、これも迷わせるようだが、`NaN == NaN`は`false`と等しく、ある値が`NaN`であるかどうかをチェックするには`isNaN`関数を使う。`NaN`はもう一つの（そして最後の）変換したら`false`となる値である。

これらの自動的な変換はとても便利なものであり得るが、しかしむしろ奇異と誤りももたらす。`+`と`*`はともに数学演算子であるにもかかわらず、例では全く異なった振る舞いをした。私自身のコードでは、`+`を文字列と文字列でない値の連結に数多く使っているが、`*`や他の数学演算子を文字列には使わないようにしている。数値から文字列への変換は常に可能でまっすぐだが、文字列から数値への変換はしばしばうまく動かない（例の最後の行のように）。文字列から数字への明示的な変換には`Number`関数を使うことができ、それにより`NaN`値が得られることによるリスクをクリアすることができる。

```javascript
show(Number("5") * 5);
```

* * *

先ほどの`&&`と`||`という真偽値の演算子の話で、私はそれらが真偽値を作ると主張した。これはちょっと単純化しすぎた話でもある。もしこれらを真偽値に適用したら、真偽値が返ってくる。しかし、他の種類の値に適用したら、これらの引数のどちらかが帰ってくる。

`||`は実際はこのようなものだ：左のものを最初として値を見る。これを真偽値に変換して`true`になったら左の値を返し、そうでなければ右の値を返す。引数を真偽値にしたときこれが正しいことをチェックしてみてほしい。なぜこんな動きになるのか？しかし実用的ではある。この例を考えてみて欲しい。:

```javascript
var input = prompt("What is your name?", "Kilgore Trout");
print("Well hello " + (input || "dear"));
```

ユーザーが'Cancel'を押すか、他の方法で名前を書かずに`prompt`のダイアログを閉じたら、変数の値は`null`もしくは`""`となる。どちらの場合も真偽値に変換された時には偽になる。`input || "dear"`という式はこの場合、'変数`input`の値か、もしくは`"dear"`という文字列'である。これは'代替'の値を提供する簡単な手だ。

`&&`は同様な、しかし別の働きをする。左側の値が真偽値に変換したときに`false`になる何らかの値であれば、その値を返し、そうなければ右の値を返すのである。

これら2つの演算子の他の特徴は右側の式は必要になってからだけ評価されるということである。`true || X`の場合、`X`が何であるかにかかわらず、結果は`true`となり、そして`X`は評価されることがなく、もしそれにside effectがあってもそれは起きない。`false && X`についても同様である。

```javascript
false || alert("I'm happening!");
true || alert("Not me.");
```