=====================================================
関数的プログラミング
=====================================================
..  Chapter 6:
..  Functional Programming

..  As programs get bigger, they also become more complex and harder to understand. We all think ourselves pretty clever, of course, but we are mere human beings, and even a moderate amount of chaos tends to baffle us. And then it all goes downhill. Working on something you do not really understand is a bit like cutting random wires on those time-activated bombs they always have in movies. If you are lucky, you might get the right one ― especially if you are the hero of the movie and strike a suitably dramatic pose ― but there is always the possibility of blowing everything up.

プログラムが大きくなるにつれて、それらはより複雑にそして理解しにくくもなっていく。我々全員が我々自身をかなり利口であると考えているが、もちろん、我々はただの人間にすぎず、そして度を超した混沌が我々を挫折させようともしているのである。それで全ては下り坂だ。本当には良く理解できていないものについて働くのは、いつも映画でやっている時限爆弾の配線を切ることに少し似ている。もし幸運なら、正しい方―もしあなたが映画のヒーローであればそれっぽくドラマチックなポーズを決めるところだ―しかし全てを吹き飛ばすことになる可能性が常にある。

..  Admittedly, in most cases, breaking a program does not cause any large explosions. But when a program, by someone's ignorant tinkering, has degenerated into a ramshackle mass of errors, reshaping it into something sensible is a terrible labour ― sometimes you might just as well start over.

確かに、多くの場合において、プログラムを壊すことは大きな爆発を引き起こしはしない。しかし、プログラムが、誰かの馬鹿げた修正によって、がたがたのエラーの集まりにに悪化した時、それを何か適当なものに作り直すことは恐ろしい労働だ―しばしば、あなたはただ最初から作り直すだろう。

..  Thus, the programmer is always looking for ways to keep the complexity of his programs as low as possible. An important way to do this is to try and make code more abstract. When writing a program, it is easy to get sidetracked into small details at every point. You come across some little issue, and you deal with it, and then proceed to the next little problem, and so on. This makes the code read like a grandmother's tale.

こうして、プログラムの複雑さを可能な限り低く抑える道をプログラマーは常に探すようになる。これをなす重要な道は、コードをより抽象的なものにしようとすることだ。プログラムを書くとき、それが全ての点において小さな枝葉に脱線してしまうのは容易だ。小さな課題に出会い、それに取り組み、そして次の小さな問題、その他に進む。これはコードをお婆ちゃんの昔話を読むようかなのようなものにする。

..  Yes, dear, to make pea soup you will need split peas, the dry kind. And you have to soak them at least for a night, or you will have to cook them for hours and hours. I remember one time, when my dull son tried to make pea soup. Would you believe he hadn't soaked the peas? We almost broke our teeth, all of us. Anyway, when you have soaked the peas, and you'll want about a cup of them per person, and pay attention because they will expand a bit while they are soaking, so if you aren't careful they will spill out of whatever you use to hold them, so also use plenty water to soak in, but as I said, about a cup of them, when they are dry, and after they are soaked you cook them in four cups of water per cup of dry peas. Let it simmer for two hours, which means you cover it and keep it barely cooking, and then add some diced onions, sliced celery stalk, and maybe a carrot or two and some ham. Let it all cook for a few minutes more, and it is ready to eat.

*ええ、あなた、豆のスープを作るには豆を、乾燥した種を取り出さないと。そして、夜の内にそれらを水に浸すか、あるいは何時間もかけてそれを調理しなければならないの。私はある時、さえない息子が豆のスープに挑戦していたことを思い出すの。彼が豆を水に浸さなかったということを信じられる？私たちの歯はほとんど折れてしまった、私たち全員のよ。何にせよ、豆を水に浸す時、そして一人あたり1カップの豆が欲しいときは水に浸した豆が大きく膨らむことに注意しないと、気をつけていないと、器からあふれ出てしまうわ、水はたっぷり用意して、だって、私が言ったとおり、1カップの豆、それが乾燥していたら、水に浸した後に豆1カップにつき4カップの水で煮てちょうだい。2時間ほどフタをしてぐつぐつ煮たら、それから角切りにしたタマネギと、スライスしたセロリの茎、あればニンジンを1本か2本とハムを加えて。それら全部をさらに2分間ほど煮たら、もう食べられるわよ。*

..  Another way to describe this recipe:

このレシピを他の方法で記述する：

..  Per person: one cup dried split peas, half a chopped onion, half a carrot, a celery stalk, and optionally ham.

::

    1あたり：乾燥豆1カップ、刻みタマネギ1/2個、にんじん1/2本、セロリの茎1本、あればハム。

..  Soak peas overnight, simmer them for two hours in four cups of water (per person), add vegetables and ham, and cook for ten more minutes.

::

    豆を1晩水に漬け、1人あたり4カップの水で2時間煮て、野菜とハムを加えて、さらに10分以上煮る。

..  This is shorter, but if you don't know how to soak peas you'll surely screw up and put them in too little water. But how to soak peas can be looked up, and that is the trick. If you assume a certain basic knowledge in the audience, you can talk in a language that deals with bigger concepts, and express things in a much shorter and clearer way. This, more or less, is what abstraction is.

これは短い、しかし、もし豆を浸すやり方を知らなければ、水が少なすぎて台無しにしてしまうだろう。しかし、豆を水に浸すやり方は探すことができ、それが秘訣である。もし聴衆が基本的な知識を確かに持っていることを仮定できれば、大きな概念を言葉数少なく語ることができ、物事を短く明瞭にすることができる。これが、多かれ少なかれ、抽象化であると言える。

..  How is this far-fetched recipe story relevant to programming? Well, obviously, the recipe is the program. Furthermore, the basic knowledge that the cook is supposed to have corresponds to the functions and other constructs that are available to the programmer. If you remember the introduction of this book, things like while make it easier to build loops, and in chapter 4 we wrote some simple functions in order to make other functions shorter and more straightforward. Such tools, some of them made available by the language itself, others built by the programmer, are used to reduce the amount of uninteresting details in the rest of the program, and thus make that program easier to work with.

どのように、このありそうもないレシピの話がプログラミングに結びつくのか？いや、明らかに、レシピはプログラムなのである。その上、料理の基本的な知識はプログラマーにとっての関数や他の構築物に符合すると仮定することができる。もし、この本の導入において、\`` while``\ のようなものでループを作るのが簡単になったこと、\ :doc:`4章</Data structures>`\ で書いた単純な関数が他の関数を短く分りやすいものにしたことを覚えていれば。そのような道具は、言語それ自体によってももたらされるが、他のものはプログラマーによって作られ、プログラムの残りから退屈な些細なことの量を減らすために使われる、そのようにしてプログラムはより簡単に動くようになる。

.. |hr| raw:: html

   <hr>

|hr|

..  Functional programming, which is the subject of this chapter, produces abstraction through clever ways of combining functions. A programmer armed with a repertoire of fundamental functions and, more importantly, the knowledge on how to use them, is much more effective than one who starts from scratch. Unfortunately, a standard JavaScript environment comes with deplorably few essential functions, so we have to write them ourselves or, which is often preferable, make use of somebody else's code (more on that in chapter 9).

この章の表題である関数的プログラミング、関数を連結するための冴えたやり方を抽象化する。プログラマーが、基礎的な関数および、より重要なそれらの関数の使い方のレパートリーを身につけることは、それらを一から作り始めるより効果的だ。不幸にも、標準JavaScript環境は嘆かわしいことに本質的な関数を少ししか持たず、それらは我々自身で書くか、より望ましいのは、他の誰かが書いたコードを使うようにしなければならない。（それについては\ :doc:`9章</Modularity>`\ で）

..  There are other popular approaches to abstraction, most notably object-oriented programming, the subject of chapter 8.

抽象化するための、他の人気のあるアプローチとして、特筆すべきはオブジェクト指向プログラミングがあり、これは\ :doc:`8章</Object-oriented Programming>`\ の課題だ。

