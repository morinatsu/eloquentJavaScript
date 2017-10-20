---
layout: default
title: 13. ブラウザ・イベント
---

ブラウザ・イベント
=====================================================

面白い機能をウェブページに追加するには、ただ、文書を監視し更新するだけでは一般的には十分ではない。ユーザーが行ったことを検知し、それに応答することも必要だ。このために、イベント・ハンドラというものを使う。キーが押されたというイベント、マウスがクリックされたというイベント、マウスの動きも一連のイベントと見ることができる。11章で、ボタンに、それが押された時のためのonclickプロパティを与えた。これは単純なイベント・ハンドラだ。

このブラウザ・イベントの働きかたは、基本的には、とても単純だ。個々のイベント・タイプと個々のDOMノードに対してハンドラを登録することが可能だ。イベントが起こったら、そのイベントに対するハンドラが、もしあれば、呼び出される。キー押下のような、いくつかのイベントには、ただイベントが起こったと分るだけでは不十分なので、どのキーが押されたかも知ることができる。そのような情報を格納するため、全てのイベントeventオブジェクトを作り、ハンドラはそれを見る。

いつイベントが起こるかにかかわらず、同時に2つのハンドラが実行されることはないことを理解することは重要だ。もし他のJavaScriptコードがまだ実行中であれば、ブラウザは次のハンドラを呼ぶ前にその終了を待つ。これはsetTimeoutのような、他の手段で起動されたコードでも維持される。プログラマのジャーゴンでは、ブラウザのJavaScriptはシングル・スレッドであり、同時に2つの'スレッド'が実行されることは無い。多くの場合において、これは良いことだ。同時に複数のことが起こるときには、いとも簡単におかしな結果になるものだ。

イベントがあって、それがハンドルされないときは、'泡のように'DOMツリーを浮かび上がっていく。これは、もしクリックがあったとき、例えばそれが段落中のリンクであれば、リンクに結びつけられたハンドラが最初に呼ばれることを意味する。そのようなハンドラが無いか、これらのハンドラがそのイベントの終了を示さないときは、リンクの親である段落のハンドラが試される。その後は、document.bodyのハンドラの番が回ってくる。最後に、イベントを扱うJavaScriptのハンドラが無くなったら、ブラウザがそれを扱う。リンクをクリックしたとき、これはそのリンクをたどるということを意味する。

見ての通り、イベントは簡単だ。それについて難しいところは、ブラウザで、サポートしている機能の大小があったり、この機能を異なるインターフェースでサポートしていることだ。普通は、最も互換性の少ないブラウザはInternetExplorerで、多くのブラウザが従っている標準を無視している。その次に、Opera、ページを去るときに発火するonunloadのような有益なイベントのいくつかを正しくサポートしておらず、またあるキーボードイベントについての混乱した情報を与える。

イベントに関するアクションで取っておきたいものが4つある。

* イベント・ハンドラを登録する。
* eventオブジェクトを取得する。
* このオブジェクトから情報を展開する。
* イベントがハンドルされたという合図を送る。

これらのうち、全てのメジャーなブラウザで共通のものは1つも無い。

イベント・ハンドリングの練習台として、1つのボタンと1つのテキスト・フィールドを持つ文書を開こう。この章の続きの間、このウインドウは開いたままに（接続したままに）しておくこと。

```javascript
attach(window.open("example_events.html"));
```

最初のアクション、ハンドラの登録は、要素のonclick（またはonkeypress、とか）プロパティを設定することで行われる。これは実際ブラウザに渡って動くが、重要な不都合もある -- 1つのイベントには1つのハンドラしか接続できない。多くの時間、これで十分だが、そうでない場合もあって、特にプログラムが他のプログラム（これもまたハンドラを追加しているかもしれない）と一緒に動かなければならない時には、これが邪魔になる。

InternetExplorerでは、ボタンのクリックのハンドラはこのように追加する。：

```javascript
$("button").attachEvent("onclick", function(){print("Click!");});
```

他のブラウザでは、このようになる。：

```javascript
$("button").addEventListener("click", function(){print("Click!");},
                             false);
```

"on"は2番めでは消されていることに注意。addEventListenerの3つめの引数、false、はこのイベントを通常のようにDOMツリーの上に'浮かび上がらせない'ようにすることを示す。代わりにTrueが与えられると、このハンドラの優先度がその下の他のハンドラより高くなるが、InternetExplorerがこれをサポートしていないため、これが有益なことはまれだ。

### <a name="Ex13-1">[演習 13.1]</a>

これら2つのモデルの非互換性をラップするregisterEventHandlerという関数を書け。3つの引数を取る：1つめはハンドラが接続するDOMノード、2つめは"クリック"や"キー押下"のようなイベントの型、3つめはハンドラの関数とする。

呼び出されたメソッドの識別には、メソッドそれ自体を見る -- もしDOMノードがattachEventというメソッドを持っていれば、これが正しいメソッドだと仮定できる。直接InternetExplorerかをチェックするよりこちらの方が望ましいことに注意しよう。もしInternetExplorerのモデルを使った新しいブラウザが登場しても、またはInternetExplorerが標準のモデルに突然スイッチしても、このコードはまだ動く。どちらも好ましくないのはもちろんだが、傷つきにくい、賢い方法でやろう。

