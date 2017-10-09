=====================================================
エラー・ハンドリング
=====================================================
..  Chapter 5:
..  Error Handling

..  Writing programs that work when everything goes as expected is a good start. Making your programs behave properly when encountering unexpected conditions is where it really gets challenging.

全てが期待通りであるときに動くプログラムを書くのは良いスタートだ。期待していない条件に遭遇したときに行儀良く振る舞うプログラムにすることは本当のチャレンジである。

..  The problematic situations that a program can encounter fall into two categories: Programmer mistakes and genuine problems. If someone forgets to pass a required argument to a function, that is an example of the first kind of problem. On the other hand, if a program asks the user to enter a name and it gets back an empty string, that is something the programmer can not prevent.

プログラムが遭遇しうる問題のある状況は2つのカテゴリに分けられる：プログラマーのミスと本来の問題だ。もし誰かが必要な引数を関数に渡すのを忘れたら、それが1つめの種類の問題の例だ。他方、もしプログラムがユーザーに名前を入力するようにと質問し、そして空の文字列が帰ってきたとしたら、それはプログラマーが防止できる問題ではない。

..  In general, one deals with programmer errors by finding and fixing them, and with genuine errors by having the code check for them and perform some suitable action to remedy them (for example, asking for the name again), or at least fail in a well-defined and clean way.

一般的に、プログラマーの誤りというのは見つけ修正することができ、本来の誤りはに対しては、コードをチェックし回復するためにふさわしいアクションを取るか（例えば、名前を聞き直すとか）、さもなくば最後には想定された、きれいなやりかたで失敗する。

.. |hr| raw:: html

   <hr>

|hr|

..  It is important to decide into which of these categories a certain problem falls. For example, consider our old power function:

重要なのはプログラムが陥ったのはこれらのうちどちらのカテゴリーのものであるか確実に判断することだ。例えば、我々の古い\ ``power``\ 関数について考えよう:

.. code-block:: javascript

    function power(base, exponent) {
      var result = 1;
      for (var count = 0; count < exponent; count++)
        result *= base;
      return result;
    }
    
..  When some geek tries to call power("Rabbit", 4), that is quite obviously a programmer error, but how about power(9, 0.5)? The function can not handle fractional exponents, but, mathematically speaking, raising a number to the halfth power is perfectly reasonable (Math.pow can handle it). In situations where it is not entirely clear what kind of input a function accepts, it is often a good idea to explicitly state the kind of arguments that are acceptable in a comment.

ギークが\ ``power("Rabbit", 4)``\ と呼び出してみたとき、それは明らかにプログラマーの誤りだが、しかし\ ``power(9, 0.5)``\ についてはどうだろうか？この関数は小数の累乗を扱うことができない、しかし、数学的な話としては、小数の累乗というのは完全に理屈に合っている（\ ``Math.pow``\ は扱える）。完全にクリアではない種類の入力を関数が受けうる状況において、コメントに受け入れ可能な引数の種類書いておくのはしばしば良いアイデアである。

|hr|

..  If a function encounters a problem that it can not solve itself, what should it do? In chapter 4 we wrote the function between:

もし関数がそれ自体では解決不能な問題に遭遇したら、どのようにすべきか？\ :doc:`4章</Data structures>`\ で書いた\ ``between``\ 関数では:

.. code-block:: javascript

    function between(string, start, end) {
      var startAt = string.indexOf(start) + start.length;
      var endAt = string.indexOf(end, startAt);
      return string.slice(startAt, endAt);
    }

..  If the given start and end do not occur in the string, indexOf will return -1 and this version of between will return a lot of nonsense: between("Your mother!", "{-", "-}") returns "our mother".

もし、文字列に存在しない\ ``start``\ と\ ``end``\ が与えられたら、\ ``indexOf``\ は\ ``-1``\ を返しこのバージョンの\ ``between``\ はナンセンスに満ちた値を返すだろう：\ ``between("Your mother!", "{-", "-}")``\ は\ ``"our mother"``\ を返す。

..  When the program is running, and the function is called like that, the code that called it will get a string value, as it expected, and happily continue doing something with it. But the value is wrong, so whatever it ends up doing with it will also be wrong. And if you are unlucky, this wrongness only causes a problem after having passed through twenty other functions. In cases like that, it is extremely hard to find out where the problem started.

このプログラムの実行中、期待された通りに文字列の値が設定されて、関数が呼ばれたため、幸運にも処理を続けることができる。しかし値は誤っていて、なんであろうと終了し、結果も誤っているだろう。
もし不幸にも、この誤りが20の他の関数に渡されるだけだったら1つの問題しか起こさないだろう。このような場合、問題が発生を見つけるのは非常に困難になる。

