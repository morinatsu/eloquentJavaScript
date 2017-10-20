---
layout: default
title: 12. ドキュメント－オブジェクト・モデル
---

ドキュメント－オブジェクト・モデル
=====================================================

HTML文書のフォームとinputタグを参照するJavaScriptオブジェクトを11章で見た。それらのオブジェクトはドキュメント－オブジェクト・モデル（DOM）と呼ばれる構造の一部である。このモデルで文書の全てのタグは表現され、見つけ出され、操作される。

HTMLドキュメントは階層構造と呼ばれるものを持つ。トップの`<html>`タグを除く、それぞれの要素（またはタグ）は他の要素、その親に含まれる。この要素は逆に子要素を含むことができる。これは家系図のように視覚化できる。：

![html]({{ "/assets/img/html.png" | prepend:site.baseurl }})

ドキュメント－オブジェクト・モデルはこのような文書の見方に基づいている。ツリーが2つのタイプの要素を含んでいることに注意：ノード、青い箱として描かれていている、単純なテキストの部品である。テキストの部品は、見たとおり、他の要素とは何か違った働きをする。1つには、これらは子要素を持つことが無い。

図に描かれた文書を含むファイル、`example_alchemy.html`を開き、コンソールに繋げてみよう。

```javascript
attach(window.open("example_alchemy.html"));
```

文書のツリーのルートのオブジェクト、htmlノードは、documentオブジェクトのdocumentElementプロパティを通じて到達できる。多くの間、文書の代わりに、そのボディ部分にアクセスする必要があって、それはdocument.bodyとなる。

これらのノードの間はnodeオブジェクトのプロパティでリンクできる。全てのDOMオブジェクトは、そのオブジェクトを含むオブジェクトがあれば、それを参照するparentNodeプロパティを持つ。これらの親要素もまたそれらの子要素を指し返すリンクを持つが、子要素は1つより多いことがあり得るので、それはchildNodesと呼ばれる疑似配列に格納される。

```javascript
show(document.body);
show(document.body.parentNode);
show(document.body.childNodes.length);
```

利便性のため、firstChildとlastChildというリンクもあって、ノードの最初と最後の子要素を指している。あるいは、子要素が無ければnullになる。

```javascript
show(document.documentElement.firstChild);
show(document.documentElement.lastChild);
```

最後に、nextSiblingとpreviousSiblingというプロパティもあって、そのノードの'次の'ノードを指している -- 同じ親要素の子要素のノードで、現在のノードの直前と直後である。繰り返すが、そのような兄弟がなければ、これらのプロパティもnullとなる。

```javascript
show(document.body.previousSibling);
show(document.body.nextSibling);
```

あるノードが単純なテキストの部品か、実際のHTMLノードかを見分けるために、nodeTypeプロパティを見ることができる。これは数値を含み、1なら正規のノード、3ならテキストのノードである。実際には、documentオブジェクトが9であるように、他の種類のオブジェクトタイプもある。しかしこのプロパティのもっとも一般的な使い途はテキストのノードとそれ以外を識別することである。

```javascript
function isTextNode(node) {
  return node.nodeType == 3;
}

show(isTextNode(document.body));
show(isTextNode(document.body.firstChild.firstChild));
```

正規のノードはnodeNameと呼ばれるプロパティを持っていて、それらが表現しているHTMLタグを示している。テキストのノードは他方、nodeValueを持ち、これにはテキストの内容が含まれる。

```javascript
show(document.body.firstChild.nodeName);
show(document.body.firstChild.firstChild.nodeValue);
```

nodeNameは常に大文字であり、もし何かと比較したいなら、そのことを勘定に入れる必要がある。

```javascript
function isImage(node) {
  return !isTextNode(node) && node.nodeName == "IMG";
}

show(isImage(document.body.lastChild));
```

### <a name="Ex12-1">[演習 12.1]</a>

asHTML関数を書け。DOMノードを与えられたら、ノードと子のノードをHTMLテキストとしての表現である文字列を生成する。属性は無視して良く、ただノードを<ノード名>とすればよい。10章のescapeHTML関数をテキストのノードの内容をエスケープするために使えるものとする。

ヒント：再帰！

```javascript
function asHTML(node) {
  if (isTextNode(node))
    return escapeHTML(node.nodeValue);
  else if (node.childNodes.length == 0)
    return "<" + node.nodeName + "/>";
  else
    return "<" + node.nodeName + ">" +
           map(asHTML, node.childNodes).join("") +
           "</" + node.nodeName + ">";
}

print(asHTML(document.body));
```