```javascript
function registerEventHandler(node, event, handler) {
  if (typeof node.addEventListener == "function")
    node.addEventListener(event, handler, false);
  else
    node.attachEvent("on" + event, handler);
}

registerEventHandler($("button"), "click",
                     function(){print("Click (2)");});
```

名前が長くて面倒でも悩まないように。後で、このラッパーをさらにラップする、拡張のラッパーを追加する。その時に短い名前を付けよう。

このチェックを1回だけにすることも可能で、ブラウザによって異なる関数をregisterEventHandlrerを定義すればいい。これはより効果的だが、ちょっとおかしい。

```javascript
if (typeof document.addEventListener == "function")
  var registerEventHandler = function(node, event, handler) {
    node.addEventListener(event, handler, false);
  };
else
  var registerEventHandler = function(node, event, handler) {
    node.attachEvent("on" + event, handler);
  };
```

イベントを取り除くのは追加とよく似ているが、このときはメソッドdetachEventとremoveEventListenerが使われる。ハンドラを取り除くには、それに繋げた関数にアクセスする必要があることに注意しよう。

```javascript
function unregisterEventHandler(node, event, handler) {
  if (typeof node.removeEventListener == "function")
    node.removeEventListener(event, handler, false);
  else
    node.detachEvent("on" + event, handler);
}
```

技術的な限界のために、イベント・ハンドラで作られた例外は、コンソールで捕まえられない。すなわち、これらはブラウザでハンドルされ、'エラー・コンソール'のようなところに隠されたり、またはポップアップ・メッセージを起こすということだ。イベント・ハンドラを書き、それが動いていないように見えるときは、それが起こしたエラーの種類のために静かに異常終了しているのかもしれない。

多くのブラウザではハンドラへの引数としてeventオブジェクトを渡す。InternetExplorerはそれをeventというトップレベルの変数に格納する。JavaScriptコードを見るときに、`event || window.event`のようなものをしばしば見るだろう。ローカルな変数のeventか、もしそれがundefinedであれば、同じ名前を付けることによってトップレベルの変数になる。

```javascript
function showEvent(event) {
  show(event || window.event);
}

registerEventHandler($("textfield"), "keypress", showEvent);
```

フィールドに何文字かタイプして、オブジェクトを見たら、それを閉めよう。

```javascript
unregisterEventHandler($("textfield"), "keypress", showEvent);
```

ユーザーがマウスをクリックしたとき、3つのイベントが生成される。最初はmousedown、マウスのボタンが押された瞬間に。それから、mouseup、その指がボタンから離れた瞬間に。最後に、click、何かがクリックされたことを示す。これが素早く2回起こったときは、dblclick（ダブル・クリック）イベントも生成される。mousedownとmouseupイベントが時間を経て発生する -- マウスボタンがしばらく押されっぱなしにされる、こともあるので注意しよう。

イベント・ハンドラに接続したとき、例えば、ボタンなら、それがクリックされたことさえ分れば良い。その一方で、ハンドラが子があるノードに結びついて、クリックが子から'伝播して'来たときは、どの子がクリックされたか知りたいと思うだろう。そのために、eventオブジェクトはtargetというプロパティ、またはsrcElement、ブラウザによっては...を持つ。

もう一つ面白い情報の部分はクリックが起こった正確な座標だ。マウスに関連するeventオブジェクトはclientXとclientYというプロパティを持ち、それがマウスのx座標とy座標をスクリーンのピクセルで持つ。文書はスクロール可能なので、マウスが文書のどの部分にあるかはこれらの座標からは分らないブラウザによっては、pageXとpageYプロパティをこの目的のために提供しているが、そうでないものも（ご想像通り）ある。幸運にも、文書がスクロールしたピクセルの数についての情報はdocument.body.scrollLeftとdocument.body.scrollTopから分かる。

このハンドラは、文書全体に結びついて、全てのマウス・クリックを拾って、それについての情報を表示する。

```javascript
function reportClick(event) {
  event = event || window.event;
  var target = event.target || event.srcElement;
  var pageX = event.pageX, pageY = event.pageY;
  if (pageX == undefined) {
    pageX = event.clientX + document.body.scrollLeft;
    pageY = event.clientY + document.body.scrollTop;
  }

  print("Mouse clicked at ", pageX, ", ", pageY,
        ". Inside element:");
  show(target);
}
registerEventHandler(document, "click", reportClick);
```

これを取り除こう。：

```javascript
unregisterEventHandler(document, "click", reportClick);
```

明らかに、全ての単純なイベントハンドラ毎に、これら全てをチェックし、手を回すことはやりたくないだろう。しばらくの間、いくつかの非互換性になれた後で、'ノーマライズされた'eventオブジェクトとブラウザ間で同様に動く関数を書こう。