|hr|

..  One ugly detail that, if you have any good taste at all, must be starting to bother you is the endlessly repeated for loop going over an array: for (var i = 0; i < something.length; i++) .... Can this be abstracted?

醜く細々としているそれが、もしあなたには全くいいものであるとしたら、終わりのなく繰り返される\ ``for``\ ループが配列を飛び越えてしまわないかが、あなたを悩ませ始めるにちがいない： ``for (var i = 0; i < something.length; i++)`` .... これは抽象化可能か？

..  The problem is that, whereas most functions just take some values, combine them, and return something, such a loop contains a piece of code that it must execute. It is easy to write a function that goes over an array and prints out every element:

問題は、ループが必ず実行されるコードの断片を含んでいるように、多くの関数が何らかの値を取り、それを連結させ、何を返すことにある。配列の全ての要素をプリントする関数を書くのは簡単だ。：

.. code-block:: javascript

    function printArray(array) {
      for (var i = 0; i < array.length; i++)
        print(array[i]);
    }

..  But what if we want to do something else than print? Since 'doing something' can be represented as a function, and functions are also values, we can pass our action as a function value:

しかし、もしこれにプリント以外のことをやらせたいのだとしたら？'何かする'を関数として書けるため、かつ関数は値でもあるため、アクションを関数の値のように渡すことができる：

.. code-block:: javascript

    function forEach(array, action) {
      for (var i = 0; i < array.length; i++)
        action(array[i]);
    }

    forEach(["Wampeter", "Foma", "Granfalloon"], print);

..  And by making use of an anonymous function, something just like a for loop can be written with less useless details:

そして無名関数を使うことで、\ ``for``\ ループのようなものを無意味な詳細なしに書くことができる：

.. code-block:: javascript

    function sum(numbers) {
      var total = 0;
      forEach(numbers, function (number) {
        total += number;
      });
      return total;
    }
    show(sum([1, 10, 100]));

..  Note that the variable total is visible inside the anonymous function because of the lexical scoping rules. Also note that this version is hardly shorter than the for loop and requires a rather clunky }); at its end ― the brace closes the body of the anonymous function, the parenthesis closes the function call to forEach, and the semicolon is needed because this call is a statement.

変数\ ``total``\ は、レキシカル・スコーピングのルールのために無名関数の中で参照できることに注意。このバージョンは\ ``for``\ ループよりほとんど短くなく、むしろ不細工な\ ``}``\ ); で終わる―無名関数の本体を閉じる中括弧、\ ``forEach``\ の関数呼び出しを閉じる小括弧、この呼び出しが文であるためにセミコロンだ、

..  You do get a variable bound to the current element in the array, number, so there is no need to use numbers[i] anymore, and when this array is created by evaluating some expression, there is no need to store it in a variable, because it can be passed to forEach directly.

変数に結びついた現在の要素を配列\ ``number``\ から取り出し、そこでは\ ``number[i]``\ を使う必要はもはやない、式の評価により配列が作られたとき、それらが\ ``forEach``\ に直接渡されるため、変数にそれを格納する必要はない。

..  The cat-code in chapter 4 contains a piece like this:

:doc:`4章</Data structures>`\ の猫のコードはこんな部分を含んでいた：

.. code-block:: javascript

    var paragraphs = mailArchive[mail].split("\n");
    for (var i = 0; i < paragraphs.length; i++)
      handleParagraph(paragraphs[i]);

..  This can now be written as...

これは今はこのように書ける...

.. code-block:: javascript

    forEach(mailArchive[mail].split("\n"), handleParagraph);

..  On the whole, using more abstract (or 'higher level') constructs results in more information and less noise: The code in sum reads 'for each number in numbers add that number to the total', instead of... 'there is this variable that starts at zero, and it counts upward to the length of the array called numbers, and for every value of this variable we look up the corresponding element in the array and add this to the total'.

全てにおいて、より抽象的な（または'高いレベル'の）構造物はより多くの情報を持ちノイズの少ない結果を出せる：\ ``sum``\ のコードは、\ *'ゼロから始まる変数があって、numbersという配列のlengthまでカウントアップされ、この変数の値毎に、対応する要素を配列から見つけ出してtotalにそれを加算する'*\ …という代わりに、\ *'numbersのnumber毎に、totalにそのnumberを加算する'*\ と読まれる。

|hr|

..  What forEach does is take an algorithm, in this case 'going over an array', and abstract it. The 'gaps' in the algorithm, in this case, what to do for each of these elements, are filled by functions which are passed to the algorithm function.

``forEach``\ がどんなアルゴリズムを取っていようと、この場合'配列に渡って'処理が行われるし、それは抽象化される。アルゴリズムのこの'切れ目'は、この場合、これらの要素のそれぞれに何をするかはこのアルゴリズム関数に渡された関数によって埋められる。

..  Functions that operate on other functions are called higher-order functions. By operating on functions, they can talk about actions on a whole new level. The makeAddFunction function from chapter 3 is also a higher-order function. Instead of taking a function value as an argument, it produces a new function.

他の関数を操作する関数は高階関数と呼ばれる。関数を操作することによって、それらは全く新しいレベルで動作を語ることができる。\ :doc:`3章</Functions>`\ の\ ``makeAddFunction``\ 関数もまた高階関数だ。引数として関数値を取る代わりに、それらは新しい関数を作る。

..  Higher-order functions can be used to generalise many algorithms that regular functions can not easily describe. When you have a repertoire of these functions at your disposal, it can help you think about your code in a clearer way: Instead of a messy set of variables and loops, you can decompose algorithms into a combination of a few fundamental algorithms, which are invoked by name, and do not have to be typed out again and again.

高階関数は正規の関数では簡単に書けない多くのアルゴリズムを一般化するのに使える。これらの関数のレパートリーを自由に使えるとき、あなたのコードについて明瞭なやり方で考える助けになる：変数とループのごたごたしたセットの代わりに、アルゴリズムを名前で呼び出される少数の基本的なアルゴリズムの組み合わせに分解することができ、そして繰り返し繰り返しそれらをタイプする必要はなくなる。

..  Being able to write what we want to do instead of how we do it means we are working at a higher level of abstraction. In practice, this means shorter, clearer, and more pleasant code.

*どうやって*\ それを行うかを書く代わりに、\ *何を*\ 望んでいるかを書けるようになることは、我々が抽象化の高いレベルで働いていることを意味する。実用上においては、これは短く、明瞭で、より好ましいコードであることを意味する。

|hr|

..  Another useful type of higher-order function modifies the function value it is given:

他の高階関数の便利なタイプは与えられた関数値を\ *変更*\ する：

.. code-block:: javascript

    function negate(func) {
      return function(x) {
        return !func(x);
      };
    }
    var isNotNaN = negate(isNaN);
    show(isNotNaN(NaN));

..  The function returned by negate feeds the argument it is given to the original function func, and then negates the result. But what if the function you want to negate takes more than one argument? You can get access to any arguments passed to a function with the arguments array, but how do you call a function when you do not know how many arguments you have?

``negate``\ により返された関数は、元の関数\ ``func``\ に対して与えられた引数を与え、それから結果を反転する。しかし反転させたい関数が1つより多い引数を持つときはどうなるだろうか？配列\ ``arguments``\ で渡された関数の任意の引数にアクセスできるが、引数の数が分らない時はどうやって関数を呼び出すのだろうか？

..  Functions have a method called apply, which is used for situations like this. It takes two arguments. The role of the first argument will be discussed in chapter 8, for now we just use null there. The second argument is an array containing the arguments that the function must be applied to.

関数は、このような状況のために使われる、\ ``apply``\ というメソッドを持つ。これは2つの引数を取る。1つめの引数の役割については\ :doc:`8章</Object-oriented Programming>`\ で論じるので、今のところはただ\ ``null``\ としておこう。2つめの引数は、その関数を適用しなければならない引数を含む配列である。

