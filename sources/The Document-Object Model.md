=====================================================
The Document-Object Model
=====================================================
..  Chapter 12:
..  The Document-Object Model

..  In chapter 11 we saw JavaScript objects referring to form and input tags from the HTML document. Such objects are part of a structure called the Document-Object Model (DOM). Every tag of the document is represented in this model, and can be looked up and interacted with.

HTML文書のフォームとinputタグを参照するJavaScriptオブジェクトを11章で見た。それらのオブジェクトはドキュメント－オブジェクト・モデル（DOM）と呼ばれる構造の一部である。このモデルで文書の全てのタグは表現され、見つけ出され、作用される。

..  HTML documents have what is called a hierarchical structure. Each element (or tag) except the top <html> tag is contained in another element, its parent. This element can in turn contain child elements. You can visualise this as a kind of family tree:

HTMLドキュメントは階層構造と呼ばれるものを持つ。トップの<html>タグを除く、それぞれの要素（またはタグ）は他の要素、その親に含まれる。この要素は逆に子要素を含むことができる。これは家系図のように視覚化できる。：

.. image:: img/html.png

..  The document-object model is based on such a view of the document. Note that the tree contains two types of elements: Nodes, which are shown as blue boxes, and pieces of simple text. The pieces of text, as we will see, work somewhat different than the other elements. For one thing, they never have children.

ドキュメント－オブジェクト・モデルはこのような文書の見方に基づいている。ツリーが2つのタイプの要素を含んでいることに注意：ノード、青い箱として描かれていている、単純なテキストの部品である。テキストの部品は、見たとおり、他の要素とは何か違った働きをする。1つには、これらは子要素を持つことが無い。

..  Open the file example_alchemy.html, which contains the document shown in the picture, and attach the console to it.

図に描かれた文書を含むファイル、example_alchemy.htmlを開き、コンソールに繋げてみよう。

.. code-block:: javascript

    attach(window.open("example_alchemy.html"));

..  The object for the root of the document tree, the html node, can be reached through the documentElement property of the document object. Most of the time, we need access to the body part of the document instead, which is at document.body.

文書のツリーのルートのオブジェクト、htmlノードは、documentオブジェクトのdocumentElementプロパティを通じて到達できる。多くの間、文書の代わりに、そのボディ部分にアクセスする必要があって、それはdocument.bodyとなる。

..  The links between these nodes are available as properties of the node objects. Every DOM object has a parentNode property, which refers to the object in which it is contained, if any. These parents also have links pointing back to their children, but because there can be more than one child, these are stored in a pseudo-array called childNodes.

これらのノードの間はnodeオブジェクトのプロパティでリンクできる。全てのDOMオブジェクトは、そのオブジェクトを含むオブジェクトがあれば、それを参照するparentNodeプロパティを持つ。これらの親要素もまたそれらの子要素を指し返すリンクを持つが、子要素は1つより多いことがあり得るので、それはchildNodesと呼ばれる疑似配列に格納される。

.. code-block:: javascript

    show(document.body);
    show(document.body.parentNode);
    show(document.body.childNodes.length);

..  For convenience, there are also links called firstChild and lastChild, pointing at the first and last child inside a node, or null when there are no children.

利便性のため、firstChildとlastChildというリンクもあって、ノードの最初と最後の子要素を指している。あるいは、子要素が無ければnullになる。

.. code-block:: javascript

    show(document.documentElement.firstChild);
    show(document.documentElement.lastChild);

..  Finally, there are properties called nextSibling and previousSibling, which point at the nodes sitting 'next' to a node ― nodes that are children of the same parent, coming before or after the current node. Again, when there is no such sibling, the value of these properties is null.

最後に、nextSiblingとpreviousSiblingというプロパティもあって、そのノードの'次の'ノードを指している--同じ親要素の子要素のノードで、現在のノードの直前と直後である。繰り返すが、そのような兄弟がなければ、これらのプロパティもnullとなる。

.. code-block:: javascript

    show(document.body.previousSibling);
    show(document.body.nextSibling);

