---
layout: default
title: 4. データ構造
---

データ構造: オブジェクトと配列
=====================================================

この章では2, 3の単純な問題の解決に没頭する。その過程で、2つの新しいデータ型、配列とオブジェクトについて論じ、それらに関するテクニックを見ていく。

次のような状況を考える：エミリーというおかしな叔母さん、彼女は50匹を超える猫（数えるのはあきらめている）と暮らしていると噂されており、定期的にあなたに自分自慢の電子メールを送り続けてくる。大抵このようなもの：

**愛しい甥へ**

**あなたがスカイダイビングをやっているとお母さんが言っていました。本当？お気を付けなさい、お若いかた！私の夫に何があったか覚えてる？2階から飛び降りただけなのに！**

**とにかく、面白いことがあるの。ドレークさんというかっこいい殿方が上の階に越してきて、毎週彼の注意を引こうとしているのだけど、彼は猫が怖いみたいなの。でなければ猫アレルギーとか？今度あったら太っちょイゴールを肩に押しつけて何が起こるか見てみようと思っているの。**

**前に話した詐欺の件は思ったより良いことになったわ。1回苦情を言っただけで、もう5回も'支払'が来てる。私の気分を悪くするために始めたのね。何かの法律に違反していることを証明できるという、あなたの意見は正しかったわ。**

**(... etc ...)**

**たくさんの愛情を、エミリー叔母さんより**

**died 27/04/2006: Black Leclère**

**born 05/04/2006 (mother Lady Penelope): Red Lion, Doctor Hobbles the 3rd, Little Iroquois**

老人の機嫌をとるために、彼女の猫の系譜を追跡して、誤って死んだ猫のことに触れるのを避けつつ、"追伸： Doctor Hobbles 2ndが今週土曜の誕生日を楽しめるといいですね！"、あるいは"Lasy Penelopeの調子はどうですか？彼女は5歳でしたよね？"といったことを返信に付け足すのはどうだろうか。あなたは叔母さんからの古い電子メールを大量に所有しており、幸運なことに叔母さんは常に彼女の猫の誕生と死をメールの末尾に正確に同じフォーマットで記している。

これらのメールを全て手作業で処理する気にはならないだろう。幸運なことに、我々は例題を必要としているから、プログラムにこの仕事をさせてみよう。スタートとして、最後の電子メールの時点で生きている猫のリストを作るプログラムを書こう。

質問する前、文通の始まった時点で、エミリー叔母さんは1匹の猫だけを飼っていた: Spotである。（その頃の彼女はまだ、むしろ平凡だった）

* * *

![猫の目]({{ "/assets/img/eyes.png" | prepend:site.baseurl }} "eyes")

* * *

プログラムをタイプし始める前に、普通は問題を解く筋道を立ててみるものだ。こういうプランだ。：

1. "Spot"だけが猫の名前の集合にあるところからスタートする。
2. アーカイブから全ての電子メールを取り出し、時系列順に並べる。
3. "born（誕生）"または"died（死亡）"から始まる段落を見る。
4. "born"から始まる段落に含まれる名前を集合に加える。
5. "died"から始まる段落に含まれる名前を集合から取り除く。

段落から名前を取り出すところはこのようになるだろう：

1. 段落からコロンを探す。
2. コロンの後ろの部分を取り上げる。
3. カンマを見てこの部分を分割して名前とする。

エミリー叔母さんが、あなたの叔母さんがそうであるように、常に正確なフォーマットを使うと信じられなかったり、名前のスペルを間違ったりすることについては保留する必要があるかもしれない。

* * *

最初に、プロパティについて語ろう。JavaScriptの多くの値は他の値との結びつきを持つ。この結びつきをプロパティと呼ぶ。全ての文字列は`length`（長さ）というプロパティを持ち、それは文字列の中の文字の数を示す数値である。

プロパティは2つの方法でアクセスできる:

```javascript
var text = "purple haze";
show(text["length"]);
show(text.length);
```

2つめの方法は1つめの短縮形で、プロパティの名前が変数名として正しいときだけ動作する -- 空白や記号を含んでいたり数字から始まっていたりしてはいけない。

`null`や`undefined`といった値にプロパティに持たせることはできない。そのような値からプロパティを読もうとしたらエラーになる。以下のコードを試し、そのような場合にブラウザがどんなメッセージを出すか見て欲しい。（ブラウザによっては、むしろ暗号めいたものになりうる）

```javascript
var nothing = null;
show(nothing.length);
```

* * *

文字列のプロパティは変更できない。見たとおりの、ちょうどの`length`であり、足したり取り去ったりすることは許されない。

オブジェクト型の値では異なる。それらの主な役目は他の値を保持することだ。それらは、こうも言える、プロパティという形の触手のセットを持つものであると。自由に変更し、取り去ったり、新しいものを付け加えたりできる。

オブジェクトはこのように書くことができる:

```javascript
var cat = {colour: "grey", name: "Spot", size: 46};
cat.size = 47;
show(cat.size);
delete cat.size;
show(cat.size);
show(cat);
```

変数のように、それぞれのプロパティは文字列でラベル付けされることで結びつけることができる。最初の文は、`"colour"`プロパティに文字列`"grey"`を格納し、`"name"`プロパティに文字列`"Spot"`で結びつけられ、`"size"`プロパティで`46`という数字を参照する1つのオブジェクトを作る。2つめの文は`size`という名前のプロパティに、変数と同じやり方で新しい値を与えている。