.. code-block:: javascript

    show(Math.min.apply(null, [5, 6]));

    function negate(func) {
      return function() {
        return !func.apply(null, arguments);
      };
    }

..  Unfortunately, on the Internet Explorer browser a lot of built-in functions, such as alert, are not really functions... or something. They report their type as "object" when given to the typeof operator, and they do not have an apply method. Your own functions do not suffer from this, they are always real functions.

残念なことに、Internet Explorerブラウザでは、\ ``alert``\ といったような、多くの組み込み関数は\ *本当の*\ 関数ではない…他のものである。これらは\ ``typeof``\ 演算子に与えられたときには\ ``"object"``\ という型であると報告され、かつ、それらは\ ``apply``\ メソッドを持たない。あなた自身の関数にはこのようなことは起こらない、それらは常に本当の関数である。

|hr|

..  Let us look at a few more basic algorithms related to arrays. The sum function is really a variant of an algorithm which is usually called reduce or fold:

配列に関する基本的なアルゴリズムをもう少し見てみよう。\ ``sum``\ 関数は本当は通常\ ``reduce``\ や\ ``fold``\ というアルゴリズムのバリエーションである：

.. code-block:: javascript

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

..  reduce combines an array into a single value by repeatedly using a function that combines an element of the array with a base value. This is exactly what sum did, so it can be made shorter by using reduce... except that addition is an operator and not a function in JavaScript, so we first had to put it into a function.

``reduce``\ は配列を、元の値に配列の1つの要素を連結する関数の使用を繰り返すことで単一の値に連結する。正確にはこれが\ ``sum``\ がやっていることで、\ ``reduce``\ を使うことで短くできる...JavaScriptにおいては演算子であり関数ではない加算を除いて、それで、我々は最初にそれを関数に押し込めなければならなかった。

..  The reason reduce takes the function as its first argument instead of its last, as in forEach, is partly that this is tradition ― other languages do it like that ― and partly that this allows us to use a particular trick, which will be discussed at the end of this chapter. It does mean that, when calling reduce, writing the reducing function as an anonymous function looks a bit weirder, because now the other arguments follow after the function, and the resemblance to a normal for block is lost entirely.

``reduce``\ が、\ ``forEach``\ のように最後ではなく、1つめの引数として関数を取る理由は、部分的にはこれが伝統である―他の言語ではそのようになっている―ことにあり、部分的には、これにより、この章の最後で論じる特別なトリックを使うことができるということにある。それはこういう意味だ。\ ``reduce``\ を呼ぶとき、変換される関数が無名関数のように書かれるのはちょっと奇妙だ。なぜなら、今、他の引数は関数の後に続き、そして普通の\ ``for``\ ブロックに似ていることは全くなくなったからである。

|hr|

演習 6.1

..  Write a function countZeroes, which takes an array of numbers as its argument and returns the amount of zeroes that occur in it. Use reduce.

数値の配列を引数として取り、その中に出てくるゼロの数を返す\ ``countZeroes``\ 関数を書け。\ ``reduce``\ を使うこと。

..  Then, write the higher-order function count, which takes an array and a test function as arguments, and returns the amount of elements in the array for which the test function returned true. Re-implement countZeroes using this function.

それから、1つの配列とテスト関数を引数に取って、テスト関数が\ ``true``\ の値を返す配列の要素の数を返す、高階関数\ ``count``\ を書け。\ ``countZeroes``\ をこの関数を使って再実装せよ。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function countZeroes(array) {
      function counter(total, element) {
        return total + (element === 0 ? 1 : 0);
      }
      return reduce(counter, 0, array);
    }

..  The weird part, with the question mark and the colon, uses a new operator. In chapter 2 we have seen unary and binary operators. This one is ternary ― it acts on three values. Its effect resembles that of if/else, except that, where if conditionally executes statements, this one conditionally chooses expressions. The first part, before the question mark, is the condition. If this condition is true, the expression after the question mark is chosen, 1 in this case. If it is false, the part after the colon, 0 in this case, is chosen.

奇妙な部分、疑問符とセミコロンのあるところでは新しい演算子を使っている。\ :doc:`2章</Basic JavaScript>`\ では単項演算子と二項演算子を見た。これは三項演算子―3つの値を操作する。これは\ ``if/else``\ を省略する効果を持ち、例外は、\ ``if``\ は条件により文を実行し、これは条件により式を選ぶところにある。1つ目の部分、疑問符の前は、条件である。もしこの条件が\ ``true``\ であれば、疑問符の後の式が選ばれ、この場合は\ ``1``\ である。もし\ ``false``\ であれば、コロンの後の部分、この場合は\ ``0``\ が選ばれる。

..  Use of this operator can make some pieces of code much shorter. When the expressions inside it get very big, or you have to make more decisions inside the conditional parts, just using plain if and else is usually more readable.

この演算子を使うとコードのある部分を短くできる。コードの中の式が大きくなり、あるいは条件の部分により多くの判定をもつようなときは、ただの平坦な\ ``if``\ と\ ``else``\ の方が読みやすい。

..  Here is the solution that uses a count function, with a function that produces equality-testers included to make the final countZeroes function even shorter:

これが\ ``count``\ 関数を使った解答であり、等価テストの関数を含むことにより、最後の\ ``countZeroes``\ もまた短くなる：

.. code-block:: javascript

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

|hr|

..  One other generally useful 'fundamental algorithm' related to arrays is called map. It goes over an array, applying a function to every element, just like forEach. But instead of discarding the values returned by function, it builds up a new array from these values.

もう一つの一般的で便利な配列に関する'基礎的なアルゴリズム'は\ ``map``\ と呼ばれる。配列について、\ ``forEach``\ のように全ての要素に関数を適用し、しかし関数に返された値を破棄する代わりに、これらの値から新しい配列を作る。

.. code-block:: javascript

    function map(func, array) {
      var result = [];
      forEach(array, function (element) {
        result.push(func(element));
      });
      return result;
    }

    show(map(Math.round, [0.01, 2, 9.89, Math.PI]));

..  Note that the first argument is called func, not function, this is because function is a keyword and thus not a valid variable name.

最初の引数を\ ``function``\ ではなく\ ``func``\ としているのは、これは\ ``function``\ はキーワードであり正しい変数名ではないからであることに注意。

|hr|

..  There once was, living in the deep mountain forests of Transylvania, a recluse. Most of the time, he just wandered around his mountain, talking to trees and laughing with birds. But now and then, when the pouring rain trapped him in his little hut, and the howling wind made him feel unbearably small, the recluse felt an urge to write something, wanted to pour some thoughts out onto paper, where they could maybe grow bigger than he himself was.

かつて、世捨て人がトランシルバニア山の深い森に住んでいた。ほとんどの時間は、山の周囲をさまよい、木々と話し、鳥と笑いあうだけであった。しかし今後、流れる雨が彼を小屋の中に閉じ込めたとき、吠える風が彼を耐えがたく小さいものと感じさせ、世捨て人は何か書かねばという焦りを感じ、流れゆく考えを紙に書き出したい、彼が彼自身がそうであるより大きなものみせかけようとするようになった。

..  After failing miserably at poetry, fiction, and philosophy, the recluse finally decided to write a technical book. In his youth, he had done some computer programming, and he figured that if he could just write a good book about that, fame and recognition would surely follow.

みじめに詩作、フィクション、哲学に陥った後、世捨て人はついに技術の本を書こうと決めた。若い時、彼はコンピュータープログラミングをしていて、それについて良い本を書くことができ、名声と認知はそれについてくると確かに考えていた。

..  So he wrote. At first he used fragments of tree bark, but that turned out not to be very practical. He went down to the nearest village and bought himself a laptop computer. After a few chapters, he realised he wanted to put the book in HTML format, in order to put it on his web-page...