ノードは、実際、既にasHTMLと同じようなものを持っている。これらのinnerHTMLプロパティはノードの中のHTMLテキストを取り出すのに使えるが、ノード自体のタグは含まれない。あるブラウザはouterHTMLもサポートしていて、これはノードそれ自体を含むが、ブラウザ全てではない。

```javascript
print(document.body.innerHTML);
```

これらのプロパティのあるものは変更可能でもある。ノードのinnerHTMLかテキストノードのnodeValueを設定することでその内容を変更できる。1つめの場合、与えられた文字列はHTMLとして解釈され、2つめの場合はプレーンテキストとして解釈されることに注意。

```javascript
  document.body.firstChild.firstChild.nodeValue =
    "Chapter 1: The deep significance of the bottle";
```

または...

```javascript
document.body.firstChild.innerHTML =
  "Did you know the 'blink' tag yet? <blink>Joy!</blink>";
```

我々はfirstChildとlastChildのシリーズを通じてノードにアクセスしてきた。これはこれで動くのだが、冗長で壊れやすい -- もしもう1つのノードを文書の最初に追加したら、document.body.firstChildはh1要素を参照しなくなって、それを前提にしてきたコードは誤りとなる。その上、ブラウザによってはタグ同士の間の空白や改行といったものにもテキストノードを追加し、他のブラウザではそうならないので、DOMツリーの正確なレイアウトは変わってしまうことがある。

代替の手段は、必要な要素にid属性を与えてアクセスすることだ。例のページでは、画像が"picture"というidを持っていて、これで見つけることができる。

```javascript
var picture = document.getElementById("picture");
show(picture.src);
picture.src = "img/ostrich.png";
```

getElementByIdとタイプするとき、最後の文字は小文字であることに注意。たくさんタイプすると、手根管症候群にもなる。document.getElementByIdはとても頻繁に使われる操作にはひどく長すぎる名前なので、JavaScriptプログラマの利便性のために、強力に省略して$としている。$は、覚えているだろうが、JavaScriptでは文字として考えられ、すなわち正しい変数名である。

```javascript
function $(id) {
  return document.getElementById(id);
}
show($("picture"));
```

DOMノードはgetElementByTagNameメソッドも持っていて（もう一つのナイスな、短い名前）、これは、タグ名を与えられたとき、そのメソッドが呼び出されたノードに含まれる、その型のノードの配列を返す。

```javascript
show(document.body.getElementsByTagName("BLINK")[0]);
```

もう一つできることとして、これらのDOMノードに新しいものを作ることもできる。これで文書に意のままに部品を追加することが可能になり、面白い効果を作るのに使える。残念ながら、これを行うためのインターフェースはとてもややこしい。しかしヘルパー関数を作ることで手当てできる。

documentオブジェクトはcreateElementとcreateTextNodeメソッドを持っている。1つめは正規のノードを作るため、2つめは、名前の通り、テキストのノードを作るためのものだ。

```javascript
var secondHeader = document.createElement("H1");
var secondTitle = document.createTextNode("Chapter 2: Deep magic");
```

次は、タイトル名をh1要素に入れ、そして文書に要素を追加したい。単純なやり方としてはappendChildメソッドであり、全ての（テキストでない）ノードで呼び出すことができる。

```javascript
secondHeader.appendChild(secondTitle);
document.body.appendChild(secondHeader);
```

時には、これらの新しいノードに属性も付けたくなるだろう。例えば、img（イメージ）タグはブラウザに表示すべき画像を知らせるsrcプロパティが無ければむしろ意味が無い。多くの属性はDOMノードのプロパティとして直接アプローチできるが、setAttributeとgetAttributeというメソッドもあり、これらでアクセスする方が一般的だ。

```javascript
var newImage = document.createElement("IMG");
newImage.setAttribute("src", "img/Hiva Oa.png");
document.body.appendChild(newImage);
show(newImage.getAttribute("src"));
```

しかし、もっと単純なノードを組み立てたいとき、単一のノード毎にいちいちdocument.createElementやdocument.createTextNodeを呼び出し、その属性や子のノードを一つ一つ追加するというのでは、とてもうんざりする。幸運にも、この仕事の多くをやってくれる関数を書くのは難しくない。それをやる前に、注意しなければならない詳細がある -- setAttributeメソッド、多くのブラウザでは上手く動くが、InternetExplorerでは常に動くとは限らない。HTML属性の名前にはJavaScriptでも特別な意味を持つものがあって、対応するオブジェクトのプロパティの名前は調整されている。具体的には、class属性がclassName、forはHTMLfor、checkedはdefaultCheckedになっている。InternetExplorerではsetAttributeとgetAttributeは元のHTMLの名前の代わりに、これらの調整された名前でも動き、混乱することがある。その上、classに結びついたstyle属性は、この章の後ろの方で論じるが、そのブラウザのsetAttributeでは設定できない。

