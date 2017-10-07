=====================================================
正規表現
=====================================================
..  Chapter 10:
..  Regular Expressions

..  At various points in the previous chapters, we had to look for patterns in string values. In chapter 4 we extracted date values from strings by writing out the precise positions at which the numbers that were part of the date could be found. Later, in chapter 6, we saw some particularly ugly pieces of code for finding certain types of characters in a string, for example the characters that had to be escaped in HTML output.

今までの章のさまざまな場で、文字列の値の中からパターンを探さねばならなかった。\ :doc:`4章</Data structures>`\ では日付の部分であるべき数字の正確な位置を書きだすことで文字列から日付を展開した。その後、\ :doc:`6章</Functional Programming>`\ では文字列の中の決まった文字、例えばHTMLの出力でエスケープされるべき文字を見つけるとりわけ醜いコードの部品を見た。

..  Regular expressions are a language for describing patterns in strings. They form a small, separate language, which is embedded inside JavaScript (and in various other programming languages, in one way or another). It is not a very readable language ― big regular expressions tend to be completely unreadable. But, it is a useful tool, and can really simplify string-processing programs.

正規表現は文字列の中のパターンを記述する言語である。小さな形の、分割された言語として、JavaScript（および他の様々なプログラミング言語で、1つないしそれ以上の方法で）に組み込まれている。あまり読みやすい言語ではない--大きな正規表現は全く読めないものになっていく。しかし、有益なツールであり、文字列処理のプログラムを本当に単純化できる。

.. |hr| raw:: html

   <hr>

|hr|

..  Just like strings get written between quotes, regular expression patterns get written between slashes (/). This means that slashes inside the expression have to be escaped.

ちょうど文字列が引用符の間に書かれるように、正規表現のパターンはスラッシュ(\ ``/``\ )の間に書かれる。これは正規表現の中のスラッシュはエスケープされなければならないということを意味する。

.. code-block:: javascript

    var slash = /\//;
    show("AC/DC".search(slash));

..  The search method resembles indexOf, but it searches for a regular expression instead of a string. Patterns specified by regular expressions can do a few things that strings can not do. For a start, they allow some of their elements to match more than a single character. In chapter 6, when extracting mark-up from a document, we needed to find the first asterisk or opening brace in a string. That could be done like this:

``search``\ メソッドは\ ``indexOf``\ に似ているが、しかし、こちらは文字列の代わりに正規表現を探す。正規表現で指定されたパターンは文字列ではできないいくつかのことができる。初めに、単一の文字より多くの要素にマッチさせることができる。\ :doc:`6章</Functional Programming>`\ で、文書のマークアップを展開するときに、最初のアスタリスクか左中括弧を探す必要があった。このようなものだった。：