..  To find out whether a node represents a simple piece of text or an actual HTML node, we can look at its nodeType property. This contains a number, 1 for regular nodes and 3 for text nodes. There are actually other kinds of objects with a nodeType, such as the document object, which has 9, but the most common use for this property is distinguishing between text nodes and other nodes.

あるノードが単純なテキストの部品か、実際のHTMLノードかを見分けるために、nodeTypeプロパティを見ることができる。これは数値を含み、1なら正規のノード、3ならテキストのノードである。実際には、documentオブジェクトが9であるように、他の種類のオブジェクトタイプもある。しかしこのプロパティのもっとも一般的な使い途はテキストのノードとそれ以外を識別することである。

.. code-block:: javascript

    function isTextNode(node) {
      return node.nodeType == 3;
    }

    show(isTextNode(document.body));
    show(isTextNode(document.body.firstChild.firstChild));

..  Regular nodes have a property called nodeName, indicating the type of HTML tag that they represent. Text nodes, on the other hand, have a nodeValue, containing their text content.

正規のノードはnodeNameと呼ばれるプロパティを持っていて、それらが表現しているHTMLタグを示している。テキストのノードは他方、nodeValueを持ち、これにはテキストの内容が含まれる。

.. code-block:: javascript

    show(document.body.firstChild.nodeName);
    show(document.body.firstChild.firstChild.nodeValue);

..  The nodeNames are always capitalised, which is something you need to take into account if you ever want to compare them to something.

nodeNameは常に大文字であり、もし何かと比較したいなら、そのことを勘定に入れる必要がある。

.. code-block:: javascript

    function isImage(node) {
      return !isTextNode(node) && node.nodeName == "IMG";
    }

    show(isImage(document.body.lastChild));

Ex. 12.1

..  Write a function asHTML which, when given a DOM node, produces a string representing the HTML text for that node and its children. You may ignore attributes, just show nodes as <nodename>. The escapeHTML function from chapter 10 is available to properly escape the content of text nodes.

asHTML関数を書け。DOMノードを与えられたら、ノードと子のノードをHTMLテキストとしての表現である文字列を生成する。属性は無視して良く、ただノードを<ノード名>とすればよい。10章のescapeHTML関数をテキストのノードの内容をエスケープするために使えるものとする。

..  Hint: Recursion!

ヒント：再帰！

.. code-block:: javascript

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

..  Nodes, in fact, already have something similar to asHTML. Their innerHTML property can be used to retrieve the HTML text inside of the node, without the tags for the node itself. Some browsers also support outerHTML, which does include the node itself, but not all of them.

ノードは、実際、既にasHTMLと同じようなものを持っている。これらのinnerHTMLプロパティはノードの中のHTMLテキストを取り出すのに使えるが、ノード自体のタグは含まれない。あるブラウザはouterHTMLもサポートしていて、これはノードそれ自体を含むが、ブラウザ全てではない。

.. code-block:: javascript

    print(document.body.innerHTML);

..  Some of these properties can also be modified. Setting the innerHTML of a node or the nodeValue of a text-node will change its content. Note that, in the first case, the given string is interpreted as HTML, while in the second case it is interpreted as plain text.

これらのプロパティのあるものは変更可能でもある。ノードのinnerHTMLかテキストノードのnodeValueを設定することでその内容を変更できる。1つめの場合、与えられた文字列はHTMLとして解釈され、2つめの場合はプレーンテキストとして解釈されることに注意。

.. code-block:: javascript

    document.body.firstChild.firstChild.nodeValue =
      "Chapter 1: The deep significance of the bottle";

..  Or ...

または...

.. code-block:: javascript

    document.body.firstChild.innerHTML =
      "Did you know the 'blink' tag yet? <blink>Joy!</blink>";

..  We have been accessing nodes by going through a series of firstChild and lastChild properties. This can work, but it is verbose and easy to break ― if we add another node at the start of our document, document.body.firstChild no longer refers to the h1 element, and code which assumes it does will go wrong. On top of that, some browsers will add text-nodes for things like spaces and newlines between tags, while others do not, so that the exact layout of the DOM tree can vary.

