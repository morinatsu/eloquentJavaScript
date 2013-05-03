=====================================================
関数
=====================================================
..  Chapter 3:
..  Functions

..  A program often needs to do the same thing in different places. Repeating all the necessary statements every time is tedious and error-prone. It would be better to put them in one place, and have the program take a detour through there whenever necessary. This is what functions were invented for: They are canned code that a program can go through whenever it wants. Putting a string on the screen requires quite a few statements, but when we have a print function we can just write print("Aleph") and be done with it.

プログラムはしばしば異なる場所で同じ事をすることを必要とする。必要な全ての文を毎回繰り返すのは退屈で失敗しがちだ。これらを1カ所にまとめておけて、必要なときにプログラムから呼び出せればもっと良いだろう。このために関数が発明された：これによりプログラムがひつようとするたびにコードを呼び出せるようになる。スクリーンに文字列を出力するのには少々の文しか必要としないが、しかし\ ``print``\ 関数があることで、我々は\ ``print("Aleph")``\ と書くだけでよくなり、そのようにしている。

..  To view functions merely as canned chunks of code doesn't do them justice though. When needed, they can play the role of pure functions, algorithms, indirections, abstractions, decisions, modules, continuations, data structures, and more. Being able to effectively use functions is a necessary skill for any kind of serious programming. This chapter provides an introduction into the subject, chapter 6 discusses the subtleties of functions in more depth.

関数を単なるコードの塊と見るのは正しくない。必要なら、これらは純粋な関数、アルゴリズム、回り道、抽象、判断、加群、継続そしてデータ構築その他といった役割を果たす。関数を効果的に使えるようになるにはプログラミングのまじめな種類の技術が必要となる。この章はこの課題への導入を提供し、\ :doc:`6章</Functional Programming>`\ でより深いことを論じよう。

.. |hr| raw:: html

   <hr>

|hr|

..  Pure functions, for a start, are the things that were called functions in the mathematics classes that I hope you have been subjected to at some point in your life. Taking the cosine or the absolute value of a number is a pure function of one argument. Addition is a pure function of two arguments.

純粋な関数、あなたが人生のどこかで学んでいると期待して数学的クラスの関数から始めよう。コサインや数の絶対値といったものは1つの引数を取る純粋な関数である。加算は2つの引数を取る純粋な関数だ。

..  The defining properties of pure functions are that they always return the same value when given the same arguments, and never have side effects. They take some arguments, return a value based on these arguments, and do not monkey around with anything else.

純粋な関数の定義は同じ引数が与えられたら常に同じ結果を返すことで、side effectを持たない。引数を取り、これらの引数を元に値を返し、それ以外の余計なことはしない。

..  In JavaScript, addition is an operator, but it could be wrapped in a function like this (and as pointless as this looks, we will come across situations where it is actually useful):

JavaScriptにおいて、加算は演算子であるが、しかしこれに似たものを関数にラップすることができる。（無意味に見えるが、これは様々な状況で実に便利なのだ）:

.. code-block:: javascript

    function add(a, b) {
      return a + b;
    }

    show(add(2, 2));

..  add is the name of the function. a and b are the names of the two arguments. return a + b; is the body of the function.

``add``\ はこの関数の名前である。\ ``a``\ と\ ``b``\ は2つの引数の名前である。\ ``return a + b;``\ はこの関数の実体である。

..  The keyword function is always used when creating a new function. When it is followed by a variable name, the resulting function will be stored under this name. After the name comes a list of argument names, and then finally the body of the function. Unlike those around the body of while loops or if statements, the braces around a function body are obligatory1.