.. code-block:: javascript

    var asteriskOrBrace = /[\{\*]/;
    var story =
      "We noticed the *giant sloth*, hanging from a giant branch.";
    show(story.search(asteriskOrBrace));

..  The [ and ] characters have a special meaning inside a regular expression. They can enclose a set of characters, and they mean 'any of these characters'. Most non-alphanumeric characters have some special meaning inside a regular expression, so it is a good idea to always escape them with a backslash1 when you use them to refer to the actual characters.

正規表現の中の\ ``[``\ と\ ``]``\ 文字は特別な意味を持つ。文字列の集合を納めることができ、'これらの文字の中のいずれか'という意味になる。英数字でない多くの文字列は正規表現の中で特別な意味を持ち、実際の文字としてそれらを参照するときに、常にそれらをバックスラッシュでエスケープするのは良いアイデアである。

|hr|

..  There are a few shortcuts for sets of characters that are needed often. The dot (.) can be used to mean 'any character that is not a newline', an escaped 'd' (\d) means 'any digit', an escaped 'w' (\w) matches any alphanumeric character (including underscores, for some reason), and an escaped 's' (\s) matches any white-space (tab, newline, space) character.

しばしば必要になる文字列のセットにはいくつかのショートカットがある。ピリオド(\ ``.``\ )は'改行以外の任意の文字'の意味に使われ、エスケープされた'd'(\ ``\d``\ )は'任意の数字'の意味、エスケープされた'w'(\ ``\w``\ )は任意の英数字（ある理由によりアンダースコアを含む）にマッチし、エスケープされた's'(\ ``\s``\ )は任意の空白文字（タブ、改行、空白）にマッチする。

.. code-block:: javascript

    var digitSurroundedBySpace = /\s\d\s/;
    show("1a 2 3d".search(digitSurroundedBySpace));

..  The escaped 'd', 'w', and 's' can be replaced by their capital letter to mean their opposite. For example, \S matches any character that is not white-space. When using [ and ], a pattern can be inverted by starting with a ^ character:

エスケープされた'd'、'w'、および's'は反対の意味にするために大文字で置き換えることができる。例えば、\ ``\S``\ は空白\ *以外*\ の任意の文字にマッチする。\ ``[``\ と\ ``]``\ を使うとき、\ ``^``\ 文字から始めることでパターンを反転することができる。：

.. code-block:: javascript

    var notABC = /[^ABC]/;
    show("ABCBACCBBADABC".search(notABC));

..  As you can see, the way regular expressions use characters to express patterns makes them A) very short, and B) very hard to read.

見ての通り、パターンを表現するための、正規表現の文字の使い方は、A) とても短く、B) とても読みにくい。

|hr|

演習 10.1

..  Write a regular expression that matches a date in the format "XX/XX/XXXX", where the Xs are digits. Test it against the string "born 15/11/2003 (mother Spot): White Fang".

``"XX/XX/XXXX"``\ の形式の日付にマッチする正規表現を書け。\ ``X``\ の場所は数字である。文字列\ ``"born 15/11/2003 (mother Spot): White Fang"``\ でそれをテストせよ。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    var datePattern = /\d\d\/\d\d\/\d\d\d\d/;
    show("born 15/11/2003 (mother Spot): White Fang".search(datePattern));

|hr|

..  Sometimes you need to make sure a pattern starts at the beginning of a string, or ends at its end. For this, the special characters ^ and $ can be used. The first matches the start of the string, the second the end.

ときどき、確実に文字列の最初でパターンが始まること、または文字列の終わりでパターンが終わることが必要となる。このために、\ ``^``\ と\ ``$``\ という特別な文字がつかわれる。1つ目は文字列の最初にマッチし、2つ目は最後にマッチする。

.. code-block:: javascript

    show(/a+/.test("blah"));
    show(/^a+$/.test("blah"));

..  The first regular expression matches any string that contains an a character, the second only those strings that consist entirely of a characters.

最初の正規表現は1つの\ ``a``\ を含む任意の文字列にマッチし、2つ目はその文字列の全てが\ ``a``\ であるときのみマッチする。

..  Note that regular expressions are objects, and have methods. Their test method returns a boolean indicating whether the given string matches the expression.

正規表現はオブジェクトであり、メソッドを持つことに注意。これらの\ ``test``\ メソッドは与えられた文字列が正規表現にマッチしたか否かの真偽値を返す。

..  The code \b matches a 'word boundary', which can be punctuation, white-space, or the start or end of the string.

``\b``\ のコードは'単語の境界'にマッチし、それは句読点、空白、文字列の最初と最後である。

.. code-block:: javascript

    show(/cat/.test("concatenate"));
    show(/\bcat\b/.test("concatenate"));

|hr|

..  Parts of a pattern can be allowed to be repeated a number of times. Putting an asterisk (*) after an element allows it to be repeated any number of times, including zero. A plus (+) does the same, but requires the pattern to occur at least one time. A question mark (?) makes an element 'optional' ― it can occur zero or one times.