とっかかりとしてはこのようになるだろう。：

```javascript
function setNodeAttribute(node, attribute, value) {
  if (attribute == "class")
    node.className = value;
  else if (attribute == "checked")
    node.defaultChecked = value;
  else if (attribute == "for")
    node.htmlFor = value;
  else if (attribute == "style")
    node.style.cssText = value;
  else
    node.setAttribute(attribute, value);
}
```

InternetExplorerが他のブラウザから逸脱している全ての場合で、何かの作業をする。細かいことは気にしない -- これはむしろ避けたい醜いトリックの類であるが、順応性の無いブラウザが我々に書くことを強制するのだ。これで、DOM要素を組み立てる単純な関数を書くことができる。

```javascript
function dom(name, attributes) {
  var node = document.createElement(name);
  if (attributes) {
    forEachIn(attributes, function(name, value) {
      setNodeAttribute(node, name, value);
    });
  }
  for (var i = 2; i < arguments.length; i++) {
    var child = arguments[i];
    if (typeof child == "string")
      child = document.createTextNode(child);
    node.appendChild(child);
  }
  return node;
}

var newParagraph =
  dom("P", null, "A paragraph with a ",
      dom("A", {href: "http://en.wikipedia.org/wiki/Alchemy"},
          "link"),
      " inside of it.");
document.body.appendChild(newParagraph);
```

dom関数はDOMノードを作る。1つめの引数はノードのタグ名で、2つめの引数はノードの属性を含むオブジェクトか、ノードに属性が不要の場合はnullである。その後に、任意の数の引数が続き、これらはそのノードの子のノードとして追加される。これが文字列だったときは、まずテキストのノードに入れられてから追加される。

ノードを他のノードに挿入する方法はappendChildだけではない。新しいノードがその親の最後にあるのでなければ、insertBeforeメソッドで他の子ノードの前に入れることができる。これは新しいノードを1つめの引数に、存在する子ノードを2つめの引数として取る。

```javascript
var link = newParagraph.childNodes[1];
newParagraph.insertBefore(dom("STRONG", null, "great "), link);
```

もし既にparentNodeを持つノードをどこかに移動したら、それは自動的に今の位置から取り除かれる -- ノードは文書中で複数の場所には存在できないのだ。

ノードを他のノードで置き換えるときはreplaceChildメソッドを使い、これは1つめの引数に新しいノード、存在するノードを2つめの引数に取る。

```javascript
newParagraph.replaceChild(document.createTextNode("lousy "),
                          newParagraph.childNodes[1]);
```

そして、最後、子ノードを取り除くremoveChildがある。これは取り除くノードの親から呼ばれ、子を引数として与えることに注意。

```javascript
newParagraph.removeChild(newParagraph.childNodes[1]);
```

### <a name="Ex12-2">[演習 12.2]</a>

引数として与えられたノードの親ノードからDOMノードを取り除く便利な関数removeElementを書け。

```javascript
function removeElement(node) {
  if (node.parentNode)
    node.parentNode.removeChild(node);
}

removeElement(newParagraph);
```

新しいノードを作るときとノードを移動するとき、このルールに従う必要がある：ノードはそれが作られた文書から、もう一つの文書に挿入することはできない。これはもし外部のフレームがあったり、ウインドウをオープンしたら、文書の部品を一方から取り上げてもう一方に移すことはできず、あるdocumentオブジェクトのメソッドで作られたノードはその文書の中に留めておかなければならない。ブラウザによっては、特にFirefoxでは、この制約は強制されておらず、このブラウザでは制約に違反しているプログラムは動くが他では動かない。

このdom関数でできる有益なものの例としては、JavaScriptオブジェクトをテーブルに要約するプログラムがある。テーブルは、HTMLでは、tで始まるタグの集合で作られる、このようなものだ。

```html
<table>
  <tbody>
    <tr> <th>Tree </th> <th>Flowers</th> </tr>
    <tr> <td>Apple</td> <td>White  </td> </tr>
    <tr> <td>Coral</td> <td>Red    </td> </tr>
    <tr> <td>Pine </td> <td>None   </td> </tr>
  </tbody>
</table>
```