`delete`キーワードはプロパティを切り捨てる。存在しないプロパティを読もうとすると`undefined`値が与えられる。

もしまだ存在しないプロパティに、`=`演算子で設定を行えば、それはオブジェクトに追加される。

```javascript
var empty = {};
empty.notReally = 1000;
show(empty.notReally);
```

オブジェクトを作るとき、プロパティに変数名として正しくない名前を付けるには引用符を付ける必要があり、大括弧を使ってアプローチする:

```javascript
var thing = {"gabba gabba": "hey", "5": 10};
show(thing["5"]);
thing["5"] = 20;
show(thing[2 + 3]);
delete thing["gabba gabba"];
```

見ての通り、大括弧の中の部分は任意の式である。それはプロパティの名前が参照されるときに文字列に変換される。プロパティ名に変数を使うこともできる。:

```javascript
var propertyName = "length";
var text = "mainline";
show(text[propertyName]);
```

`in`演算子はオブジェクトがそのプロパティを確かに持っているかをテストする。これは真偽値を作る。

```javascript
var chineseBox = {};
chineseBox.content = chineseBox;
show("content" in chineseBox);
show("content" in chineseBox.content);
```

* * *

オブジェクト値をコンソール上で見るとき、それらはプロパティを見るためにクリックすることができる。出力ウインドウは'inspect'ウインドウに代わる。右上の小さな'x'は出力ウインドウに戻るのに使え、左矢印は前回見たオブジェクトのプロパティに戻るのに使える。

```javascript
show(chineseBox);
```

* * *

### <a name="Ex4-1">[演習 4.1]</a>

猫の問題を解くにあたり、名前の'セット'(集合)について話そう。セットは同じ値を1つしか入れることのできないコレクションである。もし名前が文字列であれば、これを名前のセットを現わすオブジェクトとして使う方法を考えられないだろうか？

このセットへの名前の追加のしかた、取り除きかた、名前がその中に入っていないかチェックする方法を示せ。

[解答を見る]

これはセットの内容を格納をオブジェクトのプロパティへのように行うことで可能になる。名前を任意の値としてプロパティに設定することにより名前を追加でき、このプロパティを削除することで名前を取り除ける。`in`演算子で名前がセットにあるかどうかチェックできる。

```javascript
var set = {"Spot": true};
// Add "White Fang" to the set
set["White Fang"] = true;
// Remove "Spot"
delete set["Spot"];
// See if "Asoka" is in the set
show("Asoka" in set);
```

* * *

オブジェクトの値は、見ての通り、変更することができる。[2章]({{ "/Basic JavaScript.html" | pretend:site.baseurl }})では値の型は全くかわらない、既に存在する値の型は変更不可能であると論じた。それらを連結したりそれらから新しい値を作ることはできるが、しかしここの文字列の値を見れば、その中のテキストは変更できない。オブジェクトでは、その一方で、値に含まれている内容をプロパティを変更することにより変えることができるのだ。

`120`と`120`という、2つの数値があるとき、全ての実用的な目的の上ではそれらは正確に同じ値であると考えられる。オブジェクトについては、同じオブジェクトへの2つの参照があるということと、同じプロパティを含む2つの異なるオブジェクトがあるということの間には違いがある。以下のコードについて考えよう:

```javascript
var object1 = {value: 10};
var object2 = object1;
var object3 = {value: 10};

show(object1 == object2);
show(object1 == object3);

object1.value = 15;
show(object2.value);
show(object3.value);
```

`object1`は`object2`は**同じ**値を掴んでいる2つの変数である。実在のオブジェクトは1つしかなく、`object1`が変更されれば`object2`の値も変わる。`object3`という変数は別のオブジェクトを指しており、最初は`object1`と同じプロパティを含んでいるが、しかし別れた人生を生きる。

JavaScriptの`==`演算子は、オブジェクトを比較するとき、もし与えられた両方の値が正確に同じ値のときだけ`true`を返す。同じ内容を持つ異なるオブジェクトを比較したら`false`となる。これはある状況では便利だが、他の状況では実用的でない。

* * *

オブジェクトの値は多くの異なる役割を果たす。セットのようなものはそれらの1つでしかない。この章で若干の他の役割を見ていき、[8章]({{ "/Object-oriented Programming.html" | pretend:site.baseurl }})において別の重要なオブジェクトの用法を見よう。

猫問題の計画において -- 実際のところ、それを計画ではなく、アルゴリズムと呼ばれる、それは我々が語っているように思っているもの -- このアルゴリズムの中で、アーカイブの中の全ての電子メールをどう扱うか話そう。このアーカイブは何に見えるだろうか？それをどこから持って来たものだろうか？

2つめの質問は今はおいておこう。[14章]({{ "/HTTP requests.html" | pretend:site.baseurl }})でプログラムにデータを取り込む方法を話し、今は電子メールは魔法のようにただそこにあるとしておこう。コンピューターの中では魔法は本当に簡単だ。

* * *

アーカイブを格納する方法はまだ興味深い疑問だ。それは多くの電子メールを含んでいる。1つの電子メールはまた文字列でもあるのは明らかであろう。アーカイブ全体は巨大な文字列に入れることができるが、しかしそれはひどく実用的でない。我々が望むのは分割された文字列のコレクションである。