彼は書いた。最初は樹皮の断片で、しかしとても実用的ではないと思い直した。彼は一番近い村に降りてラップトップコンピュータを買った。2,3の章を書いた後、HTML形式で本を出したいと気付いた。彼のウェブページに載せるために…。

|hr|

..  Are you familiar with HTML? It is the method used to add mark-up to pages on the web, and we will be using it a few times in this book, so it would be nice if you know how it works, at least generally. If you are a good student, you could go search the web for a good introduction to HTML now, and come back here when you have read it. Most of you probably are lousy students, so I will just give a short explanation and hope it is enough.

HTMLに慣れているだろうか？ウェブ上のページにマークアップを追加するための方法で、我々はこの本でも何度か使っていて、もしあなたが、最低でも一般的にそれがどう動くか知っていたら良いと思う。もし良い生徒であれば、今すぐ、HTMLへの良い導入を得るためウェブを検索するだろう、そして読み終わってから戻ってくるだろう。あなた方の多くはおそらくひどい生徒だろうから、私が短い説明を与え、それで十分だと期待するとしよう。

..  HTML stands for 'HyperText Mark-up Language'. An HTML document is all text. Because it must be able to express the structure of this text, information about which text is a heading, which text is purple, and so on, a few characters have a special meaning, somewhat like backslashes in JavaScript strings. The 'less than' and 'greater than' characters are used to create 'tags'. A tag gives extra information about the text in the document. It can stand on its own, for example to mark the place where a picture should appear in the page, or it can contain text and other tags, for example when it marks the start and end of a paragraph.

HTMLとは'HyperText Mark-up Language'のことだ。HTML文書はすべてテキストである。なぜなら、このテキストはヘッディングだとか、このテキストは紫色だとか、その他の情報、このテキストの構造を表現できなければならないからで、少数の文字がJavaScriptの文字列におけるバックスラッシュのような特別な意味を持っている。'より小さい(<)'と'より大きい(>)'は'タグ'を作るために使われる。タグは文書の中のテキストに拡張された情報を与える。それ自身で成り立ち、例えば、ページの中で画像を見せる場所をマークしたりする。あるいはテキストや他のタグを中に入れたり、例えば段落の最初と最後をマークしたりする。

..  Some tags are compulsory, a whole HTML document must always be contained in between html tags. Here is an example of an HTML document:

いくつかのタグは必須であり、完全なHTML文書は常に\ ``html``\ タグの間に含まれなければならない。ここにHTML文書の例を挙げる：

.. code-block:: html

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

..  Elements that contain text or other tags are first opened with <tagname>, and afterwards finished with </tagname>. The html element always contains two children: head and body. The first contains information about the document, the second contains the actual document.

テキストや他のタグを含む要素は、最初の\ ``<タグ名>``\ により始まり、後の\ ``</タグ名>``\ で終わる。\ ``html``\ 要素は常に2つの子供を含む：\ ``head``\ と\ ``body``\ だ。1つ目は文書\ *についての*\ 情報を含み、2つ目は実際の文書を含んでいる。

..  Most tag names are cryptic abbreviations. h1 stands for 'heading 1', the biggest kind of heading. There are also h2 to h6 for successively smaller headings. p means 'paragraph', and img stands for 'image'. The img element does not contain any text or other tags, but it does have some extra information, src="img/ostrich.png", which is called an 'attribute'. In this case, it contains information about the image file that should be shown here.

多くのタグ名は暗号のように省略されている。\ ``h1``\ は'heading 1'、最も大きな見出しである。引き続き\ ``h2``\ から\ ``h6``\ というより小さい見出しもある。\ ``p``\ は'paragraph'の意味で、\ ``img``\ は'image'だ。\ ``img``\ 要素はテキストや他のタグを含まず、拡張された情報を持つ、\ ``src="img/ostrich.png"``\ のようなものは属性と呼ばれる。この場合、それはここに表示すべき画像ファイルの情報である。

..  Because < and > have a special meaning in HTML documents, they can not be written directly in the text of the document. If you want to say '5 < 10' in an HTML document, you have to write '5 &lt; 10', where 'lt' stands for 'less than'. '&gt;' is used for '>', and because these codes also give the ampersand character a special meaning, a plain '&' is written as '&amp;'.

``<``\ と\ ``>``\ がHTML文書において特別な意味を持つため、文書中のテキストにそれらを直接書くことはできない。もし\ ``'5 < 10'``\ とHTML文書内に書きたければ、\ ``'5 &lt; 10'``\ と書かねばならず、\ ``'lt'``\ は'less than（より小さい）'の意味で、\ ``'&gt;'``\ は\ ``'>'``\ を書くのに使われる。そしてこれらのコードもまたアンパサンド(&)文字に特別な意味を与えるため、ただの\ ``'&'``\ は\ ``'&amp;'``\ と書かれる。

..  Now, those are only the bare basics of HTML, but they should be enough to make it through this chapter, and later chapters that deal with HTML documents, without getting entirely confused.

今の、これらはHTMLのほんの基本的なことだけであるが、この章でやることに関しては十分であろうし、後の章で完全に混乱なしにHTML文書を扱う。

|hr|

..  The JavaScript console has a function viewHTML that can be used to look at HTML documents. I stored the example document above in the variable stroustrupQuote, so you can view it by executing the following code:

JavaScriptコンソールはHTML文書を見るための\ ``viewHTML``\ 関数を持っている。上記の例の文書を\ ``stroustrupQuote``\ 変数に格納してあるので、下記のコードを実行することでそれを見ることができる：

.. code-block:: javascript

    viewHTML(stroustrupQuote);

..  If you have some kind of pop-up blocker installed or integrated in your browser, it will probably interfere with viewHTML, which tries to show the HTML document in a new window or tab. Try to configure the blocker to allow pop-ups from this site.

もしある種のポップアップブロッカーがインストールされていたり、ブラウザに組み込まれていたら、おそらく\ ``viewHTML``\ と連絡し、HTML文書を新しいウインドウかタブで開こうとするだろう。このサイトからのポップアップを許可するようブロッカーを設定してみてほしい。

|hr|

..  So, picking up the story again, the recluse wanted to have his book in HTML format. At first he just wrote all the tags directly into his manuscript, but typing all those less-than and greater-than signs made his fingers hurt, and he constantly forgot to write &amp; when he needed an &. This gave him a headache. Next, he tried to write the book in Microsoft Word, and then save it as HTML. But the HTML that came out of that was fifteen times bigger and more complicated than it had to be. And besides, Microsoft Word gave him a headache.

ストーリーに戻ると、世捨て人は彼の本をHTML形式することを望んだ。最初に彼は全てのタグを直接彼の原稿に書きこもうとしたが、全ての「より小さい」と「より大きい」記号をタイプすることは彼の指を傷つけ、かつ彼は、\ ``&``\ が必要な時は\ ``&amp;``\ を書くのだということはすっかり忘れていた。これで彼は頭痛になった。次に、彼はMicrsoft Wordでこの本を書き、HTMLとして保存しよう試みた。しかし、HTMLは15倍の大きさになり、彼が考えるより複雑なものになった。こうして、Microsoft Wordも彼を頭痛にした。

..  The solution that he eventually came up with was this: He would write the book as plain text, following some simple rules about the way paragraphs were separated and the way headings looked. Then, he would write a program to convert this text into precisely the HTML that he wanted.

やっとたどり着いた解決はこのようなものだ：彼はこの本をプレインテキストで書く、段落と見出しに見えるものを単純なルールに従って分離する。それから、このテキストを彼の望みに合ったHTMLに変換するプログラムを書くのだ。

..  The rules are this:

このようなルールだ：

#. 段落は空白行によって分離される。
#. '%'記号から始まる段落は見出しとする。より多くの'%'記号があれば、より小さい見出しとする。
#. 段落の中で、アスタリスクの間に挟まれたテキストは強調する。
#. 脚注は括弧の間に書く。

