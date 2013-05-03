============
導入
============
..  Chapter 1:
..  Introduction

.. include:: inclusion.txt

..  When personal computers were first introduced, most of them came equipped with a simple programming language, usually a variant of BASIC. Interacting with the computer was closely integrated with this language, and thus every computer-user, whether he wanted to or not, would get a taste of it. Now that computers have become plentiful and cheap, typical users don't get much further than clicking things with a mouse. For most people, this works very well. But for those of us with a natural inclination towards technological tinkering, the removal of programming from every-day computer use presents something of a barrier.

パーソナルコンピューターが最初に導入されたとき、その多くはシンプルなプログラミング言語を備えていて、通常それはBASICの変種だった。コンピューターとのやりとりはこの言語に統合され、そのため全てのコンピューターユーザーは好むと好まざるに係わらず、それに触れざるを得なかった。現在コンピューターは豊富で安価なものとなり、典型的なユーザーはマウスクリック以上のことをしなくなった。多くの人々にとって、それは良いことだ。しかし、我々のような、テクノロジーいじりに自然に惹かれるような者にとっては、コンピューターを使う毎日からプログラミングが取り除かれるということは、何らかの障壁が設けられたようなものだ。

..  Fortunately, as an effect of developments in the World Wide Web, it so happens that every computer equipped with a modern web-browser also has an environment for programming JavaScript. In today's spirit of not bothering the user with technical details, it is kept well hidden, but a web-page can make it accessible, and use it as a platform for learning to program.

幸運なことに、ワールドワイドウェブの開発の影響で、全てのコンピューターがモダンなWebブラウザを備え、JavaScriptによるプログラミング環境を持つことになった。今日ではユーザーは技術的な細部に飽きさせられることない。それはうまく隠されたままだ。しかしWebページはアクセス可能になり、プログラミングの学習のプラットフォームとして使うことができるようになった。

..  That is what this (hyper-)book tries to do.

この本で行おうとしているのがそれだ。

..  raw:: html

    <hr>

..  I do not enlighten those who are not eager to learn, nor arouse those who are not anxious to give an explanation themselves. If I have presented one corner of the square and they cannot come back to me with the other three, I should not go over the points again.
..  ― Confucius

*私は学習意欲のない者、彼ら自身を解説することに気を払わない者を啓蒙しようとは思わない。もし四角形の1つの角を見せても彼らが残りの3つを私に返さないとしたら、私は再びそのポイントを示すことはしない。*
― 孔子

..  Besides explaining JavaScript, this book tries to be an introduction to the basic principles of programming. Programming, it turns out, is hard. The fundamental rules are, most of the time, simple and clear. But programs, while built on top of these basic rules, tend to become complex enough to introduce their own rules, their own complexity. Because of this, programming is rarely simple or predictable. As Donald Knuth, who is something of a founding father of the field, says, it is an art.

JavaScriptの解説に加えて、この本ではプログラミングの基本原則を紹介しようとしている。プログラミングは明らかに難しい。基礎的なルールは、多くの間、単純でクリアなこととされている。しかし、プログラムはこの基本的なルールに従って作られていても、それ自身のルールや、それ自身の複雑さを持ち込むために、複雑になりがちである。このためプログラミングが単純だったり予測可能であることはまれだ。この分野における父であると見られているDonald Knuthはそれは\ *アート*\ であると言った。

..  To get something out of this book, more than just passive reading is required. Try to stay sharp, make an effort to solve the exercises, and only continue on when you are reasonably sure you understand the material that came before.

この本から何か得ようとするなら、受動的に読む以上のことが必要となる。鋭くあること、演習を解くために努力すること、そして理解が深まったと確認できるまで続けること。

..  raw:: html

    <hr>

..  The computer programmer is a creator of universes for which he alone is responsible. Universes of virtually unlimited complexity can be created in the form of computer programs.
..  ― Joseph Weizenbaum, Computer Power and Human Reason

*コンピュータープログラマーは彼だけが責任を負う宇宙の創造者だ。仮想的で限界の無い複雑さの宇宙はコンピュータープログラムという形で創造される。*
― Joseph Weizenbaum, コンピューターパワーと人間の理性