物のコレクションとしてオブジェクトを使う。このようなオブジェクトを作ることができる:

```javascript
var mailArchive = {"the first e-mail": "Dear nephew, ...",
                   "the second e-mail": "..."
                   /* and so on ... */};
```

しかしこれで電子メールを最初から最後まで見るのは困難だ ― プログラムはどうやってこれらのプロパティ名を推測するのか？より推測しやすいプロパティ名によってこれを解決できる。:

```javascript
var mailArchive = {0: "Dear nephew, ... (mail number 1)",
                   1: "(mail number 2)",
                   2: "(mail number 3)"};

for (var current = 0; current in mailArchive; current++)
  print("Processing e-mail #", current, ": ", mailArchive[current]);
```

まさにこの用途に使うための特別な種類のオブジェクトがある。これらは配列と呼ばれ、便利なことに、配列に格納されている値の数を含む`length`プロパティのような、この種のコレクションを便利に使うための操作をいくつも提供している。

新しい配列は大括弧（`[`と`]`）を使って作られる。:

```javascript
var mailArchive = ["mail one", "mail two", "mail three"];

for (var current = 0; current < mailArchive.length; current++)
  print("Processing e-mail #", current, ": ", mailArchive[current]);
```

この例では、もう要素の番号を明示的に書く必要はなくなった。1つめのものは自動的に0番となり、2つめのものは1番、というようになる。

なぜ0から始まるか？人間は1から数えがちだ。直感的でないように見えるが、コレクションの要素の番号を0から始めることはしばしばより実用的なのだ。今はただそのようにしておけば、あなたもだんだん気に入ってくるだろう。

要素0から始めることはまた`x`要素があるコレクションにおいて、最後の要素の位置が`x - 1`になるということをも意味する。これが例の`for`ループで`current < mailArchive.length`をチェックする理由である。`mailArchive.length`の位置にある要素はなく、`current`がこの値になり次第、ループが終了する。

* * *

### <a name="Ex4-2">[演習 4.2]</a>

1つの引数を取り、0から与えられた数値までの全ての正の数値を含む配列を返す`range`関数を書け。

空の配列を作るには単純に`[]`とタイプすればよい。オブジェクトへのプロパティを追加を思い出して、配列についてもそのように、`=`演算子で値を割り当てれば良い。`length`プロパティは要素の追加により自動的に更新される。

[解答を見る]

```javascript
function range(upto) {
  var result = [];
  for (var i = 0; i <= upto; i++)
    result[i] = i;
  return result;
}
show(range(4));
```

今までやってきたように、ループの変数に`counter`や`current`と名前を付ける代わりに、ここでは単純に`i`とした。ループ変数に通常`i`、`j`または`k`といった単一の文字を使うのは、プログラマーの間に広く広まっている習慣だ。その起源の多くは面倒さにある：7文字よりは1文字タイプするのがいいし、`counter`とか`current`といった名前は明らかに全く変数の意味を表してない。

もしプログラムが意味のない1文字の変数を使いすぎたら、信じられないほど混乱したものとなるだろう。私自身のプログラムでは、これを2, 3の一般的な場合にとどめようとしている。小さいループがこれらの場合の1つ。もし、ループが他のループを含んでいたら、そしてそれが`i`みたいな変数名でもあり、内側のループが外側のループで使っている変数を更新していたら、すべてが壊れるだろう。`j`を内側のループの中で使うことは可能だが、全般的に、ループの本体が大きいときは、意味が明確になる変数名を使うべきである。

* * *

文字列と配列オブジェクトの両方とも、`length`プロパティに加えて、関数値を参照する幾つかのプロパティを持つ。

```javascript
var doh = "Doh";
print(typeof doh.toUpperCase);
print(doh.toUpperCase());
```

全ての文字列は`toUpperCase`プロパティを持つ。呼ばれたら、全ての文字が大文字に変換された文字列のコピーを返す。`toLowerCase`というのもまたあって、推測通りのことをする。

注意すべきは、`toUpperCase`は何の引数もなく呼ばれるにもかかわらず、関数は何らかの方法で文字列`"Doh"`にアクセスし、その値をプロパティとしていることだ。ここでどのようなことが起こっているかの正確なことは[8章]({{ "/Object-oriented Programming.html" | pretend:site.baseurl }})で記述しよう。

"`toUpperCase`は文字列オブジェクトのメソッドである"というように、関数を含むプロパティは一般的にメソッドと呼ばれる。

```javascript
var mack = [];
mack.push("Mack");
mack.push("the");
mack.push("Knife");
show(mack.join(" "));
show(mack.pop());
show(mack);
```

`push`メソッドは配列と結びついており、配列に値を付け加えるのに使われる。先ほどの演習ので使った`result[i] = i`の代わりになる。それから`pop`というものがあり、これは`push`の反対である：配列から最後の値を取り出して返す。`join`は文字列の配列から1つの大きな文字列を組み立てる。与えられたパラメータは配列の値の間に挿入される。

* * *

猫の話に戻る。配列が電子メールのアーカイブを格納する良い手立てであることは分かった。このページでは、`retrieveMail`関数でこの配列を（魔法のように）手に入れることができる。この続きの処理をロケット工学その他の助けを借りずに行おう。:

```javascript
var mailArchive = retrieveMails();

for (var i = 0; i < mailArchive.length; i++) {
  var email = mailArchive[i];
  print("Processing e-mail #", i);
  // Do more things...
}
```

生きている猫のセットを現わす方法も決めなければならない。次の問題は、それから、電子メールから`"born"`か`"died"`で始まる段落を見つけることだ。

* * *

1つめの問題は段落とは正確には何であるかということになるだろう。この場合、文字列の値自体は十分な助けとならない：JavaScriptのテキストの概念では'文字の連なり'よりも深い意味はなく、ここにおいての段落の定義は決める必要がある。

先に、我々は改行のキャラクターというのを見た。多くの人々が段落を区切るのにこれらを使う。我々は段落を、これからは、改行文字から始まる、あるいは本文の最初から始まり、次の改行文字か内容自体の終わりによって終わる電子メールのを一部分であると考える。

そして我々は文字列を段落に分割するアルゴリズムを我々自身で書く必要はない。文字列は既に`split`という名のメソッドを持っており、それは配列の`join`メソッドの（ほとんど）反対である。引数として与えられた文字列を基準として文字列を配列に分割し格納する。

```javascript
var words = "Cities of the Interior";
show(words.split(" "));
```

このように、改行(`"\n"`)で分割し、電子メールを段落に分割するのに使える。

* * *

### <a name="Ex4-3">[演習 4.3]</a>

`split`と`join`は正確には互いの反対であるとは言えない。`string.split(x).join(x)`は常に元の値となるが、しかし`array.join(x).split(x)`はそうならない。`.join(" ").split(" ")`が元と異なる例を挙げられるか？

[解答を見る]

```javascript
var array = ["a", "b", "c d"];
show(array.join(" ").split(" "));
```

* * *

"born"と"died"のどちらからも始まらない段落はプログラムによって無視できる。文字列が確かにある語から始まるかどうかテストするにはどうすればよいか？`charAt`メソッドを文字列から特定の文字を取り出すのに使える。`x.charAt(0)`は1つめの文字を与え、`1`であれば2つめの文字、のように続く。文字列が"born"から始まるかチェックする1つの方法は:

```javascript
var paragraph = "born 15-11-2003 (mother Spot): White Fang";
show(paragraph.charAt(0) == "b" && paragraph.charAt(1) == "o" &&
     paragraph.charAt(2) == "r" && paragraph.charAt(3) == "n");
```

しかしこれは少々かっこ悪い -- 10文字からなる言葉をチェックすることを想像して欲しい。ここで学ばなければならないこと：行が途方もなく、複数行にまたがるほど長くなることがありうる。同様の役割を果たしている元の行の最初の要素を、新しい行の先頭に並べることで、結果を読みやすくすることができる。

文字列は`slice`というメソッドも持っている。これは文字列の1つめの引数で与えられた位置の文字から、2つめの引数で与えられた位置の文字の前（その位置の文字は結果に含まれない）までを一部分をコピーする。これで先に書いたチェックを簡単に書くことができる。

```javascript
show(paragraph.slice(0, 4) == "born");
```

* * *

### <a name="Ex4-4">[演習 4.4]</a>

両方とも文字列である2つの引数を取り、1つめの引数が2つめの引数と同じ文字から始まっていれば`true`を、そうでなければ`false`を返す`startWith`関数を書け。

[解答を見る]

```javascript
function startsWith(string, pattern) {
  return string.slice(0, pattern.length) == pattern;
}

show(startsWith("rotation", "rot"));
```

* * *

`charAt`と`slice`で文字列に存在しない部分が指定されたら何が起こるだろう？今見せた`startsWith`でマッチすべき文字列よりパターンの方が長ければどうなる？

```javascript
show("Pip".charAt(250));
show("Nop".slice(1, 10));
```

`charAt`は与えられた位置に文字がなければ`""`を返し、そして`slice`は存在しない新しい文字列の一部をただ残す。

そう、`startsWith`あのバージョンも動く。`startsWith("Idiots", "Most honoured colleagues")`が呼び出されたとき、`string`が十分な文字を持っていなければ、`slice`の呼び出しは常に`pattern`より短い文字列を返すだろう。そのため、`==`での比較は`false`を返し、それは正しい。

それは常に異常な（しかし正しい）プログラムへの入力を考慮する時間を取る助けになる。これらは通常corner caseと呼ばれ、全ての'正常な'入力で完璧に動くプログラムがcorner caseでヘマすることはとてもよくあることだ。

* * *

猫問題で解決されていないのは段落から名前を展開するところだけとなった。アルゴリズムはこうだ:

1. 段落からコロンを探す。
2. コロンより後の部分を取り出す。
3. コンマを見て、取り出した部分を名前に分割する。

これは`"died"`から始まる段落と`"born"`から始まる段落の両方で起こる。これを関数にいれて、異なる種類の段落を扱う2つのコードの両方で使つのはいいアイデアだ。

* * *

### <a name="Ex4-5">[演習 4.5]</a>

段落を引数とし、名前の配列を返す`catNames`関数を書けるか？

文字列は、文字列の中の文字または文字列の一部の（最初の）位置を見つける`indexOf`メソッドを持つ。また`slice`は引数を1つだけ与えられたとき、与えられた位置から文字列の最後までの全体を返す。