..  Paragraphs are separated by blank lines.
..  A paragraph that starts with a '%' symbol is a header. The more '%' symbols, the smaller the header.
..  Inside paragraphs, pieces of text can be emphasised by putting them between asterisks.
..  Footnotes are written between braces.

|hr|

..  After he had struggled painfully with his book for six months, the recluse had still only finished a few paragraphs. At this point, his hut was struck by lightning, killing him, and forever putting his writing ambitions to rest. From the charred remains of his laptop, I could recover the following file:

彼の本との痛々しい奮闘の後から6ヶ月が過ぎたが、世捨て人はまだ少数の段落しか終わっていなかった。このポイントで、彼の小屋は雷に打たれ、彼は殺され、残りを書こうという野望は永遠に叶わなくなった。彼の黒焦げのラップトップの残骸から、下記のファイルを復元することができた。
::

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

|hr|

..  To honour the memory of our good recluse, I would like to finish his HTML-generating program for him. A good approach to this problem goes like this:

良き世捨て人の記憶を称え、彼のHTML生成プログラムを完成させよう。この問題に対する良いアプローチはこのようになる：

#. 全ての空行毎に区切ることで、ファイルを段落に分ける。
#. '%'文字を見出し行から取り除き、それを見出しとしてマークアップする。
#. 段落自体のテキストを処理し、それを通常のパーツと、強調されたパーツと、脚注に分ける。
#. 全ての脚注を文書の末尾に移動し、そこに番号を残す。
#. それぞれの部分を正しいHTMLタグでラップする。
#. 全てを1つのHTML文書に連結する。

..  Split the file into paragraphs by cutting it at every empty line.
..  Remove the '%' characters from header paragraphs and mark them as headers.
..  Process the text of the paragraphs themselves, splitting them into normal parts, emphasised parts, and footnotes.
..  Move all the footnotes to the bottom of the document, leaving numbers1 in their place.
..  Wrap each piece into the correct HTML tags.
..  Combine everything into a single HTML document.

..  This approach does not allow footnotes inside emphasised text, or vice versa. This is kind of arbitrary, but helps keep the example code simple. If, at the end of the chapter, you feel like an extra challenge, you can try to revise the program to support 'nested' mark-up.

このアプローチでは強調されたテキストの中の脚注や、その逆のものが許されない。これは自由裁量の類だが、例のコードを単純なものにする助けになる。もし、この章の終わりに、拡張された挑戦をやろうかと思ったら、'ネストされたマークアップ'をサポートするようプログラムを書き換えてみることができる。

..  The whole manuscript, as a string value, is available on this page by calling recluseFile function.

原稿全体は、文字列の値として、このページでは\ ``recluseFile``\ 関数を呼び出すことで使用可能になる。

|hr|

..  Step 1 of the algorithm is trivial. A blank line is what you get when you have two newlines in a row, and if you remember the split method that strings have, which we saw in chapter 4, you will realise that this will do the trick:

アルゴリズムのステップ１はありふれたものだ。1つの行に2つの改行があったら、空行であるとして、もし、\ :doc:`4章</Data structures>`\ で見た、文字列が持つ\ ``split``\ メソッドを覚えていたら、このトリックを実現するのである：

.. code-block:: javascript

    var paragraphs = recluseFile().split("\n\n");
    print("Found ", paragraphs.length, " paragraphs.");

|hr|

演習 6.2

..  Write a function processParagraph that, when given a paragraph string as its argument, checks whether this paragraph is a header. If it is, it strips off the '%' characters and counts their number. Then, it returns an object with two properties, content, which contains the text inside the paragraph, and type, which contains the tag that this paragraph must be wrapped in, "p" for regular paragraphs, "h1" for headers with one '%', and "hX" for headers with X '%' characters.

引数として段落の文字列を与えられたら、これが見出しであるか判定し、見出しであれば\ ``'%'``\ 文字を消すとともに数を数え、それから、段落の中のテキストを含む\ ``content``\ 、およびこの段落が何でラップされるべきか、通常の段落であれば\ ``"p"``\ 、1つの\ ``'%'``\ の見出しであれば\ ``"h1"``\ 、\\ ``x``\ 個の\ ``'%'``\ 文字の見出しであれば\ ``"hX"``\ を含む\ ``type``\ と2つのプロパティを持つオブジェクトを返す\ ``processParagraph``\ 関数を書け。

..  Remember that strings have a charAt method that can be used to look at a specific character inside them.

文字列が\ ``charAt``\ メソッドを持ち、個別の文字が段落の中にあるかどうか探すことができることを忘れずに。

..  [show solution]

[解答を見る]

.. code-block:: javascript

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

|hr|

..  This is where we can try out the map function we saw earlier.

先ほど見た\ ``map``\ 関数を試してみよう。

.. code-block:: javascript

    var paragraphs = map(processParagraph,
                         recluseFile().split("\n\n"));

..  And bang, we have an array of nicely categorised paragraph objects. We are getting ahead of ourselves though, we forgot step 3 of the algorithm:

*良し*\ 、段落オブジェクトがうまく分類された配列を得ることができた。うまくいったけれど、アルゴリズムのステップ3を忘れていた。

..  Process the text of the paragraphs themselves, splitting them into normal parts, emphasised parts, and footnotes.

*段落のテキスト自体を処理して、通常のパーツと、強調されたパーツと、脚注に分割しよう。*

..  Which can be decomposed into:

このように分解する：

#. もし段落がアスタリスクから始まっていれば、強調されたパーツとして格納する。
#. もし段落が左括弧"{"から始まっていたら、脚注として格納する。
#. どちらでもなく、最初に強調されたパーツや脚注として取り出されてなければ、または文字列の終わりでなければ、通常のテキストとして格納する。
#. もし段落の中に何か残っていたら、再び1から始める。

..  If the paragraph starts with an asterisk, take off the emphasised part and store it.
..  If the paragraph starts with an opening brace, take off the footnote and store it.
..  Otherwise, take off the part until the first emphasised part or footnote, or until the end of the string, and store it as normal text.
..  If there is anything left in the paragraph, start at 1 again.

|hr|

演習 6.3

..  Build a function splitParagraph which, given a paragraph string, returns an array of paragraph fragments. Think of a good way to represent the fragments.

与えられた段落の文字列を、段落の断片の配列として返す\ ``splitParagraph``\ 関数を作れ。断片を表現する良い方法を考えよう。

..  The method indexOf, which searches for a character or sub-string in a string and returns its position, or -1 if not found, will probably be useful in some way here.

文字または文字列の一部を文字列から探しその位置を返す、あるいは見つからなかったら\ ``-1``\ を返す、\ ``indexOf``\ メソッドがおそらくここで便利に使えるだろう。

..  This is a tricky algorithm, and there are many not-quite-correct or way-too-long ways to describe it. If you run into problems, just think about it for a minute. Try to write inner functions that perform the smaller actions that make up the algorithm.

これはトリッキーなアルゴリズムで、多くは全く正しくなかったり、あるいはあまりにも遠回りなやり方になるだろう。問題に当たったら、考えるのは少しの時間だけにしよう。アルゴリズムを作り上げるための、より小さなアクションを実現する内側の関数を書いてみよう。

..  [show solution]

[解答を見る]

..  Here is one possible solution:

あり得る解の1つがこれだ：

.. code-block:: javascript

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

..  Note the over-eager use of map and reduce in the takeNormal function. This is a chapter about functional programming, so program functionally we will! Can you see how this works? The map produces an array of positions where the given characters were found, or the end of the string if they were not found, and the reduce takes the minimum of them, which is the next point in the string that we have to look at.

