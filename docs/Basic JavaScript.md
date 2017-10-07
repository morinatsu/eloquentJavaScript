=====================================================
基本のJavaScript: 値、変数、および制御の流れ
=====================================================
..  Chapter 2:
..  Basic JavaScript: values, variables, and control flow

..  Inside the computer's world, there is only data. That which is not data, does not exist. Although all data is in essence just a sequence of bits1, and is thus fundamentally alike, every piece of data plays its own role. In JavaScript's system, most of this data is neatly separated into things called values. Every value has a type, which determines the kind of role it can play. There are six basic types of values: Numbers, strings, booleans, objects, functions, and undefined values.

コンピューターの世界の中にはただデータしか存在しない。データでないものは存在しない。全てのデータは本質的にはただのビット\ [#f1]_\ の連続であり、そうして本来的に等しく、全てのデータはそれ自身の役割を演じている。JavaScriptのシステムでは多くのデータは値と呼ばれるものにきちんと分割されている。全ての値は型を持ち、それはそれが演じる役割を現わしている。基本的な型は6つある：数値、文字列、真偽値、オブジェクト、関数そして未定義の値である。

..  To create a value, one must merely invoke its name. This is very convenient. You don't have to gather building material for your values, or pay for them, you just call for one and woosh, you have it. They are not created from thin air, of course. Every value has to be stored somewhere, and if you want to use a gigantic number of them at the same time you might run out of computer memory. Fortunately, this is only a problem if you need them all simultaneously. As soon as you no longer use a value, it will dissipate, leaving behind only a few bits. These bits are recycled to make the next generation of values.

値を作るには、それにただ名前を与えるだけでよい。これはとても便利だ。値のためにその構成要素を集めたり、何かを支払ったりする必要はなく、名前を呼んで口笛を吹くだけで手に入れることができる。もちろん、空中から生じるのではない。全ての値はどこかに格納されている必要があり、もし巨大な数の値を使用することを望むならコンピューターのメモリからはみ出ることになる。幸運にも、これは全てを同時に必要とした場合の問題である。値を使わなくなったら、それは分散し、わずかなビットを残して消える。これらのビットは新しい値を生成するために再利用される。

.. |hr| raw:: html

   <hr>

|hr|

..  Values of the type number are, as you might have deduced, numeric values. They are written the way numbers are usually written:

数値(number)型の値は、あなたの想像通り、数値の値だ。数値が普通に書かれるとおりに書かれる:

.. code-block:: javascript

    144

..  Enter that in the console, and the same thing is printed in the output window. The text you typed in gave rise to a number value, and the console took this number and wrote it out to the screen again. In a case like this, that was a rather pointless exercise, but soon we will be producing values in less straightforward ways, and it can be useful to 'try them out' on the console to see what they produce.

コンソールにこれを入力すると、同じものが出力ウインドウにプリントされる。打ち込んだテキストは数値の値になり、コンソールはこの数値を取ってスクリーンに書き込む。このような場合、むしろこれは無意味な演習であり、我々はすぐに、あまりまっすぐでない方法で値を作るだろうが、作られたものをコンソールで試してみるには便利である。

..  This is what 144 looks like in bits:

144をビットで表現するとこうなる\ [#f2]_\ :

.. code-block:: javascript

    0100000001100010000000000000000000000000000000000000000000000000

..  The number above has 64 bits. Numbers in JavaScript always do. This has one important repercussion: There is a limited amount of different numbers that can be expressed. With three decimal digits, only the numbers 0 to 999 can be written, which is 103 = 1000 different numbers. With 64 binary digits, 264 different numbers can be written. This is a lot, more than 1019 (a one with nineteen zeroes).

上記の数値は64ビットである。JavaScriptの数値は常にそうなる。これには重要な影響がある：異なる数を表現するのに限度があるということだ。3桁の10進数では、0から999までの数しか書けず、それは10\ :sup:`3` = 1000の異なる数値となる。64桁の2進数では、2\ :sup:`64`\ の異なる数を書くことができ、これは多くの、10\ :sup:`19`\ （1の後に0が19個）より大きい。

..  Not all whole numbers below 1019 fit in a JavaScript number though. For one, there are also negative numbers, so one of the bits has to be used to store the sign of the number. A bigger issue is that non-whole numbers must also be represented. To do this, 11 bits are used to store the position of the fractional dot within the number.

10\ :sup:`19`\ 以下のの自然数がJavaScriptで扱えるわけではない。負の数値というものもあり、その符号を格納するためにビットの1つが使われる。自然数でない数値も表現しなければならないというより大きな問題もある。このために、11ビットが数値の中のピリオドの位置を格納するために使われる。

..  That leaves 52 bits3. Any whole number less than 252, which is over 1015, will safely fit in a JavaScript number. In most cases, the numbers we are using stay well below that, so we do not have to concern ourselves with bits at all. Which is good. I have nothing in particular against bits, but you do need a terrible lot of them to get anything done. When at all possible, it is more pleasant to deal with bigger things.

残る52ビット\ [#f3]_\ 、2\ :sup:`52`\ より小さい自然数、10\ :sup:`15`\ を超えたところ、これがJavaScriptが安全に扱える数値である。多くの場合、我々が使う数値はこれより小さく、ビットを全て使い切ることを考える必要はない。これは良いことだ。物事をなすためにひどく多くのビットを\ *必要とした*\ としても、個々のビットに対して何かすることはない。全ての可能性としても、大きなものを扱うのに好都合だ。

..  Fractional numbers are written by using a dot.

小数を書くにはドットを使う。:

.. code-block:: javascript

    9.81

..  For very big or very small numbers, one can also use 'scientific' notation by adding an e, followed by the exponent of the number:

とても大きいまたはとても小さい数には、\ ``e``\ に続けて指数となる数値を加えることで'指数表現'を使うこともできる。:

.. code-block:: javascript

    2.998e8

..  Which is 2.998 * 108 = 299800000.

これは2.998 * 10\ :sup:`8` = 299800000である。

..  Calculations with whole numbers (also called integers) that fit in 52 bits are guaranteed to always be precise. Unfortunately, calculations with fractional numbers are generally not. Like π (pi) can not be precisely expressed by a finite amount of decimal digits, many numbers lose some precision when only 64 bits are available to store them. This is a shame, but it only causes practical problems in very specific situations. The important thing is to be aware of it, and treat fractional digital numbers as approximations, not as precise values.

自然数（整数とも呼ばれる）の計算は52ビットまで正確に行えることが保証されている。不運にも、小数の計算は全般的には異なる。π（円周率）のような数は10進数で正確に表現することができず、多くの数は64ビットだけで格納しようとすると若干正確さが失われる。これは残念なことだが、とても特別な状況でしか実用上の問題とはならない。正確な値ではなく近似値を使うことで小数を扱うことができるようになるということが重要だ。

..  The main thing to do with numbers is arithmetic. Arithmetic operations such as addition or multiplication take two number values and produce a new number from them. Here is what they look like in JavaScript:

|hr|

数値の主な使い途は計算だ。足し算やかけ算のような計算操作は2つの数値の値から新しい数値を作る。JavaScriptではこのようになる:

.. code-block:: javascript

    100 + 4 * 11

..  The + and * symbols are called operators. The first stands for addition, and the second for multiplication. Putting an operator between two values will apply it to those values, and produce a new value.

``+``\ と\ ``*``\ 記号は演算子と呼ばれる。一つめは足し算、2つめはかけ算だ。2つの値の間に演算子を入れれば、それらの値に適用され、新しい値が作られる。

..  Does the example mean 'add 4 and 100, and multiply the result by 11', or is the multiplication done before the adding? As you might have guessed, the multiplication happens first. But, as in mathematics, this can be changed by wrapping the addition in parentheses:

この例は'4と100を足して、その結果に11をかける'か、あるいはかけ算が足し算より先だろうか？お察しの通り、数学でのように、かけ算が最初で、追加の括弧で挟むことでこれを変更することができる。:

.. code-block:: javascript

    (100 + 4) * 11

..  For subtraction, there is the - operator, and division can be done with /. When operators appear together without parentheses, the order in which they are applied is determined by the precedence of the operators. The first example shows that multiplication has a higher precedence than addition. Division and multiplication always come before subtraction and addition. When multiple operators with the same precendence appear next to each other (1 - 1 + 1) they are applied left-to-right.

引き算には\ ``-``\ 演算子があり、そして割り算は\ ``/``\ でできる。括弧無しで演算子が現われたとき、演算子の優先順位に従ってそれが適用される。最初の例ではかけ算が足し算より高い優先順位を持っていることが分った。割り算とかけ算は常に引き算や足し算より優先される。互いに同じ優先順位の演算子が複数あるとき(\ ``1 - 1 + 1``\ )は左から右に適用される。

..  Try to figure out what value this produces, and then run it to see if you were correct...

どんな値ができるか計算し、それから実行してあなたの計算が正しかったか確認してみてほしい。:

.. code-block:: javascript

    115 * 4 - 4 + 88 / 2

..  These rules of precedence are not something you should worry about. When in doubt, just add parentheses.

優先順位のルールがあなたの考えたものと違っていたと嘆かなくてもいい。疑わしいときは括弧を追加するだけだ。

..  There is one more arithmetic operator which is probably less familiar to you. The % symbol is used to represent the modulo operation. X modulo Y is the remainder of dividing X by Y. For example 314 % 100 is 14, 10 % 3 is 1, and 144 % 12 is 0. Modulo has the same precedence as multiplication and division.

もう一つ数学的な演算子があり、それはおそらくあなたがよく知らないものだ。\ ``%``\ 記号はmodulo計算をするときに使われる。\ ``X`` modulo ``Y``\ は\ ``X`` を\ ``Y``\ で割ったときの余りである。例えば\ ``314 % 100``\ は\ ``14``\ 、そして ``10 % 3``\ は\ ``1``\ 、\ ``144 % 12``\ は\ ``0``\ だ。moduloはかけ算や割り算と同じ優先順位を持つ。

|hr|

..  The next data type is the string. Its use is not as evident from its name as with numbers, but it also fulfills a very basic role. Strings are used to represent text, the name supposedly derives from the fact that it strings together a bunch of characters. Strings are written by enclosing their content in quotes:

次のデータ型は文字列(string)だ。その名前から想像される使い途は数値のそれより明白だとは言えないが、これもまたとても基本的な役割である。文字列はテキストを表現するのに使われ、その名前はおそらく、それが文字の束を繋いだものだという事実から来ている。文字列は引用符でその内容を囲んで書かれる。:

.. code-block:: javascript

    "Patch my boat with chewing gum."

..  Almost anything can be put between double quotes, and JavaScript will make a string value out of it. But a few characters are tricky. You can imagine how putting quotes between quotes might be hard. Newlines, the things you get when you press enter, can also not be put between quotes, the string has to stay on a single line.

二重引用符の間はほとんど何でも良く、JavaScriptはそれを外して文字列の値を作る。しかし、トリッキーな文字が若干ある。引用符の中に引用符をどう入れるか想像するのは難しいだろう。新しい行、エンターキーを押したときのもの、あれを引用符の中に入れつつ、文字列は単一行のままにすることはできないのだろうか。

..  To be able to have such characters in a string, the following trick is used: Whenever a backslash ('\') is found inside quoted text, it indicates that the character after it has a special meaning. A quote that is preceded by a backslash will not end the string, but be part of it. When an 'n' character occurs after a backslash, it is interpreted as a newline. Similarly, a 't' after a backslash means a tab character4.

そのような文字を文字列に入れるには、以下のトリックを使う：引用されたテキストの中にバックスラッシュ('\ ``\``\ ')があれば、それは続くキャラクターが特別な意味を持つことを示す。バックスラッシュに続く引用符は文字列の終わりではなく、その一部となる。バックスラッシュの次に'\ ``n``\ 'があれば、それは新しい行と解釈される。同様に、バックスラッシュの次の'\ ``t``\ 'は4文字文のタブ\ [#f4]_\ を意味する。:

.. code-block:: javascript

    "This is the first line\nAnd this is the second"

..  There are of course situations where you want a backslash in a string to be just a backslash, not a special code. If two backslashes follow each other, they will collapse right into each other, and only one will be left in the resulting string value:

バックスラッシュを文字列の中で特殊なコードではなく、ただバックスラッシュとしたい状況ももちろんあるだろう。もし2つのバックスラッシュが互いに重なれば、互いの右を折りたたみ、文字列としての結果には一つだけが残る。:

.. code-block:: javascript

    "A newline character is written like \"\\n\"."

..  Strings can not be divided, multiplied, or subtracted. The + operator can be used on them. It does not add, but it concatenates, it glues two strings together.

文字列は割り算したり、かけ算したり、引き算したりできない。\ ``+``\ 演算子だけは使うことが\ *できる*\ 。加算ではなく、連結になる。2つの文字列を互いに結びつける。:

.. code-block:: javascript

    "con" + "cat" + "e" + "nate"

..  There are more ways of manipulating strings, but these are discussed later.

文字列を操作する方法はもっとあるが、それは後ほど。

|hr|

..  Not all operators are symbols, some are written as words. For example, the typeof operator, which produces a string value naming the type of the value you give it.

全ての演算子が記号というわけではなく、単語として書かれるものもある。例えば、\ ``typeof``\ 演算子、これは値の持つ型の名前を文字列の値を作る。:

.. code-block:: javascript

    typeof 4.5

..  The other operators we saw all operated on two values, typeof takes only one. Operators that use two values are called binary operators, while those that take one are called unary operators. The minus operator can be used both as a binary and a unary operator:

我々が見てきた他の演算子は2つの値を演算するが、\ ``typeof``\ は1つだけだ。2つの値を使う演算子は2項演算子と呼ばれ、1つのものは単項演算子と呼ばれる。マイナス演算子は二項演算子としても単項演算子としても使われる。:

.. code-block:: javascript

    - (10 - 2)

|hr|

..  Then there are values of the boolean type. There are only two of these: true and false. Here is one way to produce a true value:

それから真偽値(boolean)型の値。これは2つしかない:\ ``true``\ と\ ``false``\ だ。\ ``true``\ の値の作りかたの1つはこう。:

.. code-block:: javascript

    3 > 2

..  And false can be produced like this:

そして\ ``false``\ の場合はこれ。:

.. code-block:: javascript

    3 < 2

..  I hope you have seen the > and < signs before. They mean, respectively, 'is greater than' and 'is less than'. They are binary operators, and the result of applying them is a boolean value that indicates whether they hold in this case.

あなたが\ ``>``\ と\ ``<``\ の記号を既に知っていることを期待している。これはそれぞれ'より大きい'と'より小さい'という意味だ。これらは二項演算子で、その結果は式を適用した結果の真偽値を持つ。

..  Strings can be compared in the same way:

文字列も同様に比較できる。:

.. code-block:: javascript

    "Aardvark" < "Zoroaster"

..  The way strings are ordered is more or less alphabetic. More or less... Uppercase letters are always 'less' than lowercase ones, so "Z" < "a" (upper-case Z, lower-case a) is true, and non-alphabetic characters ('!', '@', etc) are also included in the ordering. The actual way in which the comparison is done is based on the Unicode standard. This standard assigns a number to virtually every character one would ever need, including characters from Greek, Arabic, Japanese, Tamil, and so on. Having such numbers is practical for storing strings inside a computer ― you can represent them as a list of numbers. When comparing strings, JavaScript just compares the numbers of the characters inside the string, from left to right.

文字列はアルファベット順の大小で比較される。大小…大文字は常に小文字より小さく、\ ``"Z" < "a"``\ （大文字のZは小文字のaより小さい）は\ ``true``\ となり、アルファベットでない文字（'\ ``!``\ ', '\ ``@``\ ', etc）も順番に含まれる。この比較は実際にはユニコード標準をベースに行われる。この標準は必要とされた全ての文字に仮想の数値を割り当て、ギリシャ文字、アラビア文字、日本語、タミル文字などなど全ての文字が含まれる。このように数値を持つのはコンピュータに文字列を格納するのには実際的で―それらを数値のリストのように扱うことができる。文字列を比較するとき、JavaScriptは文字列の中の文字の数値を左から右へと比較するだけである。

..  Other similar operators are >= ('is greater than or equal to'), <= (is less than or equal to), == ('is equal to'), and != ('is not equal to').

他に同様の演算子\ ``>=``\ （'大なりイコール'）、\ ``<=``\ （'小なりイコール'）、\ ``==``\ （'等しい'）、そして\ ``!=``\ （'等しくない'）がある。:

.. code-block:: javascript

    "Itchy" != "Scratchy"

::

    5e2 == 500

..  There are also some useful operations that can be applied to boolean values themselves. JavaScript supports three logical operators: and, or, and not. These can be used to 'reason' about booleans.

|hr|

真偽値それ自体に適用することもできる便利なものもある。JavaScriptは3つの論理演算子をサポートしている：\ *論理積(and)*\ 、\ *論理和(or)*\ 、そして\ *否定(not)*\ 。これらは真偽の'判断'に使うことができる。

..  The && operator represents logical and. It is a binary operator, and its result is only true if both of the values given to it are true.

``&&``\ 演算子は\ *論理積*\ を示す。これは二項演算子で、両方の値がともに\ ``true``\ のときだけ\ ``true``\ となる。:

.. code-block:: javascript

    true && false

..  \|| is the logical or, it is true if either of the values given to it is true

``||``\ は\ *論理和*\ で、与えられた値のどちらかが\ ``true``\ であれば\ ``true``\ となる。:

.. code-block:: javascript

    true || false

..  Not is written as an exclamation mark, !, it is a unary operator that flips the value given to it, !true is false, and !false is true.

否定は\ ``!``\ 、感嘆符として書かれ、与えられた値を反転させる単項演算子であり、\ ``!true``\ は\ ``false``\ 、そして\ ``!false``\ は\ ``true``\ となる。

|hr|

演習 2.1

.. code-block:: javascript

    ((4 >= 6) || ("grass" != "green")) &&
       !(((12 * 2) == 144) && true)

..  Is this true? For readability, there are a lot of unnecessary parentheses in there. This simple version means the same thing:

これは真であろうか？読みやすくするため、不要な括弧も書いてある。同じものをもっと単純に書いたのがこれ。:

.. code-block:: javascript

    (4 >= 6 || "grass" != "green") &&
       !(12 * 2 == 144 && true)

..  [show solution]

[解答を見る]

..  Yes, it is true. You can reduce it step by step like this:

これは真である。このように1つ1つ確かめることができる。:

.. code-block:: javascript

    (false || true) && !(false && true)
    true && !false
    true

..  I hope you noticed that "grass" != "green" is true. Grass may be green, but it is not equal to green.

``"grass" != "green"``\ であるということに気づいてくれることを期待する。Grass（草）はgreen（緑色）かもしれないが、それは緑色と等しいということではない。

..  It is not always obvious when parentheses are needed. In practice, one can usually get by with knowing that of the operators we have seen so far, || has the lowest precedence, then comes &&, then the comparison operators (>, ==, etcetera), and then the rest. This has been chosen in such a way that, in simple cases, as few parentheses as possible are necessary.

常に括弧が必要というわけではない。実践では、我々がこれまで見てきた演算子、\ ``||``\ は\ ``&&``\ より低い優先順位を持ち、それから比較演算子（\ ``>``\ 、\ ``==``\ 、他）はその後に残るといった知識は普通のこととなる。どのようなやり方を選ぼうと、単純なケースでは可能な限り括弧を少なくすることが必要だ。

|hr|

..  All the examples so far have used the language like you would use a pocket calculator. Make some values and apply operators to them to get new values. Creating values like this is an essential part of every JavaScript program, but it is only a part. A piece of code that produces a value is called an expression. Every value that is written directly (such as 22 or "psychoanalysis") is an expression. An expression between parentheses is also an expression. And a binary operator applied to two expressions, or a unary operator applied to one, is also an expression.

全ての例で電卓のように言語を使うことができるだろう。値を設定し演算子を適用して新たな値を作る。このような値の作成は全てのJavaScriptの本質の1つであるが、ただ1つに留まるものでもない。値を作成するコードは式と呼ばれる。全ての値はそれが（\ ``22``\ や\ ``"psycoanalysis"``\ のように）直接書かれたものであっても1つの式である。括弧で囲われた1つの式もまた1つの式であり、2つの式に適用された2項演算子、1つの式に適用された単項演算子もまた1つの式である。

..  There are a few more ways of building expressions, which will be revealed when the time is ripe.

式を作る方法はもう2,3あるが、それは時が来たら見せよう。

..  There exists a unit that is bigger than an expression. It is called a statement. A program is built as a list of statements. Most statements end with a semicolon (;). The simplest kind of statement is an expression with a semicolon after it. This is a program:

式より大きな単位もある。それは文と呼ばれる、プログラムは文のリストとして作られる。多くの文はセミコロン(\ ``;``\ )で終了する。最も単純な文は式の後にセミコロンを付けたものである。こんなものもプログラムだ。:

.. code-block:: javascript

    1;
    !false;

..  It is a useless program. An expression can be content to just produce a value, but a statement only amounts to something if it somehow changes the world. It could print something to the screen ― that counts as changing the world ― or it could change the internal state of the program in a way that will affect the statements that come after it. These changes are called 'side effects'. The statements in the example above just produce the values 1 and true, and then immediately throw them into the bit bucket5. This leaves no impression on the world at all, and is not a side effect.

これは使い途のないプログラムだ。式ができるのは値を作ることだけだが、文は世界になにがしかの変化をもたらす。スクリーンに何かをプリントし―世界を変えた分だけカウントが変わる―またはプログラムの内部的な状態が文の後に影響を受けて変わるかもしれない。この変化は'side effect'と呼ばれる。例の文は\ ``1``\ および\ ``true``\ という値を作り、それをビットのバケツ\ [#f5]_\ の中に投げ込む。これは世界に何の印象も残さないので、side effectではない。

|hr|

..  How does a program keep an internal state? How does it remember things? We have seen how to produce new values from old values, but this does not change the old values, and the new value has to be immediately used or it will dissipate again. To catch and hold values, JavaScript provides a thing called a variable.

プログラムが内部的な状態を持つとはどういうことだろうか？何かを覚えているのだろうか？我々は古い値から新しい値の作り方を見てきたが、それは古い値に何の変化も与えず、新しい値はその場で使われるかそのまま消えるかのどちらかであった。新しい値を捉え捕まえておくために、JavaScriptは変数と呼ばれるものを提供する。:

.. code-block:: javascript

    var caught = 5 * 5;

..  A variable always has a name, and it can point at a value, holding on to it. The statement above creates a variable called caught and uses it to grab hold of the number that is produced by multiplying 5 by 5.

変数は常に名前を持ち、それがその保持する値を示す。上記の文は\ ``caught``\ という変数を作り、それを\ ``5``\ かける\ ``5``\ のかけ算によってできた数を保持しておくことに使う。

..  After running the above program, you can type the word caught into the console, and it will retrieve the value 25 for you. The name of a variable is used to fetch its value. caught + 1 also works. A variable name can be used as an expression, and thus can be part of bigger expressions.

上記のプログラムの実行後、\ ``caught``\ という語をコンソールにタイプし、\ ``25``\ という値を取り出すことができる。変数の名前はその値を取り出すのに使われる。\ ``caught + 1``\ もまた動作する。変数の名前は1つの式として使うことができ、それをより大きな式の一部とすることができる。

..  The word var is used to create a new variable. After var, the name of the variable follows. Variable names can be almost every word, but they may not include spaces. Digits can be part of variable names, catch22 is a valid name, but the name must not start with one. The characters '$' and '_' can be used in names as if they were letters, so $_$ is a correct variable name.

``var``\ という語は新しい変数を作るのに使われる。\ ``var``\ の後、変数の名前が続く。変数名がほとんど全ての語を使えるが、中に空白を含めてはいけない。数字は中に入れても良く、\ ``catch22``\ は正しい名前だが、数字を名前の最初に持ってくることはできない。'\ ``$``\ 'と'\ ``_``\ 'という文字は名前に使用できるので、\ ``$_$``\ も正しい変数名である。

..  If you want the new variable to immediately capture a value, which is often the case, the = operator can be used to give it the value of some expression.

もし新しい変数に値を入れたければ、しばしばあるケースだが、\ ``=``\ 演算子を使うことで式の値を変数に与えることができる。

..  When a variable points at a value, that does not mean it is tied to that value forever. At any time, the = operator can be used on existing variables to yank them away from their current value and make them point to a new one.

変数が値を示すとき、それは、それらの結びつきが永遠のものであるということを意味しない。いつでも、\ ``=``\ 演算子を使って既にある値を取り去って新しいものに差し替えることができる。:

.. code-block:: javascript

    caught = 4 * 4;

|hr|

..  You should imagine variables as tentacles, rather than boxes. They do not contain values, they grasp them ― two variables can refer to the same value. Only the values that the program still has a hold on can be accessed by it. When you need to remember something, you grow a tentacle to hold on to it, or re-attach one of your existing tentacles to a new value: To remember the amount of dollars that Luigi still owes you, you could do...

変数に箱よりむしろタコの触手のようなイメージを持つかもしれない。それらは値を\ ``容れる``\ のではなく、\ ``捕まえる``\ ―2つの変数が同じ値を指すように。プログラムが保持する値はただアクセスできるだけだ。もし何かを覚えておく必要があるとき、新しい値を保持する触手を育て、あるいは既にある触手に再度捕まえさせる：ルイージに貸してる金を覚えておくには、こんなことができる...。:

.. code-block:: javascript

    var luigiDebt = 140;

..  Then, every time Luigi pays something back, this amount can be decremented by giving the variable a new number:

その後、ルイージがいくらか支払ってくる度に、変数に新しい数値を与えていくことによって負債を減らしていく。:

.. code-block:: javascript

    luigiDebt = luigiDebt - 35;

..  The collection of variables and their values that exist at a given time is called the environment. When a program starts up, this environment is not empty. It always contains a number of standard variables. When your browser loads a page, it creates a new environment and attaches these standard values to it. The variables created and modified by programs on that page survive until the browser goes to a new page.

一度に与えられた変数のコレクションとそれらの変数を環境と呼ぶ。プログラムがスタートしたとき、その環境は空ではない。幾つかの標準変数が常に含まれている。ブラウザがページをロードするとき、新しい環境が作りこれらの標準変数に触れる。そのページのプログラムにより作られ更新された変数はブラウザが新しいページに行くまで生き残る。

..  A lot of the values provided by the standard environment have the type 'function'. A function is a piece of program wrapped in a value. Generally, this piece of program does something useful, which can be evoked using the function value that contains it. In a browser environment, the variable alert holds a function that shows a little dialog window with a message. It is used like this:

|hr|

標準環境により'関数'型の多くの変数が提供される。関数は1つの値に包まれたプログラムの一片である。全般的に、便利な何らかのプログラムの一片は、その関数を含む値という形で使われる。ブラウザ環境では、\ ``alert``\ 変数が短いメッセージをダイアログウインドウに表示する関数を保持している。:

.. code-block:: javascript

    alert("Also, your hair is on fire.");

..  Executing the code in a function is called invoking or applying it. The notation for doing this uses parentheses. Every expression that produces a function value can be invoked by putting parentheses after it. The string value between the parentheses is given to the function, which uses it as the text to show in the dialog window. Values given to functions are called parameters or arguments. alert needs only one of them, but other functions might need a different number.

関数の中のコードを実行することは呼び出しあるいは適用と呼ばれる。それを示すには括弧を使う。括弧の後に続く全ての式が関数の値を作るときに使われる。括弧の中の文字列がこの関数に与えられたら、それはダイアログウインドウの中に表示するテキストとして使われる。関数に与えられる値はパラメーターまたは引数と呼ばれる。\ ``alert``\ が必要とするは1つだが、他の関数は異なる数だけ必要とすることもある。

|hr|

..  Showing a dialog window is a side effect. A lot of functions are useful because of the side effects they produce. It is also possible for a function to produce a value, in which case it does not need to have a side effect to be useful. For example, there is a function Math.max, which takes two arguments and gives back the biggest of the two:

ダイアログウインドウを表示するのはside effectである。多くの関数が便利なのはそれらの作るside effectによる。値を作るための関数というのも可能であり、この場合は便利なside effectを持つ必要はない。例えば、\ ``Math.max``\ 関数というものがあり、これは2つの値のうち大きい方を返す。:

.. code-block:: javascript

    alert(Math.max(2, 4));

..  When a function produces a value, it is said to return it. Because things that produce values are always expressions in JavaScript, function calls can be used as a part of bigger expressions:

関数が値を作るとき、それを返却するという。なぜならJavaScriptでは作られた値は常に式だからであり、関数呼び出しはより大きな式の一部として使われうるからである。:

.. code-block:: javascript

    alert(Math.min(2, 4) + 100);

..  Chapter 3 discusses writing your own functions.

:doc:`3章</Functions>`\ であなた自身の関数を書く方法を議論する。

|hr|

..  As the previous examples show, alert can be useful for showing the result of some expression. Clicking away all those little windows can get on one's nerves though, so from now on we will prefer to use a similar function, called print, which does not pop up a window, but just writes a value to the output area of the console. print is not a standard JavaScript function, browsers do not provide it for you, but it is made available by this book, so you can use it on these pages.

先の例で見たように、\ ``alert``\ は式の結果を表示するのに便利だ。クリックして小さなウインドウは消してしまい、これからは\ ``print``\ という同じような関数を使うことにしよう。これはウィンドウをポップアップさせず、コンソールの出力エリアに値を書き出す。\ ``print``\ は標準JavaScript関数ではないので、ブラウザはそれを提供しないが、本書ではページの中で使えるようにしてあるので、これらのページの中で使用することはできる。:

.. code-block:: javascript

    print("N");

..  A similar function, also provided on these pages, is show. While print will display its argument as flat text, show tries to display it the way it would look in a program, which can give more information about the type of the value. For example, string values keep their quotes when given to show:

これらのページで提供している同様の関数で、\ ``show``\ というものもある。\ ``print``\ はその引数を平坦なテキストとして表示するが、\ ``show``\ はプログラムの中と同じやりかたで表示しようとし、値の型に関するより詳しい情報を与えてくれる。例えば、引用符の中の文字列型の値を\ ``show``\ に与える。:

.. code-block:: javascript

    show("N");

..  The standard environment provided by browsers contains a few more functions for popping up windows. You can ask the user an OK/Cancel question using confirm. This returns a boolean, true if the user presses 'OK', and false if he presses 'Cancel'.

ブラウザから提供される標準環境はウインドウをポップアップするもう2,3の関数を提供する。ユーザーに確認をとるためにOK/Cancelを\ ``confirm``\ を使って質問することができる。これは真偽値を返却し、'OK'が押されたら\ ``true``\ 、'Cancel'が押された\ ``false``\ となる。

.. code-block:: javascript

    show(confirm("Shall we, then?"));

..  prompt can be used to ask an 'open' question. The first argument is the question, the second one is the text that the user starts with. A line of text can be typed into the window, and the function will return this as a string.

``prompt``\ は'open'な質問をするときに使える。最初の引数が質問で、2つめがユーザーがそこから始めるテキストである。1行のテキストがウインドウにタイプされたら、関数はこれを文字列として返す。:

.. code-block:: javascript

    show(prompt("Tell us everything you know.", "..."));

|hr|

..  It is possible to give almost every variable in the environment a new value. This can be useful, but also dangerous. If you give print the value 8, you won't be able to print things anymore. Fortunately, there is a big 'Reset' button on the console, which will reset the environment to its original state.

環境のほとんど全ての変数に新しい値を与えることも可能だ。これはとても便利だが、危険でもある。もし\ ``8``\ という値を\ ``print``\ に与えたら、それ以降何もプリントすることができなくなる。幸運なことに、コンソールには'Reset'という大きなボタンがあり、それで環境を元の状態にリセットすることができる。

|hr|

..  One-line programs are not very interesting. When you put more than one statement into a program, the statements are, predictably, executed one at a time, from top to bottom.

1行のプログラムはあまり面白くない。より多くの文をプログラムに入れたら、それらの文は、お察しの通り上から下まで一気に実行される。

.. code-block:: javascript

    var theNumber = Number(prompt("Pick a number", ""));
    print("Your number is the square root of " +
          (theNumber * theNumber));

..  The function Number converts a value to a number, which is needed in this case because the result of prompt is a string value. There are similar functions called String and Boolean which convert values to those types.

``Number``\ 関数は値を数値に変換し、それは\ ``prompt``\ の結果が文字列であるために必要となる。\ ``String``\ や\ ``Boolean``\ といった同様の関数がありそれらはそれぞれの型に値を変換する。

|hr|

..  Consider a program that prints out all even numbers from 0 to 12. One way to write this is:

0から12までの偶数をプリントするプログラムを考える。1つのやり方としてはこのように書ける。:

.. code-block:: javascript

    print(0);
    print(2);
    print(4);
    print(6);
    print(8);
    print(10);
    print(12);

..  That works, but the idea of writing a program is to make something less work, not more. If we needed all even numbers below 1000, the above would be unworkable. What we need is a way to automatically repeat some code.

これは動くが、プログラムを書くためのアイデアとして\ *よい仕事とは言えない*\ 。もし1000以下の偶数全てが必要となったとき、上のやりかたではとても動かない。コードを自動的に繰り返してくれる方法が必要だ。

.. code-block:: javascript

    var currentNumber = 0;
    while (currentNumber <= 12) {
      print(currentNumber);
      currentNumber = currentNumber + 2;
    }

..  You may have seen while in the introduction chapter. A statement starting with the word while creates a loop. A loop is a disturbance in the sequence of statements, it may cause the program to repeat some statements multiple times. In this case, the word while is followed by an expression in parentheses (the parentheses are compulsory here), which is used to determine whether the loop will loop or finish. As long as the boolean value produced by this expression is true, the code in the loop is repeated. As soon as it is false, the program goes to the bottom of the loop and continues as normal.

おそらく\ ``while``\ を\ :doc:`導入</Introduction>`\ の章で見ているだろう。\ ``while``\ という語から始まる文はループを作る。ループは文の順列を実行し続け、複数回その文を繰り返す。この場合、\ ``while``\ に続く括弧の中の式（括弧はここになければならない）、これはループがそのままループし続けるか終了するかの判定に使われる。この式が作る真偽値が\ ``true``\ である間、ループの中のコードが繰り返され、\ ``false``\ になったらすぐに、プログラムはループの下に行き通常のようにに実行を続ける。

..  The variable currentNumber demonstrates the way a variable can track the progress of a program. Every time the loop repeats, it is incremented by 2, and at the beginning of every repetition, it is compared with the number 12 to decide whether to keep on looping.

``currentNumber``\ 変数は変数によってプログラムの進行を追うことができることを見せてくれる。ループが繰り返される度に、\ ``2``\ つずつ加算され、繰り返しの最初で、ループを続けるかどうかの判断のために\ ``12``\ という数値と比較される。

..  The third part of a while statement is another statement. This is the body of the loop, the action or actions that must take place multiple times. If we did not have to print the numbers, the program could have been:

``while``\ の3つめの部分は他の文である。これはループの本体であり、複数回繰り返される1つないし複数の動作である。もし数をプリントする必要がなければ、プログラムはこのようになっていた。:

.. code-block:: javascript

    var currentNumber = 0;
    while (currentNumber <= 12)
      currentNumber = currentNumber + 2;

..  Here, currentNumber = currentNumber + 2; is the statement that forms the body of the loop. We must also print the number, though, so the loop statement must consist of more than one statement. Braces ({ and }) are used to group statements into blocks. To the world outside the block, a block counts as a single statement. In the earlier example, this is used to include in the loop both the call to print and the statement that updates currentNumber.

ここでは、\ ``currentNumber = currentNumber + 2;``\ がループの本体の形を取る文である。我々は数をプリントしなければならないので、ループの文は1行より多くなった。中括弧（\ ``{``\ と\ ``}``\ ）がブロックの中に文の負ループを入れるために使われる。ブロックの外側の世界にとっては、ブロックは単一の行として数えられる。先ほどの例では、ループに\ ``print``\ の呼び出しと\ ``currentNumber``\ の更新の両方を含めるためにこれが使われた。

.. _EX22:

演習 2.2

..  Use the techniques shown so far to write a program that calculates and shows the value of 210 (2 to the 10th power). You are, obviously, not allowed to use a cheap trick like just writing 2 * 2 * ....

このテクニックを使って、2\ :sup:`10`\ （2の10乗）を計算するプログラムを書け。当然、\ ``2 * 2 * ...``\ みたいな安っぽいトリックを使って書いてはいけない。

..  If you are having trouble with this, try to see it in terms of the even-numbers example. The program must perform an action a certain amount of times. A counter variable with a while loop can be used for that. Instead of printing the counter, the program must multiply something by 2. This something should be another variable, in which the result value is built up.

もしこれでトラブルにあったら、偶数の例を見てみること。プログラムは複数回動作を繰り返さなければならない。カウンター変数が\ ``while``\ のループとともに使われていた。カウンターをプリントする代わりに、このプログラムは何かを2倍しなければならない。この何かは結果を作り上げるのに使う別の変数になるだろう。

..  Don't worry if you don't quite see how this would work yet. Even if you perfectly understand all the techniques this chapter covers, it can be hard to apply them to a specific problem. Reading and writing code will help develop a feeling for this, so study the solution, and try the next exercise.

まだうまく動かなくても嘆かなくていい。もしこの章がカバーするテクニックを完璧に理解できていれば、難しいのはそれを個々のプログラムに適用することだけだ。コードの読み書きは感覚を育てる助けになり、解答を学習して、次の演習に挑もう。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    var result = 1;
    var counter = 0;
    while (counter < 10) {
      result = result * 2;
      counter = counter + 1;
    }
    show(result);

..  The counter could also start at 1 and check for <= 10, but, for reasons that will become apparent later on, it is a good idea to get used to counting from 0.

``counter``\ は\ ``1``\ から始まり\ ``<= 10``\ とチェックされるが、しかし、後に述べる理由により、0から始めるよりいいアイデアである。

..  Obviously, your own solutions aren't required to be precisely the same as mine. They should work. And if they are very different, make sure you also understand my solution.

当然、あなた自身の解答が私の示すものと全く同じである必要はない。それが動くなら。そしてもし全然違っていれば、わたしの解答も確実に理解すること。

|hr|

演習 2.3

..  With some slight modifications, the solution to the previous exercise can be made to draw a triangle. And when I say 'draw a triangle' I mean 'print out some text that almost looks like a triangle when you squint'.

ちょっとだけ変更して、先ほどの演習の解答から三角形を書くプログラムを作ろう。私が言う'三角形を書く'というのは、ぱっと見三角形に見えるものをプリントアウトするという意味だ。

..  Print out ten lines. On the first line there is one '#' character. On the second there are two. And so on.

10行プリントアウトする。1行めでは'\ ``#``\ '文字が1つ。2行目では2つ。そのように。

..  How does one get a string with X '#' characters in it? One way is to build it every time it is needed with an 'inner loop' ― a loop inside a loop. A simpler way is to reuse the string that the previous iteration of the loop used, and add one character to it.

どうしたらX個の'\ ``#``\ 'を文字列として得られるだろうか？1つめの手では毎回文字列を作るために'inner loop'―ループの中のループ必要とする。もっと簡単な手はループの前回の繰り返しで使った値を再利用し、それに1つ文字をくっつけるというものだ。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    var line = "";
    var counter = 0;
    while (counter < 10) {
      line = line + "#";
      print(line);
      counter = counter + 1;
    }

|hr|

..  You will have noticed the spaces I put in front of some statements. These are not required: The computer will accept the program just fine without them. In fact, even the line breaks in programs are optional. You could write them as a single long line if you felt like it. The role of the indentation inside blocks is to make the structure of the code clearer to a reader. Because new blocks can be opened inside other blocks, it can become hard to see where one block ends and another begins in a complex piece of code. When lines are indented, the visual shape of a program corresponds to the shape of the blocks inside it. I like to use two spaces for every open block, but tastes differ.

私が幾つかの文の前につけた空白に気づいただろう。これらは必要なものではない：コンピュータはそれらがなくてもプログラムを受け付ける。実際、プログラムの行毎の改行もオプショナルなものだ。もしそういうのが好みなら全てを単一の長い行に書くこともできた。ブロックの中のインデントの役割はコードの構造を読み手にとって明らかにすることだ。新しいブロックは他の中のブロックで始めることができるため、どこでブロックが始まるのか複雑なコードの中では読み取りにくくなるかもしれないのだ。行がインデントされていれば、プログラムのみかけがそのなかのブロックの形と同様になる。私はブロックを始める度に2つ空白を空けるのが好みだが、違う人もいる。

..  On browsers other than Opera, the field in the console where you can type programs will help you by automatically adding these spaces. This may seem annoying at first, but when you write a lot of code it becomes a huge time-saver. Pressing the tab key will re-indent the line your cursor is currently on.

Opera以外のブラウザでは、プログラムをタイプするコンソールのフィールドには自動的に空白が追加されるだろう。最初は戸惑うかもしれないが、たくさんコードを書く時には大きな時間短縮になる。タブキーを押すと今カーソルがある場所から再度インデントするだろう。

..  In some cases, JavaScript allows you to omit the semicolon at the end of a statement. In other cases, it has to be there or strange things will happen. The rules for when it can be safely omitted are complex and weird. In this book, I won't leave out any semicolons, and I strongly urge you to do the same in your own programs.

いくつかの場合において、JavaScriptは文の終わりのセミコロンの省略を許す。他の場合は、何かおかしな事が起こる。安全に省略できるというルールは複雑で奇異である。本書では、私はセミコロンを付け忘れたくないし、あなた自身のプログラムにおいてもそうすべきと強くおすすめする。

|hr|

..  The uses of while we have seen so far all show the same pattern. First, a 'counter' variable is created. This variable tracks the progress of the loop. The while itself contains a check, usually to see whether the counter has reached some boundary yet. Then, at the end of the loop body, the counter is updated.

``while``\ を使うにあたり同じようなパターンを見てきた。1つめは'counter'変数が作られるということだ。この変数はループの進行を追跡する。\ ``while``\ 自身に、通常は何らかの境界にまだ届いてないかのチェックが含まれている。それから、ループの本体の終わりで、カウンターが更新される。

..  A lot of loops fall into this pattern. For this reason, JavaScript, and similar languages, also provide a slightly shorter and more comprehensive form:

多くのループがこのパターンになる。この理由で、JavaScriptや同様の言語はもっと短一括的な書き方も提供している。:

.. code-block:: javascript

    for (var number = 0; number <= 12; number = number + 2)
      show(number);

..  This program is exactly equivalent to the earlier even-number-printing example. The only change is that all the statements that are related to the 'state' of the loop are now on one line. The parentheses after the for should contain two semicolons. The part before the first semicolon initialises the loop, usually by defining a variable. The second part is the expression that checks whether the loop must still continue. The final part updates the state of the loop. In most cases this is shorter and clearer than a while construction.

このプログラムは正確にさきほどの偶数をプリントするサンプルと同じである。違いはループの状態に関する全ての文が今や1行になっていることだけだ。\ ``for``\ の後の括弧には2つのセミコロンが含まれる。1つめのセミコロンの前の部分はループの\ ``初期化``\ が、通常は変数を定義することで行われる。2つめの部分はループを継続すべきかどうかの条件を\ *チェック*\ する式である。最後の部分はループの状態を\ *更新*\ する。多くの場合これで\ ``while``\ 構造が短くクリアになる。

|hr|

..  I have been using some rather odd capitalisation in some variable names. Because you can not have spaces in these names ― the computer would read them as two separate variables ― your choices for a name that is made of several words are more or less limited to the following: fuzzylittleturtle, fuzzy_little_turtle, FuzzyLittleTurtle, or fuzzyLittleTurtle. The first one is hard to read. Personally, I like the one with the underscores, though it is a little painful to type. However, the standard JavaScript functions, and most JavaScript programmers, follow the last one. It is not hard to get used to little things like that, so I will just follow the crowd and capitalise the first letter of every word after the first.

私は幾つかの変数名で大文字を使ってきた。変数名には空白を入れられないからである―コンピュータはそれらを2つに別れた変数として読むかもしれない―複数の語からなる名前の作り方については多かれ少なかれ次のように限定され選択となるだろう：\ ``fuzzylittleturtle``\ 、\ ``fuzzy_little_turtle``\ 、\ ``FuzzyLittleTurtle``\ 、または\ ``fuzzyLittleTurtle``\ 。最初のものは読みにくい。個人的に、私はアンダースコアを付けたものが好きだ、タイプするにはちょっと困るけど。しかしながら、標準JavaScript関数では、そして多くのJavaScriptプログラマーが最後のものに従っている。このようにするのは難しくない、大勢に従って1つめより後の語の先頭の文字を大文字にするだけだ。

..  In a few cases, such as the Number function, the first letter of a variable is also capitalised. This was done to mark this function as a constructor. What a constructor is will become clear in chapter 8. For now, the important thing is not to be bothered by this apparent lack of consistency.

わずかなケースでは、例えば\ ``Number``\ 関数のような、変数名の最初の文字もまた大文字にされている。これはこの関数がコンストラクタであることを示す。コンストラクタとは\ :doc:`8章 </Object-oriented Programming>`\ において明らかにする。今は、協調性を欠くことで失敗しないことが重要だ。

..  Note that names that have a special meaning, such as var, while, and for may not be used as variable names. These are called keywords. There are also a number of words which are 'reserved for use' in future versions of JavaScript. These are also officially not allowed to be used as variable names, though some browsers do allow them. The full list is rather long:

特別な意味を持つ名前も注記しよう、\ ``var``\ 、\ ``while``\ そして\ ``for``\ のようなものは変数名として使ってはいけない。これらはキーワードと呼ばれる。JavaScriptの将来のバージョンで'予約語'とされている幾つかの語もある。これらは公式に変数名として使うことが許されていないが、幾つかのブラウザでは許されてしまっている。全体のリストは長い:

.. code-block:: javascript

    abstract boolean break byte case catch char class const continue
    debugger default delete do double else enum export extends false
    final finally float for function goto if implements import in
    instanceof int interface long native new null package private
    protected public return short static super switch synchronized
    this throw throws transient true try typeof var void volatile
    while with

..  Don't worry about memorising these for now, but remember that this might be the problem when something does not work as expected. In my experience, char (to store a one-character string) and class are the most common names to accidentally use.

今覚えきれないと嘆くことはないが、これによって何かが期待通りに動かない問題が起こるかもしれないということは覚えておこう。私の経験では、\ ``char``\ （1文字の文字列を格納する）と\ ``class``\ がもっとも誤って使われがちな一般名だ。

|hr|

演習 2.4

..  Rewrite the solutions of the previous two exercises to use for instead of while.

先の2つの演習の解答を\ ``while``\ の代わりに\ ``for``\ を使って書き直せ。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    var result = 1;
    for (var counter = 0; counter < 10; counter = counter + 1)
      result = result * 2;
    show(result);

..  Note that even if no block is opened with a '{', the statement in the loop is still indented two spaces to make it clear that it 'belongs' to the line above it.

'\ ``{``\ 'から始まるブロックがなくても、上の行のループに含まれることを明示するため、ループの中の文に2つの空白のインデントを行うことを忘れずに。

.. code-block:: javascript

    var line = "";
    for (var counter = 0; counter < 10; counter = counter + 1) {
      line = line + "#";
      print(line);
    }

|hr|

..  A program often needs to 'update' a variable with a value that is based on its previous value. For example counter = counter + 1. JavaScript provides a shortcut for this: counter += 1. This also works for many other operators, for example result *= 2 to double the value of result, or counter -= 1 to count downwards.

プログラムは変数の以前の値をもとに変数の値を更新することをしばしば必要とする。例えば\ ``counter = counter + 1``\ 。JavaScriptはこれの短縮形を提供する：\ ``counter += 1``\ 。他の多くの演算子でもこれは動き、例えば\ ``result \*= 2``\ は\ ``result``\ の値を2倍にし、あるいは\ ``counter -= 1``\ はカウントダウンである。

..  For counter += 1 and counter -= 1 there are even shorter versions: counter++ and counter--.

``counter += 1``\ と\ ``counter -= 1``\ にはさらに短い書き方がある：\ ``counter++`` と\ ``counter--``

..  Loops are said to affect the control flow of a program. They change the order in which statements are executed. In many cases, another kind of flow is useful: skipping statements.

|hr|

ループはプログラムの制御フローに影響すると言われる。それは文が実行される順序を変更する。多くの場合では、フローの別の種類が便利だ：文をスキップすること。

..  We want to show all numbers below 20 which are divisible both by 3 and by 4.

20以下の数について、3と4の両方で割り切れるものを全て表示する。

.. code-block:: javascript

    for (var counter = 0; counter < 20; counter++) {
      if (counter % 3 == 0 && counter % 4 == 0)
        show(counter);
    }

..  The keyword if is not too different from the keyword while: It checks the condition it is given (between parentheses), and executes the statement after it based on this condition. But it does this only once, so that the statement is executed zero or one time.

キーワード\ ``if``\ はキーワード\ ``while``\ とそんなに大きく違わない：与えられた条件（括弧の中の）をチェックし、その条件に従って後の文を実行する。しかしこれは1回だけ行われので、文が実行されるのは0回もしくは1回である。

..  The trick with the modulo (%) operator is an easy way to test whether a number is divisible by another number. If it is, the remainder of their division, which is what modulo gives you, is zero.

modulo(\ ``%``\ )演算子のトリックが数値が他の数値で割り切れるかどうかテストする簡単な方法である。もし割り切れるなら、割り算の余りとしてmoduloは0を返す。

..  If we wanted to print all numbers below 20, but put parentheses around the ones that are not divisible by 4, we can do it like this:

20以下の全ての数を、しかし4で割り切れない数は括弧付きでプリントしたいなら、このように書くことができる:

.. code-block:: javascript

    for (var counter = 0; counter < 20; counter++) {
      if (counter % 4 == 0)
        print(counter);
      if (counter % 4 != 0)
        print("(" + counter + ")");
    }

..  But now the program has to determine whether counter is divisible by 4 two times. The same effect can be gotten by appending an else part after an if statement. The else statement is executed only when the if's condition is false.

しかし今のプログラムは\ ``counter``\ が\ ``4``\ で割り切れるか否かの判定を2回行っている。\ ``else``\ 部を\ ``if``\ 文に追加することで同じ効果を得ることが可能だ。\ ``else``\ 文は\ ``if``\ の条件がfalseであったときだけ実行される。

.. code-block:: javascript

    for (var counter = 0; counter < 20; counter++) {
      if (counter % 4 == 0)
        print(counter);
      else
        print("(" + counter + ")");
    }

..  To stretch this trivial example a bit further, we now want to print these same numbers, but add two stars after them when they are greater than 15, one star when they are greater than 10 (but not greater than 15), and no stars otherwise.

このちょっとした例をもう少し発展させて、同じ数が15より大きければ星2つを、10より大きければ（しかし15より大きくなければ）星1つを、どちらでもなければ星はなしでプリントさせてみたいとする。

.. code-block:: javascript

    for (var counter = 0; counter < 20; counter++) {
      if (counter > 15)
        print(counter + "**");
      else if (counter > 10)
        print(counter + "*");
      else
        print(counter);
    }

..  This demonstrates that you can chain if statements together. In this case, the program first looks if counter is greater than 15. If it is, the two stars are printed and the other tests are skipped. If it is not, we continue to check if counter is greater than 10. Only if counter is also not greater than 10 does it arrive at the last print statement.

これは\ ``if``\ 文は互いに連鎖できるということのデモンストレーションである。この場合、プログラムは\ ``counter``\ が\ ``15``\ より大きいかを最初に見る。もしそうであれば、星2つを印刷して他のテストはスキップする。もしそうでなければ、続けて\ ``counter``\ が\ ``10``\ より大きいかをチェックする。\ ``counter``\ が\ ``10``\ より大きくなければ最後の\ ``print``\ 文にたどり着く。

演習 2.5

..  Write a program to ask yourself, using prompt, what the value of 2 + 2 is. If the answer is "4", use alert to say something praising. If it is "3" or "5", say "Almost!". In other cases, say something mean.

``pronpt``\ を用いて、2 + 2がどんな値になるか、あなた自身に質問するプログラムを書け。もし答えが"4"であれば、\ ``alert``\ で賞賛の言葉を。もし"3"か"5"であれば、"Almost!"を。他の場合は、何かそのような意味の言葉を表示させなさい。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    var answer = prompt("You! What is the value of 2 + 2?", "");
    if (answer == "4")
      alert("You must be a genius or something.");
    else if (answer == "3" || answer == "5")
      alert("Almost!");
    else
      alert("You're an embarrassment.");

|hr|

..  When a loop does not always have to go all the way through to its end, the break keyword can be useful. It immediately jumps out of the current loop, continuing after it. This program finds the first number that is greater than 20 and divisible by 7:

ループはその終了に続く道を必ずしも進まないとき、\ ``break``\ キーワードが便利である。直ちに現在のループから脱出し、その後から実行を続ける。このプログラムは20より大きくて7で割り切れる最初の数を求める。

.. code-block:: javascript

    for (var current = 20; ; current++) {
      if (current % 7 == 0)
        break;
    }
    print(current);

..  The for construct does not have a part that checks for the end of the loop. This means that it is dependant on the break statement inside it to ever stop. The same program could also have been written as simply...

この\ ``for``\ 構造ではループの終了チェックの部分を持たない。これは中の\ ``break``\ 文にその終了をゆだねているということを意味する。同じプログラムをより単純に書くこともできた…。

.. code-block:: javascript

    for (var current = 20; current % 7 != 0; current++)
      ;
    print(current);

..  In this case, the body of the loop is empty. A lone semicolon can be used to produce an empty statement. Here, the only effect of the loop is to increment the variable current to its desired value. But I needed an example that uses break, so pay attention to the first version too.

この場合、ループの本体は空である。単独のセミコロンは空文を作るのに使われる。ここでは、ループの効果として変数\ ``current``\ を加算する事だけを望んでいる。しかし私は最初のバージョンの\ ``break``\ 文を使った例の方が注意を引くので、望ましいと思う。

|hr|

演習 2.6

..  Add a while and optionally a break to your solution for the previous exercise, so that it keeps repeating the question until a correct answer is given.

前の演習の解答にを\ ``while``\ と\ ``break``\ を加えて、正しい答えが得られるまで質問を繰り返すようにせよ。

..  Note that while (true) ... can be used to create a loop that does not end on its own account. This is a bit silly, you ask the program to loop as long as true is true, but it is a useful trick.

``while(true)``\ について...それ自体では終了しないループを作るのに使える。\ ``true``\ が\ ``true``\ である限りループするというプログラムはちょっとバカバカしいが、しかし有用なトリックである。

..  [show solution]

[解答を見る]

.. code-block:: javascript

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

..  Because the first if's body now has two statements, I added braces around all the bodies. This is a matter of taste. Having an if/else chain where some of the bodies are blocks and others are single statements looks a bit lopsided to me, but you can make up your own mind about that.

最初の\ ``if``\ の本体が2つの文を持つため、本体の周りに中括弧を付け足した。これは好みの問題だ。1つの\ ``if/else``\ 連鎖の中である本体がブロックでありかつ他が単一行であるとちょっとバランスが悪いように私には思えるのだが、しかしあなたはあなた自身の好みで決めて良い。

..  Another solution, arguably nicer, but without break:

もう一つの解答は、良さそうに見えるが、\ ``break``\ を使ってない。:

.. code-block:: javascript

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

|hr|

..  In the solution to the previous exercise there is a statement var answer;. This creates a variable named answer, but does not give it a value. What happens when you take the value of this variable?

先の演習の解答の中に\ ``var answer;``\ という文があった。これは\ ``answer``\ という名の変数を作るが、しかしそれに値を与えていない。この変数の値を取り出したらどうなるだろうか？

.. code-block:: javascript

    var mysteryVariable;
    show(mysteryVariable);

..  In terms of tentacles, this variable ends in thin air, it has nothing to grasp. When you ask for the value of an empty place, you get a special value named undefined. Functions which do not return an interesting value, such as print and alert, also return an undefined value.

タコにしてみれば、この変数は空気のようなもので、掴めるものは何もない。空である場所の値を聞いたとき、\ ``undefined``\ という特別な値が得られる。\ ``print``\ や\ ``alert``\ のように、興味を引くような値を返さない関数もまた\ ``undefined``\ という値を返す。

.. code-block:: javascript

    show(alert("I am a side effect."));

..  There is also a similar value, null, whose meaning is 'this variable is defined, but it does not have a value'. The difference in meaning between undefined and null is mostly academic, and usually not very interesting. In practical programs, it is often necessary to check whether something 'has a value'. In these cases, the expression something == undefined may be used, because, even though they are not exactly the same value, null == undefined will produce true.

同じような値として\ ``null``\ というものがあり、これは'この変数は定義されているが、しかし値を持たない'という意味である。\ ``undefined``\ と\ ``null``\ の違いはもっとも学術的で、普通はあまり興味を引かない。実用的なプログラムでは、しばしば何かが'値を持っているか'をチェックするのに必要になる。これらの場合、\ ``something == undefined``\ という式が使われ、それらが正確には同じ値ではないにも係わらず、\ ``null == undefined``\ は\ ``true``\ となる。

|hr|

..  Which brings us to another tricky subject...

別のトリッキーな問題を持ち込むもの...

.. code-block:: javascript

    show(false == 0);
    show("" == 0);
    show("5" == 5);

..  All these give the value true. When comparing values that have different types, JavaScript uses a complicated and confusing set of rules. I am not going to try to explain them precisely, but in most cases it just tries to convert one of the values to the type of the other value. However, when null or undefined occur, it only produces true if both sides are null or undefined.

これらは全て\ ``true``\ の値になる。型が異なる変数同士を比較したとき、JavaScriptは面倒で混乱したルールを使う。あまり精密な説明はしたくないが、多くの場合片方の値をもう片方の値の型に変換しようと試みられるのである。しかしながら、\ ``null`` \や\ ``undefined``\ があったとき、もし両方の値が\ ``null``\ または\ ``undefined``\ のときには\ ``true``\ にしかならない。

..  What if you want to test whether a variable refers to the value false? The rules for converting strings and numbers to boolean values state that 0 and the empty string count as false, while all the other values count as true. Because of this, the expression variable == false is also true when variable refers to 0 or "". For cases like this, where you do not want any automatic type conversions to happen, there are two extra operators: === and !==. The first tests whether a value is precisely equal to the other, and the second tests whether it is not precisely equal.

もし変数が\ ``false``\ という値を参照しているかをテストしたいときは？文字列や数値を真偽値に変換するルールは\ ``0``\ または空の文字列は\ ``false``\ とし、他の値はすべて\ ``true``\ とする、である。このため、変数が\ ``0``\ か\ ``""``\ であれば\ ``変数 == false``\ という式もまた\ ``true``\ である。これに似た場合、自動的な型変換を\ *望まない*\ 場合、2つの拡張演算子がある：\ ``===``\ および\ ``!==``\ 。1つめのものは値が他方と精密に等しいか、2つめは値が精密に等しくないかをテストする。

.. code-block:: javascript

    show(null === undefined);
    show(false === 0);
    show("" === 0);
    show("5" === 5);

..  All these are false.

これら全て\ ``false``\ である。

|hr|

..  Values given as the condition in an if, while, or for statement do not have to be booleans. They will be automatically converted to booleans before they are checked. This means that the number 0, the empty string "", null, undefined, and of course false, will all count as false.

``if``\ 、\ ``while``\ 、\ ``for``\ 文の条件として与えられる値は真偽値でないかもしれない。これらはチェックの前に真偽値に自動的に型変換されるだろう。これは数値の\ ``0``\ や空の文字列\ ``""``\ 、\ ``null``\ 、\ ``undefined``\ そしてもちろん\ ``false``\ が、全て\ ``false``\ として扱われることを意味する。

..  The fact that all other values are converted to true in this case makes it possible to leave out explicit comparisons in many situations. If a variable is known to contain either a string or null, one could check for this very simply...

他の値が全て\ ``true``\ として扱われることはこの場合、多くの状況で明示的な比較を省略することを可能にする。変数の内容がが文字列か\ ``null``\ であると分っているとき、これをとても単純にチェックできる...。

.. code-block:: javascript

    var maybeNull = null;
    // ... maybeNullに文字列を入れるかもしれない謎のコード
    if (maybeNull)
      print("maybeNull has a value");

..  Except in the case where the mystery code gives maybeNull the value "". An empty string is false, so nothing is printed. Depending on what you are trying to do, this might be wrong. It is often a good idea to add an explicit === null or === false in cases like this to prevent subtle mistakes. The same occurs with number values that might be 0.

このケースで期待されるのは謎のコードが\ ``maybeNull``\ に\ ``""``\ の値を入れることである。空の文字列は\ ``false``\ であるから、何もプリントされない。何を試すかによっているが、これは\ *間違っているかもしれない*\ 。しばしば明示的に\ ``=== null``\ または\ ``=== false``\ を加える良いアイデアが微妙なミスを防ぐ。数値に対する\ ``0``\ についても同様である。

|hr|

..  The line that talks about 'mystery code' in the previous example might have looked a bit suspicious to you. It is often useful to include extra text in a program. The most common use for this is adding some explanations in human language to a program.

先の例の'謎のコード'と書かれた行がちょっと奇異に見えたかもしれない。プログラムに追加のテキストを入れられればしばしば便利だ。プログラムに人間の言葉で説明を加えことがもっとも一般的な使い途だ。

.. code-block:: javascript

    // The variable counter, which is about to be defined, is going
    // to start with a value of 0, which is zero.
    var counter = 0;
    // Now, we are going to loop, hold on to your hat.
    while (counter < 100 /* counter is less than one hundred */)
    /* Every time we loop, we INCREMENT the value of counter,
       Seriously, we just add one to it. */
      counter++;
    // And then, we are done.

..  This kind of text is called a comment. The rules are like this: '/*' starts a comment that goes on until a '*/' is found. '//' starts another kind of comment, which goes on until the end of the line.

この種類のテキストをコメントと呼ぶ。このようなルールである：'\ ``/*``\ 'で始まり'\ ``*/``\ 'が見つかるまでがコメントとなる。'\ ``//``\ '始まりは別の種類のコメントで、その行の終わりまで続く。

..  As you can see, even the simplest programs can be made to look big, ugly, and complicated by simply adding a lot of comments to them.

見ての通り、単純なプログラムにたくさんのコメントを単純に付け加えてしまうと、大きく、醜く、面倒になってしまうことがある。

|hr|

..  There are some other situations that cause automatic type conversions to happen. If you add a non-string value to a string, the value is automatically converted to a string before it is concatenated. If you multiply a number and a string, JavaScript tries to make a number out of the string.

他の状況でも自動的な型変換が起こることがある。もし文字列型でない値を文字型に付け加えたら、その値は前に連結された文字列に自動的に変換される。もし数値を文字列にかけ算したら、JavaScriptは文字列から数値を作り出そうとする。

.. code-block:: javascript

    show("Apollo" + 5);
    show(null + "ify");
    show("5" * 5);
    show("strawberry" * 5);

..  The last statement prints NaN, which is a special value. It stands for 'not a number', and is of type number (which might sound a little contradictory). In this case, it refers to the fact that a strawberry is not a number. All arithmetic operations on the value NaN result in NaN, which is why multiplying it by 5, as in the example, still gives a NaN value. Also, and this can be disorienting at times, NaN == NaN equals false, checking whether a value is NaN can be done with the isNaN function. NaN is another (the last) value that counts as false when converted to a boolean.

最後の文は\ ``NaN``\ という特別な値をプリントする。これは'not a number'であり、数値型の値（少々矛盾して聞こえるかもしれないが）である。この場合、\ ``strawberry``\ は実際に数値でない。\ ``NaN``\ からの全ての数学的な演算の結果は\ ``NaN``\ になり、それがこの例のように\ ``5``\ をかけたらそれも\ ``NaN``\ なったことの理由である。また、これも迷わせるようだが、\ ``NaN == NaN``\ は\ ``false``\ と等しく、ある値が\ ``NaN``\ であるかどうかをチェックするには\ ``isNaN``\ 関数を使う。\ ``NaN``\ はもう一つの（そして最後の）変換したら\ ``false``\ となる値である。

..  These automatic conversions can be very convenient, but they are also rather weird and error prone. Even though + and * are both arithmetic operators, they behave completely different in the example. In my own code, I use + to combine strings and non-strings a lot, but make it a point not to use * and the other numeric operators on string values. Converting a number to a string is always possible and straightforward, but converting a string to a number may not even work (as in the last line of the example). We can use Number to explicitly convert the string to a number, making it clear that we might run the risk of getting a NaN value.

これらの自動的な変換はとても便利なものであり得るが、しかしむしろ奇異と誤りももたらす。\ ``+``\ と\ ``*``\ はともに数学演算子であるにもかかわらず、例では全く異なった振る舞いをした。私自身のコードでは、\ ``+``\ を文字列と文字列でない値の連結に数多く使っているが、\ ``*``\ や他の数学演算子を文字列には使わないようにしている。数値から文字列への変換は常に可能でまっすぐだが、文字列から数値への変換はしばしばうまく動かない（例の最後の行のように）。文字列から数字への明示的な変換には\ ``Number``\ 関数を使うことができ、それにより\ ``NaN``\ 値が得られることによるリスクをクリアすることができる。

.. code-block:: javascript

    show(Number("5") * 5);

|hr|

..  When we discussed the boolean operators && and || earlier, I claimed they produced boolean values. This turns out to be a bit of an oversimplification. If you apply them to boolean values, they will indeed return booleans. But they can also be applied to other kinds of values, in which case they will return one of their arguments.

先ほどの\ ``&&``\ と\ ``||``\ という真偽値の演算子の話で、私はそれらが真偽値を作ると主張した。これはちょっと単純化しすぎた話でもある。もしこれらを真偽値に適用したら、真偽値が返ってくる。しかし、他の種類の値に適用したら、これらの引数のどちらかが帰ってくる。

..  What || really does is this: It looks at the value to the left of it first. If converting this value to a boolean would produce true, it returns this left value, and otherwise it returns the one on its right. Check for yourself that this does the correct thing when the arguments are booleans. Why does it work like that? It turns out this is very practical. Consider this example:

``||``\ は実際はこのようなものだ：左のものを最初として値を見る。これを真偽値に変換して\ ``true``\ になったら左の値を返し、そうでなければ右の値を返す。引数を真偽値にしたときこれが正しいことをチェックしてみてほしい。なぜこんな動きになるのか？しかし実用的ではある。この例を考えてみて欲しい。:

.. code-block:: javascript

    var input = prompt("What is your name?", "Kilgore Trout");
    print("Well hello " + (input || "dear"));

..  If the user presses 'Cancel' or closes the prompt dialog in some other way without giving a name, the variable input will hold the value null or "". Both of these would give false when converted to a boolean. The expression input || "dear" can in this case be read as 'the value of the variable input, or else the string "dear"'. It is an easy way to provide a 'fallback' value.

ユーザーが'Cancel'を押すか、他の方法で名前を書かずに\ ``prompt``\ のダイアログを閉じたら、変数の値は\ ``null``\ もしくは\ ``""``\ となる。どちらの場合も真偽値に変換された時には偽になる。\ ``input || "dear"``\ という式はこの場合、'変数\ ``input``\ の値か、もしくは\ ``"dear"``\ という文字列'である。これは'代替'の値を提供する簡単な手だ。

..  The && operator works similarly, but the other way around. When the value to its left is something that would give false when converted to a boolean, it returns that value, and otherwise it returns the value on its right.

``&&``\ は同様な、しかし別の働きをする。左側の値が真偽値に変換したときに\ ``false``\ になる何らかの値であれば、その値を返し、そうなければ右の値を返すのである。

..  Another property of these two operators is that the expression to their right is only evaluated when necessary. In the case of true || X, no matter what X is, the result will be true, so X is never evaluated, and if it has side effects they never happen. The same goes for false && X.

これら2つの演算子の他の特徴は右側の式は必要になってからだけ評価されるということである。\ ``true || X``\ の場合、\ ``X``\ が何であるかにかかわらず、結果は\ ``true``\ となり、そして\ ``X``\ は評価されることがなく、もしそれにside effectがあってもそれは起きない。\ ``false && X``\ についても同様である。

.. code-block:: javascript

    false || alert("I'm happening!");
    true || alert("Not me.");


..  Bits are any kinds of two-valued things, usually described as <code>0</code>s and <code>1</code>s. Inside the computer, they take forms like a high or low electrical charge, a strong or weak signal, a shiny or dull spot on the surface of a CD.
..  If you were expecting something like <code>10010000</code> here ― good call, but read on. JavaScript's numbers are not stored as integers.
..  Actually, 53, because of a trick that can be used to get one bit for free. Look up the 'IEEE 754' format if you are curious about the details.
..  When you type string values at the console, you'll notice that they will come back with the quotes and backslashes the way you typed them.To get special characters to show properly, you can do <code>print(&quot;a\nb&quot;)</code>― why this works, we will see in a moment.
..  The bit bucket is supposedly the place where old bits are kept. On some systems, the programmer has to manually empty it now and then. Fortunately, JavaScript comes with a fully-automatic bit-recycling system.

.. rubric:: 脚注

.. [#f1] ビットとは2種類の値を持つもので、通常\ ``0``\ と\ ``1``\ とで表される。コンピュータの中では、電圧の高低、信号の強弱といったような形をとり、CDの面においては反射の善し悪しという形を取る。
.. [#f2] ここで、もし\ ``10010000``\ であることを期待していたのであれば、いい読みである。が、そのまま読み続けよう。JavaScriptの数は整数として格納されないのだ。
.. [#f3] 本当だ。53であるのは1ビットを未使用にしているからだ。詳細が気になるなら'IEEE 754'フォーマットを探そう。
.. [#f4] 文字列の値をコンソールにタイプする時、引用符とバックスラッシュをタイプして戻るように注意しよう。特殊文字を明示的に表すため、\ ``print ('a\nb')``\ とすることもできる。―なぜこれが動くのか、後で説明しよう。
.. [#f5] ビットのバケツにはおそらく古いビットが溜まっている場所になっている。あるシステムでは、プログラマは手作業でそれを空にしなければならなかった。幸運にも、JavaScriptは全自動でビットをリサイクルするシステムを備えている。
