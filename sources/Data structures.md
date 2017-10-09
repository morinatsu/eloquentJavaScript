=====================================================
データ構造: オブジェクトと配列
=====================================================
..  Chapter 4:
..  Data structures: Objects and Arrays

..  This chapter will be devoted to solving a few simple problems. In the process, we will discuss two new types of values, arrays and objects, and look at some techniques related to them.

この章では2,3の単純な問題の解決に没頭する。その過程で、2つの新しいデータ型、配列とオブジェクトについて論じ、それらに関するテクニックを見ていく。

..  Consider the following situation: Your crazy aunt Emily, who is rumoured to have over fifty cats living with her (you never managed to count them), regularly sends you e-mails to keep you up to date on her exploits. They usually look like this:

..      Dear nephew,
..      Your mother told me you have taken up skydiving. Is this true? You watch yourself, young man! Remember what happened to my husband? And that was only from the second floor!

..      Anyway, things are very exciting here. I have spent all week trying to get the attention of Mr. Drake, the nice gentleman who moved in next door, but I think he is afraid of cats. Or allergic to them? I am going to try putting Fat Igor on his shoulder next time I see him, very curious what will happen.

..      Also, the scam I told you about is going better than expected. I have already gotten back five 'payments', and only one complaint. It is starting to make me feel a bit bad though. And you are right that it is probably illegal in some way.

..      (... etc ...)

..      Much love, Aunt Emily

..      died 27/04/2006: Black Leclère

..      born 05/04/2006 (mother Lady Penelope): Red Lion, Doctor Hobbles the 3rd, Little Iroquois

次のような状況を考える：エミリーというおかしな叔母さん、彼女は50匹を超える猫（数えるのはあきらめている）と暮らしていると噂されており、定期的にあなたに自分自慢の電子メールを送り続けてくる。大抵このようなもの：

*愛しい甥へ*

*あなたがスカイダイビングをやっているとお母さんが言っていました。本当？お気を付けなさい、若いかた！私の夫に何があったか覚えてる？2階から飛び降りただけなのに！*

*とにかく、面白いことがあるの。ドレークさんというかっこいい殿方が上の階に越してきて、毎週彼の注意を引こうとしているのだけど、彼は猫が怖いみたいなの。でなければ猫アレルギーとか？今度あったら太っちょイゴールを肩に押しつけて何が起こるか見てみようと思っているの。*

*前に話した詐欺の件は思ったより良いことになったわ。1回苦情を言っただけで、もう5回も'支払'が来てる。私の気分を悪くするために始めたのね。何かの法律に違反していることを証明できるという、あなたの意見は正しかったわ。*

*(... etc ...)*

*たくさんの愛情を、エミリー叔母さんより*

*died 27/04/2006: Black Leclère*

*born 05/04/2006 (mother Lady Penelope): Red Lion, Doctor Hobbles the 3rd, Little Iroquois*

..  To humour the old dear, you would like to keep track of the genealogy of her cats, so you can add things like "P.S. I hope Doctor Hobbles the 2nd enjoyed his birthday this Saturday!", or "How is old Lady Penelope doing? She's five years old now, isn't she?", preferably without accidentally asking about dead cats. You are in the possession of a large quantity of old e-mails from your aunt, and fortunately she is very consistent in always putting information about the cats' births and deaths at the end of her mails in precisely the same format.

老人の機嫌をとるために、彼女の猫の系譜を追跡して、誤って死んだ猫のことに触れるのを避けつつ、"追伸： Doctor Hobbles 2ndが今週土曜の誕生日を楽しめるといいですね！"、あるいは"Lasy Penelopeの調子はどうですか？彼女は5歳でしたよね？"といったことを返信に付け足すのはどうだろうか。あなたは叔母さんからの古い電子メールを大量に所有しており、幸運なことに叔母さんは常に彼女の猫の誕生と死をメールの末尾に正確に同じフォーマットで記している。

..  You are hardly inclined to go through all those mails by hand. Fortunately, we were just in need of an example problem, so we will try to work out a program that does the work for us. For a start, we write a program that gives us a list of cats that are still alive after the last e-mail.

これらのメールを全て手作業で処理する気にはならないだろう。幸運なことに、我々は例題を必要としているから、プログラムにこの仕事をさせてみよう。スタートとして、最後の電子メールの時点で生きている猫のリストを作るプログラムを書こう。

..  Before you ask, at the start of the correspondence, aunt Emily had only a single cat: Spot. (She was still rather conventional in those days.)

質問する前、文通の始まった時点で、エミリー叔母さんは1匹の猫だけを飼っていた:Spotである。（その頃の彼女はまだ、むしろ平凡だった）

.. |hr| raw:: html

   <hr>

|hr|

.. image:: /img/eyes.png
   :align: center

|hr|

..  It usually pays to have some kind of clue what one's program is going to do before starting to type. Here's a plan:

プログラムをタイプし始める前に、普通は問題を解く筋道を立ててみるものだ。こういうプランだ。：

..  Start with a set of cat names that has only "Spot" in it.
..  Go over every e-mail in our archive, in chronological order.
..  Look for paragraphs that start with "born" or "died".
..  Add the names from paragraphs that start with "born" to our set of names.
..  Remove the names from paragraphs that start with "died" from our set.

#. "Spot"だけが猫の名前のセットにあるところからスタートする。
#. アーカイブから全ての電子メールを取り出し、時系列順に並べる。
#. "born（誕生）"または"died（死亡）"から始まる段落を見る。
#. "born"から始まる段落に含まれる名前をセットに加える。
#. "died"から始まる段落に含まれる名前をセットから取り除く。


..  Where taking the names from a paragraph goes like this:

段落から名前を取り出すところはこのようになるだろう：

..  Find the colon in the paragraph.
..  Take the part after this colon.
..  Split this part into separate names by looking for commas.

#. 段落からコロンを探す。
#. コロンの後ろの部分を取り上げる。
#. カンマを見てこの部分を分割して名前とする。 

..  It may require some suspension of disbelief to accept that aunt Emily always used this exact format, and that she never forgot or misspelled a name, but that is just how your aunt is.

エミリー叔母さんが、あなたの叔母さんがそうであるように、常に正確なフォーマットを使うと信じられなかったり、名前のスペルを間違ったりすることについては保留する必要があるかもしれない。

|hr|

..  First, let me tell you about properties. A lot of JavaScript values have other values associated with them. These associations are called properties. Every string has a property called length, which refers to a number, the amount of characters in that string.

最初に、プロパティについて語ろう。JavaScriptの多くの値は他の値との結びつきを持つ。この結びつきをプロパティと呼ぶ。全ての文字列は\ ``length``\ （長さ）というプロパティを持ち、それは文字列の中の文字の数を示す数値である。

..  Properties can be accessed in two ways:

プロパティは2つの方法でアクセスできる:

.. code-block:: javascript

    var text = "purple haze";
    show(text["length"]);
    show(text.length);

..  The second way is a shorthand for the first, and it only works when the name of the property would be a valid variable name ― when it doesn't have any spaces or symbols in it and does not start with a digit character.

2つめの方法は1つめの短縮形で、プロパティの名前が変数名として正しいときだけ動作する―空白や記号を含んでいたり数字から始まっていたりしてはいけない。

..  The values null and undefined do not have any properties. Trying to read properties from such a value produces an error. Try the following code, if only to get an idea about the kind of error-message your browser produces in such a case (which, for some browsers, can be rather cryptic).

``null``\ や\ ``undefined``\ といった値をプロパティに持たせることはできない。そのような値をプロパティから読もうとしたらエラーになる。以下のコードを試し、そのような場合にブラウザがどんなメッセージを出すか見て欲しい。（ブラウザによっては、むしろ暗号めいたものになりうる）

.. code-block:: javascript

    var nothing = null;
    show(nothing.length);

|hr|

..  The properties of a string value can not be changed. There are quite a few more than just length, as we will see, but you are not allowed to add or remove any.

文字列のプロパティは変更できない。見たとおりの、ちょうどの\ ``length``\ であり、足したり取り去ったりすることは許されない。

..  This is different with values of the type object. Their main role is to hold other values. They have, you could say, their own set of tentacles in the form of properties. You are free to modify these, remove them, or add new ones.

オブジェクト型の値では異なる。それらの主な役目は他の値を保持することだ。それらは、こうも言える、プロパティという形の触手のセットを持つものであると。自由に変更し、取り去ったり、新しいものを付け加えたりできる。

..  An object can be written like this:

オブジェクトはこのように書くことができる:

.. code-block:: javascript

    var cat = {colour: "grey", name: "Spot", size: 46};
    cat.size = 47;
    show(cat.size);
    delete cat.size;
    show(cat.size);
    show(cat);

..  Like variables, each property attached to an object is labelled by a string. The first statement creates an object in which the property "colour" holds the string "grey", the property "name" is attached to the string "Spot", and the property "size" refers to the number 46. The second statement gives the property named size a new value, which is done in the same way as modifying a variable.