``takeNormal``\ 関数の中の\ ``map``\ と\ ``reduce``\ は大食いであることに注意。ここは関数的プログラミングについての章なので、プログラムが関数的になっているのは我々の意図したとおりだ！これがどう動くかわかるだろうか？\ ``map``\ は与えられた文字が見つかった位置や、またはそれらの文字が見つからなくなったら文字列の終わり位置の配列を作りだし、そして\ ``reduce``\ はそれらの最小のものを取り、次に探すべき文字列の開始位置とする。

..  If you'd write that out without mapping and reducing you'd get something like this:

もしmapとreduceを使わずに書いたらこのようになっただろう：

.. code-block:: javascript

    var nextAsterisk = text.indexOf("*");
    var nextBrace = text.indexOf("{");
    var end = text.length;
    if (nextAsterisk != -1)
      end = nextAsterisk;
    if (nextBrace != -1 && nextBrace < end)
      end = nextBrace;

..  Which is even more hideous. Most of the time, when a decision has to be made based on a series of things, even if there are only two of them, writing it as array operations is nicer than handling every value in a separate if statement. (Fortunately, chapter 10 describes an easier way to ask for the first occurrence of 'this or that character' in a string.)

これはもっと醜い。多くの時間を、物事の順番を基準に判定するときに費やしており、もしそれらが2つしかないにしても、配列操作として書く方が全ての値を別々の\ ``if``\ 文で処理するよりましだ。（幸運にも、'この、またはあの'文字が最初にどこに出現するかを判定する、より簡単な手段を\ :doc:`10章</Regular Expressions>`\ で説明する）

..  If you wrote a splitParagraph that stored fragments in a different way than the solution above, you might want to adjust it, because the functions in the rest of the chapter assume that fragments are objects with type and content properties.

もし上記と異なる方法で断片を格納する\ ``splitParagraph``\ を書いていたら、それを修正したくなるかもしれない、なぜなら章の残りの関数は断片を\ ``type``\ および\ ``content``\ プロパティをもつものと仮定しているからである。

|hr|

..  We can now wire processParagraph to also split the text inside the paragraphs, my version can be modified like this:

我々は今や\ ``processParagraph``\ で段落の中のテキストも分割することができ、私のバージョンはこのように変更できる：

.. code-block:: javascript

    function processParagraph(paragraph) {
      var header = 0;
      while (paragraph.charAt(0) == "%") {
        paragraph = paragraph.slice(1);
        header++;
      }

      return {type: (header == 0 ? "p" : "h" + header),
              content: splitParagraph(paragraph)};
    }

..  Mapping that over the array of paragraphs gives us an array of paragraph objects, which in turn contain arrays of fragment objects. The next thing to do is to take out the footnotes, and put references to them in their place. Something like this:

段落の配列を、fragmentオブジェクトの配列を含むparagraphオブジェクトの配列にマッピングする。次にやることは脚注を取りだし、それへの参照を作ることだ。このようになる。：

.. code-block:: javascript

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

..  The replaceFootnote function is called on every fragment. When it gets a fragment that should stay where it is, it just returns it, but when it gets a footnote, it stores this footnote in the footnotes array, and returns a reference to it instead. In the process, every footnote and reference is also numbered.

``replaceFootnote``\ 関数は断片毎に呼び出される。それが断片であればその場にそのまま返すが、脚注であれば、この脚注を\ ``footnote``\ 配列に格納し、代わりに参照を返す。この処理の中で、脚注と参照には番号が付けられる。

|hr|

..  That gives us enough tools to extract the information we need from the file. All that is left now is generating the correct HTML.

これで必要な情報を展開する道具立ては揃った。残るはそれを正しいHTMLに生成することだ。

..  A lot of people think that concatenating strings is a great way to produce HTML. When they need a link to, for example, a site where you can play the game of Go, they will do:

多くの人々が文字列を連結することがHTMLを作る王道だと考える。彼らは、例えば、囲碁で遊べるサイトへリンクすることを必要とするとき、このようにするだろう。:

.. code-block:: javascript

    var url = "http://www.gokgs.com/";
    var text = "Play Go!";
    var linkText = "<a href=\"" + url + "\">" + text + "</a>";
    print(linkText);

..  (Where a is the tag used to create links in HTML documents.) ... Not only is this clumsy, but when the string text happens to include an angular bracket or an ampersand, it is also wrong. Weird things will happen on your website, and you will look embarrassingly amateurish. We wouldn't want that to happen. A few simple HTML-generating functions are easy to write. So let us write them.

（このaはHTML文書の中にリンクを作るときに使われるタグである）...不細工なだけでなく、\ ``text``\ 文字列に不等号やアンパサンドが含まれていたときには、処理を誤る。あなたのウェブサイトに奇怪なことが起こり、とても素人っぽく見えるだろう。起こって欲しくないことだ。単純なHTMLを生成する関数は簡単に書ける。それを書こう。

|hr|

..  The secret to successful HTML generation is to treat your HTML document as a data structure instead of a flat piece of text. JavaScript's objects provide a very easy way to model this:

HTMLの生成をうまくやる秘訣はHTML文書をテキストの平坦な部品として扱う代わりに、データ構造として扱うことにある。JavaScriptのオブジェクトはとても簡単にこれをモデル化する手段を提供している。：

.. code-block:: javascript

    var linkObject = {name: "a",
                      attributes: {href: "http://www.gokgs.com/"},
                      content: ["Play Go!"]};

..  Each HTML element contains a name property, giving the name of the tag it represents. When it has attributes, it also contains an attributes property, which contains an object in which the attributes are stored. When it has content, there is a content property, containing an array of other elements contained in this element. Strings play the role of pieces of text in our HTML document, so the array ["Play Go!"] means that this link has only one element inside it, which is a simple piece of text.

それぞれのHTML要素は表示されるタグ名を与える\ ``name``\ プロパティを含んでいる。それが属性を持つときには、属性が格納されたオブジェクトを含む\ ``attributes``\ プロパティも含まれる。それが中身を持っていれば、この要素に含まれる他の要素の配列を含む\ ``content``\ プロパティもある。文字列はHTML文書においてテキストの部品の役割をし、配列\ ``["Play Go!"]``\ はこのリンクはテキストの単純な部品である1つの要素だけを持つことを示している。

..  Typing in these objects directly is clumsy, but we don't have to do that. We provide a shortcut function to do this for us:

これらのオブジェクトをタイプするのはかっこ悪いが、その必要はない。これをやるためのショートカットになる関数を作ろう。：

.. code-block:: javascript

    function tag(name, content, attributes) {
      return {name: name, attributes: attributes, content: content};
    }

..  Note that, since we allow the attributes and content of an element to be undefined if they are not applicable, the second and third argument to this function can be left off when they are not needed.

もしそれが適切でなければ、我々は要素の\ ``attributes``\ や\ ``content``\ を定義しないこともできるようにし、この関数の2番目か3番目の引数は必要でなければ無視されることに注意。

..  tag is still rather primitive, so we write shortcuts for common types of elements, such as links, or the outer structure of a simple document:

``tag``\ はまだむしろ原始的であるから、リンクのような共通のタイプの要素や単純な文書の外側についてはショートカットを書こう。：

.. code-block:: javascript

    function link(target, text) {
      return tag("a", [text], {href: target});
    }

    function htmlDoc(title, bodyContent) {
      return tag("html", [tag("head", [tag("title", [title])]),
                          tag("body", bodyContent)]);
    }

|hr|

演習 6.4

..  Looking back at the example HTML document if necessary, write an image function which, when given the location of an image file, will create an img HTML element.

もし必要なら例のHTML文書を見返して、画像ファイルの場所を与えられたときにHTMLびimg要素を作る\ ``image``\ 関数を書け。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function image(src) {
      return tag("img", [], {src: src});
    }

|hr|

..  When we have created a document, it will have to be reduced to a string. But building this string from the data structures we have been producing is very straightforward. The important thing is to remember to transform the special characters in the text of our document...

