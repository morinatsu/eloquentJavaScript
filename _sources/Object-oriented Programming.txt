=====================================================
オブジェクト指向プログラミング
=====================================================
..  Chapter 8:
..  Object-oriented Programming

..  In the early nineties, a thing called object-oriented programming stirred up the software industry. Most of the ideas behind it were not really new at the time, but they had finally gained enough momentum to start rolling, to become fashionable. Books were being written, courses given, programming languages developed. All of a sudden, everybody was extolling the virtues of object-orientation, enthusiastically applying it to every problem, convincing themselves they had finally found the right way to write programs.

90年代の初め、オブジェクト指向プログラミングと呼ばれるものがソフトウェア産業をかき立てた。その背景にあるアイデアはその時点でも本当に新しいものではなかったが、転がり始めるとついに十分な勢いを得て、ファッショナブルなものとなった。本が書かれ、コースが与えられ、プログラミング言語が開発された。全ては突然で、皆がオブジェクト指向の美点を賞賛し、熱狂的に全ての問題に適用し、彼ら自身は\ *プログラムを正しく書く方法*\ をついに見つけたのだと信じていた。

..  These things happen a lot. When a process is hard and confusing, people are always on the lookout for a magic solution. When something looking like such a solution presents itself, they are prepared to become devoted followers. For many programmers, even today, object-orientation (or their view of it) is the gospel. When a program is not 'truly object-oriented', whatever that means, it is considered decidedly inferior.

多くのことが起こった。プロセスが困難で混乱しているとき、人々は常に魔法的な解決に用心する。それ自身が与える解決が、そのようなものに見えるとき、彼らは献身的な花になる準備をする。多くのプログラマーにとって、今日でも、オブジェクト指向（または彼らがそのようにみなすもの）は福音であった。プログラムが'真のオブジェクト指向でない'とき、それは劣ったものであると判断されることも意味していた。

..  Few fads have managed to stay popular for as long as this one, though. Object-orientation's longevity can largely be explained by the fact that the ideas at its core are very solid and useful. In this chapter, we will discuss these ideas, along with JavaScript's (rather eccentric) take on them. The above paragraphs are by no means meant to discredit these ideas. What I want to do is warn the reader against developing an unhealthy attachment to them.

おかしな流行もこれと同じだけ続いた、にもかかわらず、オブジェクト指向の寿命は、その核心のアイデアがとても充実していて有益だったという事実によって大いに説明できる。この章では、JavaScriptの（むしろエキセントリックな）受容に沿って、これらのアイデアについて論じよう。上の段にはその信用を傷つけようという意図はない。私が望むのはそれらに無分別な傾倒を抱くことに対しての注意喚起だ。

.. |hr| raw:: html

   <hr>

|hr|

..  As the name suggests, object-oriented programming is related to objects. So far, we have used objects as loose aggregations of values, adding and altering their properties whenever we saw fit. In an object-oriented approach, objects are viewed as little worlds of their own, and the outside world may touch them only through a limited and well-defined interface, a number of specific methods and properties. The 'reached list' we used at the end of chapter 7 is an example of this: We used only three functions, makeReachedList, storeReached, and findReached to interact with it. These three functions form an interface for such objects.

その名が示すとおり、オブジェクト指向プログラミングはオブジェクトに関係がある。これまでは、我々はオブジェクトをデータのゆるい集まりとして使ってきており、それがふさわしいと思えるときにプロパティを付けたり変更したりしてきた。オブジェクト指向アプローチにおいては、オブジェクトはそれ自身の小さな世界とみなされ、外の世界からは限定的で予め定義されたインターフェース、個別のメソッドやプロパティのみを通じてそれに触れるようになる。\ :doc:`7章</Searching>`\ で使った'リーチド・リスト'はこれの1つの例だ： \ ``makeReachedList``\ 、\ ``storeReached``\ 、\ ``findReached``\ の3つの関数だけを使ってそれを操作できる。これらの3つの関数がそれらのオブジェクトのインターフェースを形作っている。

..  The Date, Error, and BinaryHeap objects we have seen also work like this. Instead of providing regular functions for working with the objects, they provide a way to create such objects, using the new keyword, and a number of methods and properties that provide the rest of the interface.

``Date``\ 、\ ``Error``\ 、\ ``BinaryHeap``\ オブジェクトもこれと同じように働く。そのオブジェクトで動く正規の関数を提供する代わりに、\ ``new``\ キーワードでそれらのオブジェクトを作り、メソッドやプロパティで残りのインターフェースを提供する。

|hr|

..  One way to give an object methods is to simply attach function values to it.

オブジェクトにメソッドを与える1つの方法は単に関数値をくっつけることだ。

.. code-block:: javascript

    var rabbit = {};
    rabbit.speak = function(line) {
      print("The rabbit says '", line, "'");
    };

    rabbit.speak("Well, now you're asking me.");

..  In most cases, the method will need to know who it should act on. For example, if there are different rabbits, the speak method must indicate which rabbit is speaking. For this purpose, there is a special variable called this, which is always present when a function is called, and which points at the relevant object when the function is called as a method. A function is called as a method when it is looked up as a property, and immediately called, as in object.method().

多くの場合、このメソッドは動いているのは\ *何者か*\ を知るために必要となるだろう。例えば、もし複数の異なるウサギがいたら、\ ``speak``\ メソッドは話しているウサギが誰かを示す。この目的のために、\ ``this``\ という特別な変数があり、これは関数がメソッドとして呼ばれたときに、関係するオブジェクトを常に示す。関数がメソッドとして呼ばれるというのは、プロパティであるかのように探された時で、\ ``object.method()``\ というのが、明示的な呼び出しである。

.. code-block:: javascript

    function speak(line) {
      print("The ", this.adjective, " rabbit says '", line, "'");
    }
    var whiteRabbit = {adjective: "white", speak: speak};
    var fatRabbit = {adjective: "fat", speak: speak};

    whiteRabbit.speak("Oh my ears and whiskers, how late it's getting!");
    fatRabbit.speak("I could sure use a carrot right now.");

|hr|

..  I can now clarify the mysterious first argument to the apply method, for which we always used null in chapter 6. This argument can be used to specify the object that the function must be applied to. For non-method functions, this is irrelevant, hence the null.

:doc:`6章</Functional Programming>`\ では常に\ ``null``\ にしておいた、\ ``apply``\ メソッドの1つめの引数の謎を今明らかにしよう。この引数は適用する関数にオブジェクトを指定するために使える。メソッドでない関数にはこれは関係がなく、そのため\ ``null``\ としていた。

.. code-block:: javascript

    speak.apply(fatRabbit, ["Yum."]);

..  Functions also have a call method, which is similar to apply, but you can give the arguments for the function separately instead of as an array:

関数は\ ``apply``\ と同じような\ ``call``\ メソッドも持っていて、しかしこれには、1つの配列の代わりに、引数を別々に分けて関数に与えることができる。：

.. code-block:: javascript

    speak.call(fatRabbit, "Burp.");

|hr|

..  The new keyword provides a convenient way of creating new objects. When a function is called with the word new in front of it, its this variable will point at a new object, which it will automatically return (unless it explicitly returns something else). Functions used to create new objects like this are called constructors. Here is a constructor for rabbits:

``new``\ キーワードは新しいオブジェクトを作る便利な方法を提供する。関数が\ ``new``\ という語を先頭に付けて呼ばれたとき、その\ ``this``\ 変数は、（明示的に他の何かを返さないときは）自動的に返された\ *新しい*\ オブジェクトを指す。このような新しいオブジェクトを作るための関数はコンストラクターと呼ばれる。ウサギのためのコンストラクタはここ。：

.. code-block:: javascript

    function Rabbit(adjective) {
      this.adjective = adjective;
      this.speak = function(line) {
        print("The ", this.adjective, " rabbit says '", line, "'");
      };
    }

    var killerRabbit = new Rabbit("killer");
    killerRabbit.speak("GRAAAAAAAAAH!");

..  It is a convention, among JavaScript programmers, to start the names of constructors with a capital letter. This makes it easy to distinguish them from other functions.

便宜上、JavaScriptのプログラマーの間ではコンストラクターの名前は大文字で始めることになっている。これで他の関数と簡単に区別できるようになる。

..  Why is the new keyword even necessary? After all, we could have simply written this:

なぜ\ ``new``\ キーワードが必要なのだろうか？結局のところ、単純にこのように書くこともできる。：

.. code-block:: javascript

    function makeRabbit(adjective) {
      return {
        adjective: adjective,
        speak: function(line) {/*etc*/}
      };
    }

    var blackRabbit = makeRabbit("black");

..  But that is not entirely the same. new does a few things behind the scenes. For one thing, our killerRabbit has a property called constructor, which points at the Rabbit function that created it. blackRabbit also has such a property, but it points at the Object function.

しかし全く同じというわけではない。\ ``new``\ は裏で他のことも行っている。その1つには、\ ``killerRabit``\ はそれを作った\ ``Rabit``\ 関数を示す\ ``constructor``\ というプロパティを持つ。\ ``blackRabbit``\ もそのようなプロパティを持っているが、それは\ ``Object``\ 関数を示している。

.. code-block:: javascript

    show(killerRabbit.constructor);
    show(blackRabbit.constructor);

|hr|

..  Where did the constructor property come from? It is part of the prototype of a rabbit. Prototypes are a powerful, if somewhat confusing, part of the way JavaScript objects work. Every object is based on a prototype, which gives it a set of inherent properties. The simple objects we have used so far are based on the most basic prototype, which is associated with the Object constructor. In fact, typing {} is equivalent to typing new Object().

``constructor`` プロパティはどこからくるのか？それはrabbitのプロトタイプの一部である。プロトタイプは、もし何かの混乱があったとき、JavaScriptのオブジェクトを動かすパワフルな方法だ。プロトタイプが元になっている全てのオブジェクトには固有のプロパティのセットが与えられている。これまで使ってきた単純なオブジェクトはもっとも基本的なプロトタイプを元にしていて、それは\ ``Object``\ コンストラクターに結びついている。実際、\ ``{}``\ とタイプするのは\ ``new Object()``\ とタイプするのに等しい。

.. code-block:: javascript

    var simpleObject = {};
    show(simpleObject.constructor);
    show(simpleObject.toString);

..  toString is a method that is part of the Object prototype. This means that all simple objects have a toString method, which converts them to a string. Our rabbit objects are based on the prototype associated with the Rabbit constructor. You can use a constructor's prototype property to get access to, well, their prototype:

``toString`` メソッドは\ ``Object``\ プロトタイプの一部だ。これは、全ての単純なオブジェクトはそれを文字列に変換する\ ``toString``\ メソッドを持つ、ということを意味する。rabbitオブジェクトは\ ``Rabbit``\ コンストラクターに結びついたプロトタイプに基づいている。コンストラクターの\ ``prototype``\ プロパティをそれ、それらのプロトタイプにアクセスするのに使える。：

.. code-block:: javascript

    show(Rabbit.prototype);
    show(Rabbit.prototype.constructor);

..  Every function automatically gets a prototype property, whose constructor property points back at the function. Because the rabbit prototype is itself an object, it is based on the Object prototype, and shares its toString method.

全ての関数は自動的に\ ``prototype``\ プロパティを得て、\ ``constructor``\ プロパティがその関数を指す返す。なぜならrabbitプロトタイプはそれ自体が\ ``Object``\ プロトタイプをもとにしたオブジェクトであるからで、その\ ``toString``\ メソッドを共有している。

.. code-block:: javascript

    show(killerRabbit.toString == simpleObject.toString);

|hr|

..  Even though objects seem to share the properties of their prototype, this sharing is one-way. The properties of the prototype influence the object based on it, but the properties of this object never change the prototype.

オブジェクトがそのプロトタイプのプロパティを共有しているように見えたとしても、その共有は一方通行のものだ。プロトタイプのプロパティはもとになっているオブジェクトに影響するが、しかしこのオブジェクトのプロパティがプロトタイプを変更することはない。

..  The precise rules are this: When looking up the value of a property, JavaScript first looks at the properties that the object itself has. If there is a property that has the name we are looking for, that is the value we get. If there is no such property, it continues searching the prototype of the object, and then the prototype of the prototype, and so on. If no property is found, the value undefined is given. On the other hand, when setting the value of a property, JavaScript never goes to the prototype, but always sets the property in the object itself.

正確なルールはこうだ：プロパティの値を探すとき、JavaScriptは、まずオブジェクト\ *それ自身*\ が持っているプロパティを探す。もし探している名前のプロパティが見つかれば、その値を得る。もし、そのようなプロパティがなければ、オブジェクトのプロトタイプにさかのぼって探し続け、さらにそのプロトタイプのプロトタイプ、へと続く。もしプロパティが見つからなかったら、\ ``undefined``\ 値が与えられる。他方、プロパティに値を\ *設定するとき*\ は、JavaScriptはプロトタイプを見ることなく、常にそのオブジェクト自身のプロパティを設定する。

.. code-block:: javascript

    Rabbit.prototype.teeth = "small";
    show(killerRabbit.teeth);
    killerRabbit.teeth = "long, sharp, and bloody";
    show(killerRabbit.teeth);
    show(Rabbit.prototype.teeth);