コンソールを使うのは関数を調べる助けとなる。例えば、`"foo:bar".indexOf(":")`とタイプしてどうなるか見てみるといい。

[解答を見る]

```javascript
function catNames(paragraph) {
  var colon = paragraph.indexOf(":");
  return paragraph.slice(colon + 2).split(", ");
}

show(catNames("born 20/09/2004 (mother Yellow Bess): " +
              "Doctor Hobbles the 2nd, Noog"));
```

トリッキーな部分、アルゴリズムの元の説明では、コロンやコンマのあとに続く空白は無視していた。`+ 2`は文字列を分割するときに使われ、コロン自体とその後の空白を取り除くために必要になる。`split`の引数にコンマと空白の両方を含めるのは、実際には名前はコンマだけではなく、空白も含めたもので分割されているからである。

この関数では問題のチェックを行っていない。この場合においては、入力が常に正しいと仮定しているからである。

* * *

残っているのはこれらの断片を互いにつなぎ合わせることだ。1つのやり方としてはこのようになるだろう。:

```javascript
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
```

これは大きく密度の高いコードの塊だ。少し時間をかけて、もう少し軽いものにしよう。しかし最初はこの結果を見てみよう。個々の猫が生きているかチェックする方法は分っている。:

```javascript
if ("Spot" in livingCats)
  print("Spot lives!");
else
  print("Good old Spot, may she rest in peace.");
```

しかし生きている全ての猫を列挙するにはどうしたらいいだろう？`in`キーワードは`for`とともに使われたとき別の意味を持つ:

```javascript
for (var cat in livingCats)
  print(cat);
```

このようなループは、セットの中の全ての名前を列挙可能なオブジェクトのプロパティの名前を全て処理する。

* * *

コードの1片は不可侵のジャングルのように見える。猫問題の例の解答はこの病気にかかっている。これに光を当てる方法の1つとしては、ただ戦略的に空行を追加するのだ。これで見やすくはなるが、しかし問題を本当に解決するものではない。

ここで必要なのはコードを砕くことだ。我々は既に助けとなる関数を`startsWith`と`catNames`との2つ書いており、そのどちらもが小さく、プログラムの理解可能な一部である。このやり方を続けていこう。

```javascript
function addToSet(set, values) {
  for (var i = 0; i < values.length; i++)
    set[values[i]] = true;
}

function removeFromSet(set, values) {
  for (var i = 0; i < values.length; i++)
    delete set[values[i]];
}
```

これらの2つの関数はセットの名前の追加と削除の面倒を見てくれる。既に解答から2つの内側のループが切り出された:

```javascript
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
```

もしそう呼んでいいなら、完全な実装だ。

なぜ`addToSet`と`removeFromSet`はなぜセットを引数として取るのか？もし望めば、これらは`livingCats`変数を直接使うことができる。理由はこの方法は現在の問題と完全に結びついていないためだ。もし`addToset`が直接`livingCats`を変更したら、それは`addCatsToCatSet`とかそのような名前で呼ばれるべきだ。この方法により今、これはより一般的で便利なツールとなっている。

もしこれらの関数を他の問題に使うことがないと十分に証明されているのであば、そのように書いておくのも便利だ。それらは'自己充足的'であり、それ自体で読み理解することが可能で、`livingCats`といった外部の変数について知る必要もないからだ。

この関数は純粋ではない：これらの`set`引数として渡されたオブジェクトを変更する。これらは本当の純粋な関数より脆くトリッキーなものとなるが、しかし、うまく動かない、または望む任意の値や変数を変更する関数よりはまだ混乱しにくい。

* * *

アルゴリズムの分解を続けよう:

```javascript
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
```

アルゴリズムの全体が関数によって、カプセルの中に封じ込められた。これは実行した後ゴミの山が残らないということを意味する：`livingCats`は今やトップレベルのものでであるかわりに、関数の中のローカル変数となり、関数の実行中にしか存在しなくなった。このセットに必要なのは`findLivingCats`を呼び出しその戻り値を使うコードだけだ。

私には`handleParagraph`もまた関数に分割した方が分りやすいように思える。しかしこれは猫アルゴリズム密接に結びついていて他の状況では意味がなくなる。その上、それは`livingCats`にアクセスする必要がある。それゆえ、それは関数内関数になる完全な候補だ。それが`findLivingCats`の中で生きているとき、そこにだけ関係するのは明らかで、その親の関数の変数にだけアクセスを持つ。

この解答は先の解答より実際に**大きい**。まだ、それを小さくそして読みやすくすることに同意してくれると望む。

* * *

このプログラムはまだ電子メールに含まれている多くの情報を無視している。誕生日、死亡日、母猫の名前などだ。

日付から始めよう：日付を格納するのに良い方法は何か？`year`、`month`、`day`の3つのプロパティを持つオブジェクトを作ることができ、それらの数値を格納することができる。

```javascript
var when = {year: 1980, month: 2, day: 1};
```

しかしJavaScriptは既にこの目的のためのオブジェクトの種類を提供している。そのようなオブジェクトを`new`キーワードを使って作ることができる。:

```javascript
var when = new Date(1980, 1, 1);
show(when);
```