我々はfirstChildとlastChildのシリーズを通じてノードにアクセスしてきた。これはこれで動くのだが、冗長で壊れやすい--もしもう1つのノードを文書の最初に追加したら、document.body.firstChildはh1要素を参照しなくなって、それを前提にしてきたコードは誤りとなる。その上、ブラウザによってはタグ同士の間の空白や改行といったものにもテキストノードを追加し、他のブラウザではそうならないので、DOMツリーの正確なレイアウトは変わってしまうことがある。

..  An alternative to this is to give elements that you need to have access to an id attribute. In the example page, the picture has an id "picture", and we can use this to look it up.

代替の手段は、必要な要素にid属性を与えてアクセスすることだ。例のページでは、画像が"picture"というidを持っていて、これで見つけることができる。

.. code-block:: javascript

    var picture = document.getElementById("picture");
    show(picture.src);
    picture.src = "img/ostrich.png";

..  When typing getElementById, note that the last letter is lowercase. Also, when typing it a lot, beware of carpal-tunnel syndrome. Because document.getElementById is a ridiculously long name for a very common operation, it has become a convention among JavaScript programmers to aggressively abbreviate it to $. $, as you might remember, is considered a letter by JavaScript, and is thus a valid variable name.

getElementByIdとタイプするとき、最後の文字は小文字であることに注意。たくさんタイプすると、手根管症候群にもなる。document.getElementByIdはとても頻繁に使われる操作にはひどく長すぎる名前なので、JavaScriptプログラマの利便性のために、強力に省略して$としている。$は、覚えているだろうが、JavaScriptでは文字として考えられ、すなわち正しい変数名である。

.. code-block:: javascript

    function $(id) {
      return document.getElementById(id);
    }
    show($("picture"));

..  DOM nodes also have a method getElementsByTagName (another nice, short name), which, when given a tag name, returns an array of all nodes of that type contained in the node it was called on.

DOMノードはgetElementByTagNameメソッドも持っていて（もう一つのナイスな、短い名前）、これは、タグ名を与えられたとき、そのメソッドが呼び出されたノードに含まれる、その型のノードの配列を返す。

.. code-block:: javascript

    show(document.body.getElementsByTagName("BLINK")[0]);

..  Another thing we can do with these DOM nodes is creating new ones ourselves. This makes it possible to add pieces to a document at will, which can be used to create some interesting effects. Unfortunately, the interface for doing this is extremely clumsy. But that can be remedied with some helper functions.

もう一つできることとして、これらのDOMノードに新しいものを作ることもできる。これで文書に意のままに部品を追加することが可能になり、面白い効果を作るのに使える。残念ながら、これを行うためのインターフェースはとてもややこしい。しかしヘルパー関数を作ることで手当てできる。

..  The document object has createElement and createTextNode methods. The first is used to create regular nodes, the second, as the name suggests, creates text nodes.

documentオブジェクトはcreateElementとcreateTextNodeメソッドを持っている。1つめは正規のノードを作るため、2つめは、名前の通り、テキストのノードを作るためのものだ。

.. code-block:: javascript

    var secondHeader = document.createElement("H1");
    var secondTitle = document.createTextNode("Chapter 2: Deep magic");

..  Next, we'll want to put the title name into the h1 element, and then add the element to the document. The simplest way to do this is the appendChild method, which can be called on every (non-text) node.

次は、タイトル名をh1要素に入れ、そして文書に要素を追加したい。単純なやり方としてはappendChildメソッドであり、全ての（テキストでない）ノードで呼び出すことができる。

.. code-block:: javascript

    secondHeader.appendChild(secondTitle);
    document.body.appendChild(secondHeader);

..  Often, you will also want to give these new nodes some attributes. For example, an img (image) tag is rather useless without an src property telling the browser which image it should show. Most attributes can be approached directly as properties of the DOM nodes, but there are also methods setAttribute and getAttribute, which are used to access attributes in a more general way:

時には、これらの新しいノードに属性も付けたくなるだろう。例えば、img（イメージ）タグはブラウザに表示すべき画像を知らせるsrcプロパティが無ければむしろ意味が無い。多くの属性はDOMノードのプロパティとして直接アプローチできるが、setAttributeとgetAttributeというメソッドもあり、これらでアクセスする方が一般的だ。