どのマウスボタンが押されたのか知るのも可能で、eventオブジェクトのbuttonプロパティを使えばいい。残念ながら、これはとても当てにならない -- あるブラウザはマウスには1つしかボタンが無いかのように装っており、他はクリックを右クリックとして報告するところを、コントロールキーが押された、とか言ってくる。

クリックから離れれば、マウスの動きも面白いかもしれない。DOMノードのmousemoveイベントはそのようその上でマウスが動いている間発火される。mouseoverとmouseoutというものもあり、これらはそのノードにマウスが入ったときと放れた時だけ発火する。この最後の型のイベントでは、target（またはsrcElement）プロパティはイベントが発火したノードを指し、relatedTarget（またはtoElement、fromElement）プロパティはマウスがやってきた(mouseoverの場合)あるいは出ていった(mouseoutの場合)ノードを指す。

mouseoverとmouseoutは、それらが子ノードを持つ要素に登録されたときトリッキーになり得る。子ノードで発火したイベントは、親要素に伝播するので、マウスが子ノードの1つに入ったら親のmouseoverイベントも見るだろう。targetとrelatedTargetプロパティはそのようなイベントを見つけ（無視）するのに使うことができる。

ユーザーが押した全てのキーは、3つのイベントを生成する：keydownとkeyup、keypress。一般的には、最初の2つをどのキーが押されたか本当に知りたい場合、例えば矢印キーが押されたときに何かしたい時に使う。keypressは、他方、タイプされた文字に興味があるときに使われる。この理由は、keyupとkeydownのイベントにはしばしば文字の情報が含まれないことがあり、そしてInternetExplorerが矢印キーのような特殊キーのイベントを生成しないことである。

どのキーが押されたか判別するのはそれ自体が全くのチャレンジになり得る。keydownとkeyupのイベントでは、eventオブジェクトはkeyCodeプロパティを持ち、これには数字が入っている。多くの場合、これらのコードはブラウザに依存しない方法でキーを識別するのに使える。キーに結びつけられたコードを見つけるのは単純な実験でできる。

```javascript
function printKeyCode(event) {
  event = event || window.event;
  print("Key ", event.keyCode, " was pressed.");
}

registerEventHandler($("textfield"), "keydown", printKeyCode);
unregisterEventHandler($("textfield"), "keydown", printKeyCode);
```

多くのブラウザにおいて、単一のキーコードがキーボードの単一の物理的なキーに結びついている。Operaブラウザでは、しかしながら、シフトキーが押されているか否かで異なるキーコードが生成される。悪いことには、これらのシフトが押された時のコードには他のキーで使われているものと同じであるものもある -- シフト-9、プログラムでは、多くのキーボードでは括弧をタイプするときに使うが、下矢印と同じコードになり、かつその識別は困難である。これはプログラムをサボタージュするよう脅かし、シフトが押された時のキー・イベントを無視するというのが通常の解決になる。

キーやマウスのイベントにおいて、シフト、コントロール、altキーが押されているか判別するには、eventオブジェクトのshiftKey、ctrlKey、altKeyプロパティを見る。

keypressイベントでは、タイプされた文字を知ることができる。eventオブジェクトはcharCodeプロパティを持ち、これは、もしラッキーなら、タイプされた文字のUnicode数値を持ち、String.fromCharCodeで1文字の文字列に変換できる。残念ながら、あるブラウザはこのプロパティを定義していない、あるいは0として定義していて、代わりに文字コードをkeyCodeプロパティに格納している。

```javascript
function printCharacter(event) {
  event = event || window.event;
  var charCode = event.charCode;
  if (charCode == undefined || charCode === 0)
    charCode = event.keyCode;
  print("Character '", String.fromCharCode(charCode), "'");
}

registerEventHandler($("textfield"), "keypress", printCharacter);
unregisterEventHandler($("textfield"), "keypress", printCharacter);
```

イベント・ハンドラはイベントのハンドリングを'停止'することができる。これには2つの方法がある。ハンドラが定義されている親ノードへイベントが伝播するのを防止することもできるし、そのようなイベントに結びついた標準的なアクションをブラウザが取るのを防止することもできる。ブラウザは常にこれに従わないことに注意しよう -- 決まった'ホットキー'を押したときのデフォルトの振る舞いを抑止すると、多くのブラウザでは、これらのキーの通常の効果の実行も実際には維持されなくなる。

多くのブラウザで、イベントの伝播の停止はeventオブジェクトのstopPropagationメソッドで、デフォルトの振る舞いの帽子はpreventDefaultメソッドで行われる。InternetExplorerでは、これらはそれぞれ、そのオブジェクトのcancelBubbleプロパティが真に、そしてreturnValueプロパティを偽に設定することで行われる。

そして非互換の長いリストについてはこの章で論じる。これはとうとうイベントを正常化する関数を書くことができ、より面白い話題に移れるということを意味する。