..  A program is many things. It is a piece of text typed by a programmer, it is the directing force that makes the computer do what it does, it is data in the computer's memory, yet it controls the actions performed on this same memory. Analogies that try to compare programs to objects we are familiar with tend to fall short, but a superficially fitting one is that of a machine. The gears of a mechanical watch fit together ingeniously, and if the watchmaker was any good, it will accurately show the time for many years. The elements of a program fit together in a similar way, and if the programmer knows what he is doing, the program will run without crashing.

プログラムは多くの物からなる。プログラマーによってタイプされたテキストの一片、それはコンピューターにあることを行わせる力であったり、コンピュータメモリの中のデータであったり、同じメモリで行われる動作を制御するするものであったりする。精巧に組み合わされた機械時計の歯車は、よい時計職人によるものであれば、多くの年月にわたって時間を示し続ける。同じように組み合わされたプログラムの要素は、もしプログラマーが彼自身の行うことを知っていればクラッシュすることなしに動き続ける。

..  A computer is a machine built to act as a host for these immaterial machines. Computers themselves can only do stupidly straightforward things. The reason they are so useful is that they do these things at an incredibly high speed. A program can, by ingeniously combining many of these simple actions, do very complicated things.

コンピューターはこれらの実体のない機械のホストとして動くように作られたものだ。コンピュータ自体は愚鈍にまっすぐなことを行うことしかできない。これらのことを非常に速いスピードで行うほうが便利だというのがその理由だ。プログラムは単純な動作を精巧に組み合わせることで、とても複雑なことを行うことを可能にする。

..  To some of us, writing computer programs is a fascinating game. A program is a building of thought. It is costless to build, weightless, growing easily under our typing hands. If we get carried away, its size and complexity will grow out of control, confusing even the one who created it. This is the main problem of programming. It is why so much of today's software tends to crash, fail, screw up.

我々の中のある者にとっては、コンピュータープログラムを書くと言うことは魅惑的なゲームだ。プログラムは思考の建築物だ。費用をかけずに建てることができ、重くなく、我々のタイプする手によって簡単に大きくできる。もし我々がいなくなれば、そのサイズと複雑さはコントロールから外れて大きくなり、それを操作する者を混乱させる。これはプログラミングの主要な問題だ。多くの今日のソフトウェアがクラッシュし、失敗し、へまをしがちな理由である。

.. When a program works, it is beautiful. The art of programming is the skill of controlling complexity. The great program is subdued, made simple in its complexity.

プログラムが働くとき、それは美しい。プログラミングの芸術は複雑さをコントロールする技術にある。偉大なプログラムは複雑さを抑えられ、単純にされたものだ。

..  raw:: html

    <hr>

..  Today, many programmers believe that this complexity is best managed by using only a small set of well-understood techniques in their programs. They have composed strict rules about the form programs should have, and the more zealous among them will denounce those who break these rules as bad programmers.

今日、多くのプログラマーは、彼らのプログラムのよく理解された技術の小さいセットを使うだけで、この複雑さはうまく管理されていると信じている。彼らはプログラムがあるべき姿についての厳格なルールを作り、これらのルールを破る者を悪いプログラマーであると熱心に非難する。

..  What hostility to the richness of programming! To try to reduce it to something straightforward and predictable, to place a taboo on all the weird and beautiful programs. The landscape of programming techniques is enormous, fascinating in its diversity, still largely unexplored. It is certainly littered with traps and snares, luring the inexperienced programmer into all kinds of horrible mistakes, but that only means you should proceed with caution, keep your wits about you. As you learn, there will always be new challenges, new territory to explore. The programmer who refuses to keep exploring will surely stagnate, forget his joy, lose the will to program (and become a manager).