.. code-block:: javascript

    var newImage = document.createElement("IMG");
    newImage.setAttribute("src", "img/Hiva Oa.png");
    document.body.appendChild(newImage);
    show(newImage.getAttribute("src"));

..  But, when we want to build more than a few simple nodes, it gets very tiresome to create every single node with a call to document.createElement or document.createTextNode, and then add its attributes and child nodes one by one. Fortunately, it is not hard to write a function to do most of the work for us. Before we do so, there is one little detail to take care of ― the setAttribute method, while working fine on most browsers, does not always work on Internet Explorer. The names of a few HTML attributes already have a special meaning in JavaScript, and thus the corresponding object properties got an adjusted name. Specifically, the class attribute becomes className, for becomes htmlFor, and checked is renamed to defaultChecked. On Internet Explorer, setAttribute and getAttribute also work with these adjusted names, instead of the original HTML names, which can be confusing. On top of that the style attribute, which, along with class, will be discussed later in this chapter, can not be set with setAttribute on that browser.

しかし、もっと単純なノードを組み立てたいとき、単一のノード毎にいちいちdocument.createElementやdocument.createTextNodeを呼び出し、その属性や子のノードを一つ一つ追加するというのでは、とてもうんざりする。幸運にも、この仕事の多くをやってくれる関数を書くのは難しくない。それをやる前に、注意しなければならない詳細がある--setAttributeメソッド、多くのブラウザでは上手く動くが、InternetExplorerでは常に動くとは限らない。HTML属性の名前にはJavaScriptでも特別な意味を持つものがあって、対応するオブジェクトのプロパティの名前は調整されている。具体的には、class属性がclassName、forはHTMLfor、checkedはdefaultCheckedになっている。InternetExplorerではsetAttributeとgetAttributeは元のHTMLの名前の代わりに、これらの調整された名前でも動き、混乱することがある。その上、classに結びついたstyle属性は、この章の後ろの方で論じるが、そのブラウザのsetAttributeでは設定できない。

..  A workaround would look something like this:

とっかかりとしてはこのようになるだろう。：

.. code-block:: javascript

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

..  For every case where Internet Explorer deviates from other browsers, it does something that works in all cases. Don't worry about the details ― this is the kind of ugly trick that we'd rather not need, but which non-conforming browsers force us to write. Having this, it is possible to write a simple function for building DOM elements.

InternetExplorerが他のブラウザから逸脱している全ての場合で、何かの作業をする。細かいことは気にしない--これはむしろ避けたい醜いトリックの類であるが、順応性の無いブラウザが我々に描くことを強制するのだ。これで、DOM要素を組み立てる単純な関数を書くことができる。

.. code-block:: javascript

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

..  The dom function creates a DOM node. Its first argument gives the tag name of the node, its second argument is an object containing the attributes of the node, or null when no attributes are needed. After that, any amount of arguments may follow, and these are added to the node as child nodes. When strings appear here, they are first put into a text node.

dom関数はDOMノードを作る。1つめの引数はノードのタグ名で、2つめの引数はノードの属性を含むオブジェクトか、ノードに属性が不要の場合はnullである。その後に、任意の数の引数が続き、これらはそのノードの子のノードとして追加される。これが文字列だったときは、まずテキストのノードに入れられてから追加される。

..  appendChild is not the only way nodes can be inserted into another node. When the new node should not appear at the end of its parent, the insertBefore method can be used to place it in front of another child node. It takes the new node as a first argument, and the existing child as second argument.

ノードを他のノードに挿入する方法はappendChildだけではない。新しいノードがその親の最後にあるのでなければ、insertBeforeメソッドで他の子ノードの前に入れることができる。これは新しいノードを1つめの引数に、存在する子ノードを2つめの引数として取る。

.. code-block:: javascript

    var link = newParagraph.childNodes[1];
    newParagraph.insertBefore(dom("STRONG", null, "great "), link);

..  If a node that already has a parentNode is placed somewhere, it is automatically removed from its current position ― nodes can not exist in the document in more than one place.