パターンの部分は何回も繰り返すことができる。要素の後ろのアスタリスク(\ ``*``\ )は、0回を含む任意の回数のそれの繰り返しを許す。プラス(\ ``+``\ )も同様だが、しかし最低1回はパターンがなければならない。疑問符(\ ``?``\ )は要素が'任意'であることを示し ― それは0回または1回である。

.. code-block:: javascript

    var parenthesizedText = /\(.*\)/;
    show("Its (the sloth's) claws were gigantic!".search(parenthesizedText));

..  When necessary, braces can be used to be more precise about the amount of times an element may occur. A number between braces ({4}) gives the exact amount of times it must occur. Two numbers with a comma between them ({3,10}) indicate that the pattern must occur at least as often as the first number, and at most as often as the second one. Similarly, {2,} means two or more occurrences, while {,4} means four or less.

必要なら、中括弧で要素の繰り返し回数をより正確にすることができる。中括弧の間の数値(\ ``{4}``\ )は繰り返しの回数を与える。それらがカンマで区切られた2つの数値(\ ``{3,10}``\ )であれば1つめが最低の回数を、2つめが最大の回数を示す。同様に、\ ``{2,}``\ は2回かそれ以上、\ ``{,4}``\ は4回かそれ以下を意味する。

.. code-block:: javascript

    var datePattern = /\d{1,2}\/\d\d?\/\d{4}/;
    show("born 15/11/2003 (mother Spot): White Fang".search(datePattern));

..  The pieces /\d{1,2}/ and /\d\d?/ both express 'one or two digits'.

``/\d{1,2}/``\ と\ ``/\d\d?/``\ の部分はともに1桁または2桁の数字である。

|hr|

演習 10.2

..  Write a pattern that matches e-mail addresses. For simplicity, assume that the parts before and after the @ can contain only alphanumeric characters and the characters . and - (dot and dash), while the last part of the address, the country code after the last dot, may only contain alphanumeric characters, and must be two or three characters long.

電子メールのアドレスにマッチするパターンを書け。単純には、\ ``@``\ の前と後ろは英数字と\ ``.``\ と\ ``-``\ （ピリオドとダッシュ）であると仮定し、アドレスの最後の部分は最後のピリオドの後に国のコードがあって、英数字のみ含み、2文字ないし3文字でなければならないとする。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    var mailAddress = /\b[\w\.-]+@[\w\.-]+\.\w{2,3}\b/;

    show(mailAddress.test("kenny@test.net"));
    show(mailAddress.test("I mailt kenny@tets.nets, but it didn wrok!"));
    show(mailAddress.test("the_giant_sloth@gmail.com"));

..  The \bs at the start and end of the pattern make sure that the second string does not match.

パターンの最初と最後の\ ``\b``\ は2つめの文字列が確実にマッチしないようにするためのものだ。

|hr|

..  Part of a regular expression can be grouped together with parentheses. This allows us to use * and such on more than one character. For example:

正規表現の部分は括弧でグループ化することができる。これで\ ``*``\ のようなものを1文字より多いパターンに使うことができる。例えば：

.. code-block:: javascript

    var cartoonCrying = /boo(hoo+)+/i;
    show("Then, he exclaimed 'Boohoooohoohooo'".search(cartoonCrying));

..  Where did the i at the end of that regular expression come from? After the closing slash, 'options' may be added to a regular expression. An i, here, means the expression is case-insensitive, which allows the lower-case B in the pattern to match the upper-case one in the string.

正規表現の最後の\ ``i``\ はどこから来たものだろうか？閉じるスラッシュの後に、正規表現の'オプション'を追加できる。この\ ``i``\ は大文字小文字を区別しないという意味で、パターン中の小文字のBを文字列中の大文字のBにマッチさせることができる。

..  A pipe character (|) is used to allow a pattern to make a choice between two elements. For example:

パイプ文字(\ ``|``\ )はパターンを2つの要素のどちらかの選択とする。例えば：

.. code-block:: javascript

    var holyCow = /(sacred|holy) (cow|bovine|bull|taurus)/i;
    show(holyCow.test("Sacred bovine!"));

