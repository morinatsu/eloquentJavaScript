---
layout: default
title: 10. 正規表現
---

正規表現
=====================================================

今までの章のさまざまな場で、文字列の値の中からパターンを探さねばならなかった。[4章]({{ "/Data structures.html" | prepend:site.baseurl }})では日付の部分であるべき数字の正確な位置を書きだすことで文字列から日付を展開した。その後、[6章]({{ "/Functional Programming.html" | prepend:site.baseurl }})では文字列の中の決まった文字、例えばHTMLの出力でエスケープされるべき文字を見つけるとりわけ醜いコードの部品を見た。

正規表現は文字列の中のパターンを記述する言語である。小さな形の、分割された言語として、JavaScript（および他の様々なプログラミング言語で、1つないしそれ以上の方法で）に組み込まれている。あまり読みやすい言語ではない -- 大きな正規表現は全く読めないものになっていく。しかし、有益なツールであり、文字列処理のプログラムを本当に単純化できる。

* * *

ちょうど文字列が引用符の間に書かれるように、正規表現のパターンはスラッシュ(`/`)の間に書かれる。これは正規表現の中のスラッシュはエスケープされなければならないということを意味する。

```javascript
var slash = /\//;
show("AC/DC".search(slash));
```

`search`メソッドは`indexOf`に似ているが、しかし、こちらは文字列の代わりに正規表現を探す。正規表現で指定されたパターンは文字列ではできないいくつかのことができる。初めに、単一の文字より多くの要素にマッチさせることができる。[6章]({{ "/Functional Programming.html" | prepend:site.baseurl }})で、文書のマークアップを展開するときに、最初のアスタリスクか左中括弧を探す必要があった。今ならそれをこのように書くことができる。：

```javascript
var asteriskOrBrace = /[\{\*]/;
var story =
  "We noticed the *giant sloth*, hanging from a giant branch.";
show(story.search(asteriskOrBrace));
```

正規表現の中の`[`と`]`文字は特別な意味を持つ。文字列の集合を納めることができ、'これらの文字の中のいずれか'という意味になる。英数字でない多くの文字列は正規表現の中で特別な意味を持ち、実際の文字としてそれらを参照するときに、常にそれらをバックスラッシュでエスケープするのは良いアイデアである。

* * *

しばしば必要になる文字列のセットにはいくつかのショートカットがある。ピリオド(`.`)は'改行以外の任意の文字'の意味に使われ、エスケープされた'd'(`\d`)は'任意の数字'の意味、エスケープされた'w'(`\w`)は任意の英数字（ある理由によりアンダースコアを含む）にマッチし、エスケープされた's'(`\s`)は任意の空白文字（タブ、改行、空白）にマッチする。

```javascript
var digitSurroundedBySpace = /\s\d\s/;
show("1a 2 3d".search(digitSurroundedBySpace));
```

エスケープされた'd'、'w'、および's'は反対の意味にするために大文字で置き換えることができる。例えば、`\S`は空白**以外**の任意の文字にマッチする。`[`と`]`を使うとき、`^`文字から始めることでパターンを反転することができる。：

```javascript
var notABC = /[^ABC]/;
show("ABCBACCBBADABC".search(notABC));
```

見ての通り、パターンを表現するための、正規表現の文字の使い方は、A) とても短く、B) とても読みにくい。

* * *

### <a name="Ex10-1">[演習 10.1]

`"XX/XX/XXXX"`の形式の日付にマッチする正規表現を書け。`X`の場所は数字である。文字列`"born 15/11/2003 (mother Spot): White Fang"`でそれをテストせよ。

[解答を見る]

```javascript
var datePattern = /\d\d\/\d\d\/\d\d\d\d/;
show("born 15/11/2003 (mother Spot): White Fang".search(datePattern));
```

* * *

しばしば、確実に文字列の最初でパターンが始まること、または文字列の終わりでパターンが終わることが必要となる。このために、`^`と`$`という特別な文字がつかわれる。1つ目は文字列の最初にマッチし、2つ目は最後にマッチする。

```javascript
show(/a+/.test("blah"));
show(/^a+$/.test("blah"));
```

最初の正規表現は1つの`a`を含む任意の文字列にマッチし、2つ目はその文字列の全てが`a`であるときのみマッチする。

正規表現はオブジェクトであり、メソッドを持つことに注意。これらの`test`メソッドは与えられた文字列が正規表現にマッチしたか否かの真偽値を返す。

`\b`のコードは'単語の境界'にマッチし、それは句読点、空白、文字列の最初と最後である。

```javascript
show(/cat/.test("concatenate"));
show(/\bcat\b/.test("concatenate"));
```

* * *

パターンの部分は何回も繰り返すことができる。要素の後ろのアスタリスク(`*`)は、0回を含む任意の回数のそれの繰り返しを許す。プラス(`+`)も同様だが、しかし最低1回はパターンがなければならない。疑問符(`?`)は要素が'任意'であることを示し -- それは0回または1回である。