..  In some cases, you will be so unconcerned about these problems that you don't mind the function misbehaving when given incorrect input. For example, if you know for sure the function will only be called from a few places, and you can prove that these places give it decent input, it is generally not worth the trouble to make the function bigger and uglier so that it can handle problematic cases.

ある場合において、正しくない入力が与えられたときに行儀悪くふるまう関数に注意しないと、問題は不確かなものになる。例えば、関数が2カ所から呼ばれているだけなのが確実なら、これらの場所が正しい入力を与えているかどうか確認することができる。トラブルにより、問題のあるケースを扱うために関数が大きく醜いものになるのは一般には悪くはない。

..  But most of the time, functions that fail 'silently' are hard to use, and even dangerous. What if the code calling between wants to know whether everything went well? At the moment, it can not tell, except by re-doing all the work that between did and checking the result of between with its own result. That is bad. One solution is to make between return a special value, such as false or undefined, when it fails.

しかし多くの場合、'静かに'失敗する関数は使うのが難しく、危険である。もし\ ``between``\を呼び出すコードが、全てがどこにあるか知ろうとしたらどうなるだろうか？ここで、全ての仕事をやり直すと言うことはできない。\ ``between``\ を実行し、\ ``between``\ 自身の結果がそれ自体に含まれているかチェックする。それはよくない。1つの解決としては\ ``between``\ が失敗したとき、\ ``false``\ とか\ ``undefined``\ のような特別な値を返すようにすることだ。:

.. code-block:: javascript

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

..  You can see that error checking does not generally make functions prettier. But now code that calls between can do something like:

エラーチェックが全般的に関数を美しくなくしたように見えるかもしれない。しかし今\ ``between``\ を呼び出すコードはこのように書ける。:

.. code-block:: javascript

    var input = prompt("Tell me something", "");
    var parenthesized = between(input, "(", ")");
    if (parenthesized != undefined)
      print("You parenthesized '", parenthesized, "'.");

|hr|

..  In many cases returning a special value is a perfectly fine way to indicate an error. It does, however, have its downsides. Firstly, what if the function can already return every possible kind of value? For example, consider this function that gets the last element from an array:

多くの場合、特別な値を返すのはエラーが起こっていることを示すのに完全で良い方法だ。それは、しかしながら、欠点もある。1つには、関数が既に可能な全て種類の値を返すものである場合はどうだろうか？例えば、配列の最後の要素を返す、この関数について考えてみよう。:

.. code-block:: javascript

    function lastElement(array) {
      if (array.length > 0)
        return array[array.length - 1];
      else
        return undefined;
    }

    show(lastElement([1, 2, undefined]));

..  So did the array have a last element? Looking at the value lastElement returns, it is impossible to say.

この配列は最後の要素を持っているか？\ ``lastElement``\ の戻り値を見る限り、そうとは言いがたい。

..  The second issue with returning special values is that it can sometimes lead to a whole lot of clutter. If a piece of code calls between ten times, it has to check ten times whether undefined was returned. Also, if a function calls between but does not have a strategy to recover from a failure, it will have to check the return value of between, and if it is undefined, this function can then return undefined or some other special value to its caller, who in turn also checks for this value.

2つめの問題は、特別な値を返すことは、しばしば別な問題を引き起こす元となるということだ。もし同じコードの部品が\ ``between``\ を10回呼び出すとき、\ ``undefined``\ が帰ってきたか10回チェックする必要がある。また、もし関数が\ ``between``\ を失敗からの回復の戦略なく呼び出したら、関数は\ ``between``\ の戻り値をチェックせねばならず、もしそれが\ ``undefined``\ だったときには、この関数は\ ``undefined``\ か他の特別な値を呼び元に返し、そこでもまたこの値をチェックすることになる。

..  Sometimes, when something strange occurs, it would be practical to just stop doing what we are doing and immediately jump back to a place that knows how to handle the problem.

しばしば、おかしなことが起こったとき、実用的なのは実行を止めて、問題の扱い方を知っている所に明示的にジャンプすることだろう。

..  Well, we are in luck, a lot of programming languages provide such a thing. Usually, it is called exception handling.

よし、我々はついている。多くのプログラミング言語がそのようなものを提供している。通常、それは例外ハンドリングと呼ばれる。

|hr|