|hr|

..  Often, looking for a pattern is just a first step in extracting something from a string. In previous chapters, this extraction was done by calling a string's indexOf and slice methods a lot. Now that we are aware of the existence of regular expressions, we can use the match method instead. When a string is matched against a regular expression, the result will be null if the match failed, or an array of matched strings if it succeeded.

時折、パターンを探すことは文字列から何かを抽出するためのただ1つめのステップでしかないことがある。以前の章では、その抽出を文字列の\ ``indexOf``\ や\ ``slice``\ メソッドを呼び出すことで行ってきた。今、我々は正規表現を知ったので、代わりに\ ``match``\ メソッドを使うことができる。文字列が正規表現にマッチさせたとき、マッチに失敗したときは\ ``null``\ が、成功したときはマッチした文字列の配列が結果となる。

.. code-block:: javascript

    show("No".match(/Yes/));
    show("... yes".match(/yes/));
    show("Giant Ape".match(/giant (\w+)/i));

..  The first element in the returned array is always the part of the string that matched the pattern. As the last example shows, when there are parenthesized parts in the pattern, the parts they match are also added to the array. Often, this makes extracting pieces of string very easy.

返される配列の最初の要素は常にパターンにマッチした文字列の部分である。最後の例で見れるように、パターンの中に括弧で囲われた部分があるとき、そこにマッチした部分も配列に追加される。しばしば、これで部品の抽出がとても簡単になる。

.. code-block:: javascript

    var parenthesized = prompt("Tell me something", "").match(/\((.*)\)/);
    if (parenthesized != null)
      print("You parenthesized '", parenthesized[1], "'");

|hr|

演習 10.3

..  Re-write the function extractDate that we wrote in chapter 4. When given a string, this function looks for something that follows the date format we saw earlier. If it can find such a date, it puts the values into a Date object. Otherwise, it throws an exception. Make it accept dates in which the day or month are written with only one digit.

:doc:`4章</Data structures>`\ で書いた\ ``extractDate``\ 関数を書き直せ。文字列を与えられたとき、この関数は先に見た日付のフォーマットに従った部分を文字列から探す。もし日付のようなものが見つかったら、\ ``Date``\ オブジェクトにその値を入れる。そうでなければ、例外を投げる。日または月が1桁だけであっても受け入れるようにすること。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function extractDate(string) {
      var found = string.match(/(\d\d?)\/(\d\d?)\/(\d{4})/);
      if (found == null)
        throw new Error("No date found in '" + string + "'.");
      return new Date(Number(found[3]), Number(found[2]) - 1,
                      Number(found[1]));
    }

    show(extractDate("born 5/2/2007 (mother Noog): Long-ear Johnson"));

..  This version is slightly longer than the previous one, but it has the advantage of actually checking what it is doing, and shouting out when it is given nonsensical input. This was a lot harder without regular expressions ― it would have taken a lot of calls to indexOf to find out whether the numbers had one or two digits, and whether the dashes were in the right places.

このバージョンは以前のものよりやや長いが、実際に行っているチェックの利点があり、無意味な入力の時は警告してくれる。これは正規表現で無ければとても難しい ― 1桁ないし2桁の数字を見つけることと、ダッシュが正しい位置にあるか調べるのに多くの\ ``indexOf``\ の呼び出しが必要となる。

|hr|

..  The replace method of string values, which we saw in chapter 6, can be given a regular expression as its first argument.

:doc:`6章</Functional Programming>`\ で見た、文字列の値の\ ``replace``\ メソッドには1つめの引数に正規表現を与えることができる。

.. code-block:: javascript

    print("Borobudur".replace(/[ou]/g, "a"));

..  Notice the g character after the regular expression. It stands for 'global', and means that every part of the string that matches the pattern should be replaced. When this g is omitted, only the first "o" would be replaced.