それぞれのtr要素はテーブルの行だ。thとtdはテーブルのセルで、tdは通常のデータのセル、thセルは'ヘッダー'のセルで、やや目立つように表示される。tbody(テーブルのボディ)タグはHTML上のテーブルを書く上で必ず含めなければならないものではないが、DOMノードからテーブルを組み立てるときは追加され、それはInternetExplorerがtbodyなしに作られたテーブルの表示を断るからである。

### <a name="Ex12-3">[演習 12.3]</a>

関数makeTableは2つの配列を引数に取る。1つめは要約すべきJavaScriptオブジェクト、そして2つめは列の名前と表示される列のオブジェクトのプロパティの文字列だ。例えば、上記の表は下記から作られる。：

```javascript
makeTable([{Tree: "Apple", Flowers: "White"},
           {Tree: "Coral", Flowers: "Red"},
           {Tree: "Pine",  Flowers: "None"}],
          ["Tree", "Flowers"]);
```

この関数を書け。

```javascript
function makeTable(data, columns) {
  var headRow = dom("TR");
  forEach(columns, function(name) {
    headRow.appendChild(dom("TH", null, name));
  });

  var body = dom("TBODY", null, headRow);
  forEach(data, function(object) {
    var row = dom("TR");
    forEach(columns, function(name) {
      row.appendChild(dom("TD", null, String(object[name])));
    });
    body.appendChild(row);
  });

  return dom("TABLE", null, body);
}

var table = makeTable(document.body.childNodes,
                      ["nodeType", "tagName"]);
document.body.appendChild(table);
```

テーブルに追加する前にオブジェクトから文字列に値を変換するのを忘れないように -- 我々のdom関数は文字列とDOMノードしか理解できないのだ。

HTMLとドキュメント -- オブジェクトモデルはスタイルシートのトピックに密接に結びついている。大きな問題で、語り尽くすことはできないが、スタイルシートのある程度の理解は多くの面白いJavaScriptのテクニックに必要なので、基本的なところを見ておこう。

古いファッションのHTMLでは、文書の要素の外見を変更するには、水平方向での中央に置くためのcenter、フォントのスタイルや色を変更するfontのような、拡張の属性を与えるか拡張のタグでそれらを囲む方法しかなかった。多くの時間が、これは文書の中に、段落やテーブルが決まった外見になるようにすることが必要なとき、属性の束とタグを全ての単一のタグの一つ一つに追加しなければならなかった。これは文書に多くのノイズを早く追加することになり、文書を手作業で変更する作業を、とても辛いものにする。

もちろん、発明するサルである人間は、解決法を編み出した。スタイルシートは'この文書にある、全ての段落はComic Sansフォントを使い、紫色、そして全てのテーブルは細い緑色の境界線を使う'というような書き方をする。一度、文書の冒頭か、別のファイルで指定すると、文書全体に効果が及ぶ。ここに例として、ヘッダーを22ポイントの大きさで中央揃え、段落が'ugly'クラスであれば先ほどのフォントと色にする、スタイルシートを挙げる。

```css
<style type="text/css">
  h1 {
    font-size: 22pt;
    text-align: center;
  }

  p.ugly {
    font-family: Comic Sans MS;
    color: purple;
  }
</style>
```

クラスはスタイルに関する概念である。例えば醜いのと良いのというように、もし段落の種類が異なっていて、全てのp要素にスタイルを設定したくないとしたら、クラスを使ってでそれらを識別することができる。上記のスタイルはこのような段落にしか適用されない。：

```html
<p class="ugly">Mirror, mirror...</p>
```

そしてこれがclassNameプロパティについてsetNodeAttribute関数で少々言及したことの意味でもある。style属性は要素にスタイルの部品を直接追加するのに使える。例えば、これは、画像に立体的な境界線を4ピクセル（'px'）の幅で与える。

```javascript
setNodeAttribute($("picture"), "style",
                 "border-width: 4px; border-style: solid;");
```

スタイルについてはまだある。：あるスタイルは親ノードから子ノードへ継承され、複雑で面白いやりかたで互いに邪魔しあうが、しかしDOMプログラミングの目的において、最も重要なことはそれぞれのDOMノードがstyleプロパティを持つこと、それらのノードのstyleを操作できること、いくつかの種類のスタイルはノードをおかしくするかもしれないということを知っておくことだ。

