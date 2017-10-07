=====================================================
モジュール性
=====================================================
..  Chapter 9:
..  Modularity

..  This chapter deals with the process of organising programs. In small programs, organisation rarely becomes a problem. As a program grows, however, it can reach a size where its structure and interpretation become hard to keep track of. Easily enough, such a program starts to look like a bowl of spaghetti, an amorphous mass in which everything seems to be connected to everything else.

この章はプログラムを整理するプロセスを扱う。小さなプログラムでは、整理が問題になることはまれだ。プログラムが大きくなるにつれて、しかしながら、その構造と解釈が追跡しづらいサイズにまで達することがある。簡単に、そのようなプログラムは皿の中のスパゲッティのように見え始め、全てが他の全てに繋がった不定形の塊となる。

..  When structuring a program, we do two things. We separate it into smaller parts, called modules, each of which has a specific role, and we specify the relations between these parts.

プログラムを構造化するとき、2つのことを行う。それぞれが個別の役割を持つ、モジュールと呼ばれる、より小さい部品に分割すること、およびそれらの部品の間の関係を設定することである。

..  In chapter 8, while developing a terrarium, we made use of a number of functions described in chapter 6. The chapter also defined a few new concepts that had nothing in particular to do with terraria, such as clone and the Dictionary type. All these things were haphazardly added to the environment. One way to split this program into modules would be:

..  A module FunctionalTools, which contains the functions from chapter 6, and depends on nothing.
.. Then ObjectTools, which contains things like clone and create, and depends on FunctionalTools.
.. Dictionary, containing the dictionary type, and depending on FunctionalTools.
.. And finally the Terrarium module, which depends on ObjectTools and Dictionary.

:doc:`8章</Object-oriented Programming>`\ で、飼育器を開発するとき、\ :doc:`6章</Functional Programming>`\ で説明したいくつもの関数を使用した。8章では、cloneとかDictionary型のような、飼育器と何の関係も無いいくつかの新しい概念も定義した。これらの全ては行き当たりばったりに環境に追加された。このプログラムをモジュールに分割する1つの方法は：

* ``FunctionTools``\ モジュールという、\ :doc:`6章</Functional Programming>`\ の関数を含み、他に依存しないものを作る。
* それから\ ``ObjectTools``\ 、\ ``clone``\ や\ ``create``\ のようなものを含み、\ ``FunctionTools``\ に依存したもの。
* ``Dictionary``\ 、辞書型を含み、\ ``FunctionTools``\ に依存したもの。
* そして最後に\ ``Terrarium``\ モジュール、\ ``ObjectTools``\ と\ ``Dictionary``\ に依存したもの。

..  When a module depends on another module, it uses functions or variables from that module, and will only work when this module is loaded.

モジュールがもう1つのモジュールに依存するとき、そのモジュールからの関数や変数を使え、そしてこのモジュールがロードされたときのみ働く。

..  It is a good idea to make sure dependencies never form a circle. Not only do circular dependencies create a practical problem (if module A and B depend on each other, which one should be loaded first?), it also makes the relation between the modules less straightforward, and can result in a modularised version of the spaghetti I mentioned earlier.

依存が循環しないように確実にするのは良いアイデアだ。循環的な依存は実用上の問題（もしモジュールAとBが互いに依存し合っていたら、どちらを先にロードすれば良いだろうか？）であるだけでなく、モジュール間の関係もわかりにくくなり、モジュール化の結果も先ほどのスパゲッティのようになるかもしれない。

.. |hr| raw:: html

   <hr>

|hr|

..  Most modern programming languages have some kind of module system built in. Not JavaScript. Once again, we have to invent something ourselves. The most obvious way to start is to put every module in a different file. This makes it clear which code belongs to which module.