```javascript
var parenthesizedText = /\(.*\)/;
show("Its (the sloth's) claws were gigantic!".search(parenthesizedText));
```

必要なら、中括弧で要素の繰り返し回数をより正確にすることができる。中括弧の間の数値(`{4}`)は繰り返しの回数を与える。それらがカンマで区切られた2つの数値(`{3,10}`)であれば1つめが最低の回数を、2つめが最大の回数を示す。同様に、`{2,}`は2回かそれ以上、`{,4}`は4回かそれ以下を意味する。

```javascript
var datePattern = /\d{1,2}\/\d\d?\/\d{4}/;
show("born 15/11/2003 (mother Spot): White Fang".search(datePattern));
```

`/\d{1,2}/`と`/\d\d?/`の部分はともに1桁または2桁の数字である。

* * *

### <a name="Ex10-2">[演習 10.2]

電子メールのアドレスにマッチするパターンを書け。単純には、`@`の前と後ろは英数字と`.`と`-`（ピリオドとダッシュ）であると仮定し、アドレスの最後の部分は最後のピリオドの後に国のコードがあって、英数字のみ含み、2文字ないし3文字でなければならないとする。

[解答を見る]

```javascript
var mailAddress = /\b[\w\.-]+@[\w\.-]+\.\w{2,3}\b/;

show(mailAddress.test("kenny@test.net"));
show(mailAddress.test("I mailt kenny@tets.nets, but it didn wrok!"));
show(mailAddress.test("the_giant_sloth@gmail.com"));
```

パターンの最初と最後の`\b`は2つめの文字列が確実にマッチしないようにするためのものだ。

* * *

正規表現の部分は括弧でグループ化することができる。これで`*`のようなものを1文字より多いパターンに使うことができる。例えば：

```javascript
var cartoonCrying = /boo(hoo+)+/i;
show("Then, he exclaimed 'Boohoooohoohooo'".search(cartoonCrying));
```

正規表現の最後の`i`はどこから来たものだろうか？閉じるスラッシュの後に、正規表現の'オプション'を追加できる。この`i`は大文字小文字を区別しないという意味で、パターン中の小文字のBを文字列中の大文字のBにマッチさせることができる。

パイプ文字(`|`)はパターンを2つの要素のどちらかの選択とする。例えば：

```javascript
var holyCow = /(sacred|holy) (cow|bovine|bull|taurus)/i;
show(holyCow.test("Sacred bovine!"));
```

* * *

時折、パターンを探すことは文字列から何かを抽出するためのただ1つめのステップでしかないことがある。以前の章では、その抽出を文字列の`indexOf`や`slice`メソッドを呼び出すことで行ってきた。今、我々は正規表現を知ったので、代わりに`match`メソッドを使うことができる。文字列が正規表現にマッチさせたとき、マッチに失敗したときは`null`が、成功したときはマッチした文字列の配列が結果となる。

```javascript
show("No".match(/Yes/));
show("... yes".match(/yes/));
show("Giant Ape".match(/giant (\w+)/i));
```

返される配列の最初の要素は常にパターンにマッチした文字列の部分である。最後の例で見れるように、パターンの中に括弧で囲われた部分があるとき、そこにマッチした部分も配列に追加される。しばしば、これで部品の抽出がとても簡単になる。

```javascript
var parenthesized = prompt("Tell me something", "").match(/\((.*)\)/);
if (parenthesized != null)
  print("You parenthesized '", parenthesized[1], "'");
```

* * *

### <a name="Ex10-3">[演習 10.3]

[4章]({{ "Data structures.html" | prepend:site.baseurl }})で書いた`extractDate`関数を書き直せ。文字列を与えられたとき、この関数は先に見た日付のフォーマットに従った部分を文字列から探す。もし日付のようなものが見つかったら、`Date`オブジェクトにその値を入れる。そうでなければ、例外を投げる。日または月が1桁だけであっても受け入れるようにすること。

[解答を見る]

```javascript
function extractDate(string) {
  var found = string.match(/(\d\d?)\/(\d\d?)\/(\d{4})/);
  if (found == null)
    throw new Error("No date found in '" + string + "'.");
  return new Date(Number(found[3]), Number(found[2]) - 1,
                  Number(found[1]));
}

show(extractDate("born 5/2/2007 (mother Noog): Long-ear Johnson"));
```

このバージョンは以前のものよりやや長いが、実用性チェックを行う上での利点があり、無意味な入力の時は警告してくれる。これは正規表現で無ければとても難しい -- 1桁ないし2桁の数字を見つけることと、ダッシュが正しい位置にあるか調べるのに多くの`indexOf`の呼び出しが必要となる。

* * *

[6章]({{ "/Functional Programming.html" | prepend:site.baseurl }})で見た、文字列の値の`replace`メソッドには1つめの引数に正規表現を与えることができる。

```javascript
print("Borobudur".replace(/[ou]/g, "a"));
```