もし既にparentNodeを持つノードをどこかに移動したら、それは自動的に今の位置から取り除かれる -- ノードは文書中で複数の場所には存在できないのだ。

..  When a node must be replaced by another one, use the replaceChild method, which again takes the new node as first argument and the existing one as second argument.

ノードを他のノードで置き換えるときはreplaceChildメソッドを使い、これは1つめの引数に新しいノード、存在するノードを2つめの引数に取る。

.. code-block:: javascript

    newParagraph.replaceChild(document.createTextNode("lousy "),
                              newParagraph.childNodes[1]);

..  And, finally, there is removeChild to remove a child node. Note that this is called on the parent of the node to be removed, giving the child as argument.

そして、最後、子ノードを取り除くremoveChildがある。これは取り除くノードの親から呼ばれ、子を引数として与えることに注意。

.. code-block:: javascript

    newParagraph.removeChild(newParagraph.childNodes[1]);

Ex. 12.2

..  Write the convenient function removeElement which removes the DOM node it is given as an argument from its parent node.

引数として与えられたノードの親ノードからDOMノードを取り除く便利な関数removeElementを書け。

.. code-block:: javascript

    function removeElement(node) {
      if (node.parentNode)
        node.parentNode.removeChild(node);
    }

    removeElement(newParagraph);

..  When creating new nodes and moving nodes around it is necessary to be aware of the following rule: Nodes are not allowed to be inserted into another document from the one in which they were created. This means that if you have extra frames or windows open, you can not take a piece of the document from one and move it to another, and nodes created with methods on one document object must stay in that document. Some browsers, notably Firefox, do not enforce this restriction, and thus a program which violates it will work fine in those browsers but break on others.

新しいノードを作るときとノードを移動するとき、このルールに従う必要がある：ノードはそれが作られた文書から、もう一つの文書に挿入することはできない。これはもし外部のフレームがあったり、ウインドウをオープンしたら、文書の部品を一方から取り上げてもう一方に移すことはできず、あるdocumentオブジェクトのメソッドで作られたノードはその文書の中に留めておかなければならない。ブラウザによっては、特にFirefoxでは、この制約は強制されておらず、このブラウザでは制約に違反しているプログラムは動くが他では動かない。

..  An example of something useful that can be done with this dom function is a program that takes JavaScript objects and summarises them in a table. Tables, in HTML, are created with a set of tags starting with ts, something like this:

このdom関数でできる有益なものの例としては、JavaScriptオブジェクトをテーブルに要約するプログラムがある。テーブルは、HTMLでは、tで始まるタグの集合で作られる、このようなものだ。

.. code-block:: html

    <table>
      <tbody>
        <tr> <th>Tree </th> <th>Flowers</th> </tr>
        <tr> <td>Apple</td> <td>White  </td> </tr>
        <tr> <td>Coral</td> <td>Red    </td> </tr>
        <tr> <td>Pine </td> <td>None   </td> </tr>
      </tbody>
    </table>

..  Each tr element is a row of the table. th and td elements are the cells of the table, tds are normal data cells, th cells are 'header' cells, which will be displayed in a slightly more prominent way. The tbody (table body) tag does not have to be included when a table is written as HTML, but when building a table from DOM nodes it should be added, because Internet Explorer refuses to display tables created without a tbody.

それぞれのtr要素はテーブルの行だ。thとtdはテーブルのセルで、tdは通常のデータのセル、thセルは'ヘッダー'のセルで、やや目立つように表示される。tbody(テーブルのボディ)タグはHTML上のテーブルを書く上で必ず含めなければならないものではないが、DOMノードからテーブルを組み立てるときは追加され、それはInternetExplorerがtbodyなしに作られたテーブルの表示を断るからである。

Ex. 12.3

..  The function makeTable takes two arrays as arguments. The first contains the JavaScript objects that it should summarise, and the second contains strings, which name the columns of the table and the properties of the objects that should be shown in these columns. For example, the following will produce the table above:

関数makeTableは2つの配列を引数に取る。1つめは要約すべきJavaScriptオブジェクト、そして2つめは列の名前と表示される列のオブジェクトのプロパティの文字列だ。例えば、上記の表は下記から作られる。：