プログラミングの豊かさへの、この敵意はなんだ！何かをまっすぐに予測可能にするために豊かさを削ぐことは、すばらしくかつ美しいもの全てをタブーに置き換えることだ。プログラミング技術の展望は大きく、その多様性という魅力があり、いまだ大きな未踏である。あなたが注意深く進むため、あなたについてのあなたの知恵を保つというだけの意味しかないことで、罠と仕掛けをちりばめ、経験のないプログラマーを全ての種類の恐ろしい過ちに導くことになることは確かだ。常に新しい挑戦者であり、新しい場所への冒険であるかのように学びなさい。冒険し続けることをやめたプログラマーは確実に、澱んで、楽しみを忘れ、プログラムする意志を失い（、そして管理者になってしまう）。

..  As far as I am concerned, the definite criterion for a program is whether it is correct. Efficiency, clarity, and size are also important, but how to balance these against each other is always a matter of judgement, a judgement that each programmer must make for himself. Rules of thumb are useful, but one should never be afraid to break them.

私の係わるところでは、プログラムの評価基準はそれが正しいかと定義する。効率、明瞭さ、サイズもまた重要だが、互いに対立するそれらをどのようにバランスさせるかは常に価値判断の問題であり、価値判断はそれぞれのプログラマーが自分自身で行わなければならないものだ。経験則は便利だが、それを破ることを恐れるべきではない。

..  raw:: html

    <hr>

..  In the beginning, at the birth of computing, there were no programming languages. Programs looked something like this:

事の始め、コンピューティングの誕生のとき、プログラミング言語がなかったとき、プログラムはこのようなものであった。::

    00110001 00000000 00000000
    00110001 00000001 00000001
    00110011 00000001 00000010
    01010001 00001011 00000010
    00100010 00000010 00001000
    01000011 00000001 00000000
    01000001 00000001 00000001
    00010000 00000010 00000000
    01100010 00000000 00000000

..  That is a program to add the numbers from one to ten together, and print out the result (1 + 2 + ... + 10 = 55). It could run on a very simple kind of computer. To program early computers, it was necessary to set large arrays of switches in the right position, or punch holes in strips of cardboard and feed them to the computer. You can imagine how this was a tedious, error-prone procedure. Even the writing of simple programs required much cleverness and discipline, complex ones were nearly inconceivable.

これは1から10までの数を足し合わせ、その結果(1 + 2 + ... + 10 = 55)をプリントするプログラムである。とても単純な種類のコンピューター上で走らせることができた。初期のコンピューターをプログラムするには、スイッチの巨大な配列を正しい位置にセットしたり、カードの束のパンチ穴をコンピュータに読み込ませる必要があった。あなたはこれは退屈で、ミスしやすい処理であるというイメージを持つかもしれない。単純なプログラムを書くのにも大変な利口さと修練が必要となり、これらを組み合わせることは想像を絶する。

..  Of course, manually entering these arcane patterns of bits (which is what the 1s and 0s above are generally called) did give the programmer a profound sense of being a mighty wizard. And that has to be worth something, in terms of job satisfaction.

もちろん、これらの不可解なビット（上記の1と0のことを全般的にそう呼ぶ）のパターンを手作業で入力することはプログラマーが強力な魔法使いであるかのように強い感覚を与える。仕事のやりがいといったような、何か価値のあるものがある。

..  Each line of the program contains a single instruction. It could be written in English like this:

このプログラムの各行は1つの命令を含んでいる。英語で書けばこのようになる。::

    1. メモリの位置0に数値0を保存する
    2. メモリの位置1に数値1を保存する
    3. メモリの位置1にある値をメモリの位置2に保存する
    4. メモリの位置2にある値から11を引く
    5. メモリの位置2にある値が0であれば、命令9から続ける
    6. メモリの位置1にある値をメモリの位置0に足す
    7. メモリの位置1にある値に1を足す
    8. 命令3から続ける
    9. メモリの位置0にある値を出力する

..  Store the number 0 in memory location 0
..  Store the number 1 in memory location 1
..  Store the value of memory location 1 in memory location 2
..  Subtract the number 11 from the value in memory location 2
..  If the value in memory location 2 is the number 0, continue with instruction 9
..  Add the value of memory location 1 to memory location 0
..  Add the number 1 to the value of memory location 1
..  Continue with instruction 3
..  Output the value of memory location 0