文書を作ったとき、それは文字列になる。しかしこの文字列をデータ構造から組み立てるときにとてもまっすぐなやりかたを取った。文書のテキストの中の特殊な文字を変形することは覚えておくべき重要なことだ...

.. code-block:: javascript

    function escapeHTML(text) {
      var replacements = [[/&/g, "&amp;"], [/"/g, "&quot;"],
                          [/</g, "&lt;"], [/>/g, "&gt;"]];
      forEach(replacements, function(replace) {
        text = text.replace(replace[0], replace[1]);
      });
      return text;
    }

..  The replace method of strings creates a new string in which all occurrences of the pattern in the first argument are replaced by the second argument, so "Borobudur".replace(/r/g, "k") gives "Bokobuduk". Don't worry about the pattern syntax here ― we'll get to that in chapter 10. The escapeHTML function puts the different replacements that have to be made into an array, so that it can loop over them and apply them to the argument one by one.

文字列の\ ``replace``\ メソッドは1つめの引数のパターンの出現した場所全てを2つめの引数で置き換えた新しい文字列を作り、\ ``"Borobudur".replace(/r/g, "k")``\ であれば\ ``"Bokobuduk"``\ になる。パターンの書き方については悩まないように―\ :doc:`10章</Regular Expressions>`\ で分かる。\ ``escapeHTML``\ 関数は配列に入れられた複数の異なる置き換えを、ループしながら1つ1つ引数の文字列に適用していく。

..  Double quotes are also replaced, because we will also be using this function for the text inside the attributes of HTML tags. Those will be surrounded by double quotes, and thus must not have any double quotes inside of them.

二重引用符も置き換えられるのは、なぜならこの関数はHTMLタグの属性の中のテキストにも使うからである。二重引用符に囲まれたそれらは、それゆえに中に二重引用符を含むことができない。

..  Calling replace four times means the computer has to go over the whole string four times to check and replace its content. This is not very efficient. If we cared enough, we could write a more complex version of this function, something that resembles the splitParagraph function we saw earlier, to go over it only once. For now, we are too lazy for this. Again, chapter 10 shows a much better way to do this.

replaceを4回呼び出しているのは、コンピューターは文字列全体のチェックと置換を4回行うという意味だ。これはとても効率的でない。もし十分な注意をしたら、先ほどの\ ``splitParagraph``\ に1度だけやったのように、この関数のより複雑なバージョンを書けるだろう。今は、これをやるには我々は忙しすぎる。もう一度、\ :doc:`10章</Regular Expressions>`\ でこれをやる良い方法を見せよう。

|hr|

..  To turn an HTML element object into a string, we can use a recursive function like this:

HTML要素のオブジェクトを文字列にするには、このような再帰関数が使える。：

.. code-block:: javascript

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

..  Note the in loop that extracts the properties from a JavaScript object in order to make HTML tag attributes out of them. Also note that in two places, arrays are being used to accumulate strings, which are then joined into a single result string. Why didn't I just start with an empty string and then add the content to it with the += operator?

HTMLタグ属性を作るためにJavaScriptオブジェクトからプロパティを展開する\ ``in``\ ループは外に出たことに注意。また、2つの場所で、配列が文字列を蓄積するのに使われ、それから1つの文字列に連結されることにも注意。なぜ空の文字列から始めてそれに中身を\ ``+=``\ 演算子で追加するのことをしないのか。

..  It turns out that creating new strings, especially big strings, is quite a lot of work. Remember that JavaScript string values never change. If you concatenate something to them, a new string is created, the old ones stay intact. If we build up a big string by concatenating lots of little strings, new strings have to be created at every step, only to be thrown away when the next piece is concatenated to them. If, on the other hand, we store all the little strings in an array and then join them, only one big string has to be created.

新しい文字列、特別に大きい文字列を作ること、実に多くの仕事になる。JavaScriptの文字列の値は変更不可能であることを覚えておくこと。もし何かをこれに連結し、新しい文字列が作られ、古い文字列もそのまま残る。もし、大きな文字列を多数の小さな文字列の連結によってつくったら、全てのステップで新しい文字列が作られ、次の部品をそれらに連結するためだけに放り出される。もし、その一方で、全ての小さな文字列を配列に格納してそれから連結するのであれば、大きな文字列が作られるのは1度だけで済む。

|hr|

..  So, let us try out this HTML generating system...

そう、このHTML生成システムを試してみよう...

.. code-block:: javascript

    print(renderHTML(link("http://www.nedroid.com", "Drawings!")));

..  That seems to work.

動くようだ。

.. code-block:: javascript

    var body = [tag("h1", ["The Test"]),
                tag("p", ["Here is a paragraph, and an image..."]),
                image("img/sheep.png")];
    var doc = htmlDoc("The Test", body);
    viewHTML(renderHTML(doc));

..  Now, I should probably warn you that this approach is not perfect. What it actually renders is XML, which is similar to HTML, but more structured. In simple cases, such as the above, this does not cause any problems. However, there are some things, which are correct XML, but not proper HTML, and these might confuse a browser that is trying to show the documents we create. For example, if you have an empty script tag (used to put JavaScript into a page) in your document, browsers will not realise that it is empty and think that everything after it is JavaScript. (In this case, the problem can be fixed by putting a single space inside of the tag, so that it is no longer empty, and gets a proper closing tag.)

今、私はこのアプローチがおそらく完全ではないことを警告しておくべきだろう。これが実際に作るものはXMLで、HTMLに近いものではあるが、より構造化されたものだ。上記のような単純なケースでは、これが問題を起こすことはない。しかしながら、正しいXMLであっても、妥当なHTMLではないことがあって、作った文書を表示しようとするブラウザを混乱させることがある。例えば、もし文書の中に空の\ ``script``\ タグ（JavaScriptをページに入れるときに使われる）があったら、ブラウザは空とは考えずにその後の全てがJavaScriptであると考えるだろう。（この場合、問題は1つの空白をタグの中に含め、空のタグでなくすことで正しくタグを閉じ、解決できる）

|hr|

演習 6.5

..  Write a function renderFragment, and use that to implement another function renderParagraph, which takes a paragraph object (with the footnotes already filtered out), and produces the correct HTML element (which might be a paragraph or a header, depending on the type property of the paragraph object).

段落オブジェクト（脚注が既に取り除かれている）を取り、正しいHTML要素（段落か見出しかは、paragraphオブジェクトの\ ``type``\ プロパティによって判断）を作る\ ``renderFragment``\ 関数を書き、それを使って\ ``renderParagraph``\ の別の実装を行え。

..  This function might come in useful for rendering the footnote references:

この関数は脚注の参照の作成において便利なものになるだろう：

.. code-block:: javascript

    function footnote(number) {
      return tag("sup", [link("#footnote" + number,
                              String(number))]);
    }

..  A sup tag will show its content as 'superscript', which means it will be smaller and a little higher than other text. The target of the link will be something like "#footnote1". Links that contain a '#' character refer to 'anchors' within a page, and in this case we will use them to make it so that clicking on the footnote link will take the reader to the bottom of the page, where the footnotes live.

``sup``\ タグは正しくは'superscript（上付きルビ）'であり、他のテキストより小さく少し高めの位置にする。リンクの先は\ ``"#footnote1"``\ のような何かだ。\ ``'#'``\ 文字を含むリンクはページ内のアンカーを参照し、この場合は脚注をクリックすると、読者がページの末尾の脚注を読めるようにするリンクを作るのに使われる。

..  The tag to render emphasised fragments with is em, and normal text can be rendered without any extra tags.

強調を行う断片を作るタグは\ ``em``\ で、通常のテキストは拡張タグ無しで作られる。

..  [show solution]

[解答を見る]

.. code-block:: javascript

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

|hr|

..  We are almost finished. The only thing that we do not have a rendering function for yet are the footnotes. To make the "#footnote1" links work, an anchor must be included with every footnote. In HTML, an anchor is specified with an a element, which is also used for links. In this case, it needs a name attribute, instead of an href.

ほとんど終わりだ。脚注を作る関数だけが後に残っている。\ ``"#footnote1"``\ のリンクを動かすには、その脚注を含むアンカーがなければならない。HTMLで、アンカーはリンクにも使われる1つの\ ``a``\ 要素である。この場合、\ ``name``\ 属性が\ ``href``\ の代わりに必要となる。

.. code-block:: javascript

    function renderFootnote(footnote) {
      var anchor = tag("a", [], {name: "footnote" + footnote.number});
      var number = "[" + footnote.number + "] ";
      return tag("p", [tag("small", [anchor, number,
                                     footnote.content])]);
    }

..  Here, then, is the function which, when given a file in the correct format and a document title, returns an HTML document:

それから、与えられた正しい形式のファイルと文書タイトルからHTML文書を返す関数がこれだ。：

.. code-block:: javascript

    function renderFile(file, title) {
      var paragraphs = map(processParagraph, file.split("\n\n"));
      var footnotes = map(renderFootnote,
                          extractFootnotes(paragraphs));
      var body = map(renderParagraph, paragraphs).concat(footnotes);
      return renderHTML(htmlDoc(title, body));
    }

    viewHTML(renderFile(recluseFile(), "The Book of Programming"));

..  The concat method of an array can be used to concatenate another array to it, similar to what the + operator does with strings.

配列の\ ``concat``\ メソッドは、文字列の\ ``+``\ 演算子と同様に他の配列を連結するのに使われる。

|hr|

..  In the chapters after this one, elementary higher-order functions like map and reduce will always be available and will be used by code examples. Now and then, a new useful tool is added to this. In chapter 9, we develop a more structured approach to this set of 'basic' functions.

この章の以降では、\ ``map``\ や\ ``reduce``\ のような基礎的な高階関数が常に利用可能で、コードの例で使われる。今後、新しい便利な道具をこれに加えよう。\ :doc:`9章</Modularity>`\ にて、この'基本的'な関数のセットをより構造的なアプローチで発展させよう。

|hr|

..  When using higher-order functions, it is often annoying that operators are not functions in JavaScript. We have needed add or equals functions at several points. Rewriting these every time, you will agree, is a pain. From now on, we will assume the existence of an object called op, which contains these functions:

高階関数を使うとき、JavaScriptでの演算子は関数でないことがしばしば悩みの元となる。\ ``add``\ や\ ``eauqls``\ の関数がいくつかのポイントで必要になる。毎回これらを書き直すのは、あなたも同意するだろうが、痛みを伴うことだ。これからは、これらの関数を含む\ ``op``\ オブジェクトが存在するものと仮定しよう。：

.. code-block:: javascript

    var op = {
      "+": function(a, b){return a + b;},
      "==": function(a, b){return a == b;},
      "===": function(a, b){return a === b;},
      "!": function(a){return !a;}
      /* and so on */
    };

..  So we can write reduce(op["+"], 0, [1, 2, 3, 4, 5]) to sum an array. But what if we need something like equals or makeAddFunction, in which one of the arguments already has a value? In that case we are back to writing a new function again.

そう、我々は配列を合計するのに、\ ``reduce(op["+"], 0, [1, 2, 3, 4, 5])``\ と書くことができる。しかし\ ``equals``\ や、引数の1つに既に値を持つ\ ``makeAddFunction``\ のようなものが必要なのだとしたら？そのような場合新しい関数を書き直さなければならない。

..  For cases like that, something called 'partial application' is useful. You want to create a new function that already knows some of its arguments, and treats any additional arguments it is passed as coming after these fixed arguments. This can be done by making creative use of the apply method of a function:

そのような場合には、'部分的な適用(partial application)'というものが便利だ。その引数の幾つかが既に知られている新しい関数を作りたいなら、追加の引数はこれら固定の引数の後に渡されるように扱えば良い。これで関数の\ ``apply``\ メソッドを創造的に使うことができる。：

.. code-block:: javascript

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


..  We want to allow binding multiple arguments at the same time, so the asArray function is necessary to make normal arrays out of the arguments objects. It copies their content into a real array, so that the concat method can be used on it. It also takes an optional second argument, which can be used to leave out some arguments at the start.

複数の引数を同時に結びつけることができるようにしたい、\ ``asArray``\ 関数は通常の配列を\ ``argments``\ オブジェクトの外に出すのに必要となる。\ ``concat``\ メソッドが使えるように、これらの中身を本物の配列にコピーする。それはオプションの2つめの引数も取ることができ、スタートから幾つかの引数を外すのに使うことができる。

..  Also note that it is necessary to store the arguments of the outer function (partial) into a variable with another name, because otherwise the inner function can not see them ― it has its own arguments variable, which shadows the one of the outer function.

外側の関数(\ ``partial``\ )の\ ``arguments``\ を別な名前の変数に格納する必要があることにも注意。なぜなら、そうでなければ内側の関数はそれをみることができない―それはそれ自身の\ ``arguments``\ 変数を持ちそれによって外側の関数の引数が隠れてしまうからである。

..  Now equals(10) can be written as partial(op["=="], 10).

これで\ ``equal(10)``\ は\ ``partial(op["=="], 10)``\ と書けるようになった。

.. code-block:: javascript

    show(map(partial(op["+"], 1), [0, 2, 4, 6, 8, 10]));

..  The reason map takes its function argument before its array argument is that it is often useful to partially apply map by giving it a function. This 'lifts' the function from operating on a single value to operating on an array of values. For example, if you have an array of arrays of numbers, and you want to square them all, you do this:

``map``\ が配列の引数の前に、関数の引数を取る理由は、それが関数として与えられた方がmapを部分的に適用するのにしばしば便利だからである。これは1つの値を操作する関数から値の配列を操作する関数に格上げする。例えば、数値の配列の配列があって、これらの全ての平方をとりたいときはこうする。：

.. code-block:: javascript

    function square(x) {return x * x;}

    show(map(partial(map, square), [[10, 100], [12, 16], [0, 1]]));

|hr|

..  One last trick that can be useful when you want to combine functions is function composition. At the start of this chapter I showed a function negate, which applies the boolean not operator to the result of calling a function:

最後のトリックは関数を連結して合成関数にしたいときに便利だ。この章のスタートで、呼び出す関数の結果に真偽値の\ *not*\ 演算子を適用する\ ``negate``\ 関数を見せた。：

.. code-block:: javascript

    function negate(func) {
      return function() {
        return !func.apply(null, arguments);
      };
    }

..  This is a special case of a general pattern: call function A, and then apply function B to the result. Composition is a common concept in mathematics. It can be caught in a higher-order function like this:

これは一般的なパターンの特殊な場合である：関数Aを呼び、それからその結果に関数Bを適用する。数学では合成は共通の概念だ。高階関数でこのように書ける。：

.. code-block:: javascript

    function compose(func1, func2) {
      return function() {
        return func1(func2.apply(null, arguments));
      };
    }

    var isUndefined = partial(op["==="], undefined);
    var isDefined = compose(op["!"], isUndefined);
    show(isDefined(Math.PI));
    show(isDefined(Math.PIE));

..  Here we are defining new functions without using the function keyword at all. This can be useful when you need to create a simple function to give to, for example, map or reduce. However, when a function becomes more complex than these examples, it is usually shorter (not to mention more efficient) to just write it out with function.

ここでは\ ``function``\ キーワードを全く使わずに新しい関数を定義している。これは例えば、\ ``map``\ や\ ``reduce``\ に渡す単純な関数を作り出すことが必要なときに便利だ。しかしながら、関数がこれらの例より複雑になるときは、\ ``function``\ としてそのまま書き出すよりも通常は短くなる（より効率的であるとは言えない）。