.. code-block:: javascript

    makeTable([{Tree: "Apple", Flowers: "White"},
               {Tree: "Coral", Flowers: "Red"},
               {Tree: "Pine",  Flowers: "None"}],
              ["Tree", "Flowers"]);

..  Write this function.

この関数を書け。

.. code-block:: javascript

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

..  Do not forget to convert the values from the objects to strings before adding them to the table ― our dom function only understands strings and DOM nodes.

テーブルに追加する前にオブジェクトから文字列に値を変換するのを忘れないように -- 我々のdom関数は文字列とDOMノードしか理解できないのだ。

..  Closely tied to HTML and the document-object model is the topic of style-sheets. It is a big topic, and I will not discuss it entirely, but some understanding of style-sheets is necessary for a lot of interesting JavaScript techniques, so we will go over the basics.

HTMLとドキュメント－オブジェクトモデルはスタイルシートのトピックに密接に結びついている。大きな問題で、語り尽くすことはできないが、スタイルシートのある程度の理解は多くの面白いJavaScriptのテクニックに必要なので、基本的なところを見ておこう。

..  In old-fashioned HTML, the only way to change the appearance of elements in a document was to give them extra attributes or to wrap them in extra tags, such as center to center them horizontally, or font to change the font style or colour. Most of the time, this meant that if you wanted the paragraphs or the tables in your document to look a certain way, you had to add a bunch of attributes and tags to every single one of them. This quickly adds a lot of noise to such documents, and makes them very painful to write or change by hand.

古いファッションのHTMLでは、文書の要素の外見を変更するには、水平方向での中央に置くためのcenter、フォントのスタイルや色を変更するfontのような、拡張の属性を与えるか拡張のタグでそれらを囲む方法しかなかった。多くの時間が、これは文書の中に、段落やテーブルが決まった外見になるようにすることが必要なとき、属性の束とタグを全ての単一のタグの一つ一つに追加しなければならなかった。これは文書に多くのノイズを早く追加することになり、文書を手作業で変更する作業を、とても辛いものにする。

..  Of course, people being the inventive monkeys they are, someone came up with a solution. Style-sheets are a way to make statements like 'in this document, all paragraphs use the Comic Sans font, and are purple, and all tables have a thick green border'. You specify them once, at the top of the document or in a separate file, and they affect the whole document. Here, for example, is a style-sheet to make headers 22 points big and centered, and make paragraphs use the font and colour mentioned earlier, when they are of the 'ugly' class.

もちろん、発明するサルである人間は、解決法を編み出した。スタイルシートは'この文書にある、全ての段落はComic Sansフォントを使い、紫色、そして全てのテーブルは細い緑色の境界線を使う'というような書き方をする。一度、文書の冒頭か、別のファイルで指定すると、文書全体に効果が及ぶ。ここに例として、ヘッダーを22ポイントの大きさで中央揃え、段落が'ugly'クラスであれば先ほどのフォントと色にする、スタイルシートを挙げる。

.. code-block:: css

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

..  Classes are a concept related to styles. If you have different kinds of paragraphs, ugly ones and nice ones for example, setting the style for all p elements is not what you want, so classes can be used to distinguish between them. The above style will only be applied to paragraphs like this:

クラスはスタイルに関する概念である。例えば醜いのと良いのというように、もし段落の種類が異なっていて、全てのp要素にスタイルを設定したくないとしたら、クラスを使ってでそれらを識別することができる。上記のスタイルはこのような段落にしか適用されない。：

.. code-block:: html

    <p class="ugly">Mirror, mirror...</p>

..  And this is also the meaning of the className property which was briefly mentioned for the setNodeAttribute function. The style attribute can be used to add a piece of style directly to an element. For example, this gives our image a solid border 4 pixels ('px') wide.

そしてこれがclassNameプロパティについてsetNodeAttribute関数で少々言及したことの意味でもある。style属性は要素にスタイルの部品を直接追加するのに使える。例えば、これは、画像に立体的な境界線を4ピクセル（'px'）の幅で与える。