```javascript
function normaliseEvent(event) {
  if (!event.stopPropagation) {
    event.stopPropagation = function() {this.cancelBubble = true;};
    event.preventDefault = function() {this.returnValue = false;};
  }
  if (!event.stop) {
    event.stop = function() {
      this.stopPropagation();
      this.preventDefault();
    };
  }

  if (event.srcElement && !event.target)
    event.target = event.srcElement;
  if ((event.toElement || event.fromElement) && !event.relatedTarget)
    event.relatedTarget = event.toElement || event.fromElement;
  if (event.clientX != undefined && event.pageX == undefined) {
    event.pageX = event.clientX + document.body.scrollLeft;
    event.pageY = event.clientY + document.body.scrollTop;
  }
  if (event.type == "keypress") {
    if (event.charCode === 0 || event.charCode == undefined)
      event.character = String.fromCharCode(event.keyCode);
    else
      event.character = String.fromCharCode(event.charCode);
  }

  return event;
}
```

stopメソッドが追加された。これはイベントの伝播とデフォルトのアクションの両方をキャンセルする。あるブラウザは既にこれを提供していて、その場合は、それをそのまま残している。

次はregisterEventHandlerとunregisterEventHandlerの便利なラッパーを書ける。

```javascript
function addHandler(node, type, handler) {
  function wrapHandler(event) {
    handler(normaliseEvent(event || window.event));
  }
  registerEventHandler(node, type, wrapHandler);
  return {node: node, type: type, handler: wrapHandler};
}

function removeHandler(object) {
  unregisterEventHandler(object.node, object.type, object.handler);
}

var blockQ = addHandler($("textfield"), "keypress", function(event) {
  if (event.character.toLowerCase() == "q")
    event.stop();
});
```

新しいaddHandler関数は、eventオブジェクトの正常化の面倒を見る新しい関数に、与えられたハンドラ関数をラップする。この個別のハンドラを取り除きたいときにはremoveHandlerに与えることでオブジェクトを戻す。'q'をテキスト・フィールドにタイプしてみよう。

```javascript
removeHandler(blockQ);
```

addHandlerと前章のdom関数を身に付け、文書操作のよりチャレンジングな事柄への準備ができた。練習として、倉庫番として知られるゲームを実装してみよう。これは古典とも言うべきものだが、あなたはこれまで知らなかったかもしれない。ルールはこうだ：壁によって作られたマス目があって、空いてるスペースだったり、1つかそれ以上の'出口'がある。このマス目に、木箱や石がいくつもあって、プレイヤーが動かせるのは小さい男だけ。男は水平と垂直の方向が空いていれば移動することができ、周りの岩を押して、その背後が空であれば運ぶことができる。ゲームのゴールは与えられた数の岩を出口まで運ぶことである。

8章の飼育器のように、倉庫番のレベルもテキストで表現できる。example_events.htmlウインドウの、変数sokobanLevelsはlevelオブジェクトの配列を含み、それぞれのlevelはfieldプロパティを持ち、これにそのレベルのテキスト表現とbouldersプロパティを入れ、bouldersプロパティはそのレベルで追い出さねばならない岩の数を示す。

```javascript
show(sokobanLevels.length);
show(sokobanLevels[1].boulders);
forEach(sokobanLevels[1].field, print);
```

このようなレベルで、#文字は壁、スペースは空の場所、0文字は岩があるところ、@はプレイヤーの開始位置、*は出口である。

しかし、ゲームをプレイするとき、このテキスト表現の見かけは変えたくない。代わりに、文書にテーブルを入れよう。小さなスタイルシート（もし中身を見たければ、sokoban.cssがそれだ）でテーブルのセルに正方形のサイズを設定し、例を文書に追加した。テーブルのそれぞれのセルは背景画像を得て、その四角のタイプ（空、壁、出口）を表現する。プレイヤーと岩の場所を見せるには、画像をこのテーブルのセルに追加し、違う妥当なセルに動かす。

このテーブルをデータの主な表現として使うこともできた -- 与えられた資格に壁があるかどうか見たいときは、ただ、テーブルの正しいセルの背景を見て、プレイヤーを探すには、正しいsrcプロパティを持つimgノードを探す。場合によっては、このアプローチが実用的だが、このプログラムではマス目のデータ構造を分離して持つことを選び、それはその方が素直だからである。

このデータ構造はオブジェクトの2次元のマス目で、プレイング・フィールドの四角の表現である。それぞれのオブジェクトはその場所の背景の種類と岩やプレイヤーがいるかどうかを示す。それは文書の表示のためのテーブルのセルへの参照も含み、画像を動かしたり、このテーブルセルを出し入れするのを簡単にする。

2種類のオブジェクトが与えられる -- 1つはプレイング・フィールドのマス目を格納し、1つはマス目上の個々のセルを表現する。もし、ふさわしい時にゲームを次のレベルに勧めたいと思ったり、散らかった現在のレベルをリセットしたい時のために、'コントローラ'のオブジェクトも必要で、フィールド・オブジェクトをふさわしい時に作ったり取り除いたりする。利便性のために、8章の最後で要点を示したプロトタイプのアプローチを使い、オブジェクト型をプロトタイプとし、new演算子よりcreateメソッドを新しいオブジェクトを作るのに使おう。