正規表現の後の`g`文字に注意。これは'グローバル'で、パターンにマッチした全ての部分が置き換わるという意味だ。この`g`を省略したら、最初の`"o"`しか置換されない。

しばしば、置換した後の文字列の部分を保持しておくことが必要になる。例えば、人々の名前を含んだ大きな文字列があり、"姓, 名"のフォーマットで、行毎に名前1つあるとする。これらの名前の姓名を入れ替え、カンマを取り除き、単純な"名 姓"フォーマットにする。

```javascript
var names = "Picasso, Pablo\nGauguin, Paul\nVan Gogh, Vincent";
print(names.replace(/([\w ]+), ([\w ]+)/g, "$2 $1"));
```

置換文字列の`$1`と`$2`はパターンの括弧で囲まれた部分への参照である。`$1`は1つめの括弧の組の部分にマッチした文字列に置き換わり、`$2`は2つめ、それから`$9`まで同様である。

パターンの中に9つより多くの括弧の部分があるときは、これでは動かない。しかし、文字列の部品を置き換えるもう一つの方法があり、他のトリッキーな状況においても有益でありうる。文字列の代わりに関数の値を`replace`メソッドの2つめの引数として与えることができる。この関数はマッチする度に毎回呼び出され、マッチしたテキストが関数の結果で置き換わる。関数に与えられる引数はマッチした要素の配列で、`match`により返される配列と同様のものだ：最初の要素はマッチした全体、その後にパターンの括弧で囲まれた部分全てが来る。

```javascript
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
```

* * *

### <a name="Ex10-4">[演習 10.4]

最後のトリックは[6章]({{ "/Functional Programming.html" | prepend:site.baseurl }})のHTMLをエスケープする関数をより効率的にすることができる。それはこのようなものであった：

```javascript
function escapeHTML(text) {
  var replacements = [["&", "&amp;"], ["\"", "&quot;"],
                      ["<", "&lt;"], [">", "&gt;"]];
  forEach(replacements, function(replace) {
    text = text.replace(replace[0], replace[1]);
  });
  return text;
}
```

同じことを行い、しかし`replaceを`1度しか呼び出さない、新しい`escapeHTML`関数を書け。

[解答を見る]

```javascript
function escapeHTML(text) {
  var replacements = {"<": "&lt;", ">": "&gt;",
                      "&": "&amp;", "\"": "&quot;"};
  return text.replace(/[<>&"]/g, function(character) {
    return replacements[character];
  });
}

print(escapeHTML("The 'pre-formatted' tag is written \"<pre>\"."));
```

`replace`オブジェクトはそれぞれの文字とそのエスケープ済みのものを手っ取り早く結びつける手段である。このように使えば安全で（i.e.`Dictionary`オブジェクトは必要ない）、なぜなら、`/[<>&"]/`式にマッチしたプロパティだけが使われるからである。

* * *

コードを書いている内は知らなかった文字列に対してマッチするパターンが必要となる場合がある。掲示板用に（とても単純な考えで）わいせつな言葉のフィルターを書くとする。わいせつな言葉を含まないメッセージだけを許したい。掲示板の管理者が、彼または彼女が受け入れがたいと考える言葉のリストを指定する。

テキストの一部を言葉のセットでチェックする、もっとも効率的な手段は正規表現を使うことだ。もし言葉のリストが配列であれば、このような正規表現を組むことができる。：

```javascript
var badWords = ["ape", "monkey", "simian", "gorilla", "evolution"];
var pattern = new RegExp(badWords.join("|"), "i");
function isAcceptable(text) {
  return !pattern.test(text);
}

show(isAcceptable("Mmmm, grapes."));
show(isAcceptable("No more of that monkeybusiness, now."));
```

言葉の周りに`\b`パターンを追加して、grapesが否認される側に分類されないようにすることもできた。それでは2つめのものも許容されてしまい、おそらくそれは正しくない。わいせつな言葉のフィルターを正しく作るのは難しい（そして通常はうるさすぎるくらいにするのが良いアイデアだ）。

`RegExp`コンストラクターへの1つめの引数はパターンの内容の文字列で、2つめは大文字小文字の無視や全体性の追加に使われる。パターンを保持する文字列を組み立てるときは、バックスラッシュに注意しなければならない。なぜなら、通常、バックスラッシュは文字列が解釈されるときに除去されるので、最終的には正規表現それ自体をエスケープしなければならないのである。

```javascript
var digits = new RegExp("\\d+");
show(digits.test("101"));
```

* * *

正規表現を知る上で最も重要なのは、それが存在し、それによって文字列を管理するコードの力がとても強力になるということだ。暗号みたいなものなので、たぶん最初の10回くらいはその詳細を見なければならないだろう。頑張れば、すぐにオカルト的なちんぷんかんぷんな言葉のような式を書けるようになるだろう。

![xkcd_regular_expressions]({{ "/assets/img/xkcd_regular_expressions.png" | prepend:site.baseurl }})

(Comic by Randall Munroe.)