正規表現の後の\ ``g``\ 文字に注意。これは'グローバル'で、パターンにマッチした全ての部分が置き換わるという意味だ。この\ ``g``\ を省略したら、最初の\ ``"o"``\ しか置換されない。

..  Sometimes it is necessary to keep parts of the replaced strings. For example, we have a big string containing the names of people, one name per line, in the format "Lastname, Firstname". We want to swap these names, and remove the comma, to get a simple "Firstname Lastname" format.

しばしば、置換した後の文字列の部分を保持しておくことが必要になる。例えば、人々の名前を含んだ大きな文字列があり、"姓, 名"のフォーマットで、行毎に名前1つあるとする。これらの名前の姓名を入れ替え、カンマを取り除き、単純な"名 姓"フォーマットにする。

.. code-block:: javascript

    var names = "Picasso, Pablo\nGauguin, Paul\nVan Gogh, Vincent";
    print(names.replace(/([\w ]+), ([\w ]+)/g, "$2 $1"));

..  The $1 and $2 the replacement string refer to the parenthesized parts in the pattern. $1 is replaced by the text that matched against the first pair of parentheses, $2 by the second, and so on, up to $9.

置換文字列の\ ``$1``\ と\ ``$2``\ はパターンの括弧で囲まれた部分への参照である。\ ``$1``\ は1つめの括弧の組の部分にマッチした文字列に置き換わり、\ ``$2``\ は2つめ、それから\ ``$9``\ まで同様である。

..  If you have more than 9 parentheses parts in your pattern, this will no longer work. But there is one more way to replace pieces of a string, which can also be useful in some other tricky situations. When the second argument given to the replace method is a function value instead of a string, this function is called every time a match is found, and the matched text is replaced by whatever the function returns. The arguments given to the function are the matched elements, similar to the values found in the arrays returned by match: The first one is the whole match, and after that comes one argument for every parenthesized part of the pattern.

パターンの中に9つより多くの括弧の部分があるときは、これでは動かない。しかし、文字列の部品を置き換えるもう一つの方法があり、他のトリッキーな状況においても有益でありうる。文字列の代わりに関数の値を\ ``replace``\ メソッドの2つめの引数として与えることができる。この関数はマッチする度に毎回呼び出され、マッチしたテキストが関数の結果で置き換わる。関数に与えられる引数はマッチした要素の配列で、\ ``match``\ により返される配列と同様のものだ：最初の要素はマッチした全体、その後にパターンの括弧で囲まれた部分全てが来る。

.. code-block:: javascript

    function eatOne(match, amount, unit) {
      amount = Number(amount) - 1;
      if (amount == 1) {
        unit = unit.slice(0, unit.length - 1);
      }
      else if (amount == 0) {
        unit = unit + "s";
        amount = "no";
      }
      return amount + " " + unit;
    }

    var stock = "1 lemon, 2 cabbages, and 101 eggs";
    stock = stock.replace(/(\d+) (\w+)/g, eatOne);

    print(stock);

|hr|

演習 10.4

..  That last trick can be used to make the HTML-escaper from chapter 6 more efficient. You may remember that it looked like this:

最後のトリックで\ :doc:`6章</Functional Programming>`\ のHTMLをエスケープする関数をより効率的にできる。このようなものであった：

.. code-block:: javascript

    function escapeHTML(text) {
      var replacements = [["&", "&amp;"], ["\"", "&quot;"],
                          ["<", "&lt;"], [">", "&gt;"]];
      forEach(replacements, function(replace) {
        text = text.replace(replace[0], replace[1]);
      });
      return text;
    }

..  Write a new function escapeHTML, which does the same thing, but only calls replace once.

同じことを行い、しかし\ ``replaceを``\ 1度しか呼び出さない、新しい\ ``escapeHTML``\ 関数を書け。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function escapeHTML(text) {
      var replacements = {"<": "&lt;", ">": "&gt;",
                          "&": "&amp;", "\"": "&quot;"};
      return text.replace(/[<>&"]/g, function(character) {
        return replacements[character];
      });
    }

    print(escapeHTML("The 'pre-formatted' tag is written \"<pre>\"."));