ゲームフィールドの四角を表現するオブジェクトから始めよう。セルに設定された背景を正しく応答し、ふさわしい画像を追加する。img/sokobanには、他の古いゲームに基づいた画像のセットが入っていて、ゲームを視覚化するのに使える。手始めに、Squareプロトタイプはこのようになった。：

```javascript
var Square = {
  construct: function(character, tableCell) {
    this.background = "empty";
    if (character == "#")
      this.background = "wall";
    else if (character == "*")
      this.background = "exit";

    this.tableCell = tableCell;
    this.tableCell.className = this.background;

    this.content = null;
    if (character == "0")
      this.content = "boulder";
    else if (character == "@")
      this.content = "player";

    if (this.content != null) {
      var image = dom("IMG", {src: "img/sokoban/" +
                                   this.content + ".gif"});
      this.tableCell.appendChild(image);
    }
  },

  hasPlayer: function() {
    return this.content == "player";
  },
  hasBoulder: function() {
    return this.content == "boulder";
  },
  isEmpty: function() {
    return this.content == null && this.background == "empty";
  },
  isExit: function() {
    return this.background == "exit";
  }
};

var testSquare = Square.create("@", dom("TD"));
show(testSquare.hasPlayer());
```

コンストラクタのcharacter引数はレベルの青写真を文字に変形して実際のSquareオブジェクトに入れるのに使う。セルの背景を設定するには、スタイルシートの（sokoban.cssで定義された）classを使い、それらはtd要素のclassNameプロパティに割り当てられている。

hasPlayerとisEmptyのようなメソッドは、この型のオブジェクトを使うコードをオブジェクトの内部から分離する手段だ。この場合には厳密には必要なものでは無いが、他のコードは良いものになるだろう。

### <a name="Ex13-2">[演習 13.2]</a>

moveContentとclearContentメソッドをSquareプロトタイプに追加せよ。1つめはもう一つのSquareオブジェクトを引数とし、引数のsquareのcontentプロパティを更新し、このcontentに結びついたimgノードを移動することで、このsquareの中身を移動する。これは岩とプレイヤーを周囲のマスに移動するのに使う。四角は現在空でないことを仮定して良い。clearContentはsquareからcontentをどこにも移動せずに取り除く。空のマスのcontentプロパティはnullでないことに注意。

12章で定義されたremoveElement関数はこの章でも利用可能で、ノードの削除に便利だ。画像はテーブルのセルの子ノードのみであり、this.tableCsll.lastChildで到達できると仮定して良い。

```javascript
Square.moveContent = function(target) {
  target.content = this.content;
  this.content = null;
  target.tableCell.appendChild(this.tableCell.lastChild);
};
Square.clearContent = function() {
  this.content = null;
  removeElement(this.tableCell.lastChild);
};
```

次のオブジェクト型はSokobanFieldという。そのコンストラクタはsokobanLevels配列を与えられ、DOMノードとSquareオブジェクトのマス目の両方を組み立てて返す。このオブジェクトはプレイヤーと周りの岩の動きの詳細も扱い、moveメソッドはプレイヤーが移動したい方向を示す引数を与えられる。

個々のsquareの識別し、方向を示すために、8章のPointオブジェクトを再び使おう。覚えているかもしれないが、これはaddメソッドを持っている。

fieldプロトタイプの基礎はこのようになる。：

```javascript
var SokobanField = {
  construct: function(level) {
    var tbody = dom("TBODY");
    this.squares = [];
    this.bouldersToGo = level.boulders;

    for (var y = 0; y < level.field.length; y++) {
      var line = level.field[y];
      var tableRow = dom("TR");
      var squareRow = [];
      for (var x = 0; x < line.length; x++) {
        var tableCell = dom("TD");
        tableRow.appendChild(tableCell);
        var square = Square.create(line.charAt(x), tableCell);
        squareRow.push(square);
        if (square.hasPlayer())
          this.playerPos = new Point(x, y);
      }
      tbody.appendChild(tableRow);
      this.squares.push(squareRow);
    }

    this.table = dom("TABLE", {"class": "sokoban"}, tbody);
    this.score = dom("DIV", null, "...");
    this.updateScore();
  },

  getSquare: function(position) {
    return this.squares[position.y][position.x];
  },
  updateScore: function() {
    this.score.firstChild.nodeValue = this.bouldersToGo +
                                      " boulders to go.";
  },
  won: function() {
    return this.bouldersToGo <= 0;
  }
};

var testField = SokobanField.create(sokobanLevels[0]);
show(testField.getSquare(new Point(10, 2)).content);
```

コンストラクタはレベルの線と文字に立ち寄って、squaresプロパティにSquareオブジェクトを格納する。プレイヤーがいるsquareに遭ったら、この位置をplayerPosに保存しておき、後で楽にプレイヤーのいるsquareを探せるようにする。getSquareはフィールドの特定のx、y位置に結びついたSquareオブジェクトを探すのに使う。フィールドの端を勘定に入れないことに注意 -- 退屈なコードを書くのを避けるため、フィールドは適切に壁に隔てられ、そこから抜け出すのは不可能であると仮定しておこう。。