多くの現代的なプログラミングは何らかの種類の組み込みのモジュールシステムを持っている。JavaScriptはそうではない。もう一度、我々自身で何かを発明しなければならない。全てのモジュールを異なるファイルに入れることから始めるのがもっとも明白なやり方だ。これでどのコードがどのモジュールにあるかが明白になる。

..  Browsers load JavaScript files when they find a <script> tag with an src attribute in the HTML of the web-page. The extension .js is usually used for files containing JavaScript code. On the console, a shortcut for loading files is provided by the load function.

ブラウザは、\ ``src``\ 属性を持つ\ ``<script>``\ タグをウェブページのHTMLに見つけたときにJavaScriptファイルをロードする。通常は拡張子\ ``.js``\ がJavaScriptのコードを含むファイルに使われる。コンソール上では、ファイルのロードのショートカットが\ ``load``\ 関数によって提供されている。

.. code-block:: javascript

    load("FunctionalTools.js");

|hr|

..  In some cases, giving load commands in the wrong order will result in errors. If a module tries to create a Dictionary object, but the Dictionary module has not been loaded yet, it will be unable to find the constructor, and will fail.

いくつかの場合、間違った順番でloadコマンドを与えると結果がエラーになる。もしモジュールが\ ``Dictionary``\ オブジェクトを作ろうとしたが、\ ``Dictionary``\ モジュールがまだロードされていなかったら、コンストラクターを見つけることができず、作成に失敗する。

..  One would imagine this to be easy to solve. Just put some calls to load at the top of the file for a module, to load all the modules it depends on. Unfortunately, because of the way browsers work, calling load does not immediately cause the given file to be loaded. The file will be loaded after the current file has finished executing. Which is too late, usually.

ある者はこれを解決するのは簡単だと思うだろう。それが依存しているモジュールをファイルの冒頭で\ ``load``\ するようにするだけだ。残念ながら、ブラウザの動作により、\ ``load``\ の呼び出しはロードするように与えられたファイルの順番にはなされない。現在のファイルの実行が\ *終了するまで*\ ファイルのロードは実行されないのだ。それでは遅すぎる、通常は。

..  In most cases, the practical solution is to just manage dependencies by hand: Put the script tags in your HTML documents in the right order.

多くの場合で、実用的な解決は手作業で依存を管理することでしかない。HTML文書上の\ ``script``\ タグを正しい順番で並べるのだ。

|hr|

..  There are two ways to (partially) automate dependency management. The first is to keep a separate file with information about the dependencies between modules. This can be loaded first, and used to determine the order in which to load the files. The second way is to not use a script tag (load internally creates and adds such a tag), but to fetch the content of the file directly (see chapter 14), and then use the eval function to execute it. This makes script loading instantaneous, and thus easier to deal with.

（部分的に）自動的な依存管理を行う2つの方法がある。1つはモジュール間の依存に関する情報をファイルに分割しておくことだ。これは最初にロードされ、ファイルがロードされる順番を明示するのに使われる。2つめの手段は\ ``script``\ タグを使わず（\ ``load``\ は内部的にそのようなタグを作って追加する）、ファイルのディレクトリ（\ :doc:`14章</HTTP requests>`\ を参照）の内容を読みとって、それから\ ``eval``\ 関数を使って実行する。これでスクリプトが即座にロードされるので、取り扱うのはより簡単になる。

..  eval, short for 'evaluate', is an interesting function. You give it a string value, and it will execute the content of the string as JavaScript code.

``eval``\ は'evaluate'の短縮形で、面白い関数だ。文字列の値を与えると、その文字列の中身をJavaScriptのコードとみなして実行するのだ。

.. code-block:: javascript

    eval("print(\"I am a string inside a string!\");");

..  You can imagine that eval can be used to do some interesting things. Code can build new code, and run it. In most cases, however, problems that can be solved with creative uses of eval can also be solved with creative uses of anonymous functions, and the latter is less likely to cause strange problems.