変数のように、それぞれのプロパティは文字列でラベル付けされることで結びつけることができる。最初の文は、\ ``"colour"``\ プロパティに文字列\ ``"grey"``\ を格納し、\ ``"name"``\ プロパティに文字列\ ``"Spot"``\ で結びつけられ、\ ``"size"``\ プロパティで\ ``46``\ という数字を参照する1つのオブジェクトを作る。2つめの文は\ ``size``\ という名前のプロパティに、変数と同じやり方で新しい値を与えている。

..  The keyword delete cuts off properties. Trying to read a non-existent property gives the value undefined.

``delete``\ キーワードはプロパティを切り捨てる。存在しないプロパティを読もうとすると\ ``undefined``\ 値が与えられる。

..  If a property that does not yet exist is set with the = operator, it is added to the object.

もしまだ存在しないプロパティに、\ ``=``\ 演算子で設定を行えば、それはオブジェクトに追加される。

.. code-block:: javascript

    var empty = {};
    empty.notReally = 1000;
    show(empty.notReally);

..  Properties whose names are not valid variable names have to be quoted when creating the object, and approached using brackets:

オブジェクトを作るとき、プロパティに変数名として正しくない名前を付けるには引用符を付ける必要があり、大括弧を使ってアプローチする:

.. code-block:: javascript

    var thing = {"gabba gabba": "hey", "5": 10};
    show(thing["5"]);
    thing["5"] = 20;
    show(thing[2 + 3]);
    delete thing["gabba gabba"];

..  As you can see, the part between the brackets can be any expression. It is converted to a string to determine the property name it refers to. One can even use variables to name properties:

見ての通り、大括弧の中の部分は任意の式である。それはプロパティの名前が参照されるときに文字列に変換される。プロパティ名に変数を使うこともできる。:

.. code-block:: javascript

    var propertyName = "length";
    var text = "mainline";
    show(text[propertyName]);

..  The operator in can be used to test whether an object has a certain property. It produces a boolean.

``in``\ 演算子はオブジェクトがそのプロパティを確かに持っているかをテストする。これは真偽値を作る。

.. code-block:: javascript

    var chineseBox = {};
    chineseBox.content = chineseBox;
    show("content" in chineseBox);
    show("content" in chineseBox.content);

|hr|

..  When object values are shown on the console, they can be clicked to inspect their properties. This changes the output window to an 'inspect' window. The little 'x' at the top-right can be used to return to the output window, and the left-arrow can be used to go back to the properties of the previously inspected object.

オブジェクト値をコンソール上で見るとき、それらはプロパティを見るためにクリックすることができる。出力ウインドウは'inspect'ウインドウに代わる。右上の小さな'x'は出力ウインドウに戻るのに使え、左矢印は前回見たオブジェクトのプロパティに戻るのに使える。

.. code-block:: javascript

    show(chineseBox);

|hr|

演習 4.1

..  The solution for the cat problem talks about a 'set' of names. A set is a collection of values in which no value may occur more than once. If names are strings, can you think of a way to use an object to represent a set of names?

猫問題の名前の'セット'についての解決である。セットは同じ値を1つしか入れることのできないコレクションである。もし名前が文字列であれば、これを名前のセットを現わすオブジェクトとして使う方法を考えられないだろうか？

..  Show how a name can be added to this set, how one can be removed, and how you can check whether a name occurs in it.

このセットへの名前の追加のしかた、取り除きかた、名前がその中に入っていないかチェックする方法を示せ。

..  [show solution]

[解答を見る]

..  This can be done by storing the content of the set as the properties of an object. Adding a name is done by setting a property by that name to a value, any value. Removing a name is done by deleting this property. The in operator can be used to determine whether a certain name is part of the set1.

これはセットの内容を格納をオブジェクトのプロパティへのように行うことで可能になる。名前を任意の値としてプロパティに設定することにより名前を追加でき、このプロパティを削除することで名前を取り除ける。\ ``in``\ 演算子で名前がセットにあるかどうかチェックできる。

.. code-block:: javascript

    var set = {"Spot": true};
    // Add "White Fang" to the set
    set["White Fang"] = true;
    // Remove "Spot"
    delete set["Spot"];
    // See if "Asoka" is in the set
    show("Asoka" in set);

|hr|

..  Object values, apparently, can change. The types of values discussed in chapter 2 are all immutable, it is impossible to change an existing value of those types. You can combine them and derive new values from them, but when you take a specific string value, the text inside it can not change. With objects, on the other hand, the content of a value can be modified by changing its properties.

オブジェクトの値は、見ての通り、変更することができる。\ :doc:`2章</Basic JavaScript>`\ では値の型は全くかわらない、既に存在する値の型は変更不可能であると論じた。それらを連結したりそれらから新しい値を作ることはできるが、しかしここの文字列の値を見れば、その中のテキストは変更できない。オブジェクトでは、その一方で、値に含まれている内容をプロパティを変更することにより変えることができるのだ。

..  When we have two numbers, 120 and 120, they can for all practical purposes be considered the precise same number. With objects, there is a difference between having two references to the same object and having two different objects that contain the same properties. Consider the following code:

``120``\ と\ ``120``\ という、2つの数値があるとき、全ての実用的な目的の上ではそれらは正確に同じ値であると考えられる。オブジェクトについては、同じオブジェクトへの2つの参照があるということと、同じプロパティを含む2つの異なるオブジェクトがあるということの間には違いがある。以下のコードについて考えよう:

.. code-block:: javascript

    var object1 = {value: 10};
    var object2 = object1;
    var object3 = {value: 10};

    show(object1 == object2);
    show(object1 == object3);

    object1.value = 15;
    show(object2.value);
    show(object3.value);
    
..  object1 and object2 are two variables grasping the same value. There is only one actual object, which is why changing object1 also changes the value of object2. The variable object3 points to another object, which initially contains the same properties as object1, but lives a separate life.

``object1``\ は\ ``object2``\ は\ *同じ*\ 値を掴んでいる2つの変数である。実在のオブジェクトは1つしかなく、\ ``object1``\ が変更されれば\ ``object2``\ の値も変わる。\ ``object3``\ という変数は別のオブジェクトを指しており、最初は\ ``object1``\ と同じプロパティを含んでいるが、しかし別れた人生を生きる。

..  JavaScript's == operator, when comparing objects, will only return true if both values given to it are the precise same value. Comparing different objects with identical contents will give false. This is useful in some situations, but unpractical in others.

JavaScriptの\ ``==``\ 演算子は、オブジェクトを比較するとき、もし与えられた両方の値が正確に同じ値のときだけ\ ``true``\ を返す。同じ内容を持つ異なるオブジェクトを比較したら\ ``false``\ となる。これはある状況では便利だが、他の状況では実用的でない。

|hr|

..  Object values can play a lot of different roles. Behaving like a set is only one of those. We will see a few other roles in this chapter, and chapter 8 shows another important way of using objects.

オブジェクトの値は多くの異なる役割を果たす。セットのようなものはそれらの1つでしかない。この章で若干の他の役割を見ていき、\ :doc:`8章</Object-oriented Programming>`\ において別の重要なオブジェクトの用法を見よう。

..  In the plan for the cat problem ― in fact, call it an algorithm, not a plan, that makes it sound like we know what we are talking about ― in the algorithm, it talks about going over all the e-mails in an archive. What does this archive look like? And where does it come from?

猫問題の計画において―実際のところ、それを計画ではなく、アルゴリズムと呼び、それは我々が語っているように思っているもの―このアルゴリズムで、アーカイブの中の全ての電子メールをどう扱うか話そう。このアーカイブは何に見えるだろうか？それをどこから持って来たものだろうか？

..  Do not worry about the second question for now. Chapter 14 talks about some ways to import data into your programs, but for now you will find that the e-mails are just magically there. Some magic is really easy, inside computers.

2つめの質問は今はおいておこう。\ :doc:`14章</HTTP requests>`\ でプログラムにデータを取り込む方法を話し、今は電子メールは魔法のようにただそこにあるとしておこう。コンピューターの中では魔法は本当に簡単だ。

|hr|

..  The way in which the archive is stored is still an interesting question. It contains a number of e-mails. An e-mail can be a string, that should be obvious. The whole archive could be put into one huge string, but that is hardly practical. What we want is a collection of separate strings.

アーカイブが格納された方法はまだ興味深い疑問だ。それは多くの電子メールを含んでいる。1つの電子メールはまた文字列でもあるのは明らかであろう。アーカイブ全体は巨大な文字列に入れることができるが、しかしそれはひどく実用的でない。我々が望むのは分割された文字列のコレクションである。

..  Collections of things are what objects are used for. One could make an object like this:

物のコレクションとしてオブジェクトを使う。このようなオブジェクトを作ることができる:

.. code-block:: javascript

    var mailArchive = {"the first e-mail": "Dear nephew, ...",
                       "the second e-mail": "..."
                       /* and so on ... */};

..  But that makes it hard to go over the e-mails from start to end ― how does the program guess the name of these properties? This can be solved by more predictable property names:

しかしこれで電子メールを最初から最後まで見るのは困難だ―プログラムはどうやってこれらのプロパティ名を推測するのか？より推測しやすいプロパティ名によってこれを解決できる。:

.. code-block:: javascript

    var mailArchive = {0: "Dear nephew, ... (mail number 1)",
                       1: "(mail number 2)",
                       2: "(mail number 3)"};

    for (var current = 0; current in mailArchive; current++)
      print("Processing e-mail #", current, ": ", mailArchive[current]);