tableノードを作るdomの中の"class"という語は文字列として引用される。これは、classがJavaScriptの'予約語'であるため、変数名やプロパティ名として使えないために必要だ。

レベルをクリアするために片付けるべき岩の数（そのレベルの岩の数の合計より少ないかもしれない）がboulderToGoである。岩が出口に運ばれたら、これから1を引いて、ゲームがもうクリアされたかをチェックする。プレイヤーに既にやっつけた数を見せるため、この数を何らかの方法で表示する。この目的のため、テキストのdiv要素を使う。divノードは固有のマークアップのないコンテナである。得点のテキストはupdateScoreメソッドで更新できる。wonメソッドはコントローラ・オブジェクトによりゲームが終了したか、プレイヤーが次のレベルに勧めるかのの判定に使われる。

もし実際にプレイング・フィールドと得点を見空ければ、何ら可能方法で文書にこれらを挿入しなければならない。このためにplaceメソッドがある。ゲームが終わった時にフィールドを簡単に取り除くためにremoveメソッドも追加しよう。

```javascript
SokobanField.place = function(where) {
  where.appendChild(this.score);
  where.appendChild(this.table);
};
SokobanField.remove = function() {
  removeElement(this.score);
  removeElement(this.table);
};

testField.place(document.body);
```

もし全部うまくいっていたら、Sokobanフィールドを今見ているはずだ。

### <a name="Ex13-3">[演習 13.3]</a>

しかし、このフィールドはまだ上手く動かない。moveメソッドを追加せよ。動きを指示するPointオブジェクト（例えば、-1,0なら左に動く）を引数に取り、ゲームの要素の動きを正しい方法で扱う。

正しい方法とはこうだ：playerPosプロパティはプレイヤーが動こうとしている場所を判別する。もしそこに岩があれば、この岩の背後を見る。そこが出口であれば、岩は取り除かれて得点が更新される。そこが空のスペースであれば、そこに岩が動く。次に、プレイヤーを動かしてみる。もし、プレイヤーが動こうとしているsquareが空でなければ、移動は無視される。

```javascript
SokobanField.move = function(direction) {
  var playerSquare = this.getSquare(this.playerPos);
  var targetPos = this.playerPos.add(direction);
  var targetSquare = this.getSquare(targetPos);

  // Possibly pushing a boulder
  if (targetSquare.hasBoulder()) {
    var pushTarget = this.getSquare(targetPos.add(direction));
    if (pushTarget.isEmpty()) {
      targetSquare.moveContent(pushTarget);
    }
    else if (pushTarget.isExit()) {
      targetSquare.moveContent(pushTarget);
      pushTarget.clearContent();
      this.bouldersToGo--;
      this.updateScore();
    }
  }
  // Moving the player
  if (targetSquare.isEmpty()) {
    playerSquare.moveContent(targetSquare);
    this.playerPos = targetPos;
  }
};
```

最初に岩を扱うことで、moveのコードはプレイヤーが普通に動く場合でも岩を押している時でも同様に動く。岩の背後のsquareをdirectionをplayerPosに2回加えることで見つけていることに注意。左に2マス進む場合でテストしよう。

```javascript
testField.move(new Point(-1, 0));
testField.move(new Point(-1, 0));
```

もしこれが動けば、もう取り出せない場所に岩を動かしたので、このフィールドを放棄しよう。

```javascript
testField.remove();
```

全ての'ゲーム・ロジック'は今のように扱われ、プレイ可能にするためにコントローラが必要だ。コントローラはSokobanGameという型のオブジェクトで、下記のように応答する。

* ゲーム・フィールドが位置すべき場所の準備
* SokobanFieldオブジェクトの組み立てと削除
* キー・イベントを捕まえて、正しい引数で現在のフィールドのmoveメソッドの呼び出し
* 現在のレベル数の保持と、そのレベルがクリアされたら次のレベルに移動
* 現在のレベルまたはゲーム全体（0レベルに戻る）をリセットするボタンの追加

再び、未完成のプロトタイプから始めよう。

```javascript
var SokobanGame = {
  construct: function(place) {
    this.level = null;
    this.field = null;

    var newGame = dom("BUTTON", null, "New game");
    addHandler(newGame, "click", method(this, "newGame"));
    var reset = dom("BUTTON", null, "Reset level");
    addHandler(reset, "click", method(this, "reset"));
    this.container = dom("DIV", null,
                         dom("H1", null, "Sokoban"),
                         dom("DIV", null, newGame, " ", reset));
    place.appendChild(this.container);

    addHandler(document, "keydown", method(this, "keyDown"));
    this.newGame();
  },

  newGame: function() {
    this.level = 0;
    this.reset();
  },
  reset: function() {
    if (this.field)
      this.field.remove();
    this.field = SokobanField.create(sokobanLevels[this.level]);
    this.field.place(this.container);
  },

  keyDown: function(event) {
    // To be filled in
  }
};
```