..  While that is more readable than the binary soup, it is still rather unpleasant. It might help to use names instead of numbers for the instructions and memory locations:

これはこの2進数のスープよりは読みやすいが、まだ意地悪だ。命令やメモリを示す数値を名前に置き換えれば助けになるかもしれない。::

    '合計'に0をセットする
    'カウント'に1をセットする
    [ループ]
      '比較'に'カウント'をセットする
      '比較'から11を引く
      もし'比較'が0ならば[終了]から続ける
      'カウント'を'合計'に足す
      'カウント'に1を足す
      [ループ]から続ける
    [終了]
    '合計'を出力する



..  At this point it is not too hard to see how the program works. Can you? The first two lines give two memory locations their starting values: total will be used to build up the result of the program, and count keeps track of the number that we are currently looking at. The lines using compare are probably the weirdest ones. What the program wants to do is see if count is equal to 11, in order to decide whether it can stop yet. Because the machine is so primitive, it can only test whether a number is zero, and make a decision (jump) based on that. So it uses the memory location labelled compare to compute the value of count - 11, and makes a decision based on that value. The next two lines add the value of count to the result, and increment count by one every time the program has decided that it is not 11 yet.

この点では、プログラムがどのように動くのか見るのが難しすぎるということはないだろう。できる？最初の2行は2つのメモリの位置に開始時の値を与えている：\ ``合計``\ はプログラムの結果を作るために使われ、\ ``カウント``\ は我々が今見ている値を追いつづける。\ ``比較``\ を用いている行はおそらく一番奇異なものだ。プログラムが求めているのは、もう実行を止めていいかどうか判定するために、\ ``カウント``\ が11と等しいかどうか見ることだ。機械はとても原始的なので、数値がゼロであるかどうかテストすることしかできず、判定（ジャンプ）はそれに基づく。そこで\ ``比較``\ と名付けたメモリの位置を\ ``カウント - 11``\ の値を計算するために使い、この値を元に判定する。次の2行は\ ``カウント``\ を結果に足して、\ ``カウント``\ を1増やすということを、プログラムが\ ``カウント``\ がまだ11になってないと判断する間、毎回続ける。


..  Here is the same program in JavaScript:

同じプログラムをJavaScriptで書いたのがこれ:

.. code-block:: javascript

    var total = 0, count = 1;
    while (count <= 10) {
      total += count;
      count += 1;
    }
    print(total);

..  This gives us a few more improvements. Most importantly, there is no need to specify the way we want the program to jump back and forth anymore. The magic word while takes care of that. It continues executing the lines below it as long as the condition it was given holds: count <= 10, which means 'count is less than or equal to 10'. Apparently, there is no need anymore to create a temporary value and compare that to zero. This was a stupid little detail, and the power of programming languages is that they take care of stupid little details for us.

これは我々に幾つかの進歩をもたらす。最も重要なのは、我々はプログラム後戻りさせるための特別なジャンプを書く必要がなく、もうそれを強いられることもなくなったということだ。魔法の言葉、\ ``while``\ がその面倒を見てくれる。それはその下の行を、与えられた条件：\ ``count <= 10``\ （\ ``count``\ が\ ``10``\ より小さいか等しいという意味）が続く限り実行し続ける。見たところ、一時的な値を作り出してゼロと比較する必要はなくなったようだ。これは愚かで些細なことであり、そしてこのような我々にとっては愚かで些細であることの面倒を見ることがプログラミング言語の力なのである。

..  Finally, here is what the program could look like if we happened to have the convenient operations range and sum available, which respectively create a collection of numbers within a range and compute the sum of a collection of numbers:

最後に、もし我々が、範囲に含まれる数のコレクションを作り、数のコレクションの合計を計算する\ ``range``\ や\ ``sum``\ といった便利な操作を使えれば書けたであろうプログラムがこれ:

.. code-block:: javascript

    print(sum(range(1, 10)));