..  Luck has it that there is a special kind of objects specifically for this kind of use. They are called arrays, and they provide some conveniences, such as a length property that contains the amount of values in the array, and a number of operations useful for this kind of collections.

まさにこの用途に使うための特別な種類のオブジェクトがある。これらは配列と呼ばれ、便利なことに、配列に格納されている値の数を含む\ ``length``\ プロパティのような、この種のコレクションを便利に使うための操作をいくつも提供している。

..  New arrays can be created using brackets ([ and ]):

新しい配列は大括弧（\ ``[``\ と\ ``]``\ ）を使って作られる。:

.. code-block:: javascript

    var mailArchive = ["mail one", "mail two", "mail three"];

    for (var current = 0; current < mailArchive.length; current++)
      print("Processing e-mail #", current, ": ", mailArchive[current]);

..  In this example, the numbers of the elements are not specified explicitly anymore. The first one automatically gets the number 0, the second the number 1, and so on.

この例では、もう要素の番号を明示的に書く必要はなくなった。1つめのものは自動的に0番となり、2つめのものは1番、というようになる。

..  Why start at 0? People tend to start counting from 1. As unintuitive as it seems, numbering the elements in a collection from 0 is often more practical. Just go with it for now, it will grow on you.

なぜ0から始まるか？人間は1から数えがちだ。直感的でないように見えるが、コレクションの要素の番号を0から始めることはしばしばより実用的なのだ。今はただそのようにしておけば、あなたもだんだん気に入ってくるだろう。

..  Starting at element 0 also means that in a collection with X elements, the last element can be found at position X - 1. This is why the for loop in the example checks for current < mailArchive.length. There is no element at position mailArchive.length, so as soon as current has that value, we stop looping.

要素0から始めることはまた\ ``x``\ 要素があるコレクションにおいて、最後の要素の位置が\ ``x - 1``\ になるということをも意味する。これが例の\ ``for``\ ループで\ ``current < mailArchive.length``\ をチェックする理由である。\ ``mailArchive.length``\ の位置にある要素はなく、\ ``current``\ がこの値になり次第、ループが終了する。

|hr|

.. _EX42:

演習 4.2

..  Write a function range that takes one argument, a positive number, and returns an array containing all numbers from 0 up to and including the given number.

1つの引数を取り、0から与えられた数値までの全ての正の数値を含む配列を返す\ ``range``\ 関数を書け。

..  An empty array can be created by simply typing []. Also remember that adding properties to an object, and thus also to an array, can be done by assigning them a value with the = operator. The length property is automatically updated when elements are added.

空の配列を作るには単純に\ ``[]``\ とタイプすればよい。オブジェクトへのプロパティを追加を思い出して、配列についてもそのように、\ ``=``\ 演算子で値を割り当てれば良い。\ ``length``\ プロパティは要素の追加により自動的に更新される。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function range(upto) {
      var result = [];
      for (var i = 0; i <= upto; i++)
        result[i] = i;
      return result;
    }
    show(range(4));

..  Instead of naming the loop variable counter or current, as I have been doing so far, it is now called simply i. Using single letters, usually i, j, or k for loop variables is a widely spread habit among programmers. It has its origin mostly in laziness: We'd rather type one character than seven, and names like counter and current do not really clarify the meaning of the variable much.

今までやってきたように、ループの変数に\ ``counter``\ や\ ``current``\ と名前を付ける代わりに、ここでは単純に\ ``i``\ とした。ループ変数に通常\ ``i``\ 、\ ``j``\ または\ ``k``\ といった単一の文字を使うのは、プログラマーの間に広く広まっている習慣だ。その起源の多くは面倒さにある：7文字よりは1文字タイプするのがいいし、\ ``counter``\ とか\ ``current``\ といった名前は明らかに全く変数の意味をなしてない。

..  If a program uses too many meaningless single-letter variables, it can become unbelievably confusing. In my own programs, I try to only do this in a few common cases. Small loops are one of these cases. If the loop contains another loop, and that one also uses a variable named i, the inner loop will modify the variable that the outer loop is using, and everything will break. One could use j for the inner loop, but in general, when the body of a loop is big, you should come up with a variable name that has some clear meaning.

もしプログラムが意味のない1文字の変数を使いすぎたら、信じられないほど混乱したものとなるだろう。私自身のプログラムでは、これを2,3の一般的な場合にとどめようとしている。小さいループがこれらの場合の1つ。もし、ループが他のループを含んでいたら、そしてそれが\ ``i``\ みたいな変数名でもあり、内側のループが外側のループで使っている変数を更新していたら、すべてが壊れるだろう。\ ``j``\ を内側のループの中で使うことは可能だが、全般的に、ループの本体が大きいときは、意味が明確になる変数名を使うべきである。

|hr|

..  Both string and array objects contain, in addition to the length property, a number of properties that refer to function values.

文字列と配列オブジェクトの両方が、\ ``length``\ プロパティに加えて、関数の値を参照する幾つかのプロパティを持つ。

.. code-block:: javascript

    var doh = "Doh";
    print(typeof doh.toUpperCase);
    print(doh.toUpperCase());

..  Every string has a toUpperCase property. When called, it will return a copy of the string, in which all letters have been converted to uppercase. There is also toLowerCase. Guess what that does.

全ての文字列は\ ``toUpperCase``\ プロパティを持つ。呼ばれたら、全ての文字が大文字に変換された文字列のコピーを返す。\ ``toLowerCase``\ というのもまたあって、推測通りのことをする。

..  Notice that, even though the call to toUpperCase does not pass any arguments, the function does somehow have access to the string "Doh", the value of which it is a property. How this works precisely is described in chapter 8.

注意すべきは、\ ``toUpperCase``\ は何の引数もなく呼ばれるにもかかわらず、関数は何らかの方法で文字列\ ``"Doh"``\ にアクセスし、その値をプロパティとしていることだ。ここでどのようなことが起こっているかの正確なことは\ :doc:`8章</Object-oriented Programming>`\ で記述しよう。

..  Properties that contain functions are generally called methods, as in 'toUpperCase is a method of a string object'.

"\ ``toUpperCase``\ は文字列オブジェクトのメソッドである"というように、関数を含むプロパティは一般的にメソッドと言われる。

.. code-block:: javascript

    var mack = [];
    mack.push("Mack");
    mack.push("the");
    mack.push("Knife");
    show(mack.join(" "));
    show(mack.pop());
    show(mack);

..  The method push, which is associated with arrays, can be used to add values to it. It could have been used in the last exercise, as an alternative to result[i] = i. Then there is pop, the opposite of push: it takes off and returns the last value in the array. join builds a single big string from an array of strings. The parameter it is given is pasted between the values in the array.

``push``\ メソッドは配列と結びついており、配列に値を付け加えるのに使われる。先ほどの演習ので使った\ ``result[i] = i``\ の代わりになる。それから\ ``pop``\ というものがあり、これは\ ``push``\ の反対である：配列から最後の値を取り出して返す。\ ``join``\ は文字列の配列から1つの大きな文字列を組み立てる。与えられたパラメータは配列の値の間に挿入される。

|hr|

..  Coming back to those cats, we now know that an array would be a good way to store the archive of e-mails. On this page, the function retrieveMails can be used to (magically) get hold of this array. Going over them to process them one after another is no rocket science anymore either:

猫の話に戻る。配列が電子メールのアーカイブを格納する良い手立てであることは分かった。このページでは、\ ``retrieveMail``\ 関数でこの配列を（魔法のように）手に入れることができる。この続きの処理をロケット工学その他の助けを借りずに行おう。:

.. code-block:: javascript

    var mailArchive = retrieveMails();

    for (var i = 0; i < mailArchive.length; i++) {
      var email = mailArchive[i];
      print("Processing e-mail #", i);
      // Do more things...
    }

..  We have also decided on a way to represent the set of cats that are alive. The next problem, then, is to find the paragraphs in an e-mail that start with "born" or "died".

生きている猫のセットを現わす方法も決めなければならない。次の問題は、それから、電子メールから\ ``"born"``\ か\ ``"died"``\ で始まる段落を見つけることだ。

|hr|

..  The first question that comes up is what exactly a paragraph is. In this case, the string value itself can't help us much: JavaScript's concept of text does not go any deeper than the 'sequence of characters' idea, so we must define paragraphs in those terms.

1つめの問題は段落とは正確には何であるかということになるだろう。この場合、文字列の値自体は十分な助けとならない：JavaScriptのテキストの概念では'文字の連なり'よりも深い意味はなく、ここにおいての段落の定義は決める必要がある。

..  Earlier, we saw that there is such a thing as a newline character. These are what most people use to split paragraphs. We consider a paragraph, then, to be a part of an e-mail that starts at a newline character or at the start of the content, and ends at the next newline character or at the end of the content.

先に、我々は改行のキャラクターというのを見た。多くの人々が段落を区切るのにこれらを使う。我々は段落を、これからは、改行文字から始まる、あるいは本文の最初から始まり、次の改行文字か内容自体の終わりによって終わる電子メールのを一部分であると考える。