.. code-block:: javascript

    setNodeAttribute($("picture"), "style",
                     "border-width: 4px; border-style: solid;");

..  There is much more to styles: Some styles are inherited by child nodes from parent nodes, and interfere with each other in complex and interesting ways, but for the purpose of DOM programming, the most important thing to know is that each DOM node has a style property, which can be used to manipulate the style of that node, and that there are a few kinds of styles that can be used to make nodes do extraordinary things.

スタイルについてはまだある。：あるスタイルは親ノードから子ノードへ継承され、複雑で面白いやりかたで互いに邪魔しあうが、しかしDOMプログラミングの目的において、最も重要なことはそれぞれのDOMノードがstyleプロパティを持つこと、それらのノードのstyleを操作できること、いくつかの種類のスタイルはノードをおかしくかもしれないということを知っておくことだ。

..  This style property refers to an object, which has properties for all the possible elements of the style. We can, for example, make the picture's border green.

このstyleプロパティは、全ての可能なスタイルの要素のプロパティを持つオブジェクトを参照する。例えば、画像の境界線を緑色にできる。

.. code-block:: javascript

    $("picture").style.borderColor = "green";
    show($("picture").style.borderColor);

..  Note that in style-sheets, the words are separated by hyphens, as in border-color, while in JavaScript, capital letters are used to mark the different words, as in borderColor.

スタイルシートでは、語はハイフンで分割され、JavaScriptでは、borderColorのように大文字が異なる語のマークに使われることに注意しよう。

..  A very practical kind of style is display: none. This can be used to temporarily hide a node: When style.display is "none", the element does not appear at all to the viewer of the document, even though it does exist. Later, display can be set to the empty string, and the element will re-appear.

とても実用的なスタイルの種類としてdisplay: noneがある。これは一時的にノードを隠すのに使える。：style.displayが"none"のとき、要素が存在しているにもかかわらず、文書のどこにも要素は表示されなくなる。後から、displayに空の文字列を設定することで、その要素は再び現われる。

.. code-block:: javascript

    $("picture").style.display = "none";

..  And, to get our picture back:

そして、画像を元に戻すには：

.. code-block:: javascript

    $("picture").style.display = "";

..  Another set of style types that can be abused in interesting ways are those related to positioning. In a simple HTML document, the browser takes care of determining the screen positions of all the elements ― each element is put next to or below the elements that come before it, and nodes (generally) do not overlap.

面白いやり方で悪用されうる、もう1つのスタイルのタイプの集合は位置取りに関するものだ。単純なHTML文書では、全ての要素のスクリーン上の表示位置の決定はブラウザによって行われる -- それぞれの要素は直前の要素の次か下に配置され、ノードは（一般的には）重ね書きされない。

..  When its position style is set to "absolute", a node is taken out of the normal document 'flow'. It no longer takes up room in the document, but sort of floats above it. The left and top styles can then be used to influence its position. This can be used for various purposes, from making a node obnoxiously follow the mouse cursor to making 'windows' open on top of the rest of the document.

そのpositionスタイルが"absolute"に設定されると、ノードは"flow"する普通の文書でなくなる。文書上で場所を取らなくなって、その上に浮かぶ。leftとtopスタイルがその位置を動かすのに使われる。不快にもマウスカーソルを追い掛けてくるノードを作ることから、'ウインドウ'を文書の残りのトップで開くようにすることまで、様々な目的で使われる。

.. code-block:: javascript

    $("picture").style.position = "absolute";
    var angle = 0;
    var spin = setInterval(function() {
      angle += 0.1;
      $("picture").style.left = (100 + 100 * Math.cos(angle)) + "px";
      $("picture").style.top = (100 + 100 * Math.sin(angle)) + "px";
    }, 100);

..  If you aren't familiar with goniometry, just believe me when I tell you that the cosine and sine stuff is used to build coordinates lying on the outline of a circle. Ten times per second, the angle at which we place the picture is changed, and new coordinates are computed. It is a common error, when setting styles like this, to forget to append "px" to your value. In most cases, setting a style to a number without a unit does not work, so you must add "px" for pixels, "%" for percent, "em" for 'ems' (the width of an M character), or "pt" for points.