コンストラクタはフィールドとともに2つのボタンとタイトルを保持するdivエレメントを組み立てる。このオブジェクトのイベントにメソッドを結びつけているメソッドがどのように使われているかに注意。

文書にSokobanゲームを入れるにはこのようにする。：

```javascript
var sokoban = SokobanGame.create(document.body);
```

### <a name="Ex13-4">[演習 13.4]</a>

今残っているのはキー・イベントのハンドラを埋めることだ。プロトタイプのkeyDownメソッドを矢印キーの押下の検知に入れ替えて、もしそれが見つかったら、プレイヤーを正しい方向に動かそう。下記の辞書がおそらく手っ取り早い。：

```javascript
var arrowKeyCodes = new Dictionary({
  37: new Point(-1, 0), // left
  38: new Point(0, -1), // up
  39: new Point(1, 0),  // right
  40: new Point(0, 1)   // down
});
```

矢印キーのハンドルの後、this.field.won()をチェックしてこの動きでクリアか判定しよう。もしプレイヤーがクリアしていたら、メッセージを表示するアラートを使い、次のレベルに移動する。もし、次のレベルがもう無ければ（sokobanLevels.lengthでチェック）、代わりにゲームを再スタートしよう。

キー押下をハンドルしたらイベントを止めておくのが、おそらく賢明で、そうしなければ上矢印や下矢印を押した時にウインドウがスクロールして、ゲームの邪魔になるだろう。

```javascript
SokobanGame.keyDown = function(event) {
  if (arrowKeyCodes.contains(event.keyCode)) {
    event.stop();
    this.field.move(arrowKeyCodes.lookup(event.keyCode));
    if (this.field.won()) {
      if (this.level < sokobanLevels.length - 1) {
        alert("Excellent! Going to the next level.");
        this.level++;
        this.reset();
      }
      else {
        alert("You win! Game over.");
        this.newGame();
      }
    }
  }
};
```

キーをこのように捕まえることに注意しなければならない -- イベントを監視するために文書にハンドラを追加したりイベントを停止するのはあまりよくない -- 文書に他の要素がある時には。例えば、文書の上のテキスト・フィールドにカーソルを移動してみよう。 -- これは動かず、Sokobanゲームの小男だけが動くだろう。もし本物のサイトで、このようなゲームがあれば、それ自身のフレームかウインドウに入れて、そのウインドウ自身のイベントだけを掴むようにするのが、おそらくベストだ。

### <a name="Ex13-5">[演習 13.5]</a>

出口まで運ばれた時、岩はむしろ突然に消える。Square.clearContentメソッドを変更して、取り除かれた岩が'落ちる'アニメーションを表示してみよう。先に岩が一瞬小さくなり、それから消える。例えば、通常の半分の大きさであれば、style.width = "50%"と同様にstyle.heightを使って、その画像を表示できる。

アニメーションのタイミングを操るのにはsetIntervalを使える。終わった後はメソッドを確実にクリアしなければならないことに注意。もしこれがそうしないと、ページが閉じられるまで、コンピュータの処理時間が浪費され続ける。

```javascript
Square.clearContent = function() {
  self.content = null;
  var image = this.tableCell.lastChild;
  var size = 100;

  var animate = setInterval(function() {
    size -= 10;
    image.style.width = size + "%";
    image.style.height = size + "%";

    if (size < 60) {
      clearInterval(animate);
      removeElement(image);
    }
  }, 70);
};
```

今、もし無駄遣いできる時間があれば、全てのレベルをクリアしてみよう。

便利な他のイベント型にはfocusとblurがある。フォームのinputのようなこれは要素が'フォーカス'を得た時に発火する。フォーカスは、明らかに、要素にフォーカスした時に起こり、例えば、それをクリックした時だ。blurはJavaScript語では'フォーカスを失う'ということで、その要素からフォーカスが放れた時に発火する。

```javascript
addHandler($("textfield"), "focus", function(event) {
  event.target.style.backgroundColor = "yellow";
});
addHandler($("textfield"), "blur", function(event) {
  event.target.style.backgroundColor = "";
});
```

フォームのinputに関するもう一つのイベントにはchangeがある。これはinputの内容が変更された時に発火する... text inputのような例外のinputもあり、その場合は要素がフォーカスを失うまで発火されない。、

```javascript
addHandler($("textfield"), "change", function(event) {
  print("Content of text field changed to '",
        event.target.value, "'.");
});
```

打ちたいだけタイプできて、inputの外をクリックしたり、タブを押したり、他の方法でフォーカスを失った時だけイベントが発火する。

フォームにはsubmitイベントもあり、それがサブミットされた時に発火する。その場所が取れるまでサブミットを抑止するために止めることができる。これで前章で見たフォームのバリデーションより良い方法が採れる。submitのハンドラを登録し、フォームの内容が正しくないうちは、それを止めるのである。その方法では、ユーザーがJavaScriptを有効にしなかったときに、即時のバリデーションだけ行われずに、フォームはまだ動くだろう。