``function``\ キーワードは新しい関数の作成で常に使われる。その後に変数名が続いたとき、この名前のもとに結果の関数が格納される。この名前の次には引数名のリストが続き、それから最後に関数の実体が続く。\ ``while``\ ループや\ ``if``\ 文のものと異なり、関数の実体の周りの中括弧は必須である\ [#f1]_\ 。

..  The keyword return, followed by an expression, is used to determine the value the function returns. When control comes across a return statement, it immediately jumps out of the current function and gives the returned value to the code that called the function. A return statement without an expression after it will cause the function to return undefined.

``return``\ キーワードには、式が続き、関数の戻り値を決める。制御が\ ``return``\ 文に来たら、現在の関数から抜け出して関数を呼び出したコードに値が返される。\ ``return``\ 文に式が無ければ関数は\ ``undefined``\ を返す。

..  A body can, of course, have more than one statement in it. Here is a function for computing powers (with positive, integer exponents):

本体にはもちろん1つ以上の文を含めることができる。これは累乗を計算する関数である（正の整数の指数による）:

.. code-block:: javascript

    function power(base, exponent) {
      var result = 1;
      for (var count = 0; count < exponent; count++)
        result *= base;
      return result;
    }

    show(power(2, 10));

..  If you solved exercise 2.2, this technique for computing a power should look familiar.

:ref:`演習2.2 <EX22>` を解いていれば、累乗の計算のためのこのテクニックは見慣れているだろう。

..  Creating a variable (result) and updating it are side effects. Didn't I just say pure functions had no side effects?

変数（\ ``result``\ ）を作り更新するのはside effectである。純粋な関数にはside effectはないと言わなかっただろうか？

..  A variable created inside a function exists only inside the function. This is fortunate, or a programmer would have to come up with a different name for every variable he needs throughout a program. Because result only exists inside power, the changes to it only last until the function returns, and from the perspective of code that calls it there are no side effects.

関数の中で作られた変数は関数の中でしか存在しない。これは幸運なことに、プログラマーがプログラム全体を通じて必要なだけの全ての変数に違う名前を設定する必要は無い。\ ``result``\ は\ ``power``\ の中にしか存在しないので、この変更は関数から戻ってくるまでのものとなり、それを呼び出すコードの見通しにはside effectはない。

|hr|

演習 3.1

..  Write a function called absolute, which returns the absolute value of the number it is given as its argument. The absolute value of a negative number is the positive version of that same number, and the absolute value of a positive number (or zero) is that number itself.

引数として与えられた数値の絶対値を返す、\ ``absolute``\ 関数を書け。負の数の絶対値は同じ数値を正の数にしたもので、正の数（または0）の絶対値はその数そのものである。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function absolute(number) {
      if (number < 0)
        return -number;
      else
        return number;
    }

    show(absolute(-144));

|hr|

..  Pure functions have two very nice properties. They are easy to think about, and they are easy to re-use.

純粋な関数には2つのとてもいい特徴がある。考えるのに簡単で、再利用しやすいことだ。

..  If a function is pure, a call to it can be seen as a thing in itself. When you are not sure that it is working correctly, you can test it by calling it directly from the console, which is simple because it does not depend on any context2. It is easy to make these tests automatic ― to write a program that tests a specific function. Non-pure functions might return different values based on all kinds of factors, and have side effects that might be hard to test and think about.

関数が純粋なものなら、それを呼び出すときはそれ自身だけを見ればいい。正しく動いていないようであれば、コンソールから直接呼び出してテストすることができ、背景に依存しないために単純である\ [#f2]_\ 。テストを自動的に行うのも簡単だ―個々の関数をテストするプログラムを書くのだ。純粋で無い関数は全ての要素によって異なる値を返すかも知れず、テストするのは難しくしうるside effectを持つ。

..  Because pure functions are self-sufficient, they are likely to be useful and relevant in a wider range of situations than non-pure ones. Take show, for example. This function's usefulness depends on the presence of a special place on the screen for printing output. If that place is not there, the function is useless. We can imagine a related function, let's call it format, that takes a value as an argument and returns a string that represents this value. This function is useful in more situations than show.

純粋な関数は自己充足的であるため、純粋でないものより、便利で広い範囲の状況において適切だ。例として\ ``show``\ を取り上げる。この関数の便利なところはプリント出力するスクリーンが特別な場所にあることによる。もしこのような場所になければ、この関数は不便だ。\ ``format``\ という似たような関数のことを想像しよう、値を引数として取り、この値を返す表現する文字列を返す。この関数はより多くの状況で\ ``show``\ より便利だ。

..  Of course, format does not solve the same problem as show, and no pure function is going to be able to solve that problem, because it requires a side effect. In many cases, non-pure functions are precisely what you need. In other cases, a problem can be solved with a pure function but the non-pure variant is much more convenient or efficient.

もちろん、\ ``format``\ は\ ``show``\ と同じ問題を解決するものではなく、純粋でない関数は問題を解くためにside effectを必要とする。多くの場合において、純粋でない関数はその必要に正確だ。そうでない場合には、問題は純粋な関数によって解かれ、純粋でないものはより便利で効果的な場合に使われるだろう。

..  Thus, when something can easily be expressed as a pure function, write it that way. But never feel dirty for writing non-pure functions.

だから、純粋な関数で表現することが簡単であるときは、そのやり方で書こう。しかし純粋でない関数を汚いもののように思わないこと。

|hr|

..  Functions with side effects do not have to contain a return statement. If no return statement is encountered, the function returns undefined.

side effectのある関数は\ ``return``\ 文を含まなくてもよい。もし、\ ``return``\ 文に出会わなければ、関数は\ ``undefined``\ を返す。

.. code-block:: javascript

    function yell(message) {
      alert(message + "!!");
    }

    yell("Yow");

|hr|

..  The names of the arguments of a function are available as variables inside it. They will refer to the values of the arguments the function is being called with, and like normal variables created inside a function, they do not exist outside it. Aside from the top-level environment, there are smaller, local environments created by function calls. When looking up a variable inside a function, the local environment is checked first, and only if the variable does not exist there is it looked up in the top-level environment. This makes it possible for variables inside a function to 'shadow' top-level variables that have the same name.

関数の引数の名前はその中の変数と同じように使える。関数の引数の値はそれが呼び出されたときの値を参照し、関数の中で作られた通常の変数と同様に、関数の外には存在しない。トップレベルの環境の他に、より小さな、ローカルな環境が関数呼び出しによって作られる。

.. code-block:: javascript

    function alertIsPrint(value) {
      var alert = print;
      alert(value);
    }

    alertIsPrint("Troglodites");

..  The variables in this local environment are only visible to the code inside the function. If this function calls another function, the newly called function does not see the variables inside the first function:

このローカルな環境の中の変数は関数の中のコードでしか見えない。もしこの関数が他の関数から呼び出されたら、新しく呼び出された関数は最初の関数の中の変数を見ない。:

.. code-block:: javascript

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

..  However, and this is a subtle but extremely useful phenomenon, when a function is defined inside another function, its local environment will be based on the local environment that surrounds it instead of the top-level environment.

しかしながら、これは巧妙だがとても便利な現象であり、関数が他の関数の\ *中で*\ 定義された時、そのローカルな環境はトップレベルのものの代わりに、それを取り巻くローカルな環境がもとになっている。

.. code-block:: javascript

    var variable = "top-level";
    function parentFunction() {
      var variable = "local";
      function childFunction() {
        print(variable);
      }
      childFunction();
    }
    parentFunction();

..  What this comes down to is that which variables are visible inside a function is determined by the place of that function in the program text. All variables that were defined 'above' a function's definition are visible, which means both those in function bodies that enclose it, and those at the top-level of the program. This approach to variable visibility is called lexical scoping.

これがもたらすのは、関数の中で見える変数はプログラムテキストの中で関数が書かれている場所によって決まるということだ。関数の定義の'上'で定義された全ての変数は見ることができ、これは関数の実体の中に入れられたものと、トップレベルのものと両方を意味する。この変数の参照へのアプローチを語彙的なスコーピングと呼ばれる。

|hr|

..  People who have experience with other programming languages might expect that a block of code (between braces) also produces a new local environment. Not in JavaScript. Functions are the only things that create a new scope. You are allowed to use free-standing blocks like this...

他のプログラミング言語の経験がある人々はブロックの中のコード（中括弧の中）もまた新しくローカルな環境を作ることを期待するかもしれない。JavaScriptでは異なる。関数のみが新しいスコープを作る。このような自由なブロックを使うことは許されている...。

.. code-block:: javascript

    var something = 1;
    {
      var something = 2;
      print("Inside: " + something);
    }
    print("Outside: " + something);

..  ... but the something inside the block refers to the same variable as the one outside the block. In fact, although blocks like this are allowed, they are utterly pointless. Most people agree that this is a bit of a design blunder by the designers of JavaScript, and ECMAScript Harmony will add some way to define variables that stay inside blocks (the let keyword).

…しかしブロックの中の\ ``something``\ はブロックの外側のものと同じものを参照している。実際、このようなブロックは許されてはいるが、まったく使い途がない。多くの人々はJavaScriptの設計上の些細な失敗だと同意しており、ECMAScript Harmonyではブロックの中での変数定義を行う手段が追加される。（\ ``let``\ キーワード）

|hr|

..  Here is a case that might surprise you:

これはあなたを驚かせるかもしれない:

.. code-block:: javascript

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

..  parentFunction returns its internal function, and the code at the bottom calls this function. Even though parentFunction has finished executing at this point, the local environment where variable has the value "local" still exists, and childFunction still uses it. This phenomenon is called closure.

``paretntFunction``\ はその内部の関数を\ *返し*\ 、下のコードがこの関数を呼び出す。\ ``parentFunction``\ はここでは既に実行が終了しているにもかかわらず、\ ``variable``\ が\ ``"local"``\ の値を持っているローカル環境は未だ存在し、そして\ ``childFunction``\ はそれを使うことができる。この現象はクロージャと呼ばれる。

|hr|

..  Apart from making it very easy to quickly see in which part of a program a variable will be available by looking at the shape of the program text, lexical scoping also allows us to 'synthesise' functions. By using some of the variables from an enclosing function, an inner function can be made to do different things. Imagine we need a few different but similar functions, one that adds 2 to its argument, one that adds 5, and so on.

プログラムのその部分を早く見るのがそれによって簡単になることを差し置いても、変数がプログラムテキストの形を見ることで、レキシカルスコーピングもまた関数の'同期'を許す。円クロージングされた関数の中の変数を使うことにより、内側の関数も異なる働きをする。少しだけ異なる同様な関数が必要なとき、引数に2を足す関数と5を足す関数が必要な時を想像しよう。

.. code-block:: javascript

    function makeAddFunction(amount) {
      function add(number) {
        return number + amount;
      }
      return add;
    }

    var addTwo = makeAddFunction(2);
    var addFive = makeAddFunction(5);
    show(addTwo(1) + addFive(1));

|hr|

..  On top of the fact that different functions can contain variables of the same name without getting tangled up, these scoping rules also allow functions to call themselves without running into problems. A function that calls itself is called recursive. Recursion allows for some interesting definitions. Look at this implementation of power:

異なる関数が同じ名前の変数をこんがらがることなしに含むことができるときという事実の上では、スコーピングのルールもまた実行の問題なしに呼び出されうる関数を許す。\ *それ自身*\ を呼び出す関数は再帰と呼ばれる。再帰は面白い定義を許す。\ ``power``\ の以下の実装を見てみよう。:

.. code-block:: javascript

    function power(base, exponent) {
      if (exponent == 0)
        return 1;
      else
        return base * power(base, exponent - 1);
    }

..  This is rather close to the way mathematicians define exponentiation, and to me it looks a lot nicer than the earlier version. It sort of loops, but there is no while, for, or even a local side effect to be seen. By calling itself, the function produces the same effect.

これはむしろ数学者による定義の解説に近く、私にとっては今までのものよりとてもよく見える。ループを整理し、しかし\ ``while``\ 、\ ``for``\ はなく、ローカルなside effectがあるようにも見えない。自分自身を呼び出すことで、この関数は同じ効果を作り出す。

..  There is one important problem though: In most browsers, this second version is about ten times slower than the first one. In JavaScript, running through a simple loop is a lot cheaper than calling a function multiple times.

1つ重要な問題がある：多くのブラウザで、この2つめのバージョンは最初のものより10倍近く遅い。JavaScriptでは、単純なループを走らせる方が、関数を何度も呼び出すよりとても安価なのだ。

|hr|

..  The dilemma of speed versus elegance is an interesting one. It not only occurs when deciding for or against recursion. In many situations, an elegant, intuitive, and often short solution can be replaced by a more convoluted but faster solution.

スピード対エレガンスさのジレンマはとても面白いものの一つだ。forと再帰の判断に留まらない。多くの状況で、エレガントであること、直感的であること、そして時には短い解決が、入り組んだしかし速い解決に置き換えられうる。

..  In the case of the power function above the un-elegant version is still sufficiently simple and easy to read. It doesn't make very much sense to replace it with the recursive version. Often, though, the concepts a program is dealing with get so complex that giving up some efficiency in order to make the program more straightforward becomes an attractive choice.

``power``\ 関数の場合はエレガントでないバージョンもまだとても単純で読みやすいものであった。再帰のバージョンでこれを入れ替えるのはセンスが良いとは言えない。しばしば、にもかかわらず、プログラムをまっすぐなものにするために、プログラムに複雑さをもたらす効率性をあきらめることは魅力的な選択になる。

..  The basic rule, which has been repeated by many programmers and with which I wholeheartedly agree, is to not worry about efficiency until your program is provably too slow. When it is, find out which parts are too slow, and start exchanging elegance for efficiency in those parts.

多くのプログラマーによって繰り返され、完全に同意されてきた基本的なルールは、プログラムが遅すぎると証明されるまで効率のことは気にすべきでないということだ。その時、遅すぎると分った部分についてだけ、エレガンスさを効率性に置き換え始めれば良いのだ。

..  Of course, the above rule doesn't mean one should start ignoring performance altogether. In many cases, like the power function, not much simplicity is gained by the 'elegant' approach. In other cases, an experienced programmer can see right away that a simple approach is never going to be fast enough.

もちろん、上記のルールはパフォーマンスのことを全く無視して良いということを意味していない。多くの場合において、\ ``power``\ 関数のようには、'エレガントな'アプローチによっては十分な単純さが得られない。他の場合、経験のあるプログラマーは単純なアプローチは十分に速くならないと考えることもある。

..  The reason I am making a big deal out of this is that surprisingly many programmers focus fanatically on efficiency, even in the smallest details. The result is bigger, more complicated, and often less correct programs, which take longer to write than their more straightforward equivalents and often run only marginally faster.

このような大きな話をする理由は、驚くほど多くのプログラマーが細かいところにおいてもファンタジックに効率性にフォーカスしていることにある。その結果は大きく、複雑で、しばしば正しさに欠けるプログラムであり、よりまっすぐで等価値のものよりも書くのに長くかかり、かつしばしばほんの少ししか速くなっていない。

|hr|

..  But I was talking about recursion. A concept closely related to recursion is a thing called the stack. When a function is called, control is given to the body of that function. When that body returns, the code that called the function is resumed. While the body is running, the computer must remember the context from which the function was called, so that it knows where to continue afterwards. The place where this context is stored is called the stack.

しかし再帰の話に戻ろう。再帰の概念はスタックというものに密接に関係している。関数が呼ばれたとき、制御は関数の実体に移る。実体が帰ってきたとき、関数を呼んだところから再開する。実体が実行されているとき、コンピューターはその関数が呼ばれたときの背景を記憶していなければならない、後でどこから続きを行うべきか知らなければならない。この背景を格納する場所がスタックである。

..  The fact that it is called 'stack' has to do with the fact that, as we saw, a function body can again call a function. Every time a function is called, another context has to be stored. One can visualise this as a stack of contexts. Every time a function is called, the current context is thrown on top of the stack. When a function returns, the context on top is taken off the stack and resumed.

これがスタックと呼ばれるものが、見てきたように、関数の実体が関数を再度呼び出したときに実際にやっていることだ。関数が呼ばれる度に、別の背景が格納される。背景の積み重ねとしてこれを見ることができる。関数が呼ばれる度に、この重なりの一番上に現在の背景が投げ込まれる。関数から戻ったとき、重なりの一番上から背景が取られて再開が始まる。

..  This stack requires space in the computer's memory to be stored. When the stack grows too big, the computer will give up with a message like "out of stack space" or "too much recursion". This is something that has to be kept in mind when writing recursive functions.

このスタックを格納するためにコンピューターのメモリ上のスペースを必要とする。スタックは大きくなりすぎたとき、コンピュータは"スタックのスペースが尽きた"とか"再帰しすぎ"のようなメッセージとともにギブアップするだろう。

.. code-block:: javascript

    function chicken() {
      return egg();
    }
    function egg() {
      return chicken();
    }
    print(chicken() + " came first.");

..  In addition to demonstrating a very interesting way of writing a broken program, this example shows that a function does not have to call itself directly to be recursive. If it calls another function which (directly or indirectly) calls the first function again, it is still recursive.

壊れたプログラムを書くとても面白い方法をこのデモンストレーションに付け加えてあり、この例では関数が直接自分自身を再帰的に呼び出すのではない。もしある関数が1つめの関数を再び呼ぶ他の関数を（直接にせよ間接にせよ）呼び出すようにしたら、それもまた再帰である。

|hr|

..  Recursion is not always just a less-efficient alternative to looping. Some problems are much easier to solve with recursion than with loops. Most often these are problems that require exploring or processing several 'branches', each of which might branch out again into more branches.

再帰は常にループのより効率的でない代替であるというわけではない。幾つかの問題はループより再帰の方が簡単に解決できる。よくあるのは探索や幾つかに別れる'枝'を処理する必要のある問題であり、枝のそれぞれがまたさらに枝分かれしているというものである。

..  Consider this puzzle: By starting from the number 1 and repeatedly either adding 5 or multiplying by 3, an infinite amount of new numbers can be produced. How would you write a function that, given a number, tries to find a sequence of additions and multiplications that produce that number?

このパズルについて考える：1という数から始めて5を足すか3をかけることを繰り返せば、無限に新しい値が作られ続ける。数値が与えられたときに、その数値を作るのにどのような順番で足し算とかけ算が行われたのか探してみる関数はどう書けば良いだろうか。

..  For example, the number 13 could be reached by first multiplying 1 by 3, and then adding 5 twice. The number 15 can not be reached at all.

例えば、13という数は最初に1に3をかけ、5を2回足す。15という数には到達不可能である。

..  Here is the solution:

これが解答である:

.. code-block:: javascript

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

..  Note that it doesn't necessarily find the shortest sequence of operations, it is satisfied when it finds any sequence at all.

もっとも短い演算を探す必要はなく、任意のものが見つかれば十分であることに注意。

..  The inner find function, by calling itself in two different ways, explores both the possibility of adding 5 to the current number and of multiplying it by 3. When it finds the number, it returns the history string, which is used to record all the operators that were performed to get to this number. It also checks whether the current number is bigger than goal, because if it is, we should stop exploring this branch, it is not going to give us our number.

``find``\ 関数の中で、自分自身を2つのことなるやり方で呼び出し、5の加算と3のかけ算の両方の探索を行っている。もし数値が見つかれば、\ ``history``\ の文字列を返し、それにはこの数値を選るまでに行った全ての演算が記録されている。現在の値が\ ``goal``\ より大きいかのチェックも行われており、それは与えた数に至らない枝である場合に探索を打ち切るためにある。

..  The use of the || operator in the example can be read as 'return the solution found by adding 5 to start, and if that fails, return the solution found by multiplying start by 3'. It could also have been written in a more wordy way like this:

この例における\ ``||``\ 演算子の用法は'\ ``start``\ に5を加算する解決を探して返し、もしそれに失敗したら、\ ``start``\ に3を積算する解決を探してそれを返す'である。このように、より言葉のように書くこともできる。

.. code-block:: javascript

    else {
      var found = find(start + 5, "(" + history + " + 5)");
      if (found == null)
        found = find(start * 3, history + " * 3");
      return found;
    }

|hr|

..  Even though function definitions occur as statements between the rest of the program, they are not part of the same time-line:

関数定義がプログラムの残りの部分にあるにも係わらず、これらは同じタイムラインにはない:

.. code-block:: javascript

    print("The future says: ", future());

    function future() {
      return "We STILL have no flying cars.";
    }

..  What is happening is that the computer looks up all function definitions, and stores the associated functions, before it starts executing the rest of the program. The same happens with functions that are defined inside other functions. When the outer function is called, the first thing that happens is that all inner functions are added to the new environment.

何が起こっているかというと、コンピューターが関数定義を探し、結びついた関数を格納するのは、プログラムの残りの部分の実行を開始する\ *前に*\ 行われる。関数が他の関数の中で定義されているときも同じようなことが起こる。外側の関数が呼び出されたとき、最初に起こるのは全ての内側の関数が新しい環境の中に付け加えられることである。

|hr|

..  There is another way to define function values, which more closely resembles the way other values are created. When the function keyword is used in a place where an expression is expected, it is treated as an expression producing a function value. Functions created in this way do not have to be given a name (though it is allowed to give them one).

関数の値を定義する別の方法は、他の値を作るときの方法にもっと似ている。\ ``function``\ キーワードが式が必要な場所で使われたとき、それは関数の値を作り出す一つの式として扱われる。この方法で作られた関数は名前が与えられていなくてもよい（与えても良い）。

.. code-block:: javascript

    var add = function(a, b) {
      return a + b;
    };
    show(add(5, 5));

..  Note the semicolon after the definition of add. Normal function definitions do not need these, but this statement has the same general structure as var add = 22;, and thus requires the semicolon.

``add``\ の定義の後のセミコロンに注意。通常の関数定義では必要ないが、この文は\ ``var add = 22;``\ と同様の全体構造を持ち、セミコロンを必要とする。

..  This kind of function value is called an anonymous function, because it does not have a name. Sometimes it is useless to give a function a name, like in the makeAddFunction example we saw earlier:

この種類の関数値は名前を持たないため、無名関数と呼ばれる。先ほどの\ ``makeAddFunction``\ のように関数名を与えても使い途がないこともある。:

.. code-block:: javascript

    function makeAddFunction(amount) {
      return function (number) {
        return number + amount;
      };
    }

..  Since the function named add in the first version of makeAddFunction was referred to only once, the name does not serve any purpose and we might as well directly return the function value.

最初のバージョンの\ ``makeAddFunction``\ で\ ``add``\ と名付けられた関数は一度しか参照されていなかったので、その名前は何の役にも立っておらず、その関数値を直接返すこともできる。

|hr|

演習 3.2

..  Write a function greaterThan, which takes one argument, a number, and returns a function that represents a test. When this returned function is called with a single number as argument, it returns a boolean: true if the given number is greater than the number that was used to create the test function, and false otherwise.

1つの引数を取り、その引数より大きいことをテストする関数を返す\ ``greaterThan``\ 関数を書け。返された関数が1つ数値の引数として呼び出されたとき、それは真偽値を返す：与えられた数値がそのテスト関数が作られたときに使われた数値より大きければ\ ``true``\ を、そうでなければ\ ``false``\ を返す。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function greaterThan(x) {
      return function(y) {
        return y > x;
      };
    }

    var greaterThanTen = greaterThan(10);
    show(greaterThanTen(9));

|hr|

..  Try the following:

下記を試してみよう。:

.. code-block:: javascript

    alert("Hello", "Good Evening", "How do you do?", "Goodbye");

..  The function alert officially only accepts one argument. Yet when you call it like this, the computer does not complain at all, but just ignores the other arguments.

``alert``\ 関数は公式には1つだけの引数を取る。このように呼び出されたとき、コンピュータは全く文句を言わずに、しかし他の引数をただ無視する。

.. code-block:: javascript

    show();

..  You can, apparently, even get away with passing too few arguments. When an argument is not passed, its value inside the function is undefined.

見たとおり、少なすぎる引数を与えることもできる。引数が与えられなかったとき、関数の中でその値は\ ``undefined``\ となる。

..  In the next chapter, we will see a way in which a function body can get at the exact list of arguments that were passed to it. This can be useful, as it makes it possible to have a function accept any number of arguments. print makes use of this:

次の章では、関数の実体が渡された引数のリストをどのように得るのか正確に見てみよう。これは多くの数の引数を受け取ることのできる関数を作るときに便利だ。\ ``print``\ はこのようにも使える:

.. code-block:: javascript

    print("R", 2, "D", 2);

..  Of course, the downside of this is that it is also possible to accidentally pass the wrong number of arguments to functions that expect a fixed amount of them, like alert, and never be told about it.

もちろん、\ ``alert``\ のように、間違って期待されていたものと異なる誤った数の引数が関数に渡されるというよろしくない面もあるが、それについては語らない。


.. Technically, this wouldn't have been necessary, but I suppose the designers of JavaScript felt it would clarify things if function bodies always had braces.
.. Technically, a pure function can not use the value of any external variables. These values might change, and this could make the function return a different value for the same arguments. In practice, the programmer may consider some variables 'constant' ― they are not expected to change ― and consider functions that use only constant variables pure. Variables that contain a function value are often good examples of constant variables.

.. rubric:: 脚注

.. [#f1] 技術的には、これは必要でないが、しかし私はJavaScriptの設計者は関数本体が常に括弧を持てば物事が明らかになると思ったと考える。
.. [#f2] 技術的には、純粋な関数は外部の変数の値を使うことができない。これらの値は変更されるかもしれず、それにより関数が同じ引数に対して異なる値を返すことになる。実用上は、プログラマーはある変数を'コンスタント'―それらは変更されないと期待される―と考えることができ、コンスタントしか使わない関数は純粋であると考える。関数の値を含む変数がしばしばコンスタント変数のいい例である。