..  And we don't even have to write the algorithm for splitting a string into paragraphs ourselves. Strings already have a method named split, which is (almost) the opposite of the join method of arrays. It splits a string into an array, using the string given as its argument to determine in which places to cut.

そして我々は文字列を段落に分割するアルゴリズムを我々自身で書く必要はない。文字列は既に\ ``split``\ という名のメソッドを持っており、それは配列の\ ``join``\ メソッドの（ほとんど）反対である。引数として与えられた文字列を基準として文字列を配列に分割し格納する。

.. code-block:: javascript

    var words = "Cities of the Interior";
    show(words.split(" "));

..  Thus, cutting on newlines ("\n"), can be used to split an e-mail into paragraphs.

このように、改行(\ ``"\n"``\ )で分割し、電子メールを段落に分割するのに使える。

|hr|

演習 4.3

..  split and join are not precisely each other's inverse. string.split(x).join(x) always produces the original value, but array.join(x).split(x) does not. Can you give an example of an array where .join(" ").split(" ") produces a different value?

``split``\ と\ ``join``\ は正確には互いの反対であるとは言えない。\ ``string.split(x).join(x)``\ は常に元の値となるが、しかし\ ``array.join(x).split(x)``\ はそうならない。\ ``.join(" ").split(" ")``\ が元と異なる例を挙げられるか？

..  [show solution]

[解答を見る]

.. code-block:: javascript

    var array = ["a", "b", "c d"];
    show(array.join(" ").split(" "));

|hr|

..  Paragraphs that do not start with either "born" or "died" can be ignored by the program. How do we test whether a string starts with a certain word? The method charAt can be used to get a specific character from a string. x.charAt(0) gives the first character, 1 is the second one, and so on. One way to check whether a string starts with "born" is:

"born"と"died"のどちらからも始まらない段落はプログラムによって無視できる。文字列が確かにある語から始まるかどうかテストするにはどうすればよいか？\ ``charAt``\ メソッドを文字列から特定の文字を取り出すのに使える。\ ``x.charAt(0)``\ は1つめの文字を与え、\ ``1``\ であれば2つめの文字、のように続く。文字列が"born"から始まるかチェックする1つの方法は:

.. code-block:: javascript

    var paragraph = "born 15-11-2003 (mother Spot): White Fang";
    show(paragraph.charAt(0) == "b" && paragraph.charAt(1) == "o" &&
         paragraph.charAt(2) == "r" && paragraph.charAt(3) == "n");

..  But that gets a bit clumsy ― imagine checking for a word of ten characters. There is something to be learned here though: when a line gets ridiculously long, it can be spread over multiple lines. The result can be made easier to read by lining up the start of the new line with the first element on the original line that plays a similar role.

しかしこれは少々かっこ悪い―10文字からなる言葉をチェックすることを想像して欲しい。ここで学ばなければならないこと：行が途方もなく、複数行にまたがるほど長くなることがありうる。同様の役割を果たしている元の行の最初の要素を、新しい行の先頭に並べることで、結果を読みやすくすることができる。

..  Strings also have a method called slice. It copies out a piece of the string, starting from the character at the position given by the first argument, and ending before (not including) the character at the position given by the second one. This allows the check to be written in a shorter way.

文字列は\ ``slice``\ というメソッドも持っている。これは文字列の1つめの引数で与えられた位置の文字から、2つめの引数で与えられた位置の文字の前（その位置の文字は結果に含まれない）までを一部分をコピーする。これで先に書いたチェックを簡単に書くことができる。

.. code-block:: javascript

    show(paragraph.slice(0, 4) == "born");

|hr|

演習 4.4

..  Write a function called startsWith that takes two arguments, both strings. It returns true when the first argument starts with the characters in the second argument, and false otherwise.

両方とも文字列である2つの引数を取り、1つめの引数が2つめの引数と同じ文字から始まっていれば\ ``true``\ を、そうでなければ\ ``false``\ を返す\ ``startWith``\ 関数を書け。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function startsWith(string, pattern) {
      return string.slice(0, pattern.length) == pattern;
    }

    show(startsWith("rotation", "rot"));

|hr|

..  What happens when charAt or slice are used to take a piece of a string that does not exist? Will the startsWith I showed still work when the pattern is longer than the string it is matched against?

``charAt``\ と\ ``slice``\ で文字列に存在しない部分が指定されたら何が起こるだろう？今見せた\ ``startsWith``\ でマッチすべき文字列よりパターンの方が長ければどうなる？

.. code-block:: javascript

    show("Pip".charAt(250));
    show("Nop".slice(1, 10));

..  charAt will return "" when there is no character at the given position, and slice will simply leave out the part of the new string that does not exist.

``charAt``\ は与えられた位置に文字がなければ\ ``""``\ を返し、そして\ ``slice``\ は存在しない新しい文字列の一部をただ残す。

..  So yes, that version of startsWith works. When startsWith("Idiots", "Most honoured colleagues") is called, the call to slice will, because string does not have enough characters, always return a string that is shorter than pattern. Because of that, the comparison with == will return false, which is correct.

そう、\ ``startsWith``\ のあのバージョンも動く。\ ``startsWith("Idiots", "Most honoured colleagues")``\ が呼び出されたとき、\ ``string``\ が十分な文字を持っていなければ、\ ``slice``\ の呼び出しは常に\ ``pattern``\ より短い文字列を返すだろう。そのため、\ ``==``\ での比較は\ ``false``\ を返し、それは正しい。

..  It helps to always take a moment to consider abnormal (but valid) inputs for a program. These are usually called corner cases, and it is very common for programs that work perfectly on all the 'normal' inputs to screw up on corner cases.

それは常に異常な（しかし正しい）プログラムへの入力を考慮する時間を取る助けになる。これらは通常corner caseと呼ばれ、全ての'正常な'入力で完璧に動くプログラムがcorner caseでヘマすることはとてもよくあることだ。

|hr|

..  The only part of the cat-problem that is still unsolved is the extraction of names from a paragraph. The algorithm was this:

猫問題で解決されていないのは段落から名前を展開するところだけとなった。アルゴリズムはこうだ:

..  Find the colon in the paragraph.
..  Take the part after this colon.
..  Split this part into separate names by looking for commas.

#. 段落からコロンを探す。
#. コロンより後の部分を取り出す。
#. コンマを見て、取り出した部分を名前に分割する。

..  This has to happen both for paragraphs that start with "died", and paragraphs that start with "born". It would be a good idea to put it into a function, so that the two pieces of code that handle these different kinds of paragraphs can both use it.

これは\ ``"died"``\ から始まる段落と\ ``"born"``\ から始まる段落の両方で起こる。これを関数にいれて、異なる種類の段落を扱う2つのコードの両方で使つのはいいアイデアだ。

|hr|

演習 4.5

..  Can you write a function catNames that takes a paragraph as an argument and returns an array of names?

段落を引数としし名前の配列を返す\ ``catNames``\ 関数を書けるか？

..  Strings have an indexOf method that can be used to find the (first) position of a character or sub-string within that string. Also, when slice is given only one argument, it will return the part of the string from the given position all the way to the end.

文字列は、文字列の中の文字または文字列の一部の（最初の）位置を見つける\ ``indexOf``\ メソッドを持つ。また\ ``slice``\ は引数を1つだけ与えられたとき、与えられた位置から文字列の最後までの全体を返す。

..  It can be helpful to use the console to 'explore' functions. For example, type "foo: bar".indexOf(":") and see what you get.

コンソールを使うのは関数を調べる助けとなる。例えば、\ ``"foo:bar".indexOf(":")``\ とタイプしてどうなるか見てみるといい。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function catNames(paragraph) {
      var colon = paragraph.indexOf(":");
      return paragraph.slice(colon + 2).split(", ");
    }

    show(catNames("born 20/09/2004 (mother Yellow Bess): " +
                  "Doctor Hobbles the 2nd, Noog"));

..  The tricky part, which the original description of the algorithm ignored, is dealing with spaces after the colon and the commas. The + 2 used when slicing the string is needed to leave out the colon itself and the space after it. The argument to split contains both a comma and a space, because that is what the names are really separated by, rather than just a comma.

トリッキーな部分、アルゴリズムの元の説明では、コロンやコンマのあとに続く空白は無視していた。\ ``+ 2``\ は文字列を分割するときに使われ、コロン自体とその後の空白を取り除くために必要になる。\ ``split``\ の引数にコンマと空白の両方を含めるのは、実際には名前はコンマだけではなく、空白も含めたもので分割されているからである。

..  This function does not do any checking for problems. We assume, in this case, that the input is always correct.

この関数では問題のチェックを行っていない。この場合においては、入力が常に正しいと仮定しているからである。

|hr|

..  All that remains now is putting the pieces together. One way to do that looks like this:

残っているのは断片を互いにつなぎ合わせることだ。1つのやり方としてはこのようになるだろう。:

.. code-block:: javascript

    var mailArchive = retrieveMails();
    var livingCats = {"Spot": true};

    for (var mail = 0; mail < mailArchive.length; mail++) {
      var paragraphs = mailArchive[mail].split("\n");
      for (var paragraph = 0;
           paragraph < paragraphs.length;
           paragraph++) {
        if (startsWith(paragraphs[paragraph], "born")) {
          var names = catNames(paragraphs[paragraph]);
          for (var name = 0; name < names.length; name++)
            livingCats[names[name]] = true;
        }
        else if (startsWith(paragraphs[paragraph], "died")) {
          var names = catNames(paragraphs[paragraph]);
          for (var name = 0; name < names.length; name++)
            delete livingCats[names[name]];
        }
      }
    }

    show(livingCats);

..  That is quite a big dense chunk of code. We'll look into making it a bit lighter in a moment. But first let us look at our results. We know how to check whether a specific cat survives:

これは大きく密度の高いコードの塊だ。少し時間をかけて、もう少し軽いものにしよう。しかし最初はこの結果を見てみよう。個々の猫が生きているかチェックする方法は分っている。:

.. code-block:: javascript

    if ("Spot" in livingCats)
      print("Spot lives!");
    else
      print("Good old Spot, may she rest in peace.");

..  But how do we list all the cats that are alive? The in keyword has a somewhat different meaning when it is used together with for:

しかし生きている全ての猫を列挙するにはどうしたらいいだろう？\ ``in``\ キーワードは\ ``for``\ とともに使われたとき別の意味を持つ:

.. code-block:: javascript

    for (var cat in livingCats)
      print(cat);

..  A loop like that will go over the names of the properties in an object, which allows us to enumerate all the names in our set.

このようなループは、セットの中の全ての名前を列挙可能なオブジェクトのプロパティの名前を全て処理する。

|hr|

..  Some pieces of code look like an impenetrable jungle. The example solution to the cat problem suffers from this. One way to make some light shine through it is to just add some strategic blank lines. This makes it look better, but doesn't really solve the problem.

コードの1片は不可侵のジャングルのように見える。猫問題の例の解答はこの病気にかかっている。これに光を当てる方法の1つとしては、ただ戦略的に空行を追加するのだ。これは良さそうに見えるが、しかし問題を本当に解決するものではない。

..  What is needed here is to break the code up. We already wrote two helper functions, startsWith and catNames, which both take care of a small, understandable part of the problem. Let us continue doing this.

ここで必要なのはコードを壊すことだ。我々は既に助けとなる関数を\ ``startsWith``\ と\ ``catNames``\ との2つ書いており、そのどちらもが小さく、プログラムの理解可能な一部である。このように続けよう。

.. code-block:: javascript

    function addToSet(set, values) {
      for (var i = 0; i < values.length; i++)
        set[values[i]] = true;
    }

    function removeFromSet(set, values) {
      for (var i = 0; i < values.length; i++)
        delete set[values[i]];
    
    }

..  These two functions take care of the adding and removing of names from the set. That already cuts out the two most inner loops from the solution:

これらの2つの関数はセットの名前の追加と削除の面倒を見てくれる。解答から2つの内側のループは既に切り出された:

.. code-block:: javascript

    var livingCats = {Spot: true};

    for (var mail = 0; mail < mailArchive.length; mail++) {
      var paragraphs = mailArchive[mail].split("\n");
      for (var paragraph = 0;
           paragraph < paragraphs.length;
           paragraph++) {
        if (startsWith(paragraphs[paragraph], "born"))
          addToSet(livingCats, catNames(paragraphs[paragraph]));
        else if (startsWith(paragraphs[paragraph], "died"))
          removeFromSet(livingCats, catNames(paragraphs[paragraph]));
      }
    }

..  Quite an improvement, if I may say so myself.

もしそう呼んでいいなら、完全な実装だ。

..  Why do addToSet and removeFromSet take the set as an argument? They could use the variable livingCats directly, if they wanted to. The reason is that this way they are not completely tied to our current problem. If addToSet directly changed livingCats, it would have to be called addCatsToCatSet, or something similar. The way it is now, it is a more generally useful tool.

なぜ\ ``addToSet``\ と\ ``removeFromSet``\ はなぜセットを引数として取るのか？もし望めば、これらは\ ``livingCats``\ 変数を直接使うことができる。理由はこの方法は現在の問題と完全に結びついていないためだ。もし\ ``addToset``\ が直接\ ``livingCats``\ を変更したら、それは\ ``addCatsToCatSet``\ とかそのような名前で呼ばれるべきだ。この方法は今、より一般的で便利なツールとなっている。

..  Even if we are never going to use these functions for anything else, which is quite probable, it is useful to write them like this. Because they are 'self sufficient', they can be read and understood on their own, without needing to know about some external variable called livingCats.

もしこれらの関数を他の問題に使うことがないと十分に証明されているのであば、このように書いておくのも便利だ。これらは'自己充足的'であり、これ自体で読み理解することが可能で、\ ``livingCats``\ といった外部の変数について知る必要もないからだ。

..  The functions are not pure: They change the object passed as their set argument. This makes them slightly trickier than real pure functions, but still a lot less confusing than functions that run amok and change any value or variable they please.

この関数は純粋ではない：これらの\ ``set``\ 引数として渡されたオブジェクトを変更する。これらは本当の純粋な関数より脆くトリッキーなものとなるが、しかし、うまく動かない、または望む任意の値や変数を変更する関数よりはまだ混乱しにくい。

|hr|

..  We continue breaking the algorithm into pieces:

アルゴリズムの分解を続けよう:

.. code-block:: javascript

    function findLivingCats() {
      var mailArchive = retrieveMails();
      var livingCats = {"Spot": true};

      function handleParagraph(paragraph) {
        if (startsWith(paragraph, "born"))
          addToSet(livingCats, catNames(paragraph));
        else if (startsWith(paragraph, "died"))
          removeFromSet(livingCats, catNames(paragraph));
      }

      for (var mail = 0; mail < mailArchive.length; mail++) {
        var paragraphs = mailArchive[mail].split("\n");
        for (var i = 0; i < paragraphs.length; i++)
          handleParagraph(paragraphs[i]);
      }
      return livingCats;
    }

    var howMany = 0;
    for (var cat in findLivingCats())
      howMany++;
    print("There are ", howMany, " cats.");

.. The whole algorithm is now encapsulated by a function. This means that it does not leave a mess after it runs: livingCats is now a local variable in the function, instead of a top-level one, so it only exists while the function runs. The code that needs this set can call findLivingCats and use the value it returns.

アルゴリズムの全体が関数によって、カプセルの中に封じ込められた。これは実行した後ゴミの山が残らないということを意味する：\ ``livingCats``\ は今やトップレベルのものでであるかわりに、関数の中のローカル変数となり、関数の実行中にしか存在しなくなった。このセットに必要のは\ ``findLivingCats``\ を呼び出しそも戻り値を使うコードだけだ。

..  It seemed to me that making handleParagraph a separate function also cleared things up. But this one is so closely tied to the cat-algorithm that it is meaningless in any other situation. On top of that, it needs access to the livingCats variable. Thus, it is a perfect candidate to be a function-inside-a-function. When it lives inside findLivingCats, it is clear that it is only relevant there, and it has access to the variables of its parent function.

私には\ ``handleParagraph``\ もまた関数に分割した方が分りやすいように思える。しかしこれは猫アルゴリズム密接に結びついていて他の状況では意味がなくなる。その上、それは\ ``livingCats``\ にアクセスする必要がある。それゆえ、それは関数内関数になる完全な候補だ。それが\ ``findLivingCats``\ の中で生きているとき、そこにだけ関係するのは明らかで、その親の関数の変数にだけアクセスを持つ。

..  This solution is actually bigger than the previous one. Still, it is tidier and I hope you'll agree that it is easier to read.

この解答は先の解答より実際に\ *大きい*\ 。まだ、それを小さくそして読みやすくすることに同意してくれると望む。

|hr|

..  The program still ignores a lot of the information that is contained in the e-mails. There are birth-dates, dates of death, and the names of mothers in there.

このプログラムはまだ電子メールに含まれている多くの情報を無視している。誕生日、死亡日、母猫の名前などだ。

..  To start with the dates: What would be a good way to store a date? We could make an object with three properties, year, month, and day, and store numbers in them.

日付から始めよう：日付を格納するのに良い方法は何か？\ ``year``\ 、\ ``month``\ 、\ ``day``\ の3つのプロパティを持つオブジェクトを作ることができ、それらの数値を格納することができる。

.. code-block:: javascript

    var when = {year: 1980, month: 2, day: 1};

..  But JavaScript already provides a kind of object for this purpose. Such an object can be created by using the keyword new:

しかしJavaScriptは既にこの目的のためのオブジェクトの種類を提供している。そのようなオブジェクトを\ ``new``\ キーワードを使って作ることができる。:

.. code-block:: javascript

    var when = new Date(1980, 1, 1);
    show(when);

..  Just like the notation with braces and colons we have already seen, new is a way to create object values. Instead of specifying all the property names and values, a function is used to build up the object. This makes it possible to define a kind of standard procedure for creating objects. Functions like this are called constructors, and in chapter 8 we will see how to write them.