このstyleプロパティは、全ての可能なスタイルの要素のプロパティを持つオブジェクトを参照する。例えば、画像の境界線を緑色にできる。

```javascript
$("picture").style.borderColor = "green";
show($("picture").style.borderColor);
```

スタイルシートでは、語はハイフンで分割され、JavaScriptでは、borderColorのように大文字が単語の間の境界として使われることに注意しよう。

とても実用的なスタイルの種類としてdisplay: noneがある。これは一時的にノードを隠すのに使える。：style.displayが"none"のとき、要素が存在しているにもかかわらず、文書のどこにも要素は表示されなくなる。後から、displayに空の文字列を設定することで、その要素は再び現われる。

```javascript
$("picture").style.display = "none";
```

そして、画像を元に戻すには：

```javascript
$("picture").style.display = "";
```

面白いやり方で悪用されうる、もう1つのスタイルのタイプの集合は位置取りに関するものだ。単純なHTML文書では、全ての要素のスクリーン上の表示位置の決定はブラウザによって行われる -- それぞれの要素は直前の要素の次か下に配置され、ノードは（一般的には）重ね書きされない。

そのpositionスタイルが"absolute"に設定されると、ノードは"flow"する普通の文書でなくなる。文書上で場所を取らなくなって、その上に浮かぶ。leftとtopスタイルがその位置を動かすのに使われる。不快にもマウスカーソルを追い掛けてくるノードを作ることから、'ウインドウ'を文書の残りのトップで開くようにすることまで、様々な目的で使われる。

```javascript
$("picture").style.position = "absolute";
var angle = 0;
var spin = setInterval(function() {
  angle += 0.1;
  $("picture").style.left = (100 + 100 * Math.cos(angle)) + "px";
  $("picture").style.top = (100 + 100 * Math.sin(angle)) + "px";
}, 100);
```

角度を測るのが苦手なら、私の言うことをただ信じてほしい。コサインとサインは円周の座標を組み立てるのに使える。秒ごとに10回、画像の場所の角度が変わり、新しい座標が計算される。ありがちな間違いは、このようにスタイルを設定したとき、"px"を値に追加するのを忘れることだ。多くの場合、単位なしの数値で設定されたスタイルは上手く動かない、なのでピクセルなら"px"を、パーセントなら"%"を、'em'（M文字の幅）なら"em"を、ポイントなら"pt"を追加しなければならない。

（もう画像を元に戻そう...）

```javascript
clearInterval(spin);
```

これらの位置の目的で0,0として扱われる場所は、文書上のノードの場所に依存している。position:absolute またはposition: relativeを持つノードがもう一つのノードの中に移されたとき、このノードの左上が使われ、そうでなければ、文書の左上になる。

遊べるDOMノードの面の最後の1つはそのサイズだ。widthとheightというスタイルのタイプがあって、要素の絶対的なサイズを設定するのに使える。

```javascript
$("picture").style.width = "400px";
$("picture").style.height = "200px";
```

しかし、要素のサイズを正確に設定したいとき、計算上のトリッキーな問題がある。あるブラウザ、ある環境では、これらのサイズをオブジェクトの外側の、境界線や内部の余白まで含めた意味と取る。他のブラウザ、他の環境では、代わりにオブジェクトの中のスペースのサイズ使われ、そして境界線や余白を持つオブジェクトの幅はカウントされない。つまり、境界線や余白を持つオブジェクトのサイズを設定しても、常に同じサイズで表示されない。

幸運にも、ノードの内側と外側のサイズを見ることができ、本当に正確なサイズが必要なときには、ブラウザの振る舞いを相殺することができる。offsetWidthとoffsetHeightプロパティは要素の外側のサイズ（文書の中でそれが取るスペース）を与え、clientWidthとclientHeightプロパティは、もしそれがあれば、その内側のスペースを与える。

```javascript
print("Outer size: ", $("picture").offsetWidth,
      " by ", $("picture").offsetHeight, " pixels.");
print("Inner size: ", $("picture").clientWidth,
      " by ", $("picture").clientHeight, " pixels.");
```

もしこの章の全ての例をやり遂げたら、いくつかの追加を自分でやってみて、我々がスタートにした貧しく小さな文書を完全に壊してみよう。少々忠告するが、本物のページにこれを行おうとしないように。全ての種類のキラキラした動きを追加する誘惑は時に強いものだ。耐えなさい。でなければ、ページが確実に読みにくくなる。もしあなたが十分遠くに行ったら、時々発作が起こるだろう。