括弧とコロンによる表記のようなものは既に見てきた通り、`new`はオブジェクトの値を作り出す方法だ。全てのプロパティに名前と値を指定する代わりに、関数を使ってオブジェクトを作ることができる。これはオブジェクトを作るための標準的な処理の種類を定義することを可能にする。このような関数はコンストラクタと呼ばれる。[8章]({{ "/Object-oriented Programming.html" | prepend:site.baseurl }})でその書き方を見よう。

`Date`コンストラクタには幾つかの異なる用法がある。

```javascript
show(new Date());
show(new Date(1980, 1, 1));
show(new Date(2007, 2, 30, 8, 20, 30));
```

見ての通り、これらのオブジェクトは日付と同様に日時を格納することができる。引数が与えられなかったとき、現在の日時を現わすオブジェクトが作られる。引数は特定の日付や時間を与えるのに使われる。引数の順番は年、月、日、時、分、病、ミリ秒である。後ろの4つはオプションで、省略されると0になる。

これらのオブジェクトでの月の番号は0から11で、混乱するかもしれない。特に日の番号は**1から始まる**。

* * *

`Date`オブジェクトの内容は`get...`メソッドの数値として得られる。

```javascript
var today = new Date();
print("Year: ", today.getFullYear(), ", month: ",
      today.getMonth(), ", day: ", today.getDate());
print("Hour: ", today.getHours(), ", minutes: ",
      today.getMinutes(), ", seconds: ", today.getSeconds());
print("Day of week: ", today.getDay());
```

`getDay`を除いたこれら全ては、`set...`の変形を持ち、`date`オブジェクトの値を変更するのに使われる。

オブジェクトの中で、日付は1970年1月1日からのミリ秒の積算として表現される。これはとても大きな数値としてイメージすることができる。

```javascript
var today = new Date();
show(today.getTime());
```

これは日付同士を比較するのに便利だ。

```javascript
var wallFall = new Date(1989, 10, 9);
var gulfWarOne = new Date(1990, 6, 2);
show(wallFall < gulfWarOne);
show(wallFall == wallFall);
// but
show(wallFall == new Date(1989, 10, 9));
```

`<`、`>`、`<=`および`=>`での日付の比較は期待している通り正確である。日付オブジェクトをそれ自身と`==`で比較したら結果は`true`であり、それもまた正しい。しかし`==`を同じ日付を持つ異なるオブジェクトと比較すれば`false`となる。えっ？

先に触れたように、`==`は異なるオブジェクトを比較したときにはそれらが同じプロパティを持っていようと`false`を返す。`>=`および`==`が多かれ少なかれ同様なやり方を持っていることを期待するから、これは少し混乱しやすく誤りやすい。2つの日付が等しいことはこのようにテストする。:

```javascript
var wallFall1 = new Date(1989, 10, 9),
    wallFall2 = new Date(1989, 10, 9);
show(wallFall1.getTime() == wallFall2.getTime());
```

* * *

日付と時刻に加えて言えば、`Date`オブジェクトはタイムゾーンについての情報も含んでいる。アムステルダムが1時の時、時期にもよるが、ロンドンは正午、ニューヨークは朝7時である。タイムゾーンを勘定に入れることによってのみ、このような時刻を比較できる。`Date`の`getTimezoneOffset`関数によってGMT（グリニッジ標準時）から何分差があるかを知ることができる。

```javascript
var now = new Date();
print(now.getTimezoneOffset());
```

* * *

### <a name="Ex4-6">[演習 4.6]</a>

```javascript
"died 27/04/2006: Black Leclère"
```

日付部分は常に段落の同じ場所にある。便利だね。このような段落を引数に取り、日付を展開したdateオブジェクトを返す`extractDate`関数を書け。

[解答を見る]

```javascript
function extractDate(paragraph) {
  function numberAt(start, length) {
    return Number(paragraph.slice(start, start + length));
  }
  return new Date(numberAt(11, 4), numberAt(8, 2) - 1,
                  numberAt(5, 2));
}

show(extractDate("died 27-04-2006: Black Leclère"));
```

これは`Number`を呼び出さなくても動くが、しかし先に触れたように、文字列をそれが数値であるかのように扱うのはよろしくない。内側の関数は`Number`と`slice`の部分を3回繰り返すのを防ぐために導入した。

月の数値の`- 1`に注意。多くの人々のように、エミリー叔母さんも月を1から数え始めるので、`Date`コンストラクタに渡す前にそれを調整する必要がある。（日の数値はこの問題を持たず、よって`Date`オブジェクトも通常の人間通りに日を数える。）

[10章]({{ "/Regular Expressions.html" | pretend:site.baseurl }})において、固定的な構造を持つ文字列を部品に展開する、より実用的で確固たる方法を見よう。

* * *

猫の格納は今までとは異なる動きをするようになる。`true`の値をセットに入れる代わりに、猫に関する情報をオブジェクトに格納する。猫が死んだら、セットから猫を取り除くのではなく、猫が死んだ日を格納する`death`プロパティをオブジェクトに追加する。

これは`addToSet`と`removeFromSet`関数が使えないものになることを意味する。同じような、しかし誕生日とその後と、母猫の名前も格納するものが必要だ。

```javascript
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
```

`catRecord`は格納オブジェクトを分離する関数だ。それはSpotのオブジェクトを作るような他の状況でも便利だ。'Record'はこのようなオブジェクトに対してしばしば使われる用語で、限定された数の値のグループとして使われる。