..  This does mean that the prototype can be be used at any time to add new properties and methods to all objects based on it. For example, it might become necessary for our rabbits to dance.

これは、プロトタイプはそれをもとにしているオブジェクトにいつでも新しいプロパティとメソッドを追加することができる、ということを意味する。例えば、ウサギたちにダンスをさせる必要がでてくるかもしれない。

.. code-block:: javascript

    Rabbit.prototype.dance = function() {
      print("The ", this.adjective, " rabbit dances a jig.");
    };

    killerRabbit.dance();

..  And, as you might have guessed, the prototypical rabbit is the perfect place for values that all rabbits have in common, such as the speak method. Here is a new approach to the Rabbit constructor:

そして、お察しの通り、プロトタイプ的なウサギは、\ ``speak``\ メソッドのように全てのウサギが共通に持っている値を完全に置き換える。これは\ ``Rabbit``\ コンストラクターへの新しいアプローチである。：

.. code-block:: javascript

    function Rabbit(adjective) {
      this.adjective = adjective;
    }
    Rabbit.prototype.speak = function(line) {
      print("The ", this.adjective, " rabbit says '", line, "'");
    };

    var hazelRabbit = new Rabbit("hazel");
    hazelRabbit.speak("Good Frith!");

|hr|

..  The fact that all objects have a prototype and receive some properties from this prototype can be tricky. It means that using an object to store a set of things, such as the cats from chapter 4, can go wrong. If, for example, we wondered whether there is a cat called "constructor", we would have checked it like this:

全てのオブジェクトはプロトタイプを持ちそしてプロトタイプ由来のプロパティを受けるという事実はトリッキーになり得る。これは、\ :doc:`4章</Data structures>`\ のように物事の集合をオブジェクトに格納するのは間違いを起こすかもしれないということを意味する。もし、例えば、\ ``"constructor"``\ という猫に出会ったかどうかをこのようにチェックしたらどうなるか。：

.. code-block:: javascript

    var noCatsAtAll = {};
    if ("constructor" in noCatsAtAll)
      print("Yes, there definitely is a cat called 'constructor'.");

..  This is problematic. A related problem is that it can often be practical to extend the prototypes of standard constructors such as Object and Array with new useful functions. For example, we could give all objects a method called properties, which returns an array with the names of the (non-hidden) properties that the object has:

これは問題をはらんでいる。関係のある問題は、\ ``Object``\ や\ ``Array``\ のような標準コンストラクターのプロトタイプを新しい有用な関数で拡張するのはしばしば実用的になりうる、ということだ。例えば、オブジェクトの持っている全ての（隠されていない）プロパティの名前の配列を返す\ ``properties``\ というメソッドを全てのオブジェクトに与えることもできる。：

.. code-block:: javascript

    Object.prototype.properties = function() {
      var result = [];
      for (var property in this)
        result.push(property);
      return result;
    };

    var test = {x: 10, y: 3};
    show(test.properties());

..  And that immediately shows the problem. Now that the Object prototype has a property called properties, looping over the properties of any object, using for and in, will also give us that shared property, which is generally not what we want. We are interested only in the properties that the object itself has.

そしてこれは明確に問題を見せる。今や\ ``Object``\ プロトタイプは\ ``proerties``\ というプロパティをもち、\ ``for``\ と\ ``in``\ を使って任意のオブジェクトのプロパティをループし、我々が一般的には望んでいない共有されたプロパティまでも与えてくれる。我々はオブジェクトそれ自体が持っているプロパティにしか興味はない。

..  Fortunately, there is a way to find out whether a property belongs to the object itself or to one of its prototypes. Unfortunately, it does make looping over the properties of an object a bit clumsier. Every object has a method called hasOwnProperty, which tells us whether the object has a property with a given name. Using this, we could rewrite our properties method like this:

幸運にも、プロパティがそのオブジェクト自身のものか、そのプロトタイプの中の1つのものかを知る手段がある。残念ながら、それは1つのオブジェクトのプロパティを少々かっこう悪くループする。すべてのオブジェクトは\ ``hasOwnPropertiy``\ というメソッドを持ち、それは与えられた名前のプロパティをオブジェクトが持っているかどうかを教えてくれる。これを使って、\ ``properties``\ メソッドをこう書き換えよう。：

.. code-block:: javascript

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

..  And of course, we can abstract that into a higher-order function. Note that the action function is called with both the name of the property and the value it has in the object.

そしてもちろん、高階関数にそれを抽象化することもできる。\ ``action``\ 関数がオブジェクトの中でプロパティの名前とその値の両方で呼ばれていることに注意しよう。

.. code-block:: javascript

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

..  But, what if we find a cat named hasOwnProperty? (You never know.) It will be stored in the object, and the next time we want to go over the collection of cats, calling object.hasOwnProperty will fail, because that property no longer points at a function value. This can be solved by doing something even uglier:

しかし、もし\ ``hasOwnProperty`` という名前の猫がいたらどうなるだろう？（あなたが知ることはないだろう。）それはオブジェクトに格納され、そして次に猫のコレクションを見ようとしたときに、そのプロパティが関数の値を指していないために、\ ``object.hasOwnProperty``\ は失敗する。かっこう悪いことをしてこれを解決できる。：

.. code-block:: javascript

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

..  (Note: This example does not currently work correctly in Internet Explorer 8, which apparently has some problems with overriding built-in prototype properties.)

.. note::
   この例は現在のところInternet Explorer 8では正しく動かない、どうやら組み込みのプロトタイプのプロパティに問題があるらしい。