..  The replacements object is a quick way to associate each character with its escaped version. Using it like this is safe (i.e. no Dictionary object is needed), because the only properties that will be used are those matched by the /[<>&"]/ expression.

``replace``\ オブジェクトはそれぞれの文字とそのエスケープ済みのものを手っ取り早く結びつける手段である。このように使えば安全で（i.e.\ ``Dictionary``\ オブジェクトは必要ない）、なぜなら、\ ``/[<>&"]/``\ 式にマッチしたプロパティだけが使われるからである。

|hr|

..  There are cases where the pattern you need to match against is not known while you are writing the code. Say we are writing a (very simple-minded) obscenity filter for a message board. We only want to allow messages that do not contain obscene words. The administrator of the board can specify a list of words that he or she considers unacceptable.

コードを書いている内は知らなかった文字列に対してマッチするパターンが必要となる場合がある。掲示板用に（とても単純な考えで）わいせつな言葉のフィルターを書くとする。わいせつな言葉を含まないメッセージだけを許したい。掲示板の管理者が、彼または彼女が受け入れがたいと考える言葉のリストを指定する。

..  The most efficient way to check a piece of text for a set of words is to use a regular expression. If we have our word list as an array, we can build the regular expression like this:

テキストの一部を言葉のセットでチェックする、もっとも効率的な手段は正規表現を使うことだ。もし言葉のリストが配列であれば、このような正規表現を組むことができる。：

.. code-block:: javascript

    var badWords = ["ape", "monkey", "simian", "gorilla", "evolution"];
    var pattern = new RegExp(badWords.join("|"), "i");
    function isAcceptable(text) {
      return !pattern.test(text);
    }

    show(isAcceptable("Mmmm, grapes."));
    show(isAcceptable("No more of that monkeybusiness, now."));

..  We could add \b patterns around the words, so that the thing about grapes would not be classified as unacceptable. That would also make the second one acceptable, though, which is probably not correct. Obscenity filters are hard to get right (and usually way too annoying to be a good idea).

言葉の周りに\ ``\b``\ パターンを追加して、grapesが否認される側に分類されないようにすることもできた。それでは2つめのものも許容されてしまい、おそらくそれは正しくない。わいせつな言葉のフィルターを正しく作るのは難しい（そして通常はうるさすぎるくらいにするのが良いアイデアだ）。

..  The first argument to the RegExp constructor is a string containing the pattern, the second argument can be used to add case-insensitivity or globalness. When building a string to hold the pattern, you have to be careful with backslashes. Because, normally, backslashes are removed when a string is interpreted, any backslashes that must end up in the regular expression itself have to be escaped:

``RegExp``\ コンストラクターへの1つめの引数はパターンの内容の文字列で、2つめは大文字小文字の無視や全体性の追加に使われる。パターンを保持する文字列を組み立てるときは、バックスラッシュに注意しなければならない。なぜなら、通常、バックスラッシュは文字列が解釈されるときに除去されるので、最終的には正規表現それ自体をエスケープしなければならないのである。

.. code-block:: javascript

    var digits = new RegExp("\\d+");
    show(digits.test("101"));

|hr|

..  The most important thing to know about regular expressions is that they exist, and can greatly enhance the power of your string-mangling code. They are so cryptic that you'll probably have to look up the details on them the first ten times you want to make use of them. Persevere, and you will soon be off-handedly writing expressions that look like occult gibberish

正規表現を知る上で最も重要なのは、それが存在し、それによって文字列を管理するコードの力がとても強力になるということだ。暗号みたいなものなので、たぶん最初の10回くらいはその詳細を見なければならないだろう。頑張れば、すぐにオカルト的なちんぷんかんぷんな言葉のような式を書けるようになるだろう。

.. image :: /img/xkcd_regular_expressions.png

(Comic by Randall Munroe.)