``eval``\ は面白いことに使えると想像できるだろう。コードによってコードを組み立て、実行することができる。多くの場合では、しかしながら、\ ``eval``\ の創造的な使用で解決できる問題は無名関数の創造的な使用によっても解決することができ、そして後者の方がおかしな問題を引き起こすようなことが少ない。

..  When eval is called inside a function, all new variables will become local to that function. Thus, when a variation of the load would use eval internally, loading the Dictionary module would create a Dictionary constructor inside of the load function, which would be lost as soon as the function returned. There are ways to work around this, but they are rather clumsy.

``eval``\ が関数の中で呼び出されたとき、全ての新しい変数がその関数ローカルなものとなる。それで、\ ``load``\ のバリエーションが\ ``eval``\ を内部で使用したとき、\ ``Dictionary``\ モジュールのロードによって、\ ``load``\ 関数の中で\ ``Dictionary``\ コンストラクターが作られ、関数から返ったときには失われてしまう。これを動かす方法はあるが、むしろ余計に不細工なものとなる。

|hr|

..  Let us quickly go over the first variant of dependency management. It requires a special file for dependency information, which could look something like this:

依存を管理する1つめの方法をざっと見てみよう。依存の情報のために特別なファイルを必要とし、それはこのようなものになる。：

.. code-block:: javascript

    var dependencies =
      {"ObjectTools.js": ["FunctionalTools.js"],
       "Dictionary.js":  ["ObjectTools.js"],
       "TestModule.js":  ["FunctionalTools.js", "Dictionary.js"]};

..  The dependencies object contains a property for each file that depends on other files. The values of the properties are arrays of file names. Note that we could not use a Dictionary object here, because we can not be sure that the Dictionary module has been loaded yet. Because all the properties in this object will end in ".js", they are unlikely to interfere with hidden properties like __proto__ or hasOwnProperty, and a regular object will work fine.

``dependencies``\ オブジェクトは、他のファイルに依存しているそれぞれのファイルをプロパティに持つ。プロパティの値はファイル名の配列である。ここで\ ``Dictionary``\ オブジェクトを使えないことに注意しよう、なぜなら\ ``Dictionary``\ モジュールはまだロードされていないからである。このオブジェクトの全てのプロパティは\ ``".js"``\ で終わっているため、\ ``__proto__``\ や\ ``hasOwnProperty``\ のような隠されたプロパティに邪魔されるようなこともなく、正規のオブジェクトは正しく動くだろう。

..  The dependency manager must do two things. Firstly it must make sure that files are loaded in the correct order, by loading a file's dependencies before the file itself. And secondly, it must make sure that no file is loaded twice. Loading the same file twice might cause problems, and is definitely a waste of time.

依存性マネージャーは2つのことをなさねばならない。1つめは、ロードされるファイルが依存しているファイルをそれ自身より先にロードすることにより、ファイルが正しい順序でロードされるのを確実にすることである。そして2つめは、同じファイルを2回ロードしないことである。同じファイルのロードは問題を引き起こすかも知れず、また明らかに時間の浪費である。

.. code-block:: javascript

    var loadedFiles = {};

    function require(file) {
      if (dependencies[file]) {
        var files = dependencies[file];
        for (var i = 0; i < files.length; i++)
          require(files[i]);
      }
      if (!loadedFiles[file]) {
        loadedFiles[file] = true;
        load(file);
      }
    }

..  The require function can now be used to load a file and all its dependencies. Note how it recursively calls itself to take care of dependencies (and possible dependencies of that dependency).

``require``\ 関数はファイルとそれが依存しているものをロードするのに使われる。依存（そして依存の依存の可能性）を扱うための再帰の呼び出し方に注意しよう。

.. code-block:: javascript

    require("TestModule.js");
    test();

|hr|

..  Building a program as a set of nice, small modules often means the program will use a lot of different files. When programming for the web, having lots of small JavaScript files on a page tends to make the page slower to load. This does not have to be a problem though. You can write and test your program as a number of small files, and put them all into a single big file when 'publishing' the program to the web.