..  The moral of this story, then, is that the same program can be expressed in long and short, unreadable and readable ways. The first version of the program was extremely obscure, while this last one is almost English: print the sum of the range of numbers from 1 to 10. (We will see in later chapters how to build things like sum and range.)

この文章の方針として、それから、同じプログラムは長く書かれていれば短く、読みにくければ読みやすいものにするべきだ。プログラムの最初のバージョンは非常に不明瞭で、最後のものはほとんど英語だ：\ ``1``\ から\ ``10``\ までの\ ``range``\ の数の\ ``sum``\ を\ ``print``\ せよ。（後の章で\ ``sum``\ や\ ``range``\ のようなものの作りかたを見よう）

..  A good programming language helps the programmer by providing a more abstract way to express himself. It hides uninteresting details, provides convenient building blocks (such as the while construct), and, most of the time, allows the programmer to add building blocks himself (such as the sum and range operations).

良いプログラミング言語はより抽象的な手段を提供することで、プログラマーが彼自身が書きたいことを書くことを助ける。興味を引かない細部を隠し、（\ ``while``\ 構造のような）便利な組み立て用のブロックを提供し、そして、多くの場合に、プログラマーが自分自身で（\ ``sum``\ や\ ``range``\ 操作のような）組み立て用の部品を加えることを可能にする。

..  raw:: html

    <hr>

..  JavaScript is the language that is, at the moment, mostly being used to do all kinds of clever and horrible things with pages on the World Wide Web. Some people claim that the next version of JavaScript will become an important language for other tasks too. I am unsure whether that will happen, but if you are interested in programming, JavaScript is definitely a useful language to learn. Even if you do not end up doing much web programming, the mind-bending programs I will show you in this book will always stay with you, haunt you, and influence the programs you write in other languages.

JavaScriptはそのような言語であり、現在、ワールドワイドウェブ上のページで全ての種類の利口な恐ろしい事柄にもっとも使われている。ある\ `人々 <http://steve-yegge.blogspot.com/2007/02/next-big-language.html>`_\ はJavaScriptの次のバージョンが他の仕事についても重要な言語になるだろうと主張している。私はそうなると確信してはいないが、もしプログラミングに興味があるなら、JavaScriptは学ぶには良い言語である。たとえ結局ウェブのプログラミングをしないのであっても、私はこの本で見せる驚くべきプログラムはいつもあなたとともにあり、あなたの所に通い、他の言語であなたが書くプログラムに影響するだろう。

..  There are those who will say terrible things about JavaScript. Many of these things are true. When I was for the first time required to write something in JavaScript, I quickly came to despise the language. It would accept almost anything I typed, but interpret it in a way that was completely different from what I meant. This had a lot to do with the fact that I did not have a clue what I was doing, but there is also a real issue here: JavaScript is ridiculously liberal in what it allows. The idea behind this design was that it would make programming in JavaScript easier for beginners. In actuality, it mostly makes finding problems in your programs harder, because the system will not point them out to you.

JavaScriptについて\ *恐ろしい*\ ことと言われているのはこのようなことだ。これらの多くのことは真実である。私が最初にJavaScriptで何かを書くことを必要としたとき、私はすぐにその言語を嫌うようになった。私がタイプしたことのほとんどが受け入れられ、しかし私が意図したものと全く違うように解釈される。私がしたいことへの道筋が得られないことが多くあり、現実の問題もここにある：JavaScriptはそれが許す限り途方もなく自由である。このデザインの背後にあるアイデアはJavaScriptでのプログラミングを初心者にとって簡単なものにしようというものだ。実際に、システムがあなたにそれを指摘しなくなるため、あなたのプログラムに問題を見つけるのは困難になるだろう。

..  However, the flexibility of the language is also an advantage. It leaves space for a lot of techniques that are impossible in more rigid languages, and it can be used to overcome some of JavaScript's shortcomings. After learning it properly, and working with it for a while, I have really learned to like this language.

しかしながら、言語の柔軟さはアドバンテージでもある。より厳格な言語では不可能なたくさんのテクニックの余地を残し、JavaScriptの短所のいくつかを克服するのに使えるかもしれない。正しく学んだ後であれば、しばらくそれは動作し、私はこの言語に\ *似た*\ ものを学ぶだろう。