角度を測るのが苦手なら、私の言うことをただ信じてほしい。コサインとサインは円周の座標を組み立てるのに使える。秒ごとに10回、画像の場所の角度が変わり、新しい座標が計算される。ありがちな間違いは、このようにスタイルを設定したとき、"px"を値に追加するのを忘れることだ。多くの場合、単位なしの数値で設定されたスタイルは上手く動かない、なのでピクセルなら"px"を、パーセントなら"%"を、'em'（M文字の幅）なら"em"を、ポイントなら"pt"を追加しなければならない。

..  (Now put the image to rest again...)

（もう画像を元に戻そう...）

.. code-block:: javascript

    clearInterval(spin);

..  The place that is treated as 0,0 for the purpose of these positions depends on the place of the node in the document. When it is placed inside another node that has position: absolute or position: relative, the top left of this node is used. Otherwise, you get the top left corner of the document.

これらの位置の目的で0,0として扱われる場所は、文書上のノードの場所に依存している。position:absolute またはposition: relativeを持つノードがもう一つのノードの中に移されたとき、このノードの左上が使われ、そうでなければ、文書の左上になる。

..  One last aspect of DOM nodes that is fun to play with is their size. There are style types called width and height, which can be used to set the absolute size of an element.

遊べるDOMノードの面の最後の1つはそのサイズだ。widthとheightというスタイルのタイプがあって、要素の絶対的なサイズを設定するのに使える。

.. code-block:: javascript

    $("picture").style.width = "400px";
    $("picture").style.height = "200px";

..  But, when you need to accurately set the size of an element, there is an tricky problem to take into account. Some browsers, in some circumstances, take these sizes to mean the outside size of the object, including any border and internal padding. Other browsers, in other circumstances, use the size of the space inside of the object instead, and do not count the width of borders and padding. Thus, if you set the size of an object that has a border or a padding, it will not always appear the same size.

しかし、要素のサイズを正確に設定したいとき、計算上のトリッキーな問題がある。あるブラウザ、ある環境では、これらのサイズをオブジェクトの外側の、境界線や内部の余白まで含めた意味と取る。他のブラウザ、他の環境では、代わりにオブジェクトの中のスペースのサイズ使われ、そして境界線や余白を持つオブジェクトの幅はカウントされない。つまり、境界線や余白を持つオブジェクトのサイズを設定しても、常に同じサイズで表示されない。

..  Fortunately, you can inspect the inner and outer size of a node, which, when you really need to accurately size something, can be used to compensate for browser behaviour. The offsetWidth and offsetHeight properties give you the outer size of your element (the space it takes up in the document), while the clientWidth and clientHeight properties give the space inside of it, if any.

幸運にも、ノードの内側と外側のサイズを見ることができ、本当に正確なサイズが必要なときには、ブラウザの振る舞いを相殺することができる。offsetWidthとoffsetHeightプロパティは要素の外側のサイズ（文書の中でそれが取るスペース）を与え、clientWidthとclientHeightプロパティは、もしそれがあれば、その内側のスペースを与える。

.. code-block:: javascript

    print("Outer size: ", $("picture").offsetWidth,
          " by ", $("picture").offsetHeight, " pixels.");
    print("Inner size: ", $("picture").clientWidth,
          " by ", $("picture").clientHeight, " pixels.");

..  If you've followed through with all the examples in this chapter, and maybe did a few extra things by yourself, you will have completely mutilated the poor little document that we started with. Now let me moralise for a moment and tell you that you do not want to do this to real pages. The temptation to add all kinds of moving bling-bling will at times be strong. Resist it, or your pages shall surely become unreadable or even, if you go far enough, induce the occasional seizure.

もしこの章の全ての例をやり遂げたら、いくつかの追加を自分でやってみて、我々がスタートにした貧しく小さな文書を完全に壊してみよう。少々の間教訓を与え、本物のページにこれを行おうとしないように言おう。全ての種類のキラキラした動きを追加する誘惑は時に強いものだ。耐えなさい。でなければ、ページが確実に読みにくくなる。たとえもしあなたが十分遠くに行っても、時々発作が起こるだろう。