良い、小さいモジュールのセットとしてプログラムを組み立てることは、プログラムが多くの異なるファイルを使うことをしばしば意味する。ウェブのプログラミングをするとき、1つのページにたくさんの、小さいJavaScriptファイルがあることはページのロードを遅くする。それでもこれは問題にはならない。数多くの小さいファイルとしてプログラムを書きテストでき、その上でウェブに公開するときに単一の大きなファイルにまとめれば良いのである。

|hr|

..  Just like an object type, a module has an interface. In simple collection-of-functions modules such as FunctionalTools, the interface usually consists of all the functions that are defined in the module. In other cases, the interface of the module is only a small part of the functions defined inside it. For example, our manuscript-to-HTML system from chapter 6 only needs an interface of a single function, renderFile. (The sub-system for building HTML would be a separate module.)

オブジェクト型のように、モジュールもインターフェースを持つ。\ ``FunctionTools``\ のような単純な関数のコレクションのモジュールでは、インターフェースは通常、モジュールで定義されている全ての関数から成り立つ。他の場合では、モジュールのインターフェースは中で定義されている関数のほんの一部のみである。例えば、\ :doc:`6章</Functional Programming>`\ の文書HTML化システムは一つの関数、\ ``renderFile``\ のインターフェースのみ必要とする。（HTML組み立てのサブシステムは別のモジュールに分割される。）

..  For modules which only define a single type of object, such as Dictionary, the object's interface is the same as the module's interface.

``Dictionary``\ 型のように、単一の型のオブジェクトしか定義しないモジュールでは、オブジェクトのインターフェースはモジュールのインターフェースと同一である。

|hr|

..  In JavaScript, 'top-level' variables all live together in a single place. In browsers, this place is an object that can be found under the name window. The name is somewhat odd, environment or top would have made more sense, but since browsers associate a JavaScript environment with a window (or 'frame'), someone decided that window was a logical name.

JavaScriptでは、'トップレベル'の変数は1つの場所でともに生きている。ブラウザでは、この場所は\ ``window``\ という名前のもとのオブジェクトである。この名前は奇妙であり、\ ``environment``\ や\ ``top``\ の方がより理にかなっていただろう、しかしブラウザがJavaScript環境をウインドウ（または'フレーム'）に結びつけてしまったため、誰かが\ ``window``\ が論理的な名前であると決定してしまった。

.. code-block:: javascript

    show(window);
    show(window.print == print);
    show(window.window.window.window.window);

..  As the third line shows, the name window is merely a property of this environment object, pointing at itself.

3行目は、\ ``window``\ という名前は、ただこの環境オブジェクトのプロパティであり、それ自体を指している、ということを示している。

|hr|

..  When much code is loaded into an environment, it will use many top-level variable names. Once there is more code than you can really keep track of, it becomes very easy to accidentally use a name that was already used for something else. This will break the code that used the original value. The proliferation of top-level variables is called name-space pollution, and it can be a rather severe problem in JavaScript ― the language will not warn you when you redefine an existing variable.

多くのコードが1つの環境にロードされると、トップレベルの変数名が多く使われる。一度自分が本当に把握できる以上にコードが大きくなると、他のところで既に使われている名前を誤って使ってしまうことが簡単に起こる。これは元のコードが持っていた値を破壊する。トップレベルの変数の増加は名前空間の汚染と呼ばれ、JavaScriptではむしろシビアな問題だ。 ― 存在している変数を再定義しても言語は警告を発しないのだ。

..  There is no way to get rid of this problem entirely, but it can be greatly reduced by taking care to cause as little pollution as possible. For one thing, modules should not use top-level variables for values that are not part of their external interface.