括弧とコロンによる表記のようなものは既に見てきた通り、\ ``new``\ はオブジェクトの値を作り出す方法だ。全てのプロパティに名前と値を指定する代わりに、関数を使ってオブジェクトを作ることができる。これはオブジェクトを作るための標準的な処理の種類を定義することを可能にする。このような関数はコンストラクタと呼ばれる。\ :doc:`8章</Object-oriented Programming>`\ でその書き方を見よう。

..  The Date constructor can be used in different ways.

``Date``\ コンストラクタには幾つかの異なる用法がある。

.. code-block:: javascript

    show(new Date());
    show(new Date(1980, 1, 1));
    show(new Date(2007, 2, 30, 8, 20, 30));

..  As you can see, these objects can store a time of day as well as a date. When not given any arguments, an object representing the current time and date is created. Arguments can be given to ask for a specific date and time. The order of the arguments is year, month, day, hour, minute, second, milliseconds. These last four are optional, they become 0 when not given.

見ての通り、これらのオブジェクトは日付と同様に日時を格納することができる。引数が与えられなかったとき、現在の日時を現わすオブジェクトが作られる。引数は特定の日付や時間を与えるのに使われる。引数の順番は年、月、日、時、分、病、ミリ秒である。後ろの4つはオプションで、省略されると0になる。

..  The month numbers these objects use go from 0 to 11, which can be confusing. Especially since day numbers do start from 1.

これらのオブジェクトでの月の番号は0から11で、混乱するかもしれない。特に日の番号は\ *1から始まる*\ 。

|hr|

..  The content of a Date object can be inspected with a number of get... methods.

``Date``\ オブジェクトの内容は\ ``get...``\ メソッドの数値として得られる。

.. code-block:: javascript

    var today = new Date();
    print("Year: ", today.getFullYear(), ", month: ",
          today.getMonth(), ", day: ", today.getDate());
    print("Hour: ", today.getHours(), ", minutes: ",
          today.getMinutes(), ", seconds: ", today.getSeconds());
    print("Day of week: ", today.getDay());

..  All of these, except for getDay, also have a set... variant that can be used to change the value of the date object.

``getDay``\ を除いたこれら全ては、\ ``set...``\ の変形を持ち、\ ``date``\ オブジェクトの値を変更するのに使われる。

..  Inside the object, a date is represented by the amount of milliseconds it is away from January 1st 1970. You can imagine this is quite a large number.

オブジェクトの中で、日付は1970年1月1日からのミリ秒の積算として表現される。これはとても大きな数値としてイメージすることができる。

.. code-block:: javascript

    var today = new Date();
    show(today.getTime());

..  A very useful thing to do with dates is comparing them.

これは日付同士を比較するのに便利だ。

.. code-block:: javascript

    var wallFall = new Date(1989, 10, 9);
    var gulfWarOne = new Date(1990, 6, 2);
    show(wallFall < gulfWarOne);
    show(wallFall == wallFall);
    // but
    show(wallFall == new Date(1989, 10, 9));

..  Comparing dates with <, >, <=, and >= does exactly what you would expect. When a date object is compared to itself with == the result is true, which is also good. But when == is used to compare a date object to a different, equal date object, we get false. Huh?

``<``\ 、\ ``>``\ 、\ ``<=``\ および\ ``=>``\ での日付の比較は期待している通り正確である。日付オブジェクトをそれ自身と\ ``==``\ で比較したら結果は\ ``true``\ であり、それもまた正しい。しかし\ ``==``\ を同じ日付を持つ異なるオブジェクトと比較すれば\ ``false``\ となる。えっ？

..  As mentioned earlier, == will return false when comparing two different objects, even if they contain the same properties. This is a bit clumsy and error-prone here, since one would expect >= and == to behave in a more or less similar way. Testing whether two dates are equal can be done like this:

先に触れたように、\ ``==``\ は異なるオブジェクトを比較したときにはそれらが同じプロパティを持っていようと\ ``false``\ を返す。\ ``>=``\ および\ ``==``\ が多かれ少なかれ同様なやり方を持っていることを期待するから、これは少し混乱しやすく誤りやすい。2つの日付が等しいことはこのようにテストする。:

.. code-block:: javascript

    var wallFall1 = new Date(1989, 10, 9),
        wallFall2 = new Date(1989, 10, 9);
    show(wallFall1.getTime() == wallFall2.getTime());

|hr|

..  In addition to a date and time, Date objects also contain information about a timezone. When it is one o'clock in Amsterdam, it can, depending on the time of year, be noon in London, and seven in the morning in New York. Such times can only be compared when you take their time zones into account. The getTimezoneOffset function of a Date can be used to find out how many minutes it differs from GMT (Greenwich Mean Time).

日付と時刻に加えて言えば、\ ``Date``\ オブジェクトはタイムゾーンについての情報も含んでいる。アムステルダムが1時の時、時期にもよるが、ロンドンは正午、ニューヨークは朝7時である。タイムゾーンを勘定に入れることによってのみ、このような時刻を比較できる。\ ``Date``\ の\ ``getTimezoneOffset``\ 関数によってGMT（グリニッジ標準時）から何分差があるかを知ることができる。

.. code-block:: javascript

    var now = new Date();
    print(now.getTimezoneOffset());

|hr|

演習 4.6

.. code-block:: javascript

    "died 27/04/2006: Black Leclère"

..  The date part is always in the exact same place of a paragraph. How convenient. Write a function extractDate that takes such a paragraph as its argument, extracts the date, and returns it as a date object.

日付部分は常に段落の同じ場所にある。便利だね。このような段落を引数に取り、日付を展開したdateオブジェクトを返す\ ``extractDate``\ 関数を書け。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function extractDate(paragraph) {
      function numberAt(start, length) {
        return Number(paragraph.slice(start, start + length));
      }
      return new Date(numberAt(11, 4), numberAt(8, 2) - 1,
                      numberAt(5, 2));
    }

    show(extractDate("died 27-04-2006: Black Leclère"));

..  It would work without the calls to Number, but as mentioned earlier, I prefer not to use strings as if they are numbers. The inner function was introduced to prevent having to repeat the Number and slice part three times.

これは\ ``Number``\ を呼び出さなくても動くが、しかし先に触れたように、文字列をそれが数値であるかのように扱うのはよろしくない。内側の関数は\ ``Number``\ と\ ``slice``\ の部分を3回繰り返すのを防ぐために導入した。

..  Note the - 1 for the month number. Like most people, Aunt Emily counts her months from 1, so we have to adjust the value before giving it to the Date constructor. (The day number does not have this problem, since Date objects count days in the usual human way.)

月の数値の\ ``- 1``\ に注意。多くの人々のように、エミリー叔母さんも月を1から数え始めるので、\ ``Date``\ コンストラクタに渡す前にそれを調整する必要がある。（日の数値はこの問題を持たず、よって\ ``Date``\ オブジェクトも通常の人間通りに日を数える。）

..  In chapter 10 we will see a more practical and robust way of extracting pieces from strings that have a fixed structure.

:doc:`10章</Regular Expressions>`\ において、固定的な構造を持つ文字列を部品に展開する、より実用的で確固たる方法を見よう。

|hr|

..  Storing cats will work differently from now on. Instead of just putting the value true into the set, we store an object with information about the cat. When a cat dies, we do not remove it from the set, we just add a property death to the object to store the date on which the creature died.

猫の格納は今までとは異なる動きをするようになる。\ ``true``\ の値をセットに入れる代わりに、猫に関する情報をオブジェクトに格納する。猫が死んだら、セットから猫を取り除くのではなく、猫が死んだ日を格納する\ ``death``\ プロパティをオブジェクトに追加する。

..  This means our addToSet and removeFromSet functions have become useless. Something similar is needed, but it must also store birth-dates and, later, the mother's name.

これは\ ``addToSet``\ と\ ``removeFromSet``\ 関数が使えないものになることを意味する。同じような、しかし誕生日とその後と、母猫の名前も格納するものが必要だ。

.. code-block:: javascript

    function catRecord(name, birthdate, mother) {
      return {name: name, birth: birthdate, mother: mother};
    }

    function addCats(set, names, birthdate, mother) {
      for (var i = 0; i < names.length; i++)
        set[names[i]] = catRecord(names[i], birthdate, mother);
    }
        function deadCats(set, names, deathdate) {
      for (var i = 0; i < names.length; i++)
        set[names[i]].death = deathdate;
    }

..  catRecord is a separate function for creating these storage objects. It might be useful in other situations, such as creating the object for Spot. 'Record' is a term often used for objects like this, which are used to group a limited number of values.

``catRecord``\ は格納オブジェクトを分離する関数だ。それはSpotのオブジェクトを作るような他の状況でも便利だ。'Record'はこのようなオブジェクトに対してしばしば使われる用語で、限定された数の値のグループとして使われる。

|hr|

..  So let us try to extract the names of the mother cats from the paragraphs.

段落からの母猫の名前の展開に挑戦しよう。

.. code-block:: javascript

    "born 15/11/2003 (mother Spot): White Fang"

..  One way to do this would be...

1つの手としてはこのようになる...。

.. code-block:: javascript

    function extractMother(paragraph) {
      var start = paragraph.indexOf("(mother ") + "(mother ".length;
      var end = paragraph.indexOf(")");
      return paragraph.slice(start, end);
    }

    show(extractMother("born 15/11/2003 (mother Spot): White Fang"));