..  raw:: html

    <hr>

..  Contrary to what the name suggests, JavaScript has very little to do with the programming language named Java. The similar name was inspired by marketing considerations, rather than good judgement. In 1995, when JavaScript was introduced by Netscape, the Java language was being heavily marketed and gaining in popularity. Apparently, someone thought it a good idea to try and ride along on this marketing. Now we are stuck with the name.

名前が示すのとは正反対に、JavaScriptはJavaと名付けられた言語にほとんど似ていない。同じような名前になっているのは良き判断よりむしろマーケティング上の考慮によるものだ。1995年に、JavaScriptがNetscapeに導入されたとき、Java言語はマーケット上重要であり、人気も上昇していた。見たところ、誰かがこの市場に乗ってみるのがいいアイデアだと考えたのだ。今では我々はこの名前にハメられている。

..  Related to JavaScript is a thing called ECMAScript. When browsers other than Netscape started to support JavaScript, or something that looked like it, a document was written to describe precisely how the language should work. The language described in this document is called ECMAScript, after the organisation that standardised it.

JavaScriptに関係あるのはECMAScriptと呼ばれているものだ。Netscape以外のブラウザがJavaScript、あるいはそれに似たもののサポートを始めたとき、言語がどのように動作するかの正確な説明がドキュメントには書かれた。組織化と標準化の後、このドキュメントに説明されている言語はECMAScriptと呼ばれた。

..  ECMAScript describes a general-purpose programming language, and does not say anything about the integration of this language in an Internet browser. JavaScript is ECMAScript plus extra tools for dealing with Internet pages and browser windows.

ECMAScriptは汎用のプログラミング言語と説明され、インターネットブラウザへのこの言語の統合については何も言われなかった。JavaScriptはECMAScriptにインターネットページとブラウザウインドウを取り扱う拡張ツールを加えたものである。

..  A few other pieces of software use the language described in the ECMAScript document. Most importantly, the ActionScript language used by Flash is based on ECMAScript (though it does not precisely follow the standard). Flash is a system for adding things that move and make lots of noise to web-pages. Knowing JavaScript won't hurt if you ever find yourself learning to build Flash movies.

ECMAScriptのドキュメントにはこの言語の他のソフトウェアでの使用についても説明されていた。最も重要なのは、Flashで使われるActionScriptもECMAScriptが土台になっていることだ（正確には標準に従っていないにも係わらず）。Flashは動くものやたくさんの音をウェブページに追加するシステムである。JavaScriptを知ることはもしあなたがFlashムービーの制作を学ぶことになったとしても害にはならないだろう。

..  JavaScript is still evolving. Since this book came out, ECMAScript 5 has been released, which is compatible with the version described here, but adds some of the functionality we will be writing ourselves as built-in methods. The newest generation of browsers provides this expanded version of JavaScript. As of 2011, 'ECMAScript harmony', a more radical extension of the language, is in the process of being standardised. You should not worry too much about these new versions making the things you learn from this book obsolete. For one thing, they will be an extension of the language we have now, so almost everything written in this book will still hold.

JavaScriptはまだ進化している。この本が出た後、ここで書かれたバージョンの互換を持ちつつ、我々自身で組み込みメソッドのようなものを書くのに使える幾つかの機能が追加されたECMAScript5がリリースされた。最新世代のブラウザはこの拡張されたバージョンのJavaScriptを提供する。2011年に'ECMAScript harmony'という、もっと根本的な言語の拡張があり、それは標準化のプロセスにある。新しいバージョンがこの本で学ぶあなたを時代遅れにすると嘆く必要はない。一つには、今行われている拡張で、この本の中に書かれているほとんどのことはそのまま残る。

..  raw:: html

    <hr>

..  Most chapters in this book contain quite a lot of code1. In my experience, reading and writing code is an important part of learning to program. Try to not just glance over these examples, but read them attentively and understand them. This can be slow and confusing at first, but you will quickly get the hang of it. The same goes for the exercises. Don't assume you understand them until you've actually written a working solution.