この問題を完全に解決する手段は無いが、可能な限り汚染が引き起こされる範囲を狭めることはできる。1つには、モジュールは外部へのインターフェースの一部でない値にはトップレベルの変数を使わないのだ。

|hr|

..  Not being able to define any internal functions and variables at all in your modules is, of course, not very practical. Fortunately, there is a trick to get around this. We write all the code for the module inside a function, and then finally add the variables that are part of the module's interface to the window object. Because they were created in the same parent function, all the functions of the module can see each other, but code outside of the module can not.

モジュールの中の全てで、内部の関数と変数を定義できないことは、もちろん、あまり実用的では無い。幸運にも、これに関するトリックが存在する。モジュールの全てのコードを関数の中に書くことができ、それから最終的に\ ``window``\ オブジェクトに、モジュールのインターフェースの一部である変数を追加するのである。同一の親関数の中で作られているため、モジュールの全ての関数は互いを参照する事ができ、しかし、モジュールの外には参照させないということができる。

.. code-block:: javascript

    function buildMonthNameModule() {
      var names = ["January", "February", "March", "April",
                   "May", "June", "July", "August", "September",
                   "October", "November", "December"];
      function getMonthName(number) {
        return names[number];
      }
      function getMonthNumber(name) {
        for (var number = 0; number < names.length; number++) {
          if (names[number] == name)
            return number;
        }
      }

      window.getMonthName = getMonthName;
      window.getMonthNumber = getMonthNumber;
    }
    buildMonthNameModule();

    show(getMonthName(11));

..  This builds a very simple module for translating between month names and their number (as used by Date, where January is 0). But note that buildMonthNameModule is still a top-level variable that is not part of the module's interface. Also, we have to repeat the names of the interface functions three times. Ugh.

これは、月の名前とその数（\ ``Date``\ においてと同様、1月は\ ``0``\ である）を変換するとても単純なモジュールだ。しかし\ ``buikdMonthNameModule``\ はまだモジュールのインターフェースの一部でないトップレベル変数であることに注意しよう。また、インターフェース関数の名前を3回も繰り返している。ぐふっ。

|hr|

..  The first problem can be solved by making the module function anonymous, and calling it directly. To do this, we have to add a pair of parentheses around the function value, or JavaScript will think it is a normal function definition, which can not be called directly.

1つめの問題はモジュール関数を無名で作り、直接呼び出すことで解決できる。これを行うため、関数の値を1組の括弧で囲んで、JavaScriptがそれを直接呼び出すことのできない正常の関数定義だと考えるようにしよう。

..  The second problem can be solved with a helper function, provide, which can be given an object containing the values that must be exported into the window object.

2つめの問題は\ ``provide``\ というヘルパー関数で解決できる。\ ``window``\ オブジェクトにexportしなければならない値を含むオブジェクトをそれに与える。

.. code-block:: javascript

    function provide(values) {
      forEachIn(values, function(name, value) {
        window[name] = value;
      });
    }

..  Using this, we can write a module like this:

これを使って、モジュールをこのように書く事ができる。：

.. code-block:: javascript

    (function() {
      var names = ["Sunday", "Monday", "Tuesday", "Wednesday",
                   "Thursday", "Friday", "Saturday"];
      provide({
        getDayName: function(number) {
          return names[number];
        },
        getDayNumber: function(name) {
          for (var number = 0; number < names.length; number++) {
            if (names[number] == name)
              return number;
          }
        }
      });
    })();

    show(getDayNumber("Wednesday"));

..  I do not recommend writing modules like this right from the start. While you are still working on a piece of code, it is easier to just use the simple approach we have used so far, and put everything at top level. That way, you can inspect the module's internal values in your browser, and test them out. Once a module is more or less finished, it is not difficult to wrap it in a function.

最初からこのように正しいモジュールを書くことを私はおすすめしない。コードの部品の作業をやっている間は、全てをトップレベルに置く、これまでのような単純なアプローチをただ使う方が簡単だ。その方法で、ブラウザでモジュール内部の値を見る事ができ、テストできる。一度モジュールが完成すれば、それを関数でラップする事は難しくない。