..  Here, instead of using the method found in the object itself, we get the method from the Object prototype, and then use call to apply it to the right object. Unless someone actually messes with the method in Object.prototype (don't do that), this should work correctly.

ここで、オブジェクト自身で見つけたメソッドの代わりに、\ ``Object``\ プロトタイプ由来のメソッドを得て、それから正しいオブジェクトに適用するために\ ``call``\ してみよう。誰かが実際に\ ``Object.prototype``\ のメソッドで何かヘマをすることがなければ（しないように）、これは正しく動くだろう。

|hr|

..  hasOwnProperty can also be used in those situations where we have been using the in operator to see whether an object has a specific property. There is one more catch, however. We saw in chapter 4 that some properties, such as toString, are 'hidden', and do not show up when going over properties with for/in. It turns out that browsers in the Gecko family (Firefox, most importantly) give every object a hidden property named __proto__, which points to the prototype of that object. hasOwnProperty will return true for this one, even though the program did not explicitly add it. Having access to the prototype of an object can be very convenient, but making it a property like that was not a very good idea. Still, Firefox is a widely used browser, so when you write a program for the web you have to be careful with this. There is a method propertyIsEnumerable, which returns false for hidden properties, and which can be used to filter out strange things like __proto__. An expression such as this one can be used to reliably work around this:

``hasOwnProperty``\ はオブジェクトが特定のプロパティを持っているかを見たいときに\ ``in``\ 演算子を使うような状況でも使うことができる。しかしながら、もう一つ問題がある。\ :doc:`4章</Data structures>`\ で見たいくつかのプロパティ、\ ``toString``\ のような、は'隠されて'いて、\ ``for/in``\ でプロパティを一通り見ても出てこない。Geckoファミリーのブラウザ（Firefox、最も重要な）は全てのオブジェクトに\ ``__proto__``\ という隠されたプロパティを与えており、それはそのオブジェクトのプロトタイプを示しているということが分る。プログラムが明示的に加えてなくても、\ ``hasOwnProperty``\ はこれについて真を返す。オブジェクトのプロトタイプへのアクセスを持つことはとても便利であり得るが、しかしそのように、それをプロパティにするのは良いアイデアではない。まだ、Firefoxは広く使われているブラウザなので、ウェブへのプログラムを書くときはこれに注意する必要がある。\ ``propertyIsEnumerable``\ というメソッドがあり、それは隠されたプロパティについて\ ``false``\ を返し、\ ``__proto__``\ のようなおかしなものを除外するのにつかうことができる。このような1つの式で信頼できるものとして働くようになる。：

.. code-block:: javascript

    var object = {foo: "bar"};
    show(Object.prototype.hasOwnProperty.call(object, "foo") &&
         Object.prototype.propertyIsEnumerable.call(object, "foo"));

..  Nice and simple, no? This is one of the not-so-well-designed aspects of JavaScript. Objects play both the role of 'values with methods', for which prototypes work great, and 'sets of properties', for which prototypes only get in the way.

上手くて単純だろうか。違う？これはJavaScriptでうまくデザインされなかった点の1つだ。オブジェクトは2つの役割を演じ、'メソッドを持つ値'の役割においては、プロトタイプは偉大な働きをし、'プロパティの集合'という役割においては、プロトタイプが唯一の邪魔になる。

|hr|

..  Writing the above expression every time you need to check whether a property is present in an object is unworkable. We could put it into a function, but an even better approach is to write a constructor and a prototype specifically for situations like this, where we want to approach an object as just a set of properties. Because you can use it to look things up by name, we will call it a Dictionary.

渡されたオブジェクトのプロパティをチェックすることが必要になる度に上記の式を毎回書くのはうまくない。関数に押し込むこともできるが、しかし、オブジェクトをプロパティの集合として扱いたいときのような特別な状況のためにコンストラクターとプロトタイプを書くのがより良いアプローチだろう。これを使うことで、名前でプロパティを見つけ出すことができるようになるから、これを\ ``Dictionary``\ （辞書）と呼ぼう。

.. code-block:: javascript

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

..  Now the whole mess related to approaching objects as plain sets of properties has been 'encapsulated' in a convenient interface: one constructor and four methods. Note that the values property of a Dictionary object is not part of this interface, it is an internal detail, and when you are using Dictionary objects you do not need to directly use it.

オブジェクトをプロパティの集合として扱うアプローチに関する上手くない点が、便利なインターフェースにより、今や完全に'カプセル化'された：1つのコンストラクターと4つのメソッドである。\ ``Dictionary``\ オブジェクトの\ ``values``\ プロパティはこのインターフェースの部分ではないことに注意しよう。\ ``Dictionary``\ オブジェクト使うにあたり、その内側の詳細を直接扱う必要はない。

..  Whenever you write an interface, it is a good idea to add a comment with a quick sketch of what it does and how it should be used. This way, when someone, possibly yourself three months after you wrote it, wants to work with the interface, they can quickly see how to use it, and do not have to study the whole program.

いつインターフェースを書こうと、それが何をしどのようにつかうものかを手早くコメントに書いておくのは良いアイデアである。このインターフェースを使いたいと思った、それを書いた3ヶ月後のあなた自身を含む誰かが、これによって、プログラムを完全に見なくても、その使い方を早く知ることができる。

..  Most of the time, when you are designing an interface, you will soon find some limitations and problems in whatever you came up with, and change it. To prevent wasting your time, it is advisable to document your interfaces only after they have been used in a few real situations and proven themselves to be practical. ― Of course, this might make it tempting to forget about documentation altogether. Personally, I treat writing documentation as a 'finishing touch' to add to a system. When it feels ready, it is time to write something about it, and to see if it sounds as good in English (or whatever language) as it does in JavaScript (or whatever programming language).

インターフェースを設計する時の多くの時間、何を思いついたにせよ、すぐにその限界や問題を見つけ、変更するだろう。時間を浪費しないようにするには、2,3の本当の状況でそれら自身を試してからインターフェースを文書化することだ。―もちろん、これは文書化のことを一時的に忘れるようにということでもある。個人的には、私は文書を書くことをシステムの'仕上げ'を加えることとして見ている。その準備ができたように感じたときが、それについて書くときで、英語（かなんかの言語）が、JavaScript（かなんかのプログラミング言語）であるかのように書くのである。

|hr|

..  The distinction between the external interface of an object and its internal details is important for two reasons. Firstly, having a small, clearly described interface makes an object easier to use. You only have to keep the interface in mind, and do not have to worry about the rest unless you are changing the object itself.

オブジェクトの外部インターフェースと内部の詳細を区別するのは2つの理由により重要だ。1つめは、小さく、明確に書かれたインターフェースはオブジェクトを使いやすくするということだ。インターフェースを守ることだけを意識していれば、オブジェクトそれ自体の残りの部分を変更することに悩まなくて済む。

..  Secondly, it often turns out to be necessary or practical to change something about the internal implementation of an object type1, to make it more efficient, for example, or to fix some problem. When outside code is accessing every single property and detail in the object, you can not change any of them without also updating a lot of other code. If outside code only uses a small interface, you can do what you want, as long as you do not change the interface.

2つめは、結局はオブジェクト型\ [#f1]_\ の内部の実装を変更することがしばしば必要になったり実用的であったりするということだ。例えば、より効率的にすること、あるいは問題を修正すること。外側のコードが単一のプロパティ毎にそのオブジェクトの詳細にアクセスしていたら、オブジェクト以外のコードをたくさん修正することなしにはオブジェクトを何も修正できなくなる。もし外側のコードが小さなインターフェースしか使っていなければ、インターフェースを変更するまでは、やりたいだけのことができる。

..  Some people go very far in this. They will, for example, never include properties in the interface of object, only methods ― if their object type has a length, it will be accessible with the getLength method, not the length property. This way, if they ever want to change their object in such a way that it no longer has a length property, for example because it now has some internal array whose length it must return, they can update the function without changing the interface.

ある人々はここからさらに先に行く。彼らは、例えば、オブジェクトのインターフェースにプロパティを含めず、メソッドだけを含めようとする ― もし彼らのオブジェクトが\ ``length``\ を持っていたら、それにはlengthプロパティではなく、\ ``getLength``\ メソッドでアクセスする。この方法で、もし彼らのオブジェクトを変更したいとき、\ ``length``\プロパティがない限り、例えば、今、内部的な配列の長さを返さなければならないなら、彼らはインターフェースを変えずに関数を変更できる。

..  My own take is that in most cases this is not worth it. Adding a getLength method which only contains return this.length; mostly just adds meaningless code, and, in most situations, I consider meaningless code a bigger problem than the risk of having to occasionally change the interface to my objects.

私自身は多くの場合これにはそれほどの価値はないと考えている。\ ``return this.length;``\ のみの\ ``getLength``\ メソッドを加えることは；ほとんど意味のないコードの追加でしかなく、多くの状況で、自分のオブジェクトのインターフェースを時々変更しなければならないリスクより、意味のないコードが増えることの方が問題が大きいと考えている。

|hr|

..  Adding new methods to existing prototypes can be very convenient. Especially the Array and String prototypes in JavaScript could use a few more basic methods. We could, for example, replace forEach and map with methods on arrays, and make the startsWith function we wrote in chapter 4 a method on strings.

既存のプロトタイプに新しいメソッドを追加することはとても便利だ。特にJavaScriptでの\ ``Array``\ や\ ``String``\ プロトタイプは2,3のより基本的なメソッドを使ってきた。例えば、\ ``forEach``\ と\ ``map``\ を配列のメソッドに代えて、4章で書いた\ ``startsWith``\ 関数を文字列のメソッドにしたり。

..  However, if your program has to run on the same web-page as another program (either written by you or by someone else) which uses for/in naively ― the way we have been using it so far ― then adding things to prototypes, especially the Object and Array prototype, will definitely break something, because these loops will suddenly start seeing those new properties. For this reason, some people prefer not to touch these prototypes at all. Of course, if you are careful, and you do not expect your code to have to coexist with badly-written code, adding methods to standard prototypes is a perfectly good technique.

しかしながら、もしプログラムが同じウェブページで、他のプログラム（あなたが書くものでも、他の誰かのものでも）と同じように実行されるなら、それは\ ``for/in``\ を素朴に―以前我々もそうしてきたように―プロトタイプ、特に\ ``Object``\ や\ ``Array``\ プロトタイプに何かを加えることは、これらのループが突然に新しいプロパティを見始めるため、間違いなく何かを壊すことになる。この理由により、これらのプロトタイプに絶対に触らないようにしている。もちろん、もし注意深ければ、かつ酷い書かれ方をしたコードと共存しなければならなくなるようなことはないと想定できれば、標準プロトタイプにメソッドを加えることは完全に良いテクニックである。

|hr|

..  In this chapter we are going to build a virtual terrarium, a tank with insects moving around in it. There will be some objects involved (this is, after all, the chapter on object-oriented programming). We will take a rather simple approach, and make the terrarium a two-dimensional grid, like the second map in chapter 7. On this grid there are a number of bugs. When the terrarium is active, all the bugs get a chance to take an action, such as moving, every half second.

この章では、バーチャルな飼育器(terrarium)、タンクとその中を動き回る昆虫を作ることにする。いくつかのオブジェクトが入り組むことになるだろう（結局、この章はオブジェクト指向プログラミングなので）。むしろ単純なアプローチを取って、飼育器は、\ :doc:`7章</Searching>`\ の2つめの地図のように二次元のマス目とする。このマス目の上に虫の数を持つ。飼育器が活動中のとき、全ての虫は移動のような行動を行うチャンスを半秒ごとに得る。

..  Thus, we chop both time and space into units with a fixed size ― squares for space, half seconds for time. This usually makes things easier to model in a program, but of course has the drawback of being wildly inaccurate. Fortunately, this terrarium-simulator is not required to be accurate in any way, so we can get away with it.

そういうわけで、時間と空間を固定されたサイズを持つ単位に分割しよう―空間には四角を、時間には半秒を。これは、通常、プログラム内で物事をモデル化するのを簡単にするが、しかしもちろん非常に不正確であるという欠点も持つ。幸運にも、この飼育器シミュレータはどのような正確さも必要としておらず、そのことは捨て置くことができる。

|hr|

..  A terrarium can be defined with a 'plan', which is an array of strings. We could have used a single string, but because JavaScript strings must stay on a single line it would have been a lot harder to type.

飼育器は文字列の配列である'計画'によって定義される。単一の文字列を使うこともできるが、JavaScriptは文字列を単一の行でしか書けないため、タイプするのがたいへんだ。

.. code-block:: javascript

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

..  The "#" characters are used to represent the walls of the terrarium (and the ornamental rocks lying in it), the "o"s represent bugs, and the spaces are, as you might have guessed, empty space.

``"#"``\ 文字は飼育器の壁（そして飼育器に置かれた装飾用の岩）を表現し、\ ``"o"``\ は虫を、そして空白はお察しの通り何もない空間を表現する。

..  Such a plan-array can be used to create a terrarium-object. This object keeps track of the shape and content of the terrarium, and lets the bugs inside move. It has four methods: Firstly toString, which converts the terrarium back to a string similar to the plan it was based on, so that you can see what is going on inside it. Then there is step, which allows all the bugs in the terrarium to move one step, if they so desire. And finally, there are start and stop, which control whether the terrarium is 'running'. When it is running, step is automatically called every half second, so the bugs keep moving.

このようなplan配列を飼育器オブジェクトを作るのに使う。このオブジェクトは飼育器の形と内容を追跡し、その中の虫を動かせる。4つのメソッドを持つ：1つめは\ ``toString``\ で、飼育器をもとの文字列と同様な文字列に変換し、その中で何が起こっているか分るようにする。それから、\ ``step``\ で、飼育器の中の全ての虫がもし彼らが望めば1ステップだけ動けるようにする。最後に\ ``start``\ と\ ``stop``\ 、飼育器が動いているかどうかを制御し、動いているなら\ ``step``\ が半秒ごとに呼び出されて、虫が動き続ける。

|hr|

演習 8.1

..  The points on the grid will be represented by objects again. In chapter 7 we used three functions, point, addPoints, and samePoint to work with points. This time, we will use a constructor and two methods. Write the constructor Point, which takes two arguments, the x and y coordinates of the point, and produces an object with x and y properties. Give the prototype of this constructor a method add, which takes another point as argument and returns a new point whose x and y are the sum of the x and y of the two given points. Also add a method isEqualTo, which takes a point and returns a boolean indicating whether the this point refers to the same coordinates as the given point.

マス目上のポイントをオブジェクトにより再度表そう。\ :doc:`7章</Searching>`\ ではpointsとともに働く\ ``point``\ 、\ ``addPoints``\ 、\ ``samePoints``\ の3つの関数を使った。今回、コンストラクターと2つのメソッドを使う。地点を表わすxとyの2つの引数の組を取り、\ ``x``\ と\ ``y``\ をプロパティに持つオブジェクトを作る\ ``Point``\ コンストラクタを書け。このコンストラクターのプロトタイプに、他の地点を引数に取り、2つの地点の\ ``x``\ と\ ``y``\ の合計した\ *新しい*\ 地点を返す\ ``add``\ メソッドを加えよ。1つの地点を引数に取り、\ ``this``\ （この）ポイントと与えられたポイントが同じ地点を指しているかどうかを真偽値として返す\ ``isEqualTo``\ メソッドも加えよ。

..  Apart from the two methods, the x and y properties are also part of the interface of this type of objects: Code which uses point objects may freely retrieve and modify x and y.

2つのメソッドとは別に、\ ``x``\ と\ ``y``\ プロパティはこの型のオブジェクトのインターフェースの一部でもある：pointオブジェクトを使うコードは自由に\ ``x``\ と\ ``y``\ を取り出し変更できる。

..  [show solution]

[解答を見る]

.. code-block:: javascript

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

..  Make sure your version of add leaves the this point intact and produces a new point object. A method which changes the current point instead would be similar to the += operator, whereas this one is like the + operator.

あなたの版の\ ``add``\ が\ ``this``\ ポイントを壊すことなく、確実に新しいpointオブジェクトを作るようにすること。現在のpointを変更するメソッドはむしろ\ ``+=``\ 演算子のようなものであるから、そいうわけでこちらは\ ``+``\ 演算子のようなものだ。

|hr|

..  When writing objects to implement a certain program, it is not always very clear which functionality goes where. Some things are best written as methods of your objects, other things are better expressed as separate functions, and some things are best implemented by adding a new type of object. To keep things clear and organised, it is important to keep the amount of methods and responsibilities that an object type has as small as possible. When an object does too much, it becomes a big mess of functionality, and a formidable source of confusion.

決まったプログラムに実装するためにオブジェクトを書くとき、機能がどこに行くかは常に明らかというわけでない。あることをオブジェクトのメソッドとして書くのが最善だとしても、他のものは分離した関数に書いた方が良く、またあるものは別の型のオブジェクトとして実装するのが最善である。物事をクリアに組織的にしておくには、オブジェクトのメソッドと応答をできる限り小さい量に抑えておくことが重要だ。1つのオブジェクトが大きすぎるとき、機能は大きなゴミの山となり、恐ろしく混乱したソースになるだろう。

..  I said above that the terrarium object will be responsible for storing its contents and for letting the bugs inside it move. Firstly, note that it lets them move, it doesn't make them move. The bugs themselves will also be objects, and these objects are responsible for deciding what they want to do. The terrarium merely provides the infrastructure that asks them what to do every half second, and if they decide to move, it makes sure this happens.

上記で、飼育器のオブジェクトはその中身を格納し、その中の虫を動かせる責任を負うだろうと言った。最初に、飼育器が虫を\ *動かす*\ 、ではなく飼育器が虫を\ *動かさせる*\ 、であることに注意して欲しい。虫たち自身もまたオブジェクトであり、そしてこれらのオブジェクトは彼らが何をしたいか判断する責任を負う。飼育器は半秒ごとに彼らに何をするか尋ねるインフラのみを提供し、そしてもし虫たちが動くことを決断したら、それでこの移動が実際に起こる。

..  Storing the grid on which the content of the terrarium is kept can get quite complex. It has to define some kind of representation, ways to access this representation, a way to initialise the grid from a 'plan' array, a way to write the content of the grid to a string for the toString method, and the movement of the bugs on the grid. It would be nice if part of this could be moved into another object, so that the terrarium object itself doesn't get too big and complex.

飼育器の中身をマス目に格納するのはとても複雑になり得る。表現の種類と、この表現へのアクセス手段、マス目を'計画'配列で初期化する方法、\ ``toString``\ メソッドのためにマス目の内容を文字列に各方法、マス目上の虫の動きを定義しよう。もしこれの部分を他のオブジェクトに移動できとしたら、飼育器のオブジェクト自体が大きすぎたり複雑になったりしないので、その方が良いだろう。

|hr|

..  Whenever you find yourself about to mix data representation and problem-specific code in one object, it is a good idea to try and put the data representation code into a separate type of object. In this case, we need to represent a grid of values, so I wrote a Grid type, which supports the operations that the terrarium will need.

1つのオブジェクトに混乱したデータ表現と問題のあるコードを見つける度に、データ表現のコードを他の型のオブジェクトに分離しようとするのは良いアイデアだ。この場合、値のマス目の表現が必要であり、そこで飼育器に必要な操作をサポートする\ ``Grid``\ 型を書く。

..  To store the values on the grid, there are two options. One can use an array of arrays, like this:

マス目上の値を格納するには、2つのオプションがある。1つは配列の配列を使う。このようになる。：

.. code-block:: javascript

    var grid = [["0,0", "1,0", "2,0"],
                ["0,1", "1,1", "2,1"]];
    show(grid[1][2]);

..  Or the values can all be put into a single array. In this case, the element at x,y can be found by getting the element at position x + y * width in the array, where width is the width of the grid.

また、単一の配列に値を入れることもできる。この場合、\ ``x``\ 、\ ``y``\ の要素は配列の中の\ ``x + y * width``\ の位置の要素として探すことができ、この\ ``width``\ というのはマス目の幅である。

.. code-block:: javascript

    var grid = ["0,0", "1,0", "2,0",
            "0,1", "1,1", "2,1"];
    show(grid[2 + 1 * 3]);

..  I chose the second representation, because it makes it much easier to initialise the array. new Array(x) produces a new array of length x, filled with undefined values.

私は2つめの表現を選んだ、なぜなら、こちらの方が配列の初期化が楽だからだ。\ ``new Array(x)``\ は\ ``undefined``\ 値で満たされた長さ\ ``x``\ の新しい配列を作る。

.. code-block:: javascript

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

|hr|

演習 8.2

..  We will also need to go over all the elements of the grid, to find the bugs we need to move, or to convert the whole thing to a string. To make this easy, we can use a higher-order function that takes an action as its argument. Add the method each to the prototype of Grid, which takes a function of two arguments as its argument. It calls this function for every point on the grid, giving it the point object for that point as its first argument, and the value that is on the grid at that point as second argument.

マス目の全ての要素に渡って、移動が必要な虫を探したり、全てを文字列に変換することも必要だ。これを簡単に作るには、アクションをその引数として取る高階関数を作るのだ。\ ``Grid``\ プロトタイプに\ ``each``\ メソッドを追加し、それは引数が2つの関数を引数として取る。それはこの関数を全てのマス目について呼び出し、その地点のpointオブジェクトを1つめの引数に、そしてマス目上のその地点の値を2つめの引数として与える。

..  Go over the points starting at 0,0, one row at a time, so that 1,0 is handled before 0,1. This will make it easier to write the toString function of the terrarium later. (Hint: Put a for loop for the x coordinate inside a loop for the y coordinate.)

``0,0``\ の地点から始めて、1行を一時に、\ ``1,0``\ は\ ``0,1``\ の前に扱われる。これで後で飼育器の\ ``toString``\ 関数を書くのが楽になる。（ヒント：\ ``x``\ の\ ``for``\ ループの組を\ ``y``\ のforループの組の中に入れよう）

..  It is advisable not to muck about in the cells property of the grid object directly, but use valueAt to get at the values. This way, if we decide (for some reason) to use a different method for storing the values, we only have to rewrite valueAt and setValueAt, and the other methods can stay untouched.

gridオブジェクトに\ ``cells``\ プロパティを直接ぶら下げず、その場所の値を取るのに\ ``valueAt``\ を使うほうが賢明だろう。この方法は、もし（何らかの理由により）値の格納に別な方法を使うことに決めたとき、\ ``valueAt``\ と\ ``setValueAt``\ を書き換えるだけで済み、他のメソッドには触らないことができる。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    Grid.prototype.each = function(action) {
      for (var y = 0; y < this.height; y++) {
        for (var x = 0; x < this.width; x++) {
          var point = new Point(x, y);
          action(point, this.valueAt(point));
        }
      }
    };

|hr|

..  Finally, to test the grid:

最後に、このgridをテストしよう。：

.. code-block:: javascript

    var testGrid = new Grid(3, 2);
    testGrid.setValueAt(new Point(1, 0), "#");
    testGrid.setValueAt(new Point(1, 1), "o");
    testGrid.each(function(point, value) {
      print(point.x, ",", point.y, ": ", value);
    });

|hr|

..  Before we can start to write a Terrarium constructor, we will have to get a bit more specific about these 'bug objects' that will be living inside it. Earlier, I mentioned that the terrarium will ask the bugs what action they want to take. This will work as follows: Each bug object has an act method which, when called, returns an 'action'. An action is an object with a type property, which names the type of action the bug wants to take, for example "move". For most actions, the action also contains extra information, such as the direction the bug wants to go.

``Terrarium``\ （飼育器）のコンストラクターを書く前に、その中に住む'虫のオブジェクト'をもう少し詳細にしよう。初めは、飼育器は彼らが取りたいアクションを尋ねると書いた。これはこのように働く：それぞれの虫のオブジェクトは\ ``act``\ メソッドを持ち、それは呼び出されたら、'アクション'を返す。アクションは\ ``type``\ プロパティを伴うオブジェクトで、その名前は虫が取りたいアクションのタイプ、例えば\ ``"move"``\ （移動）になる。多くのアクションにおいて、アクションは虫が動きたい方向のような拡張の情報も持つ。

..  Bugs are terribly myopic, they can only see the squares directly around them on the grid. But these they can use to base their action on. When the act method is called, it is given an object with information about the surroundings of the bug in question. For each of the eight directions, it contains a property. The property indicating what is above the bug is called "n", for North, the one indicating what is above and to the left "ne", for North-East, and so on. To look up the direction these names refer to, the following dictionary object is useful:

虫たちは恐るべき近眼で、すぐ隣のマス目しか見ることができない。しかし、彼らはそれをベースに行動する。\ ``act``\ メソッドが呼ばれたとき、虫の周囲の情報を持つオブジェクトを与えられる。8つの方向のそれぞれについて、その中にプロパティを持つ。プロパティは虫の上なら北の\ ``"n"``\ 、左上なら北東の\ ``"ne"``\ というようなことを示し、残りも同様とする。その名前で参照している方向を探すには、以下の辞書オブジェクトが有用である。：

.. code-block:: javascript

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

..  When a bug decides to move, he indicates in which direction he wants to go by giving the resulting action object a direction property that names one of these directions. We can make a simple, stupid bug that always just goes south, 'towards the light', like this:

虫が移動することを決断したとき、彼は、これらの方向の一つの名前が入った\ ``direction``\ プロパティを持つactionオブジェクトを結果として与えることで、動きたい方向を示す。'光に向かって'、常に南にしか行かない、単純な、頭の悪い虫はこのようになる。：

.. code-block:: javascript

    function StupidBug() {};
    StupidBug.prototype.act = function(surroundings) {
      return {type: "move", direction: "s"};
    };

|hr|

..  Now we can start on the Terrarium object type itself. First, its constructor, which takes a plan (an array of strings) as argument, and initialises its grid.

これで\ ``Terrarium``\ オブジェクト型それ自体に取り組めるようになった。最初に、計画（文字列の配列）を引数に取り、そのマス目を初期化する、そのコンストラクターだ。

.. code-block:: javascript

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

..  wall is an object that is used to mark the location of walls on the grid. Like a real wall, it doesn't do much, it just sits there and takes up space.

``wall``\ はマス目の中で壁の位置を示すオブジェクトだ。本物の壁のように、何もせず、ただそこにあってスペースを埋める。

|hr|

..  The most straightforward method of a terrarium object is toString, which transforms a terrarium into a string. To make this easier, we mark both the wall and the prototype of the StupidBug with a property character, which holds the character that represents them.

一番わかりやすい飼育器オブジェクトのメソッドは\ ``toString``\ で、これは飼育器を文字列に変換する。これを簡単に作るために、\ ``wall``\ と\ ``StupidBug``\ のプロトタイプに、それを表現する文字を持つ\ ``character``\ プロパティを付けてマークする。

.. code-block:: javascript

    wall.character = "#";
    StupidBug.prototype.character = "o";

    function characterFromElement(element) {
      if (element == undefined)
        return " ";
      else
        return element.character;
    }

    show(characterFromElement(wall));

|hr|

演習 8.3

..  Now we can use the each method of the Grid object to build up a string. But to make the result readable, it would be nice to have a newline at the end of every row. The x coordinate of the positions on the grid can be used to determine when the end of a line is reached. Add a method toString, which takes no arguments and returns a string which, when given to print, shows a nice two-dimensional view of the terrarium.

これで、\ ``Grid``\ オブジェクトの\ ``each``\ メソッドを文字列を組み立てるのに使うことができる。しかし読みやすい結果を作るためには、行の終わり毎に改行を入れるのがいいだろう。grid上の位置の\ ``x``\ は行の終わりに着いたかどうかの判定に使える。引数を取らず、飼育器をうまく2次元の視点で\ ``print``\ するための文字列を返す\ ``toString``\ メソッドを追加しよう。

..  [show solution]

[解答を見る]

.. code-block:: javascript

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

..  And to try it out...

そしてこれを試そう...

.. code-block:: javascript

    var terrarium = new Terrarium(thePlan);
    print(terrarium.toString());

|hr|

..  It is possible that, when trying to solve the above exercise, you have tried to access this.grid inside the function that you pass as an argument to the grid's each method. This will not work. Calling a function always results in a new this being defined inside that function, even when it is not used as a method. Thus, any this variable outside of the function will not be visible.

可能なら、上記の演習を解くときに、gridの\ ``each``\ に渡される引数である関数の中の\ ``this.grid``\ へのアクセスを試みてみよう。これは動かない。関数の呼び出しは、それがメソッドとして呼ばれたものでなくても、常に新しい\ ``this``\ の中の関数の中で定義されたものを返す。それゆえ、関数の外の\ ``this``\ 変数は見ることができない。

..  Sometimes it is straightforward to work around this by storing the information you need in a variable, like endOfLine, which is visible in the inner function. If you need access to the whole this object, you can store that in a variable too. The name self (or that) is often used for such a variable.

内側の関数から\ *参照できる*\ ``endOfLine``\ のように、変数の中に必要な情報を格納することによって、\ ``this``\ の周りで動かすのがしばしばわかりやすい手段である。もし完全な\ ``this``\ オブジェクトにアクセスする必要があるなら、それも変数に入れてしまえる。\ ``self``\ (または\ ``that``\ )という名前がしばしばそのような変数のために使われる。

..  But all these extra variables can get messy. Another good solution is to use a function similar to partial from chapter 6. Instead of adding arguments to a function, this one adds a this object, using the first argument to the function's apply method:

しかし全てのこれらの拡張変数はかっこう悪いかもしれない。他の良い解決法は、\ :doc:`6章</Functional Programming>`\ の\ ``partial``\ と同じような関数を使うことだ。これは、関数に引数を追加する代わりに\ ``this``\ オブジェクトを追加し、最初の引数を関数の\ ``apply``\ メソッドに使う：

.. code-block:: javascript

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

..  This way, you can bind an inner function to this, and it will have the same this as the outer function.

この手で、内側の関数を\ ``this``\ に\ ``bind``\ して、それが外側の関数のものであるかのように同じ\ ``this``\ を得られる。

|hr|

演習 8.4

..  In the expression bind(testArray.push, testArray) the name testArray still occurs twice. Can you design a function method, which allows you to bind an object to one of its methods without naming the object twice?

``bind(testArray.push, testArray)``\ の式のtestArrayの名前が2回出てくる。オブジェクトの名前を2回も\ *使うことなく*\ 、オブジェクトとその中の1つのメソッドを結びつける関数\ ``method``\ を設計できるだろうか？

..  [show solution]

[解答を見る]

..  It is possible to give the name of the method as a string. This way, the method function can look up the correct function value for itself.

メソッドの名前を文字列で与えれば可能だ。こうすれば、\ ``method``\ 関数は正しい関数値をそれ自身から見つけることができる。

.. code-block:: javascript

    function method(object, name) {
      return function() {
        object[name].apply(object, arguments);
      };
    }

    var pushTest = method(testArray, "push");

|hr|

..  We will need bind (or method) when implementing the step method of a terrarium. This method has to go over all the bugs on the grid, ask them for an action, and execute the given action. You might be tempted to use each on the grid, and just handle the bugs we come across. But then, when a bug moves South or East, we will come across it again in the same turn, and allow it to move again.

飼育器の\ ``step``\ メソッドを実装するには\ ``bind``\ （または\ ``method``\ ）が必要だ。このメソッドはマス目上の全てのバグに、そのアクションを尋ね、与えられたアクションを実行する。gridの\ ``each``\ を使って、出会った虫をただ扱いたいと思うかもしれない。しかし、それなら、南東に動いた虫が動いたとき、同じターンなのに、その虫が2回動けてしまう。

..  Instead, we first gather all the bugs into an array, and then process them. This method gathers bugs, or other things that have an act method, and stores them in objects that also contain their current position:

その代わりに、まず1つの配列に全ての虫を集めて、それからその虫たちを処理する。この虫を捕まえるメソッド、または\ ``act``\ メソッドを持つ他のものは、虫を、その現在の位置とともに1つのオブジェクトに格納する。：

.. code-block:: javascript

    Terrarium.prototype.listActingCreatures = function() {
      var found = [];
      this.grid.each(function(point, value) {
        if (value != undefined && value.act)
          found.push({object: value, point: point});
      });
      return found;
    };

|hr|

演習 8.5

..  When asking a bug to act, we must pass it an object with information about its current surroundings. This object will use the direction names we saw earlier ("n", "ne", etcetera) as property names. Each property holds a string of one character, as returned by characterFromElement, indicating what the bug can see in that direction.

虫に行動するか聞くとき、その周囲いついての情報のオブジェクトを渡さなければならない。このオブジェクトは先ほどの名前（\ ``"n"``\ 、\ ``"ne"``\ 、etc.）をプロパティ名に使う。それぞれのプロパティは、\ ``characterFromElement``\ によって、その方向で虫が出会うことになるものを示す、1文字の文字列を持つ。

..  Add a method listSurroundings to the Terrarium prototype. It takes one argument, the point at which the bug is currently standing, and returns an object with information about the surroundings of that point. When the point is at the edge of the grid, use "#" for the directions that go outside of the grid, so the bug will not try to move there.

``listSurroundings``\ メソッドを\ ``Terrarium``\ プロトタイプに追加せよ。虫が今いる場所のpointを引数として取り、その地点の周囲の情報のオブジェクトを返す。その地点がマス目の端だったときは、\ ``"#"``\ をマス目の外側の方向として使い、虫がマス目から抜け出そうとしないようにせよ。

..  Hint: Do not write out all the directions, use the each method on the directions dictionary.

.. hint::
   全ての方向について書き出さず、\ ``directions``\ 辞書の\ ``each``\ メソッドを使おう。

..  [show solution]

[解答を見る]

.. code-block:: javascript

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

..  Note the use of the grid variable to work around the this problem.

``grid``\ 変数を使うに当たっては\ ``this``\ 周りの問題に注意しよう。

|hr|

..  Both above methods are not part of the external interface of a Terrarium object, they are internal details. Some languages provide ways to explicitly declare certain methods and properties 'private', and make it an error to use them from outside the object. JavaScript does not, so you will have to rely on comments to describe the interface to an object. Sometimes it can be useful to use some kind of naming scheme to distinguish between external and internal properties, for example by prefixing all internal ones with an underscore ('_'). This will make accidental uses of properties that are not part of an object's interface easier to spot.

上記の両方のメソッドはいずれも\ ``Terrarium``\ オブジェクトの外部インターフェースではなく、内側の詳細だ。いくつかの言語では明示的に決まったメソッドとプロパティを'プライベートな'ものとし、オブジェクトの外側からの呼び出しをエラーにする。JavaScriptはそうではなく、オブジェクトのインターフェースの記述はコメントに頼ることになる。外側と内側のプロパティを識別するのに、例えば、全ての内部プロパティの接頭辞の下線(\ ``'_'``\ )など、名前のスキーマを使うことはしばしば有益である。これにより、たまたまオブジェクトのインターフェースに含まれない部分をスポット的に使うことができる。

|hr|

..  Next is one more internal method, the one that will ask a bug for an action and carry it out. It takes an object with object and point properties, as returned by listActingCreatures, as argument. For now, it only knows about the "move" action:

次はもう一つの内部メソッドで、虫たちの次のアクションを聞いて、それを実行する。引数として、\ ``object``\ と\ ``listActingCreatures``\ の返り値である\ ``point``\ プロパティを持つオブジェクトを取る。今のところ、知っているのは\ ``"move"``\ アクションのみである。：

.. code-block:: javascript

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

..  Note that it checks whether the chosen direction is inside of the grid and empty, and ignores it otherwise. This way, the bugs can ask for any action they like ― the action will only be carried out if it is actually possible. This acts as a layer of insulation between the bugs and the terrarium, and allows us to be less precise when writing the bugs' act methods ― for example the StupidBug just always travels South, regardless of any walls that might stand in its way.

選んだ方向がマス目の中かつ空であることをチェックし、他は無視していることに注意。これにより、虫たちがどのようなアクションを取ろうとしても--もしそれが実際に可能なときのみ実行される。これが虫と飼育器の間の絶縁体の層として振る舞い、虫の\ ``act``\ メソッドを書くときに精密さを低くすることができる--例えば\ ``StupidBug``\ （愚かな虫）は、その道に壁があろうと無かろうと常に南にしか進まない。

|hr|

..  These three internal methods then finally allow us to write the step method, which gives all bugs a chance to do something (all elements with an act method ― we could also give the wall object one if we so desired, and make the walls walk).

これら3つの内部メソッドにより、ついに\ ``step``\ メソッドを書けるようになった。全ての虫に何か（\ ``act``\ メソッドを持つ全ての要素―もし望むなら\ ``wall``\ オブジェクトにそれを与えれば、歩く壁を作れる）する機会を与える。

.. code-block:: javascript

    Terrarium.prototype.step = function() {
      forEach(this.listActingCreatures(),
              bind(this.processCreature, this));
    };

..  Now, let us make a terrarium and see whether the bugs move...

今、飼育器が作られ、虫が動くところを見よう...

.. code-block:: javascript

    var terrarium = new Terrarium(thePlan);
    print(terrarium);
    terrarium.step();
    print(terrarium);

|hr|

..  Wait, how come the above calls print(terrarium) and ends up displaying the output of our toString method? print turns its arguments to strings using the String function. Objects are turned to strings by calling their toString method, so giving your own object types a meaningful toString is a good way to make them readable when printed out.

待った。上記の\ ``print(terrarium)``\ の呼び出しと\ ``toString``\ メソッドの出力の表示の終了をどのようにしようか？\ ``print``\ はその引数を\ ``String``\ 関数を使って文字列に変える。オブジェクトはその\ ``toString``\ メソッドを呼び出すことで文字列に変わるので、オブジェクトをプリントアウトするときに読みやすいようにするには、オブジェクト型に意味のある\ ``toString``\ を与えるのが良い手である。

.. code-block:: javascript

    Point.prototype.toString = function() {
      return "(" + this.x + "," + this.y + ")";
    };
    print(new Point(5, 5));

|hr|

..  As promised, Terrarium objects also get start and stop methods to start or stop their simulation. For this, we will use two functions provided by the browser, called setInterval and clearInterval. The first is used to cause its first argument (a function, or a string containing JavaScript code) to be executed periodically. Its second argument gives the amount of milliseconds (1/1000 second) between invocations. It returns a value that can be given to clearInterval to stop its effect.

約束したように、\ ``Terrarium``\ オブジェクトはシミュレーションの開始と終了の\ ``start``\ と\ ``stop``\ メソッドも持つ。このために、ブラウザが提供する、\ ``setInterval``\ と\ ``clearInterval``\ の2つの関数を使う。1つめの関数はその1つめの引数（関数、またはJavaScriptのコードを含む文字列）を定期的に実行する。2つめの引数には発動の間隔をミリ秒（1/1000秒）で与える。その効果を止めるために\ ``clearInterval``\ に与えるための値が戻り値として返る。

.. code-block:: javascript

    var annoy = setInterval(function() {print("What?");}, 400);

..  And...

そして...

.. code-block:: javascript

    clearInterval(annoy);

..  There are similar functions for one-shot time-based actions. setTimeout causes a function or string to be executed after a given amount of milliseconds, and clearTimeout cancels such an action.

時間ベースのワンショットのアクションのために同様の関数がある。\ ``setTimeout``\ は関数か文字列をミリ秒で与えられた時間の後で実行し、\ ``clearTimeout``\ はそのアクションをキャンセルする。

|hr|

.. code-block:: javascript

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

|hr|

..  Now we have a terrarium with some simple-minded bugs, and we can run it. But to see what is going on, we have to repeatedly do print(terrarium), or we won't see what is going on. That is not very practical. It would be nicer if it would print automatically. It would also look better if, instead of printing a thousand terraria below each other, we could update a single printout of the terrarium. For that second problem, this page conveniently provides a function called inPlacePrinter. It returns a function like print which, instead of adding to the output, replaces its previous output.

今我々は単細胞な虫がいる飼育器を持っていて、それを実行することができる。しかし、起こっていることを見続けようにも、\ ``print(Terrarium)``\ を繰り返さなければそれを見ることができない。これはとても実用的でない。自動的にプリントされるようにしたほうがいいだろう。もしそれもできるなら、1000の飼育器をプリントする代わりに、1つの飼育器のプリントアウトを更新できたほうがいい。2つめの問題には、このページが便利なことに\ ``inPlacePrinter``\ という関数を提供している。それは\ ``print``\ のような関数だが、出力を追加する代わりに、前回の出力を置き換える。

.. code-block:: javascript

    var printHere = inPlacePrinter();
    printHere("Now you see it.");
    setTimeout(partial(printHere, "Now you don't."), 1000);

..  To cause the terrarium to be re-printed every time it changes, we can modify the step method as follows:

飼育器を変更毎に再プリントするため、\ ``step``\ メソッドをこのように書き換えよう：

.. code-block:: javascript

    Terrarium.prototype.step = function() {
      forEach(this.listActingCreatures(),
              bind(this.processCreature, this));
      if (this.onStep)
        this.onStep();
    };

..  Now, when an onStep property has been added to a terrarium, it will be called on every step.

今、飼育器に追加した\ ``onStep``\ プロパティは、ステップ毎に呼び出される。

.. code-block:: javascript

    var terrarium = new Terrarium(thePlan);
    terrarium.onStep = partial(inPlacePrinter(), terrarium);
    terrarium.start();

..  Note the use of partial ― it produces an in-place printer applied to the terrarium. Such a printer only takes one argument, so after partially applying it there are no arguments left, and it becomes a function of zero arguments. That is exactly what we need for the onStep property.

``partial``\ の使用に注意―これは飼育器に適用するためのin-placeプリンターを作る。このプリンターは1つだけ引数を取り、それが部分的に適用されたら残りの引数はなく、引数がゼロの関数となる。まさにそうなることが\ ``onStep``\ プロパティに必要だ。

..  Don't forget to stop the terrarium when it is no longer interesting (which should be pretty soon), so that it does not keep wasting your computer's resources:

面白いことが起こらなくなったら（それはあっという間だろう）飼育器を止めるのを忘れないように。コンピュータの資源を浪費することはない：

.. code-block:: javascript

    terrarium.stop();

|hr|

..  But who wants a terrarium with just one kind of bug, and a stupid bug at that? Not me. It would be nice if we could add different kinds of bugs. Fortunately, all we have to is to make the elementFromCharacter function more general. Right now it contains three cases which are typed in directly, or 'hard-coded':

しかし1種類の虫、しかも単細胞な虫しかいない飼育器を欲しい人がいるだろうか？私は嫌だ。もし別の種類の虫を加えることができたら、それがいいだろう。幸運にも、やることは\ ``elementFromCharacter``\ をより一般的にすることだけだ。今ここには直接タイプされ、または'ハードコード'された3つのケースが含まれている。：

.. code-block:: javascript

    function elementFromCharacter(character) {
      if (character == " ")
        return undefined;
      else if (character == "#")
        return wall;
      else if (character == "o")
        return new StupidBug();
    }

..  The first two cases we can leave intact, but the last one is way too specific. A better approach would be to store the characters and the corresponding bug-constructors in a dictionary, and look for them there:

最初の2つはそのままにしておけるが、最後の1つは個別的すぎるやり方だ。よりよいアプローチは文字と対応するコンストラクターを辞書に格納することで、このようになる。：

.. code-block:: javascript

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

..  Note how the register method is added to creatureTypes ― this is a dictionary object, but there is no reason why it shouldn't support an additional method. This method looks up the character associated with a constructor, and stores it in the dictionary. It should only be called on constructors whose prototype does actually have a character property.

``creatureTypes``\ に追加された\ ``register``\ メソッドに注意―これは辞書オブジェクトだが、しかし追加のメソッドをサポートしていけない理由はない。このメソッドはコンストラクターに結びついた文字を探し出し、辞書に格納する。そのプロトタイプが実際に\ ``character``\ プロパティを持っているコンストラクターについてだけ呼び出すことができる。

..  elementFromCharacter now looks up the character it is given in creatureTypes, and raises an exception when it comes across an unknown character.

``elementFromCharacter``\ は今、\ ``creatureTypes``\ から与えられた文字を探しだし、未知の文字に遭えば例外を起こす。

|hr|

..  Here is a new bug type, and the call to register its character in creatureTypes:

これが新しい虫のタイプだ。そしてその文字を\ ``creatureTypes``\ に登録するためにregisterを呼び出す。

.. code-block:: javascript

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

..  Can you figure out what it does?

どんな虫か解釈できるだろうか？

|hr|

.. _EX86:

演習 8.6

..  Create a bug type called DrunkBug which tries to move in a random direction every turn, never mind whether there is a wall there. Remember the Math.random trick from chapter 7.

壁があろうと無かろうと、毎ターンランダムな方向に進もうとする\ ``DrunkBug``\ という虫の型を作れ。7章の\ ``Math.random``\ のトリックを忘れないこと。

..  [show solution]

[解答を見る]

..  To pick a random direction, we will need an array of direction names. We could of course just type ["n", "ne", ...], but that duplicates information, and duplicated information makes me nervous. We could also use the each method in directions to build the array, which is better already.

ランダムな方向を取るために、方向の名前の配列が必要になる。もちろん、\ ``["n", "ne", ...]``\ とタイプするだけでも可能だが、それでは情報が重複し、重複した情報は我々をナーバスにする。配列を組み立てるために\ ``direction``\ の\ ``each``\ メソッドを使うこともでき、既にあるものよりその方がいいだろう。

..  But there is clearly a generality to be discovered here. Getting a list of the property names in a dictionary sounds like a useful tool to have, so we add it to the Dictionary prototype.

しかし、明らかに一般性のある方法がここにある。プロパティ名のリストを辞書に得るというのは有益なツールに思えるので、\ ``Dictionary``\ プロトタイプにそれを加えよう。

.. code-block:: javascript

    Dictionary.prototype.names = function() {
      var names = [];
      this.each(function(name, value) {names.push(name);});
      return names;
    };

    show(directions.names());

..  A real neurotic programmer would immediately restore symmetry by also adding a values method, which returns a list of the values stored in the dictionary. But I guess that can wait until we need it.

本物の神経症のプログラマーは、辞書の中に格納された値のリストを返す、\ ``values``\ メソッドも加えて、明示的に対称性を戻したいと思うだろう。しかし、\ `それが必要になるまで <http://www.c2.com/cgi/wiki?YouArentGonnaNeedIt>`_\ 待とうというほうに賛成だ。

..  Here is a way to take a random element from an array:

配列からランダムな要素を取り出す方法をここに示す。：

.. code-block:: javascript

    function randomElement(array) {
      if (array.length == 0)
        throw new Error("The array is empty.");
      return array[Math.floor(Math.random() * array.length)];
    }

    show(randomElement(["heads", "tails"]));

..  And the bug itself:

そして虫そのもの。：

.. code-block:: javascript

    function DrunkBug() {};
    DrunkBug.prototype.act = function(surroundings) {
      return {type: "move",
              direction: randomElement(directions.names())};
    };
    DrunkBug.prototype.character = "~";

    creatureTypes.register(DrunkBug);

|hr|

..  So, let us test out our new bugs:

さて、新しい虫をテストしよう。：

.. code-block:: javascript

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

..  Notice the bouncing bugs bouncing off the drunk ones? Pure drama. Anyway, when you are done watching this fascinating show, shut it down:

跳ねる虫が酔っ払った奴にぶつかって跳ね返ったら？劇的だ。いずれにせよ、この魅惑的なショーを見終わったら、それを閉じよう。：

.. code-block:: javascript

    terrarium.stop();

|hr|

..  We now have two kinds of objects that both have an act method and a character property. Because they share these traits, the terrarium can approach them in the same way. This allows us to have all kinds of bugs, without changing anything about the terrarium code. This technique is called polymorphism, and it is arguably the most powerful aspect of object-oriented programming.

今の2種類のオブジェクトはともに1つの\ ``act``\ メソッドと\ ``character``\ プロパティを持っている。なぜなら、これらの特徴を共有することで、飼育器は同じ手段でそれらにアプローチできるからだ。これにより、飼育器についてのコードを全く変えることなしに、全ての種類の虫を持つことができる。このテクニックはポリモーフィズムとよばれ、ほぼ間違いなく、オブジェクト指向プログラミングの最もパワフルな面である。

..  The basic idea of polymorphism is that when a piece of code is written to work with objects that have a certain interface, any kind of object that happens to support this interface can be plugged into the code, and it will just work. We already saw simple examples of this, like the toString method on objects. All objects that have a meaningful toString method can be given to print and other functions that need to convert values to strings, and the correct string will be produced, no matter how their toString method chooses to build this string.

ポリモーフィズムの基本的なアイデアは、コードの部分が決まったインターフェースを持つオブジェクトとともに動くよう書かれていれば、このインターフェースをサポートする任意の種類のオブジェクトがコードに接続でき、そしてそれがそのまま動くということだ。これの単純な例は、既に見てきたオブジェクトの\ ``toString``\ メソッドである。意味のある\ ``toString``\ メソッドを持つ全てのオブジェクトは\ ``print``\ 、および文字列に値を変換する必要のある他の関数に与えられることができ、そして文字列を組み立てるのに選ばれたそれらの\ ``toString``\ メソッドがどのようなものであろうと、正しい文字列が作られる。

..  Similarly, forEach works on both real arrays and the pseudo-arrays found in the arguments variable, because all it needs is a length property and properties called 0, 1, and so on, for the elements of the array.

同様に、\ ``forEach``\ も変数\ ``arguments``\ の中に見つかった本物の配列と偽物の配列の両方で動き、なぜならそれが必要としているものは\ ``length``\ プロパティと、配列の要素を示す、\ ``0``\ 、\ ``1``\ 、とかそのような、プロパティで全部だからである。

|hr|

..  To make life in the terrarium more life-like, we will add to it the concepts of food and reproduction. Each living thing in the terrarium gets a new property, energy, which is reduced by performing actions, and increased by eating things. When it has enough energy, a thing can reproduce2, generating a new creature of the same kind.

飼育器の中の生命をより生命らしくするために、食物と繁殖の概念を追加しよう。飼育器の中のそれぞれの生物は、新しいプロパティ、\ ``energy``\ を得て、それは行動することで減り、物を食べることで増える。energyが十分なとき、生物は繁殖する\ [#f2]_\ ことができ、同じ種類の新しい生物が生成される。

..  If there are only bugs, wasting energy by moving around and eating each other, a terrarium will soon succumb to the forces of entropy, run out of energy, and become a lifeless wasteland. To prevent this from happening (too quickly, at least), we add lichen to the terrarium. Lichen do not move, they just use photo-synthesis to gather energy, and reproduce.

もし、虫しかいなければ、移動によるエネルギーの消費と共食いにより、飼育器はエントロピーの力に屈してしまい、エネルギーは枯渇し、生命の無い荒れ地になるだろう。これの（少なくとも、早すぎる）発生を防ぐために、飼育器に地衣類を生やそう。地衣類は動かず、光合成でエネルギーを集め、繁殖する。

..  To make this work, we will need a terrarium with a different processCreature method. We could just replace the method of to the Terrarium prototype, but we have become very attached to the simulation of the bouncing and drunk bugs, and we would hate to break our old terrarium.

この働きをつくるために、異なる\ ``processCreature``\メソッドを持つ飼育器が必要だ。\ ``Terrarium``\ プロトタイプのメソッドをただ置き換えることもできるが、しかし飛び跳ねる虫や依った虫のシミュレーションに合わせたいし、古い飼育器を壊すのは嫌だ。

..  What we can do is create a new constructor, LifeLikeTerrarium, whose prototype is based on the Terrarium prototype, but which has a different processCreature method.

できるのは、新しいコンストラクター、\ ``Terrarium``\ プロトタイプをベースとしつつ、異なる\ ``processCreature``\ メソッドを持つ\ ``LifeLikeTerrarium``\ を作ることだ。

|hr|

..  There are a few ways to do this. We could go over the properties of Terrarium.prototype, and add them one by one to LifeLikeTerrarium.prototype. This is easy to do, and in some cases it is the best solution, but in this case there is a cleaner way. If we make the old prototype object the prototype of the new prototype object (you may have to re-read that a few times), it will automatically have all its properties.

これを行うには2つの方法がある。\ ``Terratium.prototype``\ を一通り見て、1つ1つ\ ``LifeLikeTerrarium.prototype``\ に加えることができる。これは簡単で、いくつかの場合には最も良い解決だ。しかし、この場合はより明らかな手段がある。もし古いprototypeオブジェクトを新しいprototypeオブジェクトのプロトタイプにしたら（あなたはここを2,3度読み返す必要があるだろう）、それは自動的にその全てのプロパティを持つ。

..  Unfortunately, JavaScript does not have a straightforward way to create an object whose prototype is a certain other object. It is possible to write a function that does this, though, by using the following trick:

残念ながら、JavaScriptは決まった他のオブジェクトをプロトタイプとして、オブジェクトを作るわかりやすい手段を持っていない。これを行う関数を書くのは可能であるが、それでも、以下のトリックが必要だ。：

.. code-block:: javascript

    function clone(object) {
      function OneShotConstructor(){}
      OneShotConstructor.prototype = object;
      return new OneShotConstructor();
    }

..  This function uses an empty one-shot constructor, whose prototype is the given object. When using new on this constructor, it will create a new object based on the given object.

この関数は、与えられたオブジェクトをプロトタイプとする、空のワンショットのコンストラクターを使う。このコンストラクターで\ ``new``\ を使ったとき、与えられたオブジェクトをもとにした新しいオブジェクトが作られる。

.. code-block:: javascript

    function LifeLikeTerrarium(plan) {
      Terrarium.call(this, plan);
    }
    LifeLikeTerrarium.prototype = clone(Terrarium.prototype);
    LifeLikeTerrarium.prototype.constructor = LifeLikeTerrarium;

..  The new constructor doesn't need to do anything different from the old one, so it just calls the old one on the this object. We also have to restore the constructor property in the new prototype, or it would claim its constructor is Terrarium (which, of course, is only really a problem when we make use of this property, which we don't).

新しいコンストラクターは古いものから異なることをする必要が無く、\ ``this``\ オブジェクトの、古いものを呼ぶだけだ。新しいプロトタイプに\ ``constructor``\ プロパティを戻すか、またはそのコンストラクターが\ ``Terrarium``\ であると主張しなければならない（もちろん、このプロパティを使う時だけ実際に問題になり、我々はそうしない）。

|hr|

..  It is now possible to replace some of the methods of the LifeLikeTerrarium object, or add new ones. We have based a new object type on an old one, which saved us the work of re-writing all the methods which are the same in Terrarium and LifeLikeTerrarium. This technique is called 'inheritance'. The new type inherits the properties of the old type. In most cases, this means the new type will still support the interface of the old type, though it might also support a few methods that the old type does not have. This way, objects of the new type can be (polymorphically) used in all the places where objects of the old type could be used.

今、LifeLikeTerrariumオブジェクトのメソッドのいくつかを置き換えたり、あるいは新しいものを加えることができる。新しいオブジェクトは古い物をベースにしているので、TerrariumとLifeLikeTerrariumで同じになるメソッドを再度書く手間は省ける。このテクニックは'継承（インヘリタンス）'と呼ばれる。新しい型は古い型のプロパティを継承する。多くの場合、これは新しい型は古い型のインターフェースをまだサポートした上で、古い型が持たないメソッドもサポートするということである。これにより、新しい型のオブジェクトは（ポリモーフィズム的に）古い型が使われていた全ての場所で使われることができる。

..  In most programming languages with explicit support for object-oriented programming, inheritance is a very straightforward thing. In JavaScript, the language doesn't really specify a simple way to do it. Because of this, JavaScript programmers have invented many different approaches to inheritance. Unfortunately, none of them is quite perfect. Fortunately, such a broad range of approaches allows a programmer to choose the most suitable one for the problem he is solving, and allows certain tricks that would be utterly impossible in other languages.

明示的にオブジェクト指向プログラミングをサポートしている、多くのプログラミング言語では、継承はとてもわかいやすいものだ。JavaScriptでは、言語は本当にそれを単純に行う手段を個別には持っていない。このため、JavaScriptプログラマーは'継承'のための多くの異なるアプローチを発明してきた。残念ながら、完璧なものはない。幸運にも、アプローチの範囲は幅広く、プログラマーは解決したい問題に最も適したもの選択でき、他の言語では全く不可能なトリックでさえ使える。

..  At the end of this chapter, I will show a few other ways to do inheritance, and the issues they have.

この章の終わりで、継承を行う他の手段と、それらの問題を見せよう。

..  Here is the new processCreature method. It is big.

これが新しい\ ``processCreature``\ メソッドである。大きくなった。

.. code-block:: javascript

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

..  The function still starts by asking the creature for an action. Then, if the action has a direction property, it immediately computes which point on the grid this direction points to and which value is currently sitting there. Three of the five supported actions need to know this, and the code would be even uglier if they all computed it separately. If there is no direction property, or an invalid one, it leaves the variables target and valueAtTarget undefined.

この関数はまだ生物に行動を聞くところから始まる。それから、もし行動が\ ``direction``\ プロパティを持っていたら、この方向のポイントと現在いるところの値をマス目上のポイントで明示的に計算する。サポートされている行動5つのうち3つについてはこれを知る必要があり、もしそれら全てを分離して計算していたらもっとコードが汚くなっただろう。\ ``direction``\ プロパティが無かったり、間違っていたら、\ ``target``\ と\ ``valueAtTarget``\ 変数は未定義となる。

..  After this, it goes over all the actions. Some actions require additional checking before they are executed, this is done with a separate if so that if a creature, for example, tries to walk through a wall, we do not generate an "Unsupported action" exception.

この後は、全ての行動に渡る。いくつかの行動は、実行する前に追加のチェックを必要とし、これは別々の\ ``if``\ で行われる。もし生物だったら、例えば、壁の中を進もうとしたり、というように。我々は\ ``"Unsupported action"``\ 例外を生成しない。

..  Note that, in the "reproduce" action, the parent creature loses twice the energy that the newborn creature gets (childbearing is not easy), and the new creature is only placed on the grid if the parent had enough energy to produce it.

``"reproduce"``\ アクションに注意、親の生物は新しい生物の得るエネルギーの2倍のエネルギーを失い（出産は楽じゃ無い）、新しい生物は親が十分なエネルギーを持っているときのみ生まれることができる。

..  After the action has been performed, we check whether the creature is out of energy. If it is, it dies, and we remove it.

行動が実行された後、生物がエネルギー切れになっていないかチェックし、もしそうであれば、それは死んだので、取り除く。

|hr|

..  Lichen is not a very complex organism. We will use the character "*" to represent it. Make sure you have defined the randomElement function from exercise 8.6, because it is used again here.

地衣類はあまり複雑な生命ではない。それを表現するのに"*"文字を使う。\ :ref:`演習8.6<EX86>`\ の\ ``randomElement``\ 関数を確実に定義すること。

.. code-block:: javascript

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

..  Lichen do not grow bigger than 20 energy, or they would get huge when they are surrounded by other lichen and have no room to reproduce.

地衣類は20エネルギーより大きく成長することは無く、または他の地衣類に周りを囲まれ繁殖するスペースが無いときだけ\ *大きくなる*\ 。

|hr|

演習 8.7

..  Create a LichenEater creature. It starts with an energy of 10, and behaves in the following way:

..  When it has an energy of 30 or more, and there is room near it, it reproduces.
..  Otherwise, if there are lichen nearby, it eats a random one.
..  Otherwise, if there is space to move, it moves into a random nearby empty square.
..  Otherwise, it waits.
..  Use findDirections and randomElement to check the surroundings and to pick directions. Give the lichen-eater "c" as its character (pac-man).

``LichenEater``\ （苔を食べる生物）を作れ。\ ``10``\ のエネルギーから始めて、下記のように振る舞う。：
::

* 30以上のエネルギーがあり、周りに空いている場所があれば、繁殖する。
* そうでなければ、もし地衣類が近くにいれば、その中の1つをランダムに食べる。
* そうでなければ、動く場所があれば、ランダムに空いている隣の四角に移動する。
* そうでなければ、待つ。

``findDirections``\ と\ ``randomElement``\ を周囲のチェックと方向の選択に使う。\ ``"c"``\ （パックマン）をLichenEaterの文字とする、

..  [show solution]

[解答を見る]

.. code-block:: javascript

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

|hr|

..  And try it out.

試してみよう。

.. code-block:: javascript

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

..  Most likely, you will see the lichen quickly over-grow a large part of the terrarium, after which the abundance of food makes the eaters so numerous that they wipe out all the lichen, and thus themselves. Ah, tragedy of nature.

おそらく、地衣類が飼育器の中の大きな部分を占めるように早く成長するのを見て、その後、大量の食料がLichenEaterを増やし、全ての地衣類を食べ尽くし、彼ら自身もいなくなる。ああ、自然の悲劇よ。

.. code-block:: javascript

    terrarium.stop();

|hr|

..  Having the inhabitants of your terrarium go extinct after a few minutes is kind of depressing. To deal with this, we have to teach our lichen-eaters about long-term sustainable farming. By making them only eat if they see at least two lichen nearby, no matter how hungry they are, they will never exterminate the lichen. This requires some discipline, but the result is a biotope that does not destroy itself. Here is a new act method ― the only change is that it now only eats when lichen.length is at least two.

飼育器の居住者が2,3分後に死滅するのでは気が滅入る。これに対処するために、LichenEaterに長期間持続可能な農業経営を教えよう。周りに最低2つの地衣類が無ければ、腹が減っていても食べないことにして、地衣類が根絶されないようにする。これにはしつけが必要だが、しかしその結果は自滅しない生活圏になるだろう。これが新しい\ ``act``\ メソッドだ ― ``lichen.length``\ が最低2あるときだけ食べるようにするところだけ変更した。

.. code-block:: javascript

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

..  Run the above lichenPlan terrarium again, and see how it goes. Unless you are very lucky, the lichen-eaters will probably still go extinct after a while, because, in a time of mass starvation, they crawl aimlessly back and forth through empty space, instead of finding the lichen that is sitting just around the corner.

上記の\ ``lichenPlan``\ 飼育器を再び動かし、どうなるか見よう。とても幸運なことがない限り、LichenEaterはしばらく後に絶滅してしまうだろう。なぜなら、多くの時間、飢えたまま、隅に生えている地衣類を見つける代わりに、空の空間を目的無く四方に這うだけになるからである。

|hr|

演習 8.8

..  Find a way to modify the LichenEater to be more likely to survive. Do not cheat ― this.energy += 100 is cheating. If you rewrite the constructor, do not forget to re-register it in the creatureTypes dictionary, or the terrarium will continue to use the old constructor.

``LichenEater``\ が生き残りやすくなるよう変更する手段を見つけよう。ズルはしないこと ― ``this.energy += 100``\ はズルだ。もしコンストラクターを書き直すなら、\ ``creatureType``\ 辞書に再登録するのを忘れないようにするか、飼育器は古いコンストラクターのものを使い続けよう。

..  [show solution]

[解答を見る]

..  One approach would be to reduce the randomness of its movement. By always picking a random direction, it will often move back and forth without getting anywhere. By remembering the last direction it went, and preferring that direction, the eater will waste less time, and find food faster.

1つのアプローチは移動のランダムさを減らすことだ。常にランダムな方向をとり続けることで、しばしばどこにも向かわずに行ったり来たりすることになる。進んだ最後の方向を覚えておいて、その方向を選ぶことにより、時間の浪費を減らし、早く食物を見つけられるようにしよう。

.. code-block:: javascript

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

..  Try it out using the previous terrarium-plan.

前回の飼育器プランを使って試してみよう。

|hr|

演習 8.9

..  A one-link food chain is still a bit rudimentary. Can you write a new creature, LichenEaterEater (character "@"), which survives by eating lichen-eaters? Try to find a way to make it fit in the ecosystem without dying out too quickly. Modify the lichenPlan array to include a few of these, and try them out.

1つにリンクした食物連鎖はまだちょっと初歩的だ。LichenEaterを食べて生きる\ ``LichenEaterEater``\ （\ ``"@"``\ 文字）という新しい生物を書くことができるだろうか？早すぎる滅亡のないエコシステムに合うような手を見つけてみよう。\ ``lichenPlan``\ 配列をこれらを2,3含むようにして、それを試そう。

..  You are on your own here. I failed to find a really good way to prevent these creatures from either going extinct right away or gobbling up all lichen-eaters and then going extinct. The trick of only eating when it spots two pieces of food doesn't work very well for them, because their food moves around so much it is rare to find two in one place. What does seem to help is making the eater-eater really fat (high energy), so that it can survive times when lichen-eaters are scarce, and only reproduces slowly, which prevents it from exterminating its food source too quickly.

あなた自身で書いてみて欲しい。私は、これらの生物が根絶したり、LichenEaterを食べ尽くして根絶させたりすることを防ぐ本当に良い手段を見つけるのに失敗した。食料が2つあるときだけ食べるというトリックはうまくいかなかった。なぜなら、食物の方も動くので2つが1つの場所で出会うことが希だったからである。LichenEaterEaterを太らせる（高いenergy）のは良さそうに思われ、LichenEaterが乏しいときは長く生き残り、繁殖だけが遅くなり、その食物が早く根絶することは防がれた。

..  The lichen and eaters go through a periodic movement ― sometimes lichen are abundant, which causes a lot of eaters to be born, which causes the lichen to become scarce, which causes the eaters to starve, which causes the lichen to become abundant, and so on. You could try to make the lichen-eater-eaters 'hibernate' (use the "wait" action for a while), when they fail to find food for a few turns. If you choose the right amount of turns for this hibernation, or have them wake up automatically when they smell lots of food, this could be a good strategy.

地衣類と捕食者達は周期的な動きをするようになる ― 地衣類が豊作になると、捕食者達が大量に生まれ、それにより地衣類が少なくなり、捕食者達が餓死し、それにより地衣類が豊富になる、そのように続く。LichenEaterEaterが、2,3ターン食物を見つけられないよう、冬眠するように作ることもできる（\ ``"wait"``\ 行動をしばらく続くようにして）。もしこの冬眠のターン数を正しく選ぶか、あるいはたくさんの食物の匂いをかぎつけたときに自動的に起きるようにしたら、これは良い戦略になるだろう。

|hr|

..  That concludes our discussion of terraria. The rest of the chapter is devoted to a more in-depth look at inheritance, and the problems related to inheritance in JavaScript.

飼育器に関する議論はここで結びとしよう。章の残りは継承と、JavaScriptにおける継承にまつわる問題をより深く見ることに費やそう。

|hr|

..  First, some theory. Students of object-oriented programming can often be heard having lengthy, subtle discussions about correct and incorrect uses of inheritance. It is important to bear in mind that inheritance, in the end, is just a trick that allows lazy3 programmers to write less code. Thus, the question of whether inheritance is being used correctly boils down to the question of whether the resulting code works correctly and avoids useless repetitions. Still, the principles used by these students provide a good way to start thinking about inheritance.

最初に、いくつかの理論では、オブジェクト指向プログラミングの生徒はしばしば、継承の使い方の正しい、正しくないという議論に酷く退屈することがある。継承を覚えることは重要である、結局は、怠惰な\ [#f3]_\ プログラマーが書くコードをへらすためのただのトリックとして。すなわち、継承を正しく使えているかどうかという疑問は結局、結果のコードが正しく、無益な繰り返しを避けられているかということになる。まだ、これらの生徒が使っている原則は継承について考え始めるための良い手段を提供する。

..  Inheritance is the creation of a new type of objects, the 'sub-type', based on an existing type, the 'super-type'. The sub-type starts with all the properties and methods of the super-type, it inherits them, and then modifies a few of these, and optionally adds new ones. Inheritance is best used when the thing modelled by the sub-type can be said to be an object of the super-type.

継承は、既存の型'スーパータイプ'をもとにした新しい型のオブジェクト'サブタイプ'を作ることである。サブタイプはスーパータイプの全てのプロパティをメソッドを初めから持ち、それらを継承し、それからこれらをいくつか変更したり、任意に新しい物を追加する。サブタイプによってモデル化されたものがスーパータイプのオブジェクト *である* ときに継承はよく使われる。

..  Thus, a Piano type could be a sub-type of an Instrument type, because a piano is an instrument. Because a piano has a whole array of keys, one might be tempted to make Piano a sub-type of Array, but a piano is no array, and implementing it like that is bound to lead to all kinds of silliness. For example, a piano also has pedals. Why would piano[0] give me the first key, and not the first pedal? The situation is, of course, that a piano has keys, so it would be better to give it a property keys, and possibly another property pedals, both holding arrays.

このように、\ ``Piano``\ （ピアノ）型は\ ``Instrument``\ （楽器）型のサブタイプであり得て、なぜならそれはピアノは楽器\ *である*\ からだ。ピアノが完全な鍵盤の配列を持っているからといって、\ ``Piano``\ を配列のサブタイプにしようとするかもしれないが、ピアノは配列\ *ではなく*\ 、そのように実装することは全ての種類の愚かさへの手綱に縛り付けるようなものだ。例えば、ピアノはペダルも持っている。なぜ\ ``piano[0]``\ が最初の鍵盤であって、最初のペダルでは無いのか？この状況においては、もちろん、ピアノは鍵盤も\ *持っている*\ ので、\ ``keys``\ プロパティと\ ``pedals``\ プロパティの両方を配列として与えるほうがいいだろう。

..  It is possible for a sub-type to be the super-type of yet another sub-type. Some problems are best solved by building a complex family tree of types. You have to take care not to get too inheritance-happy, though. Overuse of inheritance is a great way to make a program into a big ugly mess.

あるサブタイプを他のサブタイプのスーパータイプとすることは可能だ。いくつかの問題が複雑な型の家系図を組み立てることによって最も良く解決される。それでも、継承の使いすぎには注意しよう。継承の過剰使用はプログラムを大きな醜い失敗作にするための大きな道だ。

|hr|

..  The working of the new keyword and the prototype property of constructors suggest a certain way of using objects. For simple objects, such as the terrarium-creatures, this way works rather well. Unfortunately, when a program starts to make serious use of inheritance, this approach to objects quickly becomes clumsy. Adding some functions to take care of common operations can make things a little smoother. Many people define, for example, inherit and method methods on objects.

``new``\ キーワードとコンストラクターの\ ``prototype``\ プロパティの働きはオブジェクトの使用の確かな道を提案する。飼育器の生物のような単純なオブジェクトには、この方法がむしろいいだろう。残念ながら、継承の厳格な使用を始める時、オブジェクトへのこのアプローチは早く不細工になる。共通の操作の世話をするいくつかの関数を加えることが物事を少しスムーズにする。多くの人々が、例えば、オブジェクトに\ ``inherit``\ と\ ``method``\ メソッドを定義する。

.. code-block:: javascript

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

..  If you search the web for the words 'JavaScript' and 'inheritance', you will come across scores of different variations on this, some of them quite a lot more complex and clever than the above.

もし'JavaScript'と'inheritance'の語でウェブを検索しても、これの多くの異なるバリエーションにしか出会わないだろう。そのうちのいくつかは上記よりもっと複雑でできがいい。

..  Note how the push method written here uses the push method from the prototype of its parent type. This is something that is done often when using inheritance ― a method in the sub-type internally uses a method of the super-type, but extends it somehow.

ここで書かれている\ ``push``\ メソッドは親の型のプロトタイプの\ ``push``\ メソッドをどのように使っているかに注意しよう。これは継承を使う時に ― サブタイプの中で内部的にスーパータイプのメソッドを、なにも拡張すること無く使う時にしばしば使われる。

|hr|

..  The biggest problem with this basic approach is the duality between constructors and prototypes. Constructors take a very central role, they are the things that give an object type its name, and when you need to get at a prototype, you have to go to the constructor and take its prototype property.

この基本的なアプローチの、大きな問題はコンストラクターとプロトタイプの間の二重性だ。コンストラクターはとても中心的な役割で、オブジェクト型それ自身の名前を与えられたものであり、そしてプロトタイプを得ることが必要なとき、コンストラクターに行きその\ ``prototype``\ プロパティを得なければならない。

..  Not only does this lead to a lot of typing ("prototype" is 9 letters), it is also confusing. We had to write an empty, useless constructor for StrangeArray in the example above. Quite a few times, I have found myself accidentally adding methods to a constructor instead of its prototype, or trying to call Array.slice when I really meant Array.prototype.slice. As far as I am concerned, the prototype itself is the most important aspect of an object type, and the constructor is just an extension of that, a special kind of method.

タイプする文字数が\ *多い*\ （\ ``"prototype"``\ は9文字）だけではなく、混乱してもいる。上記の例では、空の、使い途の無いコンストラクターを\ ``StrangeArray``\ のために書かなければならない。何度か、間違ってコンストラクターの代わりにそのプロトタイプにメソッドを追加したり、本当は\ ``Array.prototype.slice``\ のつもりで\ ``Array.slice``\ を呼び出そうとしたりしたことがある。私に関する限り、プロトタイプそれ自体がオブジェクト型の重要な面であり、コンストラクターはただその拡張であり、特別な種類のメソッドでしかない。

|hr|

..  With a few simple helper methods added to Object.prototype, it is possible to create an alternative approach to objects and inheritance. In this approach, a type is represented by its prototype, and we will use capitalised variables to store these prototypes. When it needs to do any 'constructing' work, this is done by a method called construct. We add a method called create to the Object prototype, which is used in place of the new keyword. It clones the object, and calls its construct method, if there is such a method, giving it the arguments that were passed to create.

少数の単純なヘルパーメソッドを\ ``Object.prototype``\ に加えることで、オブジェクトと継承に代わりのアプローチを作ることが可能になる。このアプローチでは、型はそのプロトタイプで表現され、それらのプロトタイプを格納するのに先頭を大文字にした変数を使うことになる。それが何か'constructing'の作業が必要なときは、\ ``construct``\ というメソッドを実行する。\ ``new``\ キーワードの代わりに使う\ ``create``\ というメソッドを\ ``Object``\ プロトタイプに加えよう。オブジェクトを複製し、そのようなメソッドがあれば、\ ``create``\ に渡された引数をそれに与えて、その\ ``construct``\ メソッドを呼び出す。

.. code-block:: javascript

    Object.prototype.create = function() {
      var object = clone(this);
      if (typeof object.construct == "function")
        object.construct.apply(object, arguments);
      return object;
    };

..  Inheritance can be done by cloning a prototype object and adding or replacing some of its properties. We also provide a convenient shorthand for this, an extend method, which clones the object it is applied to and adds to this clone the properties in the object that it is given as an argument.

継承はプロトタイプ・オブジェクトの複製とそのプロパティのいくつかの追加またはで置き換えで行える。これの便利な短縮形、\ ``extend``\ メソッドも提供する。これは適用されたオブジェクトを複製し、この複製に、引数として与えられたオブジェクトのプロパティを加える。

.. code-block:: javascript

    Object.prototype.extend = function(properties) {
      var result = clone(this);
      forEachIn(properties, function(name, value) {
        result[name] = value;
      });
      return result;
    };

..  In a case where it is not safe to mess with the Object prototype, these can of course be implemented as regular (non-method) functions.

``Object``\ プロトタイプを壊す危険がないとはいえない場合は、これらはもちろん通常の（メソッドで無い）関数として実装できる。

|hr|

..  An example. If you are old enough, you may at one time have played a 'text adventure' game, where you move through a virtual world by typing commands, and get textual descriptions of the things around you and the actions you perform. Now those were games!

1つの例として、もしあなたがそれなりの歳なら、かつて1度は'テキスト・アドベンチャー'ゲームを遊んだことがあるかもしれない。バーチャルな世界をコマンドをタイプすることで動き回り、周囲の物事と行った行動について、テキストによる説明を得るのだ。そのようなゲームだ！

..  We could write the prototype for an item in such a game like this.

そんなゲームのアイテムのプロトタイプはこのように書く。

.. code-block:: javascript

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

..  Inherit from it like this...

これをこのように継承する...

.. code-block:: javascript

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

..  Leaving out the compulsory prototype part makes things like calling Item.construct from DetailedItem's constructor slightly simpler. Note that it would be a bad idea to just do this.name = name in DetailedItem.construct. This duplicates a line. Sure, duplicating the line is shorter than calling the Item.construct function, but if we end up adding something to this constructor later, we have to add it in two places.

強制的な\ ``prototype``\ プロトタイプの部分から抜け出すのは、\ ``DetailedItem``\ のコンストラクターから\ ``Item.construct``\ を呼び出すような事をちょっと単純にする。\ ``DetailedItem.construct``\ の中で\ ``this.name = name``\ とするのはあまり良いアイデアでは無いことに注意しよう。これは行を重複させる。確かに、行を重複させると\ ``Item.construct``\ 関数を呼び出すより短くなる、しかし、もしこのコンストラクターに後から何か付け加えようとしたときに、2カ所でそれを加えなければならなくなる。

|hr|

..  Most of the time, a sub-type's constructor should start by calling the constructor of the super-type. This way, it starts with a valid object of the super-type, which it can then extend. In this new approach to prototypes, types that need no constructor can leave it out. They will automatically inherit the constructor of their super-type.

多くの場合、サブタイプのコンストラクターはスーパータイプのコンストラクターを呼び出すことから始まる。この手段により、スーパータイプとして正しい型のオブジェクトとして始めて、それから拡張することができる。このプロトタイプへの新しいアプローチでは、コンストラクターを必要としない型はそのままでよい。自動的にそのスーパータイプのコンストラクターを継承する。

.. code-block:: javascript

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

..  Even though SmallItem does not define its own constructor, creating it with a name argument works, because it inherited the constructor from the Item prototype.

``SmallItem``\ はそれ自体のコンストラクターが定義されていないにもかかわらず、\ ``name``\ 引数が働くように作られ、なぜならそれは\ ``Item``\ プロトタイプからコンストラクターを継承しているからである。

|hr|

..  JavaScript has an operator called instanceof, which can be used to determine whether an object is based on a certain prototype. You give it the object on the left hand side, and a constructor on the right hand side, and it returns a boolean, true if the constructor's prototype property is the direct or indirect prototype of the object, and false otherwise.

JavaScriptは\ ``instanceof``\ という演算子を持ち、オブジェクトがどのプロトタイプをもとに作られたか求めることができる。左辺にオブジェクト、右辺にコンストラクターを与えれば、真偽値が返ってきて、\ ``true``\ であればコンストラクターの\ ``prototype``\ プロパティは直接あるいは間接的にそのオブジェクトのプロトタイプであり、\ ``false``\ であればそうではない。

..  When you are not using regular constructors, using this operator becomes rather clumsy ― it expects a constructor function as its second argument, but we only have prototypes. A trick similar to the clone function can be used to get around it: We use a 'fake constructor', and apply instanceof to it.

正規のコンストラクターを使わないとき、この演算子はむしろややこしい ― コンストラクター関数がその2つめの引数として期待されているが、しかし我々はプロトタイプしか持っていない。\ ``clone``\ 関数と同様のトリックをそのために使うことができる。：'でっち上げのコンストラクター'を使って、それに\ ``instanceof``\ を適用しよう。

.. code-block:: javascript

    Object.prototype.hasPrototype = function(prototype) {
      function DummyConstructor() {}
      DummyConstructor.prototype = prototype;
      return this instanceof DummyConstructor;
    };

    show(pencil.hasPrototype(Item));
    show(pencil.hasPrototype(DetailedItem));

|hr|

.. Next, we want to make a small item that has a detailed description. It seems like this item would have to inherit both from DetailedItem and SmallItem. JavaScript does not allow an object to have multiple prototypes, and even if it did, the problem would not be quite that easy to solve. For example, if SmallItem would, for some reason, also define an inspect method, which inspect method should the new prototype use?

次に、細かい説明を持った小さなアイテムを作ることにしよう。それは\ ``DetailedItem``\ と\ ``SmallItem``\ の両方を継承したように見える。JavaScriptはオブジェクトが複数のプロトタイプを持つことを、実際にそうであろうと許していない、問題を解決するのは簡単ではない。例えば、\ ``SmallItem``\ が、何らかの理由で、\ ``inspect``\ メソッドも定義し、その\ ``inspect``\ メソッドを新しいプロトタイプが使うとしたら？

..  Deriving an object type from more than one parent type is called multiple inheritance. Some languages chicken out and forbid it altogether, others define complicated schemes for making it work in a well-defined and practical way. It is possible to implement a decent multiple-inheritance framework in JavaScript. In fact there are, as usual, multiple good approaches to this. But they all are too complex to be discussed here. Instead, I will show a very simple approach which suffices in most cases.

複数の親の型からオブジェクト型を引き出すことを多重継承という。ある言語はそれに怯えて完全に禁止し、他のものは、よく定義され実用的な手段で、それが動くように複雑な計画を定義している。JavaScriptでまともな多重継承を実装することは可能だ。実際には、いつも通り、これに対する複数のアプローチが存在する。しかしこれら全てはここで論じるには複雑すぎる。代わりに、多くの場合に十分であろう、ごく単純なアプローチを見せよう。

|hr|

..  A mix-in is a specific kind of prototype which can be 'mixed into' other prototypes. SmallItem can be seen as such a prototype. By copying its kick and take methods into another prototype, we mix smallness into this prototype.

mix-inは他のプロトタイプを混ぜ合わせることのできる特別な種類のプロトタイプだ。\ ``SmallItem``\ はそのようなプロトタイプであるように見ることができる。その\ ``kick``\ と\ ``take``\ メソッドを他のプロトタイプにコピーすることで、小ささをこのプロトタイプに混ぜよう。

.. code-block:: javascript

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

..  Remember that forEachIn only goes over the object's own properties, so it will copy kick and take, but not the constructor that SmallItem inherited from Item.

``forEachIn``\ はオブジェクト\ *それ自体*\ が持つプロパティだけに対して動き、\ ``kick``\ と\ ``take``\ だけがコピーされ、\ ``SmallItem``\ が\ ``Item``\ から継承したコンストラクターはコピーされないことを忘れないように。

|hr|

..  Mixing prototypes gets more complex when the mix-in has a constructor, or when some of its methods 'clash' with methods in the prototype that it is mixed into. Sometimes, it is workable to do a 'manual mix-in'. Say we have a prototype Monster, which has its own constructor, and we want to mix that with DetailedItem.

プロトタイプの混ぜ合わせはmix-inがコンストラクターを持つとき、またはそのメソッドのあるものが混ぜ合わされるプロトタイプのメソッドを'壊す'ときはより複雑になる。時折、'手動で混ぜ合わせる'ことで動く。それ自体のコンストラクターを持つ、\ ``Monster``\ プロトタイプを持っていて、それを\ ``DetailedItem``\ に混ぜたいとする。

.. code-block:: javascript

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

..  But note that this causes Item constructor to be called twice when creating a DetailedMonster ― once through the DetailedItem constructor, and once through the Monster constructor. In this case there is not much harm done, but there are situations where this would cause a problem.

しかし、これは、\ ``DetailedMonster``\ を作るにあたり\ ``Item``\ コンストラクターの2回の呼び出しをもたらすことに注意しよう ― 1回目は\ ``DetailedItem``\ コンストラクター、もう1回は\ ``Monster``\ コンストラクター。この場合はあまり害にはならないが、問題を引き起こすような状況もある。

|hr|

..  But don't let those complications discourage you from making use of inheritance. Multiple inheritance, though extremely useful in some situations, can be safely ignored most of the time. This is why languages like Java get away with forbidding multiple inheritance. And if, at some point, you find that you really need it, you can search the web, do some research, and figure out an approach that works for your situation.

しかし、そのような複雑さは、あなたに継承の使用を思いとどまらせるようなものではない。多重継承は、ある状況では大いに有用であるが、多くの場合においては無視するのが安全だ。Javaのような言語で多重継承を禁止しているのはこの理由による。そしてもし、いくつかの点で、それが本当に必要になったとき、ウェブを検索し、研究し、状況で動くアプローチを考えだそう。

..  Now that I think about it, JavaScript would probably be a great environment for building a text adventure. The ability to change the behaviour of objects at will, which is what prototypical inheritance gives us, is very well suited for this. If you have an object hedgehog, which has the unique habit of rolling up when it is kicked, you can just change its kick method.

今考えているのは、JavaScriptはテキスト・アドベンチャーを組み立てるのにおそらくいい環境だろうということだ。プロトタイプの継承が我々にもたらした、オブジェクトの振る舞いを意のままに変更する能力は、これによく合致している。もし\ ``hedgehog``\ のオブジェクトを持っていたら、その\ ``kick``\ メソッドを変更するだけで、蹴られたら丸まるというユニークな癖をそれに持たせることができる。、

..  Unfortunately, the text adventure went the way of the vinyl record and, while once very popular, is nowadays only played by a small population of enthusiasts.

残念ながら、テキスト・アドベンチャーはビニール製のレコードとともに去ってしまい、一度はとても人気があったものの、現在では少数の\ `ファン <http://groups.google.com/group/rec.arts.int-fiction/topics>`_\ のみによってプレイされている。

.. Such types are usually called 'classes' in other programming languages.

.. To keep things reasonably simple, the creatures in our terrarium reproduce asexually, all by themselves.

.. Laziness, for a programmer, is not necessarily a sin. The kind of people who will industriously do the same thing over and over again tend to make great assembly-line workers and lousy programmers.

.. rubric:: 脚注

.. [#f1] これらのタイプは他のプログラミング言語では通常'クラス'と呼ばれる。
.. [#f2] 物事を合理的に単純に保つため、飼育器の中の生物は全て無性生殖であることとする。
.. [#f3] 怠惰はプログラマーにとって必ずしも悪ではない。せっせと同じ事を何度も繰り返す種類の人々は偉大な組み立てライン工やひどいプログラマーになる。