この本の多くの章には実にたくさんのコードが含まれる\ [#f1]_\ 。私の経験では、読み書きすることがプログラムの学習での重要な位置を占める。これらの例を一見して分らなくても、注意深く読み理解すること。初めは時間がかかり混乱するかもしれないが、じきに自分のものにできるだろう。演習も同じ事だ。正しく動く回答を書けるようになるまでは理解できたとは思わないこと。

..  Because of the way the web works, it is always possible to look at the JavaScript programs that people put in their web-pages. This can be a good way to learn how some things are done. Because most web programmers are not 'professional' programmers, or consider JavaScript programming so uninteresting that they never properly learned it, a lot of the code you can find like this is of a very bad quality. When learning from ugly or incorrect code, the ugliness and confusion will propagate into your own code, so be careful who you learn from.

ウェブの仕組みとして、人々はウェブページとしてJavaScriptプログラムを見ることが可能である。それは物事がどうやってなされているか知るには良い。多くのウェブプログラマーが’プロフェッショナルな’プログラマーではないため、またはJavaScriptのプログラミング、単体テストを学んでいないため、あなたが見ることができるコードの多くはとても低い品質のものである。醜く正しくないコードから学べばあなた自身のコードにも醜さと混乱が伝染するから、何から学ぶかに注意を払うべきだ。

..  raw:: html

    <hr>

..  To allow you to try out programs, both the examples and the code you write yourself, this book makes use of something called a console. If you are using a modern graphical browser (Internet Explorer version 6 or higher, Firefox 1.5 or higher, Opera 9 or higher, Safari 3 or higher), the pages in this book will show a bar at the bottom of your screen. You can open the console by clicking on the little arrow on the far right of this bar.

例とあなた自身が書いたコードの両方のプログラムを、あなたが試すことができるよう、コンソールといわれるものを本書に用意している\ [#f2]_\ 。もしあなたがモダンなグラフィック機能を備えたブラウザ（InternetExplorer6以降、Firefox1.5以降、Opera9以降、Safari3以降）であれば、本書の各ページの下にバーが表示される。右端の矢印をクリックするとコンソールを開くことができる。

..  The console contains three important elements. There is the output window, which is used to show error messages and things that programs print out. Below that, there is a line where you can type in a piece of JavaScript. Try typing in a number, and pressing the enter key to run what you typed. If the text you typed produced something meaningful, it will be shown in the output window. Now try typing wrong!, and press enter again. The output window will show an error message. You can use the arrow-up and arrow-down keys to go back to previous commands that you typed.

コンソールはこれらの重要な要素を含む。出力ウインドウというのがあり、ここにはエラーメッセージやプログラムがプリントアウトしたものを見るのに使われる。その下にあなたがJavaScriptをタイプできる行がある。数値をタイプしエンターキーを押せばあなたがタイプしたものが実行される。何か意味のあるテキストを入力すれば出力ウインドウに表示される。ここで\ ``wrong``\ とタイプしてエンターを押してみよう！出力ウインドウにはエラーメッセージが表示される。上下の矢印キーで以前タイプした命令を遡ることができる。

..  For bigger pieces of code, those that span multiple lines and which you want to keep around for a while, the field on the right can be used. The 'Run' button is used to execute programs written in this field. It is possible to have multiple programs open at the same time. Use the 'New' and 'Load' buttons to add a new program (empty or from a file on the web). When there is more than one open program, the menu next to the 'Run' button can be used to choose which one is being shown. The 'Close' button, as you might expect, closes a program.

もっと大きな複数行にわたるようなコードをしばらくの間試したいなら、右のフィールドを使える。'Run'ボタンはこのフィールドに書かれたコードを実行するのに使う。複数のプログラムを同時に開くこともできる。'New'ボタンと'Load'ボタンを使い新しいプログラム（空あるいはウェブ上のファイル）を加えよう。1つ以上のプログラムを開いているとき、'Run'ボタンの次のメニューは表示されるものを選ぶのに使われる。'Close'ボタンは、あなたの期待通り、プログラムを閉じる。

..  Example programs in this book always have a small button with an arrow in their top-right corner, which can be used to run them. The example we saw earlier looked like this::

本書のプログラム例の右上には常に小さな矢印があり、プログラムを実行するのに使う。先ほど我々が見たこの例。:

.. code-block:: javascript

    var total = 0, count = 1;
    while (count <= 10) {
      total += count;
      count += 1;
    }
    print(total);

..  Run it by clicking the arrow. There is also another button, which is used to load the program into the console. Do not hesitate to modify it and try out the result. The worst that could happen is that you create an endless loop. An endless loop is what you get when the condition of the while never becomes false, for example if you choose to add 0 instead of 1 to the count variable. Now the program will run forever.

矢印をクリックし実行しよう。他のボタンは、コンソールにこのプログラムをロードする。ためらうことなくプログラムを変更しその結果を試してみよう。最悪、無限ループを作ってしまうことがある。無限ループとは例えばカウントの変数に\ ``1``\ の代わりに\ ``0``\ を足したときのように、\ ``while``\ の条件が永遠にfalseにならないことだ。このときプログラムは永遠に走り続ける。

..  Fortunately, browsers keep an eye on the programs running inside them. Whenever one of them is taking suspiciously long to finish, they will ask you if you want to cut it off.

幸運にも、ブラウザはその中で走っているプログラムを監視し続ける。終了までに特に長くかかるものがあれば、ブラウザはあなたに実行を中断しないかと尋ねるだろう。

..  raw:: html

    <hr>

..  In some later chapters, we will build example programs that consist of many blocks of code. Often, you have to run every one of them for the program to work. As you may have noticed, the arrow in a block of code turns purple after the block has been run. When reading a chapter, try to run every block of code you come across, especially those that 'define' something new (you will see what that means in the next chapter).

後の章で、我々は多くのブロックを持つプログラム例を作るだろう。しばしば、それらの一つ一つを実行しプログラムを動かしたくなるだろう。注意していれば、コードブロックの矢印が実行後に紫色になる。章を読みながら現われ、何か新しいものを'define'する全てのコードのブロックを試していこう。（その意味は次の章で見られる）

..  It is, of course, possible that you can not read a chapter in one sitting. This means you will have to start halfway when you continue reading, but if you don't run all the code starting from the top of the chapter, some things might not work. By holding the shift key while pressing the 'run' arrow on a block of code, all blocks before that one will be run as well, so when you start in the middle of a chapter, hold shift the first time you run a piece of code, and everything should work as expected.

それは、もちろん、一気に章を読みすすめることができないかもしれないということである。読み進める途中で、章の最初のコードは上手く動かないかもしれない。シフトキーを押している間、コードのブロック上の'run'矢印は、それが動く前の全てのブロックも同じように実行し、章の途中から始めた時などは、最初にコードを実行するときシフトを押せば、全て期待通りに動くだろう。

..  raw:: html

    <hr>

..  Finally, the little face in the top-left corner of your screen can be used to send me, the author, a message. If you have a comment, or you find a passage ridiculously confusing, or you just spot a spelling error, tell me about it. Sending a message can be done without leaving the page, so it won't interrupt your reading.

最後に、左上の隅にある小さい顔は、著者である私にメッセージを送るのに使える。もし、コメントがあったり、途方もなく道に迷ったり、スペルミスなどを見つけたときは教えて欲しい。ページから離れることなくメッセージを送れるから、あなたの読解が妨げられることもないだろう。

.. 'Code' is the substance that programs are made of. Every piece of a program, whether it is a single line or the whole thing, can be referred to as 'code'.

.. rubric:: 脚注

.. [#f1] ‘コード’とはプログラムを作る要素である。プログラムの全ての部品は、単一行であろうと全体であろうと‘コード’と呼ばれる。
.. [#f2] （訳注）現時点では翻訳文にコンソールおよび著者へのメッセージ送信の両機能を組み込めていません。本家のコンソールは\ `こちら <http://eloquentjavascript.net/paper.html>`_\ 。