|hr|

..  There are cases where a module will export so many variables that it is a bad idea to put them all into the top-level environment. In cases like this, you can do what the standard Math object does, and represent the module as a single object whose properties are the functions and values it exports. For example...

モジュールが多くの変数をエクスポートするだろう時に、全てをトップレベルの環境に置く事が良くないアイデアである場合もある。このようなケースの場合、標準の\ ``Math``\ オブジェクトがやるように、モジュールをプロパティが関数とそれがエクスポートする値である単一のオブジェクトとして表現することができる。例えば...

.. code-block:: javascript

    var HTML = {
      tag: function(name, content, properties) {
        return {name: name, properties: properties, content: content};
      },
      link: function(target, text) {
        return HTML.tag("a", [text], {href: target});
      }
      /* ... many more HTML-producing functions ... */
    };

..  When you need the content of such a module so often that it becomes cumbersome to constantly type HTML, you can always move it into the top-level environment using provide.

そのようなモジュールの内容が必要なときに、絶えず\ ``HTML``\ とタイプすることはしばしば面倒であるので、\ ``provide``\ を使っていつでもトップレベルの環境に移動することができる。

.. code-block:: javascript

    provide(HTML);
    show(link("http://docs.sun.com/source/816-6408-10/object.htm",
              "This is how objects work."));

..  You can even combine the function and object approaches, by putting the internal variables of the module inside a function, and having this function return an object containing its external interface.

モジュールの内部の変数を関数の中に入れ、そしてこの関数がその外部へのインターフェースを含むオブジェクト返すことで、関数とオブジェクトのアプローチを連携することができる。

|hr|

..  When adding methods to standard prototypes, such as those of Array and Object a similar problem to name-space pollution occurs. If two modules decide to add a map method to Array.prototype, you might have a problem. If these two versions of map have the precise same effect, things will continue to work, but only by sheer luck.

``Array``\ や\ ``Object``\ のような標準のプロトタイプにメソッドを追加したとき、名前空間の汚染と同様の問題が起きる。もし2つのモジュールが\ ``Array.prototype``\ に\ ``map``\ メソッドを追加しようと判断したら、問題を抱えることになる。もしこれら2つのバージョンの\ ``map``\ が正確に同様の効果を持っていたら、物事は働き続けるだろうが、それは全くの幸運によるものだ。

|hr|

..  Designing an interface for a module or an object type is one of the subtler aspects of programming. On the one hand, you do not want to expose too many details. They will only get in the way when using the module. On the other hand, you do not want to be too simple and general, because that might make it impossible to use the module in complex or specialised situations.

モジュールやオブジェクト型のインターフェースの設計はプログラミングの微妙な面の1つだ。一方では、あまり多くの詳細を晒そうとは思わないだろう。モジュールを使う時だけその手段を得られる。そのもう一方で、あまり単純で一般的に\ *しすぎたく無い*\ とも思う。なぜならそれによってモジュールを、複雑な、あるいは特別な状況で使うのが不可能になるかもしれないからだ。

..  Sometimes the solution is to provide two interfaces, a detailed 'low-level' one for complicated things, and a simple 'high-level' one for straightforward situations. The second one can usually be built very easily using the tools provided by the first one.

時折、その解決は、複雑な物事のための詳細な'低レベル'のもの、およびまっすぐな状況のための単純な'高レベル'のもの2つのインターフェースを提供することである。2つめのものは、通常、1つめのものによって提供されたツールを使って簡単に組み立てることができる。

..  In other cases, you just have to find the right idea around which to base your interface. Compare this to the various approaches to inheritance we saw in chapter 8. By making prototypes the central concept, rather than constructors, we managed to make some things considerably more straightforward.