..  The theory behind exception handling goes like this: It is possible for code to raise (or throw) an exception, which is a value. Raising an exception somewhat resembles a super-charged return from a function ― it does not just jump out of the current function, but also out of its callers, all the way up to the top-level call that started the current execution. This is called unwinding the stack. You may remember the stack of function calls that was mentioned in chapter 3. An exception zooms down this stack, throwing away all the call contexts it encounters.

例外ハンドリングの理論はこのようなものだ：コードが例外という値を起こす（または投げる）ことを可能にする。例外を起こすというのは関数からの強制的な飛び出しに似ている―現在の関数から飛び出すだけでなく、その呼び元の外、現在実行中の場所からトップレベルまでの全ての道を登り上がる。これはスタック解放と呼ばれる。\ :doc:`3章</Functions>`\ の関数呼び出しのスタックを思い出そう。例外はこのスタックを飛び降り、遭遇した呼び出しの背景を全て捨てる。

..  If they always zoomed right down to the base of the stack, exceptions would not be of much use, they would just provide a novel way to blow up your program. Fortunately, it is possible to set obstacles for exceptions along the stack. These 'catch' the exception as it is zooming down, and can do something with it, after which the program continues running at the point where the exception was caught.

それらが常に元のスタックを飛び降りるなら、例外は使いづらい物になるだろう、それらはプログラムをいいものにする別の手段も提供する。幸運にも、それは例外の障害物をスタックに沿ってセットすることを可能にする。これらは飛び降りてきた例外を'捕まえ'、それに何かをすることができ、例外が捕まった場所からプログラムは実行を続ける。

..  An example:

例:

.. code-block:: javascript

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

..  throw is the keyword that is used to raise an exception. The keyword try sets up an obstacle for exceptions: When the code in the block after it raises an exception, the catch block will be executed. The variable named in parentheses after the word catch is the name given to the exception value inside this block.

``throw``\ は例外を起こすのに使うキーワードだ。\ ``try``\ キーワードは例外への障害をセットアップする：その後ろのブロックの中のコードで、例外が起こったら、\ ``catch``\ ブロックが実行される。\ ``catch``\ キーワードの後の括弧の中の変数名はブロックの中で、例外の値に名前を与えるものだ。

..  Note that the function lastElementPlusTen completely ignores the possibility that lastElement might go wrong. This is the big advantage of exceptions ― error-handling code is only necessary at the point where the error occurs, and the point where it is handled. The functions in between can forget all about it.

``lastElementPlusTen``\ 関数は\ ``lastElement``\ の中で起こった誤りを完全に無視することに注意。これは例外のもつ大きな長所だ―エラーハンドリングのコードはエラーが起こるかもしれない場所、そしてエラーを扱う場所にだけ必要なのだ。その間の関数ではすべて忘れて良い。

..  Well, almost.

そう、ほとんど。

|hr|

..  Consider the following: A function processThing wants to set a top-level variable currentThing to point to a specific thing while its body executes, so that other functions can have access to that thing too. Normally you would of course just pass the thing as an argument, but assume for a moment that that is not practical. When the function finishes, currentThing should be set back to null.

この続きを考えよう：\ ``processThing``\ 関数は、実行中の実体が何であるかを示す\ ``curentThing``\ というトップレベルの変数の設定を必要とし、他の関数もこれにアクセスする。通常は引数として渡すだけだろうが、しかしこれでは実用的ではない。関数が終了したとき、\ ``currentThing``\ も\ ``null``\ を返す。

.. code-block:: javascript

    var currentThing = null;

    function processThing(thing) {
      if (currentThing != null)
        throw "Oh no! We are already processing a thing!";

      currentThing = thing;
      /* よく分らない処理の実行中... */
      currentThing = null;
    }

..  But what if the complicated processing raises an exception? In that case the call to processThing will be thrown off the stack by the exception, and currentThing will never be reset to null.

しかし、もしよく分らない処理が例外を投げたらどうなるだろうか？その場合\ ``processThing``\ の呼び出しは例外のためスタックから放り出され、\ ``currentThing``\ は\ ``null``\ にリセットされなくなるだろう。

..  try statements can also be followed by a finally keyword, which means 'no matter what happens, run this code after trying to run the code in the try block'. If a function has to clean something up, the cleanup code should usually be put into a finally block:

``try``\ 文には\ ``finally``\ キーワードもまた続けて書くことができ、それは'\ *何が*\ 起ころうと、このコードを\ ``try``\ ブロックのコードの実行後に実行する'という意味である。もし関数がクリアする必要がある何かを持っていたら、それをクリーンアップするコードは通常\ ``finally``\ ブロックに書かれるべきだ。:

.. code-block:: javascript

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

|hr|

..  A lot of errors in programs cause the JavaScript environment to raise an exception. For example:

プログラムがJavaScript環境にもたらす多くの誤りは例外を起こす。例えば:

.. code-block:: javascript

    try {
      print(Sasquatch);
    }
    catch (error) {
      print("Caught: " + error.message);
    }

..  In cases like this, special error objects are raised. These always have a message property containing a description of the problem. You can raise similar objects using the new keyword and the Error constructor:

このような場合、特別なエラーオブジェクトが起こる。これらは常に問題の説明を含む\ ``message``\ プロパティを持つ。同様なオブジェクトを\ ``new``\ キーワードと\ ``Error``\ コンストラクタで起こすことができる。:

.. code-block:: javascript

    throw new Error("Fire!");

|hr|

..  When an exception goes all the way to the bottom of the stack without being caught, it gets handled by the environment. What this means differs between the different browsers, sometimes a description of the error is written to some kind of log, sometimes a window pops up describing the error.

例外が捕まることなくスタックの底から出て来たとき、それは環境で扱われる。これは異なるブラウザ間では違いが起こることを意味し、それはしばしばログに書かれるエラーの説明であったり、エラーを説明するポップアップであったりする。

..  The errors produced by entering code in the console on this page are always caught by the console, and displayed among the other output.

このページのコンソールに入力されたコードが作ったエラーは常にコンソールに捕まって表示され、他には出力されない。

|hr|

..  Most programmers consider exceptions purely an error-handling mechanism. In essence, though, they are just another way of influencing the control flow of a program. For example, they can be used as a kind of break statement in a recursive function. Here is a slightly strange function which determines whether an object, and the objects stored inside it, contain at least seven true values:

多くのプログラマーは例外は純粋にエラーハンドリングの仕掛けであると考える。本質的には、しかしながら、プログラムの制御フローに影響を与えるもう一つの手段であるというだけだ。例えば、再帰関数の\ ``break``\ 文のように使うこともできる。これはオブジェクト、ないしさらにその中のオブジェクトが、その中に少なくとも7つの\ ``true``\ の値を含んでいるかを調べる相当奇妙な関数だ。:

.. code-block:: javascript

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

..  The inner function count is recursively called for every object that is part of the argument. When the variable counted reaches seven, there is no point in continuing to count, but just returning from the current call to count will not necessarily stop the counting, since there might be more calls below it. So what we do is just throw a value, which will cause the control to jump right out of any calls to count, and land at the catch block.

内側の\ ``count``\ 関数は、引数のオブジェクト毎に再帰的に呼び出される。
変数\ ``counted``\ が7に達したときは、数え続ける意味はない、しかしその下に他の呼び出しがあるかもしれないので、現在の\ ``count``\ の呼び出しから戻っても、数を数えるのを停止する必要はない。制御が\ ``count``\ の呼び出しから飛び抜けて、\ ``cactch``\ ブロックに着地するように値を投げるだけだ。

..  But just returning true in case of an exception is not correct. Something else might be going wrong, so we first check whether the exception is the object FoundSeven, created specifically for this purpose. If it is not, this catch block does not know how to handle it, so it raises it again.

しかし、例外の場合に\ ``true``\ をただ返すだけというのは正しくない。何か他の間違いが起こっていないか、例外が\ ``FoundSeven``\ オブジェクトかどうかを最初にチェックした上で、この目的のための個別のものを作る。もしそうでなければ、この\ ``catch``\ ブロックはそれの扱い方が分らず、エラーがもう一度起こることになる。

..  This is a pattern that is also common when dealing with error conditions ― you have to make sure that your catch block only handles exceptions that it knows how to handle. Throwing string values, as some of the examples in this chapter do, is rarely a good idea, because it makes it hard to recognise the type of the exception. A better idea is to use unique values, such as the FoundSeven object, or to introduce a new type of objects, as described in chapter 8.

これはエラー条件を扱う際の共通のパターンでもある―\ ``catch``\ ブロックは扱いが分る例外だけを確実に扱うようにしなければならない。この章のいくつかの例のように、文字列の値を投げるのは、例外のタイプを認識するのを難しくするために、良いアイデアであることは希だ。\ ``FoundSeven``\ のようなユニークな値を使うか、\ :doc:`8章</Object-oriented Programming>`\ で説明するような新しいタイプのオブジェクトを導入するのがベターだ。