* * *

段落からの母猫の名前の展開に挑戦しよう。

```javascript
"born 15/11/2003 (mother Spot): White Fang"
```

1つの手としてはこのようになる...。

```javascript
function extractMother(paragraph) {
  var start = paragraph.indexOf("(mother ") + "(mother ".length;
  var end = paragraph.indexOf(")");
  return paragraph.slice(start, end);
}

show(extractMother("born 15/11/2003 (mother Spot): White Fang"));
```

`IndexOf`はパターンの終了位置ではなく開始位置を返すため、開始位置を`"(mother "`の長さで調整することに注意しよう。

* * *

### <a name="Ex4-7">[演習 4.7]</a>

`extractMother`はより一般的なやりかたで行うことができる。3つの引数を取る`between`関数を書け。それらの引数は全て文字列であり、1つめの引数から2つめと3つめの引数で与えられたパターンの間の部分を返す。

`between("born 15/11/2003 (mother Spot): White Fang", "(mother ", ")")`であれば`"Spot"`。`between("bu ] boo [ bah ] gzz", "[ ", " ]")`であれば`"bah"`を返すように。

2つめのテストが動くようにするには、`indexOf`の2つめの、探し始める位置を指定するオプショナルな引数が便利だ。

[解答を見る]

```javascript
function between(string, start, end) {
  var startAt = string.indexOf(start) + start.length;
  var endAt = string.indexOf(end, startAt);
  return string.slice(startAt, endAt);
}
show(between("bu ] boo [ bah ] gzz", "[ ", " ]"));
```

* * *

`between`があれば`extractMother`を簡単に書けるようになる。:

```javascript
function extractMother(paragraph) {
  return between(paragraph, "(mother ", ")");
}
```

* * *

新しい、猫アルゴリズムの実装はこのようになる。:

```javascript
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
```

拡張したデータによりついにエミリー叔母さんが話すことについての手がかりを得ることができるようになった。このような関数は便利になりうる。:

```javascript
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
```

1つめの`catInfo`の`return`文は脱出口として使われる。与えられた名前の猫のデータがなければ、関数の残りの部分は意味がないので、即座に値を返し、残りのコードが実行されるのを防止する。

過去において、プログラマーのグループは複数の`return`文を含む関数は犯罪的だと確かに考えていた。このアイデアはどのコードが実行され、どのコードが実行されなかったかを見るのを困難にする。[5章]({{"/Error Handling.html" | pretend:site.baseurl }})にて論じる他のテクニックはこのアイデアの背景となる理由を多かれ少なかれ時代遅れにしたが、しかし'shortcut'な`return`文の使用を犯罪であるかのように考える人にもまだ時折出会うことだろう。

* * *

### <a name="Ex4-8">[演習 4.8]</a>

`catInfo`で使われた`formatDate`関数は月と日が1桁の長さしか持たない場合でも、その前にゼロを追加しない。それを行う新しいバージョンを書け。

[解答を見る]

```javascript
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
```

* * *

### <a name="Ex4-9">[演習 4.9]</a>

引数として与えられた猫のリストを含むオブジェクトから生存中で最も高齢の猫の名前を返す`oldestCat`関数を書け。

[解答を見る]

```javascript
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
```

`if`文の条件が少々怖く見えたかも知れない。これは'今処理中の猫が死んでおらず、かつ`oldest`が`null`または処理中の猫が`oldest`の猫より高齢な時だけ、処理中の猫を`oldest`変数の中に格納する'と読める。

この関数は生きている猫が`data`の中にいなければ`null`を返すことに注意。あなたの解答ではそのような場合どうなるだろうか？

* * *

もう配列には慣れたので、それに関係するものを見せよう。関数が呼ばれたとき、`arguments`という特殊な変数が関数の本体の実行中に環境に追加される。この変数はオブジェクトを配列であるかのように参照する。`0`は1つめのプロパティ、`1`は2つめといったように、関数に与えられた全ての引数が処理される。それはまた`length`プロパティも持つ。

このオブジェクトは本物の配列ではなく、`push`のようなメソッドは持たないし、`length`プロパティのように何か追加されたときに自動的に更新されたりもしない。それがなぜなのか、私は本当のところを知らないが、しかしこれには注意が必要だ。

```javascript
function argumentCounter() {
  print("You gave me ", arguments.length, " arguments.");
}
argumentCounter("Death", "Famine", "Pestilence");
```

`print`のように、関数は任意の数の引数を取ることもできる。これらは典型的には`arguments`オブジェクトの値に何らかの処理を行いながらループする。他のものは呼び出し元から与えられなかったときに適当なデフォルトの値になる、オプショナルな引数を取る。

```javascript
function add(number, howmuch) {
  if (arguments.length < 2)
    howmuch = 1;
  return number + howmuch;
}

show(add(6));
show(add(6, 4));
```

* * *

### <a name="Ex4-10">[演習 4.10]</a>