他の場合においては、インターフェースをベースに正しいアイデアを探すしかない。これと、\ :doc:`8章</Object-oriented Programming>`\ で見たいろいろな継承のアプローチを比較しよう。むしろコンストラクターより、プロトタイプを中心の概念とすることによって、物事をかなり素直に作ることができる。

..  The best way to learn to the value of good interface design is, unfortunately, to use bad interfaces. Once you get fed up with them, you'll figure out a way to improve them, and learn a lot in the process. Try not to assume that a lousy interface is 'just the way it is'. Fix it, or wrap it in a new interface that is better (we will see an example of this in chapter 12).

良いインターフェース設計の価値を学ぶ最善の手は、残念ながら、悪いインターフェースを使うことだ。一度これらを与えられたら、それを改善する手を考え出し、その過程で多くを学ぶだろう。質の悪いインターフェースを'こうするしかない'ものであると仮定しないこと。それを直すか、あるいは新しいインターフェースでラップする方が良い。（\ :doc:`12章</The Document-Object Model>`\ でその例を見よう）

|hr|

..  There are functions which require a lot of arguments. Sometimes this means they are just badly designed, and can easily be remedied by splitting them into a few more modest functions. But in other cases, there is no way around it. Typically, some of these arguments have a sensible 'default' value. We could, for example, write yet another extended version of range.

多くの引数を必要とする関数がある。時々これはただ悪いデザインであることを意味し、複数の適度な関数に分割することで簡単に改善できる。しかし、他の場合においては、そのような手段が無い。典型的には、これらの引数のいくつかはふさわしい'デフォルトの'値を持っている。例えば、\ ``range``\ のまた別の拡張版を書くことができる。

.. code-block:: javascript

    function range(start, end, stepSize, length) {
      if (stepSize == undefined)
        stepSize = 1;
      if (end == undefined)
        end = start + stepSize * (length - 1);

      var result = [];
      for (; start <= end; start += stepSize)
        result.push(start);
      return result;
    }

    show(range(0, undefined, 4, 5));

..  It can get hard to remember which argument goes where, not to mention the annoyance of having to pass undefined as a second argument when a length argument is used. We can make passing arguments to this function more comprehensive by wrapping them in an object.

``length``\ 引数が使われるとき、2つめの引数として\ ``undefined``\ を渡すことの面倒さに言及することなく、その引数が何を示すか覚えておくのは難しいだろう。それらを1つのオブジェクトにラップすることで、この関数へ渡す引数をより包括的なものにすることができる。

.. code-block:: javascript

    function defaultTo(object, values) {
      forEachIn(values, function(name, value) {
        if (!object.hasOwnProperty(name))
          object[name] = value;
      });
    }

    function range(args) {
      defaultTo(args, {start: 0, stepSize: 1});
      if (args.end == undefined)
        args.end = args.start + args.stepSize * (args.length - 1);

      var result = [];
      for (; args.start <= args.end; args.start += args.stepSize)
        result.push(args.start);
      return result;
    }

    show(range({stepSize: 4, length: 5}));

..  The defaultTo function is useful for adding default values to an object. It copies the properties of its second argument into its first argument, skipping those that already have a value.

``defaultTo``\ 関数はデフォルト値をオブジェクトに追加するのに有益だ。その1つめの引数に2つめの引数のプロパティをコピーし、既にその値が存在しているときはスキップする。

|hr|

..  A module or group of modules that can be useful in more than one program is usually called a library. For many programming languages, there is a huge set of quality libraries available. This means programmers do not have to start from scratch all the time, which can make them a lot more productive. For JavaScript, unfortunately, the amount of available libraries is not very large.

1本より多くのプログラムにとって有益になり得るモジュールまたはモジュールのグループは、通常ライブラリと呼ばれる。多くのプログラミング言語では、良質のライブラリの巨大なセットが利用可能である。これはプログラマーがいつも一から作るところから始めなくても良く、その方がより生産的だということを意味する。JavaScriptでは、残念ながら、利用可能なライブラリの数はあまり大きくない。