windowオブジェクトは文書が完全にロードされた時に発火するloadイベントを持ち、もしスクリプトが何らかの種類の初期化のために、完全に表示されるまで待つことを必要としているときに有益だ。例えば、この本の現在の章ページの演習の解答を隠すスクリプト。まだロードされていないうちは演習を見ることができない。unloadイベントもあって、文書からユーザーが離れた時に発火するが、全てのブラウザで正しくサポートされているものではない。

多くの場合、文書のレイアウトはブラウザに任せるのがベストであるが、JavaScriptの部品で作られた文書のノードだけが正確なサイズを設定できるという効果もある。これをやるなら、ウインドウのresizeイベントを監視し、ウインドウがリサイズされる度に確実にその要素のサイズを再計算すること。

最後に、イベントハンドラについてあなたが知らないだろうことを言わなければならない。InternetExplorerブラウザ（これを書いた時点では、最も多数のウェブ・サーファーに使用されているブラウザである）には、値が正常にクリアされないというバグがある：使われなくなっても、PCのメモリに残ったままになる。これはメモリ・リークとして知られていて、そして、一旦メモリがリークしすぎると、コンピューターは深刻に遅くなる。

このリークはいつ起こるのか？InternetExplorerのガベージ・コレクタの不具合による。このシステムは使わなくなった値を再利用する目的のもので、DOMノードが、そのプロパティの中の1つか、あるいはより間接的な手段で、正常なJavaScriptオブジェクトを参照していて、一方、そのオブジェクトからもDOMノードが参照し返されているとき、両方のオブジェクトが回収されないのだ。これはDOMノードとJavaScriptオブジェクトが異なるシステムで回収されるということによる -- DOMノードを掃除するシステムはJavaScriptオブジェクトから参照されているノードを消さないようにするし、反対に通常のJavaScript値の回収もそのようになっている。

上記の説明のように、問題はイベント・ハンドラに関するものに留まらない。このコードは、例えば、ほんの少し回収不能なメモリを作る。：

```javascript
var jsObject = {link: document.body};
document.body.linkBack = jsObject;
```

InetnetExplorerのようなブラウザは別のページに移動しても、ここに表示したdocument.bodyをまだ保持している。このバグの理由はイベント・ハンドラにしばしば結びついて、ハンドラの登録時に非常に簡単に循環したリンクを作る。DOMノードはそのハンドラへの参照を持っていて、ハンドラは、多くの場合、DOMノードを参照を持っている。この参照が故意によって作られなかった時でも、JavaScriptのスコーピング・ルールは暗黙に追加する傾向がある。この関数を考えよう。：

```javascript
function addAlerter(element) {
  addHandler(element, "click", function() {
    alert("Alert! ALERT!");
  });
}
```

addHandler関数によって作られた関数はelement変数を'見る'ことができる。それが使われないことは、問題にならない -- ただ見ることができるからで、それへの参照がもたれる。この関数を同じ要素のイベントハンドラとして登録することで、循環を作ることができる。

この問題を扱う手段が3つある。1つめのアプローチは、とても人気のある者で、それを無視するということだ。多くのスクリプトは少ししかリークしない、長い間、多くのページを扱って、初めて問題が注意を要するものになる。加えて、問題がとても微妙なのであれば、誰がその責任をあなたに問うだろうか？このアプローチを与えられたプログラマーはしばしばマイクロソフトのそのショボいプログラミングをひどく非難し、問題は彼らの失敗にはないという状態なので、それを自分たちで直そうとはしない。

そのような理由には全く価値がない、もちろん。しかし半分のユーザーがあなたの作ったウェブページで問題にあったら、それが実際的な問題であることを否定するのは難しい。これが'真面目な'サイトで働く人々が、通常はいかなるメモリもリークしないように試みる理由である。これが2つめのアプローチをもたらした：骨惜しみせず、DOMオブジェクトと正規のオブジェクトの間の循環が作られないように確実な仕事をするということだ。これは、例えば、上記のハンドラをこのように書き換えるという意味だ。：

```javascript
function addAlerter(element) {
  addHandler(element, "click", function() {
    alert("Alert! ALERT!");
  });
  element = null;
}
```

今element変数はDOMノードを指しておらず、ハンドラはメモリリークしない。このアプローチは実行可能だが、プログラマに本当に注意を払うことを要求する。

3つめの解決法、最後に、リークするような構造を作ることをあまり気にしない、しかし、それができる時には確実に掃除する。これは、必要でなくなったらイベント・ハンドラの登録を外し、onunloadイベントに、ページがアンロードされたら必要でなくなったハンドラの登録を外す処理を登録するということだ。addHandler関数のように、イベント登録システムを拡張することは可能で、これを自動化する。このアプローチを取った時は、イベント・ハンドラだけがメモリ・リークを起こしうるソースであるとは考えないこと -- DOMノードオブジェクトにプロパティを追加すれば同じような問題が引き起こされるかもしれない。