..  Notice how the start position has to be adjusted for the length of "(mother ", because indexOf returns the position of the start of the pattern, not its end.

``IndexOf``\ はパターンの終了位置ではなく開始位置を返すため、開始位置を\ ``"(Moither "``\ の長さで調整することに注意しよう。

|hr|

演習 4.7

..  The thing that extractMother does can be expressed in a more general way. Write a function between that takes three arguments, all of which are strings. It will return the part of the first argument that occurs between the patterns given by the second and the third arguments.

``extractMother``\ はより一般的なやりかたで行うことができる。3つの引数を取る\ ``between``\ 関数を書け。それらの引数は全て文字列であり、1つめの引数から2つめと3つめの引数で与えられたパターンの間の部分を返す。

..  So between("born 15/11/2003 (mother Spot): White Fang", "(mother ", ")") gives "Spot".

..  between("bu ] boo [ bah ] gzz", "[ ", " ]") returns "bah".

``between("born 15/11/2003 (mother Spot): White Fang", "(mother ", ")")``\ であれば\ ``"Spot"``\ 。\ ``between("bu ] boo [ bah ] gzz", "[ ", " ]")``\ であれば\ ``"bah"``\ を返すように。

..  To make that second test work, it can be useful to know that indexOf can be given a second, optional parameter that specifies at which point it should start searching.

2つめのテストが動くようにするには、\ ``indexOf``\ の2つめの、探し始める位置を指定するオプショナルな引数が便利だ。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function between(string, start, end) {
      var startAt = string.indexOf(start) + start.length;
      var endAt = string.indexOf(end, startAt);
      return string.slice(startAt, endAt);
    }
    show(between("bu ] boo [ bah ] gzz", "[ ", " ]"));

|hr|

..  Having between makes it possible to express extractMother in a simpler way:

``between``\ があれば\ ``extractMother``\ を簡単に書けるようになる。:

.. code-block:: javascript

    function extractMother(paragraph) {
      return between(paragraph, "(mother ", ")");
    }
    
..  The new, improved cat-algorithm looks like this:

|hr|

新しい、猫アルゴリズムの実装はこのようになる。:

.. code-block:: javascript

    function findCats() {
      var mailArchive = retrieveMails();
      var cats = {"Spot": catRecord("Spot", new Date(1997, 2, 5),
                  "unknown")};

      function handleParagraph(paragraph) {
        if (startsWith(paragraph, "born"))
          addCats(cats, catNames(paragraph), extractDate(paragraph),
                  extractMother(paragraph));
        else if (startsWith(paragraph, "died"))
          deadCats(cats, catNames(paragraph), extractDate(paragraph));
      }

      for (var mail = 0; mail < mailArchive.length; mail++) {
        var paragraphs = mailArchive[mail].split("\n");
        for (var i = 0; i < paragraphs.length; i++)
          handleParagraph(paragraphs[i]);
      }
      return cats;
    }

    var catData = findCats();

..  Having that extra data allows us to finally have a clue about the cats aunt Emily talks about. A function like this could be useful:

拡張したデータによりついにエミリー叔母さんが話すことについての手がかりを得ることができるようになった。このような関数は便利になりうる。:

.. code-block:: javascript

    function formatDate(date) {
      return date.getDate() + "/" + (date.getMonth() + 1) +
             "/" + date.getFullYear();
    }

    function catInfo(data, name) {
      if (!(name in data))
        return "No cat by the name of " + name + " is known.";

      var cat = data[name];
      var message = name + ", born " + formatDate(cat.birth) +
                    " from mother " + cat.mother;
      if ("death" in cat)
        message += ", died " + formatDate(cat.death);
      return message + ".";
    }

    print(catInfo(catData, "Fat Igor"));

..  The first return statement in catInfo is used as an escape hatch. If there is no data about the given cat, the rest of the function is meaningless, so we immediately return a value, which prevents the rest of the code from running.

1つめの\ ``catInfo``\ の\ ``return``\ 文は脱出口として使われる。与えられた名前の猫のデータがなければ、関数の残りの部分は意味がないので、即座に値を返し、残りのコードが実行されるのを防止する。

..  In the past, certain groups of programmers considered functions that contain multiple return statements sinful. The idea was that this made it hard to see which code was executed and which code was not. Other techniques, which will be discussed in chapter 5, have made the reasons behind this idea more or less obsolete, but you might still occasionally come across someone who will criticise the use of 'shortcut' return statements.

過去において、プログラマーのグループは複数の\ ``return``\ 文を含む関数は犯罪的だと確かに考えていた。このアイデアはどのコードが実行され、どのコードが実行されなかったかを見るのを困難にする。\ :doc:`5章</Error Handling>`\ にて論じる他のテクニックはこのアイデアの背景となる理由を多かれ少なかれ時代遅れにしたが、しかし'shortcut'な\ ``return``\ 文の使用を犯罪であるかのように考える人にもまだ時折出会うことだろう。

|hr|

演習 4.8

..  The formatDate function used by catInfo does not add a zero before the month and the day part when these are only one digit long. Write a new version that does this.

``catInfo``\ で使われた\ ``formatDate``\ 関数は月と日が1桁の長さしか持たない場合でも、その前にゼロを追加しない。それを行う新しいバージョンを書け。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function formatDate(date) {
      function pad(number) {
        if (number < 10)
          return "0" + number;
        else
          return number;
      }
      return pad(date.getDate()) + "/" + pad(date.getMonth() + 1) +
                 "/" + date.getFullYear();
    }
    print(formatDate(new Date(2000, 0, 1)));

|hr|

演習 4.9

..  Write a function oldestCat which, given an object containing cats as its argument, returns the name of the oldest living cat.

引数として与えられた猫のリストを含むオブジェクトから生存中で最も高齢の猫の名前を返す\ ``oldestCat``\ 関数を書け。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function oldestCat(data) {
      var oldest = null;

      for (var name in data) {
        var cat = data[name];
        if (!("death" in cat) &&
            (oldest == null || oldest.birth > cat.birth))
          oldest = cat;
      }

      if (oldest == null)
        return null;
      else
        return oldest.name;
    }

    print(oldestCat(catData));

..  The condition in the if statement might seem a little intimidating. It can be read as 'only store the current cat in the variable oldest if it is not dead, and oldest is either null or a cat that was born after the current cat'.

``if``\ 文の条件が少々怖く見えたかも知れない。これは'今処理中の猫が死んでおらず、かつ\ ``oldest``\ が\ ``null``\ または処理中の猫が\ ``oldest``\ の猫より高齢な時だけ、処理中の猫を\ ``oldest``\ 変数の中に格納する'と読める。

..  Note that this function returns null when there are no living cats in data. What does your solution do in that case?

この関数は生きている猫が\ ``data``\ の中にいなければ\ ``null``\ を返すことに注意。あなたの解答ではそのような場合どうなるだろうか？

|hr|

..  Now that we are familiar with arrays, I can show you something related. Whenever a function is called, a special variable named arguments is added to the environment in which the function body runs. This variable refers to an object that resembles an array. It has a property 0 for the first argument, 1 for the second, and so on for every argument the function was given. It also has a length property.

もう配列には慣れたので、それに関係するものを見せよう。関数が呼ばれたとき、\ ``arguments``\ という特殊な変数が関数の本体の実行中に環境に追加される。この変数はオブジェクトを配列であるかのように参照する。\ ``0``\ は1つめのプロパティ、\ ``1``\ は2つめといったように、関数に与えられた全ての引数が処理される。それはまた\ ``length``\ プロパティも持つ。

..  This object is not a real array though, it does not have methods like push, and it does not automatically update its length property when you add something to it. Why not, I never really found out, but this is something one needs to be aware of.

このオブジェクトは本物の配列ではなく、\ ``push``\ のようなメソッドは持たないし、\ ``length``\ プロパティのように何か追加されたときに自動的に更新されたりもしない。それがなぜなのか、私は本当のところを知らないが、しかしこれには注意が必要だ。

.. code-block:: javascript

    function argumentCounter() {
      print("You gave me ", arguments.length, " arguments.");
    }
    argumentCounter("Death", "Famine", "Pestilence");

..  Some functions can take any number of arguments, like print does. These typically loop over the values in the arguments object to do something with them. Others can take optional arguments which, when not given by the caller, get some sensible default value.

``print``\ のように、関数は任意の数の引数を取ることもできる。これらは典型的には\ ``arguments``\ オブジェクトの値に何らかの処理を行いながらループする。他のものは呼び出し元から与えられなかったときに適当なデフォルトの値になる、オプショナルな引数を取る。

.. code-block:: javascript

    function add(number, howmuch) {
      if (arguments.length < 2)
        howmuch = 1;
      return number + howmuch;
    }

    show(add(6));
    show(add(6, 4));

|hr|

演習 4.10

..  Extend the range function from exercise 4.2 to take a second, optional argument. If only one argument is given, it behaves as earlier and produces a range from 0 to the given number. If two arguments are given, the first indicates the start of the range, the second the end.