..  But recently this seems to be improving. There are a number of good libraries with 'basic' tools, things like map and clone. Other languages tend to provide such obviously useful things as built-in standard features, but with JavaScript you'll have to either build a collection of them for yourself or use a library. Using a library is recommended: It is less work, and the code in a library has usually been tested more thoroughly than the things you wrote yourself.

しかしこれは最近改善されているようだ。\ ``map``\ や\ ``clone``\ のような'基本的な'ツールの良いライブラリが数を多くある。他の言語では、このように明らかに有益なものは組み込みの標準として提供される傾向にあるが、JavaScriptではこれらのコレクションを自分自身で組むか、ライブラリを使わねばならない。ライブラリを使うことを勧める：楽だし、ライブラリのコードは通常、自分で書いたものより多くのテストを受けている。

..  Covering these basics, there are (among others) the 'lightweight' libraries prototype, mootools, jQuery, and MochiKit. There are also some larger 'frameworks' available, which do a lot more than just provide a set of basic tools. YUI (by Yahoo), and Dojo seem to be the most popular ones in that genre. All of these can be downloaded and used free of charge. My personal favourite is MochiKit, but this is mostly a matter of taste. When you get serious about JavaScript programming, it is a good idea to quickly glance through the documentation of each of these, to get a general idea about the way they work and the things they provide.

これらの基本をカバーし、（他に比べて）'ライトウェイトな'ライブラリがある。\ `prototype <http://www.prototypejs.org/>`_\ 、\ `mootools <http://mootools.net/>`_\ 、\ `jQuery <http://jquery.com/>`_\ 、そして\ `MochiKit <http://mochikit.com/>`_\ 。より大きな'フレームワーク'も利用可能であり、基本的なツールの集合を提供する以上のことをしてくれる。\ `YUI <http://developer.yahoo.com/yui/>`_\ (Yahoo製)、\ `Dojo <http://dojotoolkit.org/>`_\ がそのジャンルの最も人気のあるもののようだ。これらの全てがダウンロード可能であり、金銭の支払なしに使うことができる。私の個人的なお気に入りはMochikitだが、しかしこれは好みの問題だ。本格的なJavaScriptプログラミングに取りかかるときに、これらそれぞれの文書をざっと眺めておき、それがどのように働くかの全般的なアイデアとそれが提供するものを見ておくと良いだろう。

..  The fact that a basic toolkit is almost indispensable for any non-trivial JavaScript programs, combined with the fact that there are so many different toolkits, causes a bit of a dilemma for library writers. You either have to make your library depend on one of the toolkits, or write the basic tools yourself and include them with the library. The first option makes the library hard to use for people who are using a different toolkit, and the second option adds a lot of non-essential code to the library. This dilemma might be one of the reasons why there are relatively few good, widely used JavaScript libraries. It is possible that, in the future, new versions of ECMAScript and changes in browsers will make toolkits less necessary, and thus (partially) solve this problem.

事実上、基本的な道具箱は、平凡で無いJavaScriptプログラムのためにほとん必須のものとなっており、多くの異なる道具箱を組み合わせることは、ライブラリの作成者にちょっとしたジレンマをもたらしている。ライブラリを1つの道具箱に依存するようにするか、基本的な道具は自分で書いてライブラリに入れるかしなければならない。1つめのオプションはライブラリを異なる道具箱を使う人々にとって使いにくいものにするし、2つめのオプションでは本質的でないたくさんのコードがライブラリに追加される。このジレンマが似たような幅広く使われるJavaScriptライブラリが存在する理由の一つになっているかもしれない。可能性としては、将来的に、ECMAScriptの新しいバージョンとブラウザの変更が、道具箱の必要性を減らし、それで（部分的には）この問題が解決されるかもしれない。