[演習4.2](#Ex4-2)の`range`関数を2つめの引数をオプションにできるように拡張しよう。もし与えられた引数が一つしかなければ、先ほどのように0から与えられた数までの範囲を作るようにするのである。もし引数が2つ与えられたら、1つめを開始、2つめを終了とする。

[解答を見る]

```javascript
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
```

オプションの引数は上記の`add`の例と全く同じようには動かない。それが与えられなかったとき、1つめの引数は`end`の役となり`start`は`0`となる。

* * *

### <a name="Ex4-11">[演習 4.11]</a>

導入でのこの1行のコードを覚えているだろう。:

```javascript
print(sum(range(1, 10)));
```

`range`を今得た。この行を動かすために必要なのは`sum`関数である。この関数は数値の配列を取って、それらの合計を返す。簡単だろうから、それを書け。

[解答を見る]

```javascript
function sum(numbers) {
  var total = 0;
  for (var i = 0; i < numbers.length; i++)
    total += numbers[i];
  return total;
}

print(sum(range(1, 10)));
```

* * *

前の章で`Math.max`関数と`Math.min`を見た。今知っていることから、これらが本当は`Math`という名のもとで格納されている`max`と`min`プロパティであることに気づいただろう。これはオブジェクトのもう1つの役割である：関連するいくつもの値を格納する倉庫である。

`Math`の中には実に多くの値があり、もしそれらが呼び出されれば、それらはグローバルな環境の中に直接場所を取ってその場所を汚す。より多くの名前を取れば、何かに、例えば、`max`といった名前を付けたくなることはなさそうなことではなく、変数の値が誤って上書きされてしまうことがありえる。

既に取られている変数を定義しようとしたとき、多くの言語ではあなたを止めたり、警告したりする。しかしJavaScriptは異なる。

いずれにせよ、`Math`内の数学的な関数や定数の完全な一揃いを見つけることができる。全ての三角関数 -- `cos`、`sin`、`tan`、`acos`、`asin`、`atan`。πとeは大文字（`PI`と`E`）で書かれており、一時は、これはそれが定数であることを示すファッショナブルな方法であった。`pow`は先ほどまでに書いた`power`関数の良い代替であり、負の数や少数の累乗も受け付ける。`sqrt`は平方根だ。`max`と`min`は2つの値の中で最大または最小のものを与える。`round`、`floor`および`ceil`は値を四捨五入したり、切り捨てたり、切り上げたりする。

`Math`の中には他にも多くの値が含まれるが、しかしこのテキストは導入であり、リファレンスではない。リファレンスは言語にあるものを探すことはできるが、必要なのはそれがどのように呼び出され、正確にはどのように働くかということである。残念ながら、JavaScript向けの網羅的で完全なリファレンスというものはない。これはその現在の形が、ブラウザの違いや拡張機能の違いや時によって結果的に混沌としているためというのが大きい。ECMA標準ドキュメントは言語の基本のよいドキュメントの導入を提供するが、しかし多かれ少なかれ読みやすいとは言えない。`Math`オブジェクトや他の要素機能のようなもののよいリファレンスは、むしろ[ここ](http://www.webreference.com/javascript/reference/core_ref/contents.html)で見つけることができるだろう。Netscapeからの古いドキュメントはまだ[Sunのウェブサイト](http://docs.sun.com/source/816-6408-10)で見つけることができ、とても助けになるが、しかし期限切れになっていてもう正しいとは言えない。

* * *

`Math`オブジェクトで何が使えるか調べる方法をすでに知っているかもしれない:

```javascript
for (var name in Math)
  print(name);
```

残念だが、何も出てこない。同様に、このように書いても:

```javascript
for (var name in ["Huey", "Dewey", "Loui"])
  print(name);
```

期待されるような`length`や`push`、`join`ではなく、`0`、`1`および`2`しか出てこない。見ての通り、プロパティやオブジェクトは隠されている。それにはこのような良い理由がある：全てのオブジェクトは、例えばオブジェクトを関連のある文字列に変換する`toString`のような少数のメソッドを持ち、例えばオブジェクトの中から格納されている猫を探すようなことはしないで良い。

なぜ`Math`のプロパティが隠されているのかは私にも分らない。誰かがそれをミステリアスな種類のオブジェクトにしようと望んだのだ。

あなたのプログラムが追加するオブジェクトの全てのプロパティは見ることができる。それらを隠す手段は残念ながらなく、それがなぜかは[8章]({{ "/Object-oriented Programming.html" | pretend:site.baseurl }})で見るが、メソッドをオブジェクトに`for`/`in`ループなしに追加できるというのはいいことだ。

* * *

幾つかのプロパティは読み取り専用であり、これらの値を得ることはできるが変更することはできない。例えば、文字列の値のプロパティは全て読み取り専用だ。

他のプロパティは'監視'可能だ。これらを変更したら**何か**が起こる。例えば、配列のlengthを小さくしたら、はみ出た要素が捨てられてしまう。:

```javascript
var array = ["Heaven", "Earth", "Man"];
array.length = 2;
show(array);
```

ブラウザによっては、オブジェクトは、それ自身のプロパティを監視するものを追加する`watch`メソッドを持っている。Internet Explorerはこれをサポートしていないので、全ての'大きな'ブラウザで実行する必要があるプログラムを書くときには使いづらい。

```javascript
var winston = {mind: "compliant"};
function watcher(propertyName, from, to) {
  if (to == "compliant")
    print("Doubleplusgood.");
  else
    print("Transmitting information to Thought Police...");
}
winston.watch("mind", watcher);

winston.mind = "rebellious";
```