:ref:`演習4.2 <EX42>` の\ ``range``\ 関数を2つめの引数をオプションにできるように拡張しよう。もし与えられた引数が一つしかなければ、先ほどのように0から与えられた数までの範囲を作るようにするのである。もし引数が2つ与えられたら、1つめを開始、2つめを終了とする。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function range(start, end) {
      if (arguments.length < 2) {
        end = start;
        start = 0;
      }
      var result = [];
      for (var i = start; i <= end; i++)
        result.push(i);
      return result;
    }

    show(range(4));
    show(range(2, 4));

..  The optional argument does not work precisely like the one in the add example above. When it is not given, the first argument takes the role of end, and start becomes 0.

上記の\ ``add``\ の例のようなオプションの引数は正しく動かない。それが与えられなかったとき、1つめの引数は\ ``end``\ の役となり\ ``start``\ は\ ``0``\ となる。

|hr|

演習 4.11

..  You may remember this line of code from the introduction:

導入でのこの行のコードを覚えているだろう。:

.. code-block:: javascript

    print(sum(range(1, 10)));

..  We have range now. All we need to make this line work is a sum function. This function takes an array of numbers, and returns their sum. Write it, it should be easy.

``range``\ を今得た。この行を動かすために必要なのは\ ``sum``\ 関数である。この関数は数値の配列を取って、それらの合計を返す。簡単だろうから、それを書け。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function sum(numbers) {
      var total = 0;
      for (var i = 0; i < numbers.length; i++)
        total += numbers[i];
      return total;
    }

    print(sum(range(1, 10)));

|hr|

..  The previous chapter showed the functions Math.max and Math.min. With what you know now, you will notice that these are really the properties max and min of the object stored under the name Math. This is another role that objects can play: A warehouse holding a number of related values.

前の章で\ ``Math.max``\ 関数と\ ``Math.min``\ を見た。今知っていることから、これらが本当は\ ``Math``\ という名のもとで格納されている\ ``max``\ と\ ``min``\ プロパティであることに気づいただろう。これはオブジェクトのもう1つの役割である：関連するいくつもの値を格納する倉庫である。

..  There are quite a lot of values inside Math, if they would all have been placed directly into the global environment they would, as it is called, pollute it. The more names have been taken, the more likely one is to accidentally overwrite the value of some variable. For example, it is not a far shot to want to name something max.

``Math``\ の中には実に多くの値があり、もしそれらが呼び出されれば、それらはグローバルな環境の中に直接場所を取ってその場所を汚す。より多くの名前を取れば、例えば、\ ``max``\ といった名前を付けたくなることはなさそうなことではなく、変数の値が誤って上書きされてしまうことがありえる。

..  Most languages will stop you, or at least warn you, when you are defining a variable with a name that is already taken. Not JavaScript.

既に取られている変数を定義しようとしたとき、多くの言語ではあなたを止めたり、警告したりする。しかしJavaScriptは異なる。

..  In any case, one can find a whole outfit of mathematical functions and constants inside Math. All the trigonometric functions are there ― cos, sin, tan, acos, asin, atan. π and e, which are written with all capital letters (PI and E), which was, at one time, a fashionable way to indicate something is a constant. pow is a good replacement for the power functions we have been writing, it also accepts negative and fractional exponents. sqrt takes square roots. max and min can give the maximum or minimum of two values. round, floor, and ceil will round numbers to the closest whole number, the whole number below it, and the whole number above it respectively.

いずれにせよ、\ ``Math``\ 内の数学的な関数や定数の完全な一揃いを見つけることができる。全ての三角関数―\ ``cos``\ 、\ ``sin``\ 、\ ``tan``\ 、\ ``acos``\ 、\ ``asin``\ 、\ ``atan``\ 。πとeは大文字（\ ``PI``\ と\ ``E``\ ）で書かれており、一時は、これはそれが定数であることを示すファッショナブルな方法であった。\ ``pow``\ は先ほどまでに書いた\ ``power``\ 関数の良い代替であり、負の数や少数の累乗も受け付ける。\ ``sqrt``\ は平方根だ。\ ``max``\ と\ ``min``\ は2つの値の中で最大または最小のものを与える。\ ``round``\ 、\ ``floor``\ および\ ``ceil``\ は値を四捨五入したり、切り捨てたり、切り上げたりする。

..  There are a number of other values in Math, but this text is an introduction, not a reference. References are what you look at when you suspect something exists in the language, but need to find out what it is called or how it worked exactly. Unfortunately, there is no one comprehensive complete reference for JavaScript. This is mostly because its current form is the result of a chaotic process of different browsers adding different extensions at different times. The ECMA standard document that was mentioned in the introduction provides a solid documentation of the basic language, but is more or less unreadable. For things like the Math object and other elementary functionality, a rather good reference can be found here. The old documentation from Netscape, which can still be found at Sun's website, can also be helpful, but is outdated and not entirely correct anymore.

``Math``\ の中には他にも多くの値が含まれるが、しかしこのテキストは導入であり、リファレンスではない。リファレンスは言語にあるものを探すことはできるが、必要なのはそれがどのように呼び出され、正確にはどのように働くかということである。残念ながら、JavaScript向けの網羅的で完全なリファレンスというものはない。これはその現在の形が、ブラウザの違いや拡張機能の違いや時によって結果的に混沌としているためというのが大きい。ECMA標準ドキュメントは言語の基本のよいドキュメントの導入を提供するが、しかし多かれ少なかれ読みやすいとは言えない。\ ``Math``\ オブジェクトや他の要素機能のようなもののよいリファレンスは、むしろ\ `ここ <http://www.webreference.com/javascript/reference/core_ref/contents.html>`_\ で見つけることができるだろう。Netscapeからの古いドキュメントはまだ\ `Sunのウェブサイト <http://docs.sun.com/source/816-6408-10>`_\ で見つけることができ、とても助けになるが、しかし期限切れになっていてもう正しいとは言えない。

|hr|

..  Maybe you already thought of a way to find out what is available in the Math object:

``Math``\ オブジェクトで何が使えるか調べる方法をすでに知っているかもしれない:

.. code-block:: javascript

    for (var name in Math)
      print(name);

..  But alas, nothing appears. Similarly, when you do this:

残念だが、何も出てこない。同様に、このように書いても:

.. code-block:: javascript

    for (var name in ["Huey", "Dewey", "Loui"])
      print(name);

..  You only see 0, 1, and 2, not length, or push, or join, which are definitely also in there. Apparently, some properties of objects are hidden. There is a good reason for this: All objects have a few methods, for example toString, which converts the object into some kind of relevant string, and you do not want to see those when you are, for example, looking for the cats that you stored in the object.

期待されるような\ ``length``\ や\ ``push``\ 、\ ``join``\ ではなく、\ ``0``\ 、\ ``1``\ および\ ``2``\ しか出てこない。見ての通り、プロパティやオブジェクトは隠されている。それにはこのような良い理由がある：全てのオブジェクトは、例えばオブジェクトを関連のある文字列に変換する\ ``toString``\ のような少数のメソッドを持ち、例えばオブジェクトの中から格納されている猫を探すようなことはしないで良い。

..  Why the properties of Math are hidden is unclear to me. Someone probably wanted it to be a mysterious kind of object.

なぜ\ ``Math``\ のプロパティが隠されているのかは私にも分らない。誰かがそれをミステリアスな種類のオブジェクトにしようと望んだのだ。

..  All properties your programs add to objects are visible. There is no way to make them hidden, which is unfortunate because, as we will see in chapter 8, it would be nice to be able to add methods to objects without having them show up in our for/in loops.

あなたのプログラムが追加するオブジェクトの全てのプロパティは見ることができる。それらを隠す手段は残念ながらなく、それがなぜかは\ :doc:`8章</Object-oriented Programming>`\ で見るが、メソッドをオブジェクトに\ ``for``\ /\ ``in``\ ループなしに追加できるというのはいいことだ。

|hr|

..  Some properties are read-only, you can get their value but not change it. For example, the properties of a string value are all read-only.

幾つかのプロパティは読み取り専用であり、これらの値を得ることはできるが変更することはできない。例えば、文字列の値のプロパティは全て読み取り専用だ。

..  Other properties can be 'watched'. Changing them causes things to happen. For example, lowering the length of an array causes excess elements to be discarded:

他のプロパティは'監視'可能だ。これらを変更したら\ *何か*\ が起こる。例えば、配列のlengthを小さくしたら、はみ出た要素が捨てられてしまう。:

.. code-block:: javascript

    var array = ["Heaven", "Earth", "Man"];
    array.length = 2;
    show(array);

..  In some browsers, objects have a method watch, which can be used to add a watcher to your own properties. Internet Explorer does not support this though, so it is of little use when writing programs that must run on all the 'big' browsers.

ブラウザによっては、オブジェクトは、それ自身のプロパティを監視するものを追加する\ ``watch``\ メソッドを持っている。Internet Explorerはこれをサポートしていないので、全ての'大きな'ブラウザで実行する必要があるプログラムを書くときには使いづらい。

.. code-block:: javascript

    var winston = {mind: "compliant"};
    function watcher(propertyName, from, to) {
      if (to == "compliant")
        print("Doubleplusgood.");
      else
        print("Transmitting information to Thought Police...");
    }
    winston.watch("mind", watcher);

    winston.mind = "rebellious";
