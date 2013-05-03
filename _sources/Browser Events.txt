=====================================================
Browser Events
=====================================================
..  Chapter 13:
..  Error Handling

..  To add interesting functionality to a web-page, just being able to inspect or modify the document is generally not enough. We also need to be able to detect what the user is doing, and respond to it. For this, we will use a thing called event handlers. Pressed keys are events, mouse clicks are events, even mouse motion can be seen as a series of events. In chapter 11, we added an onclick property to a button, in order to do something when it was pressed. This is a simple event handler.

面白い機能をウェブページに追加するには、ただ、文書を監視し更新するだけでは一般的には十分ではない。ユーザーが行ったことを検知し、それに応答することも必要だ。このために、イベント・ハンドラというものを使う。キーが押されたというイベント、マウスがクリックされたというイベント、マウスの動きも一連のイベントと見ることができる。11章で、ボタンに、それが押された時のためのonclickプロパティを与えた。これは単純なイベント・ハンドラだ。

..  The way browser events work is, fundamentally, very simple. It is possible to register handlers for specific event types and specific DOM nodes. Whenever an event occurs, the handler for that event, if any, is called. For some events, such as key presses, knowing just that the event occurred is not good enough, you also want to know which key was pressed. To store such information, every event creates an event object, which the handler can look at.

このブラウザ・イベントの働きかたは、基本的には、とても単純だ。個々のイベント・タイプと個々のDOMノードに対してハンドラを登録することが可能だ。イベントが起こったら、そのイベントに対するハンドラが、もしあれば、呼び出される。キー押下のような、いくつかのイベントには、ただイベントが起こったと分るだけでは不十分なので、どのキーが押されたかも知ることができる。そのような情報を格納するため、全てのイベントeventオブジェクトを作り、ハンドラはそれを見る。

..  It is important to realise that, even though events can fire at any time, no two handlers ever run at the same moment. If other JavaScript code is still running, the browser waits until it finishes before it calls the next handler. This also holds for code that is triggered in other ways, such as with setTimeout. In programmer jargon, browser JavaScript is single-threaded, there are never two 'threads' running at the same time. This is, in most cases, a good thing. It is very easy to get strange results when multiple things happen at the same time.

いつイベントが起こるかにかかわらず、同時に2つのハンドラが実行されることはないことを理解することは重要だ。もし他のJavaScriptコードがまだ実行中であれば、ブラウザは次のハンドラを呼ぶ前にその終了を待つ。これはsetTimeoutのような、他の手段で起動されたコードでも維持される。プログラマのジャーゴンでは、ブラウザのJavaScriptはシングル・スレッドであり、同時に2つの'スレッド'が実行されることは無い。多くの場合において、これは良いことだ。同時に複数のことが起こるときには、いとも簡単におかしな結果になるものだ。

..  An event, when not handled, can 'bubble' through the DOM tree. What this means is that if you click on, for example, a link in a paragraph, any handlers associated with the link are called first. If there are no such handlers, or these handlers do not indicate that they have finished handling the event, the handlers for the paragraph, which is the parent of the link, are tried. After that, the handlers for document.body get a turn. Finally, if no JavaScript handlers have taken care of the event, the browser handles it. When clicking a link, this means that the link will be followed.

イベントがあって、それがハンドルされないときは、'泡のように'DOMツリーを浮かび上がっていく。これは、もしクリックがあったとき、例えばそれが段落中のリンクであれば、リンクに結びつけられたハンドラが最初に呼ばれることを意味する。そのようなハンドラが無いか、これらのハンドラがそのイベントの終了を示さないときは、リンクの親である段落のハンドラが試される。その後は、document.bodyのハンドラの番が回ってくる。最後に、イベントを扱うJavaScriptのハンドラが無くなったら、ブラウザがそれを扱う。リンクをクリックしたとき、これはそのリンクをたどるということを意味する。

..  So, as you see, events are easy. The only hard thing about them is that browsers, while all supporting more or less the same functionality, support this functionality through different interfaces. As usual, the most incompatible browser is Internet Explorer, which ignores the standard that most other browsers follow. After that, there is Opera, which does not properly support some useful events, such as the onunload event which fires when leaving a page, and sometimes gives confusing information about keyboard events.

見ての通り、イベントは簡単だ。それについて難しいところは、ブラウザで、サポートしている機能の大小があったり、この機能を異なるインターフェースでサポートしていることだ。普通は、最も互換性の少ないブラウザはInternetExplorerで、多くのブラウザが従っている標準を無視している。その次に、Opera、ページを去るときに発火するonunloadのような有益なイベントのいくつかを正しくサポートしておらず、またあるキーボードイベントについての混乱した情報を与える。

..  There are four event-related actions one might want to take.

..  Registering an event handler.
.. Getting the event object.
.. Extracting information from this object.
.. Signalling that an event has been handled.

.. None of them work the same across all major browsers.

イベントに関するアクションで取っておきたいものが4つある。

* イベント・ハンドラを登録する。
* eventオブジェクトを取得する。
* このオブジェクトから情報を展開する。
* イベントがハンドルされたという合図を送る。

これらのうち、全てのメジャーなブラウザで共通のものは1つも無い。

..  As a practice field for our event-handling, we open a document with a button and a text field. Keep this window open (and attached) for the rest of the chapter.

イベント・ハンドリングの練習台として、1つのボタンと1つのテキスト・フィールドを持つ文書を開こう。この章の続きの間、このウインドウは開いたままに（接続したままに）しておくこと。

.. code-block:: javascript

    attach(window.open("example_events.html"));

..  The first action, registering a handler, can be done by setting an element's onclick (or onkeypress, and so on) property. This does in fact work across browsers, but it has an important drawback ― you can only attach one handler to an element. Most of the time, one is enough, but there are cases, especially when a program has to be able to work together with other programs (which might also be adding handlers), that this is annoying.

最初のアクション、ハンドラの登録は、要素のonclick（またはonkeypress、とか）プロパティを設定することで行われる。これは実際ブラウザに渡って動くが、重要な不都合もある -- 1つのイベントには1つのハンドラしか接続できない。多くの時間、これで十分だが、そうでない場合もあって、特にプログラムが他のプログラム（これもまたハンドラを追加しているかもしれない）と一緒に動かなければならない時には、これが邪魔になる。

..  In Internet Explorer, one can add a click handler to a button like this:

InternetExplorerでは、ボタンのクリックのハンドラはこのように追加する。：

.. code-block:: javascript

    $("button").attachEvent("onclick", function(){print("Click!");});

..  On the other browsers, it goes like this:

他のブラウザでは、このようになる。：

.. code-block:: javascript

    $("button").addEventListener("click", function(){print("Click!");},
                                 false);

..  Note how "on" is left off in the second case. The third argument to addEventListener, false, indicates that the event should 'bubble' through the DOM tree as normal. Giving true instead can be used to give this handler priority over the handlers 'beneath' it, but since Internet Explorer does not support such a thing, this is rarely useful.

"on"は2番めでは消されていることに注意。addEventListenerの3つめの引数、false、はこのイベントを通常のようにDOMツリーの上に'浮かび上がらせない'ようにすることを示す。代わりにTrueが与えられると、このハンドラの優先度がその下の他のハンドラより高くなるが、InternetExplorerがこれをサポートしていないため、これが有益なことはまれだ。

Ex. 13.1

..  Write a function called registerEventHandler to wrap the incompatibilities of these two models. It takes three arguments: first a DOM node that the handler should be attached to, then the name of the event type, such as "click" or "keypress", and finally the handler function.

これら2つのモデルの非互換性をラップするregisterEventHandlerという関数を書け。3つの引数を取る：1つめはハンドラが接続するDOMノード、2つめは"クリック"や"キー押下"のようなイベントの型、3つめはハンドラの関数とする。

..  To determine which method should be called, look for the methods themselves ― if the DOM node has a method called attachEvent, you may assume that this is the correct method. Note that this is much preferable to directly checking whether the browser is Internet Explorer. If a new browser arrives which uses Internet Explorer's model, or Internet Explorer suddenly switches to the standard model, the code will still work. Both are rather unlikely, of course, but doing something in a smart way never hurts.

呼び出されたメソッドの識別には、メソッドそれ自体を見る -- もしDOMノードがattachEventというメソッドを持っていれば、これが正しいメソッドだと仮定できる。直接InternetExplorerかをチェックするよりこちらの方が望ましいことに注意しよう。もしInternetExplorerのモデルを使った新しいブラウザが登場しても、またはInternetExplorerが標準のモデルに突然スイッチしても、このコードはまだ動く。どちらも好ましくないのはもちろんだが、傷つきにくい、賢い方法でやろう。

.. code-block:: javascript

    function registerEventHandler(node, event, handler) {
      if (typeof node.addEventListener == "function")
        node.addEventListener(event, handler, false);
      else
        node.attachEvent("on" + event, handler);
    }

    registerEventHandler($("button"), "click",
                         function(){print("Click (2)");});

..  Don't fret about the long, clumsy name. Later on, we will have to add an extra wrapper to wrap this wrapper, and it will have a shorter name.

名前が長くて面倒でも悩まないように。後で、このラッパーをさらにラップする、拡張のラッパーを追加する。その時に短い名前を付けよう。

..  It is also possible to do this check only once, and define registerEventHandler to hold a different function depending on the browser. This is more efficient, but a little strange.

このチェックを1回だけにすることも可能で、ブラウザによって異なる関数をregisterEventHandlrerを定義すればいい。これはより効果的だが、ちょっとおかしい。

.. code-block:: javascript

    if (typeof document.addEventListener == "function")
      var registerEventHandler = function(node, event, handler) {
        node.addEventListener(event, handler, false);
      };
    else
      var registerEventHandler = function(node, event, handler) {
        node.attachEvent("on" + event, handler);
      };

..  Removing events works very much like adding them, but this time the methods detachEvent and removeEventListener are used. Note that, to remove a handler, you need to have access to the function you attached to it.

イベントを取り除くのは追加とよく似ているが、このときはメソッドdetachEventとremoveEventListenerが使われる。ハンドラを取り除くには、それに繋げた関数にアクセスする必要があることに注意しよう。

.. code-block:: javascript

    function unregisterEventHandler(node, event, handler) {
      if (typeof node.removeEventListener == "function")
        node.removeEventListener(event, handler, false);
      else
        node.detachEvent("on" + event, handler);
    }

..  Exceptions produced by event handlers can, because of technical limitations, not be caught by the console. Thus, they are handled by the browser, which might mean they get hidden in some kind of 'error console' somewhere, or cause a message to pop up. When you write an event handler and it does not seem to work, it might be silently aborting because it causes some kind of error.

技術的な限界のために、イベント・ハンドラで作られた例外は、コンソールで捕まえられない。すなわち、これらはブラウザでハンドルされ、'エラー・コンソール'のようなところに隠されたり、またはポップアップ・メッセージを起こすということだ。イベント・ハンドラを書き、それが動いていないように見えるときは、それが起こしたエラーの種類のために静かに異常終了しているのかもしれない。

..  Most browsers pass the event object as an argument to the handler. Internet Explorer stores it in the top-level variable called event. When looking at JavaScript code, you will often come across something like event || window.event, which takes the local variable event or, if that is undefined, the top-level variable by that same name.

多くのブラウザではハンドラへの引数としてeventオブジェクトを渡す。InternetExplorerはそれをeventというトップレベルの変数に格納する。JavaScriptコードを見るときに、event || window.eventのようなものをしばしば見るだろう。ローカルな変数のeventか、もしそれがundefinedであれば、同じ名前を付けることによってトップレベルの変数になる。

.. code-block:: javascript

    function showEvent(event) {
      show(event || window.event);
    }

    registerEventHandler($("textfield"), "keypress", showEvent);

..  Type a few characters in the field, look at the objects, and shut it up again:

フィールドに何文字かタイプして、オブジェクトを見たら、それを閉めよう。

.. code-block:: javascript

    unregisterEventHandler($("textfield"), "keypress", showEvent);

..  When the user clicks his mouse, three events are generated. First mousedown, at the moment the mouse button is pressed. Then, mouseup, at the moment it is released. And finally, click, to indicate something was clicked. When this happens two times in quick succession, a dblclick (double-click) event is also generated. Note that it is possible for the mousedown and mouseup events to happen some time apart ― when the mouse button is held for a while.

ユーザーがマウスをクリックしたとき、3つのイベントが生成される。最初はmousedown、マウスのボタンが押された瞬間に。それから、mouseup、その指がボタンから離れた瞬間に。最後に、click、何かがクリックされたことを示す。これが素早く2回起こったときは、dblclick（ダブル・クリック）イベントも生成される。mousedownとmouseupイベントが時間を経て発生する -- マウスボタンがしばらく押されっぱなしにされる、こともあるので注意しよう。

..  When you attach an event handler to, for example, a button, the fact that it has been clicked is often all you need to know. When the handler, on the other hand, is attached to a node that has children, clicks from the children will 'bubble' up to it, and you will want to find out which child has been clicked. For this purpose, event objects have a property called target... or srcElement, depending on the browser.

イベント・ハンドラに接続したとき、例えば、ボタンなら、それがクリックされたことさえ分れば良い。その一方で、ハンドラが子があるノードに結びついて、クリックが子から'伝播して'来たときは、どの子がクリックされたか知りたいと思うだろう。そのために、eventオブジェクトはtargetというプロパティ、またはsrcElement、ブラウザによっては...を持つ。

..  Another interesting piece of information are the precise coordinates at which the click occurred. Event objects related to the mouse contain clientX and clientY properties, which give the x and y coordinates of the mouse, in pixels, on the screen. Documents can scroll, though, so often these coordinates do not tell us much about the part of the document that the mouse is over. Some browsers provide pageX and pageY properties for this purpose, but others (guess which) do not. Fortunately, the information about the amount of pixels the document has been scrolled can be found in document.body.scrollLeft and document.body.scrollTop.

もう一つ面白い情報の部分はクリックが起こった正確な座標だ。マウスに関連するeventオブジェクトはclientXとclientYというプロパティを持ち、それがマウスのx座標とy座標をスクリーンのピクセルで持つ。文書はスクロール可能なので、マウスが文書のどの部分にあるかはこれらの座標からは分らないブラウザによっては、pageXとpageYプロパティをこの目的のために提供しているが、そうでないものも（ご想像通り）ある。幸運にも、文書がスクロールしたピクセルの数についての情報はdocument.body.scrollLeftとdocument.body.scrollTopから分かる。

..  This handler, attached to the whole document, intercepts all mouse clicks, and prints some information about them.

このハンドラは、文書全体に結びついて、全てのマウス・クリックを拾って、それについての情報を表示する。

.. code-block:: javascript

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

..  And get rid of it again:

これを取り除こう。：

.. code-block:: javascript

    unregisterEventHandler(document, "click", reportClick);

..  Obviously, writing all these checks and workarounds is not something you want to do in every single event handler. In a moment, after we have gotten acquainted with a few more incompatibilities, we will write a function to 'normalise' event objects to work the same across browsers.

明らかに、全ての単純なイベントハンドラ毎に、これら全てをチェックし、手を回すことはやりたくないだろう。しばらくの間、いくつかの非互換性になれた後で、'ノーマライズされた'eventオブジェクトとブラウザ間で同様に動く関数を書こう。

..  It is also sometimes possible to find out which mouse button was pressed, using the which and button properties of event objects. Unfortunately, this is very unreliable ― some browsers pretend mouses have only one button, others report right-clicks as clicks during which the control key was held down, and so on.

どのマウスボタンが押されたのか知るのも可能で、eventオブジェクトのbuttonプロパティを使えばいい。残念ながら、これはとても当てにならない -- あるブラウザはマウスには1つしかボタンが無いかのように装っており、他はクリックを右クリックとして報告するところを、コントロールキーが押された、とか言ってくる。

..  Apart from clicks, we might also be interested in the movement of the mouse. The mousemove event of a DOM node is fired whenever the mouse moves while it is over that element. There are also mouseover and mouseout, which are fired only when the mouse enters or leaves a node. For events of this last type, the target (or srcElement) property points at the node that the event is fired for, while the relatedTarget (or toElement, or fromElement) property gives the node that the mouse came from (for mouseover) or left to (for mouseout).

クリックから離れれば、マウスの動きも面白いかもしれない。DOMノードのmousemoveイベントはそのようその上でマウスが動いている間発火される。mouseoverとmouseoutというものもあり、これらはそのノードにマウスが入ったときと放れた時だけ発火する。この最後の型のイベントでは、target（またはsrcElement）プロパティはイベントが発火したノードを指し、relatedTarget（またはtoElement、fromElement）プロパティはマウスがやってきた(mouseoverの場合)あるいは出ていった(mouseoutの場合)ノードを指す。

..  mouseover and mouseout can be tricky when they are registered on an element that has child nodes. Events fired for the child nodes will bubble up to the parent element, so you will also see a mouseover event when the mouse enters one of the child nodes. The target and relatedTarget properties can be used to detect (and ignore) such events.

mouseoverとmouseoutは、それらが子ノードを持つ要素に登録されたときトリッキーになり得る。子ノードで発火したイベントは、親要素に伝播するので、マウスが子ノードの1つに入ったら親のmouseoverイベントも見るだろう。targetとrelatedTargetプロパティはそのようなイベントを見つけ（無視）するのに使うことができる。

..  For every key that the user presses, three events are generated: keydown, keyup, and keypress. In general, you should use the first two in cases where you really want to know which key was pressed, for example when you want to do something when the arrow keys are pressed. keypress, on the other hand, is to be used when you are interested in the character that is being typed. The reason for this is that there is often no character information in keyup and keydown events, and Internet Explorer does not generate a keypress event at all for special keys such as the arrow keys.

ユーザーが押した全てのキーは、3つのイベントを生成する：keydownとkeyup、keypress。一般的には、最初の2つをどのキーが押されたか本当に知りたい場合、例えば矢印キーが押されたときに何かしたい時に使う。keypressは、他方、タイプされた文字に興味があるときに使われる。この理由は、keyupとkeydownのイベントにはしばしば文字の情報が含まれないことがあり、そしてInternetExplorerが矢印キーのような特殊キーのイベントを生成しないことである。

..  Finding out which key was pressed can be quite a challenge by itself. For keydown and keyup events, the event object will have a keyCode property, which contains a number. Most of the time, these codes can be used to identify keys in a reasonably browser-independant way. Finding out which code corresponds to which key can be done by simple experiments...

どのキーが押されたか判別するのはそれ自体全くのチャレンジになり得る。keydownとkeyupのイベントでは、eventオブジェクトはkeyCodeプロパティを持ち、これには数字が入っている。多くの場合、これらのコードはブラウザに依存しない方法でキーを識別するのに使える。キーに結びつけられたコードを見つけるのは単純な実験でできる。

.. code-block:: javascript

    function printKeyCode(event) {
      event = event || window.event;
      print("Key ", event.keyCode, " was pressed.");
    }

    registerEventHandler($("textfield"), "keydown", printKeyCode);
    unregisterEventHandler($("textfield"), "keydown", printKeyCode);

..  In most browsers, a single key code corresponds to a single physical key on your keyboard. The Opera browser, however, will generate different key codes for some keys depending on whether shift is pressed or not. Even worse, some of these shift-is-pressed codes are the same codes that are also used for other keys ― shift-9, which on most keyboards is used to type a parenthesis, gets the same code as the down arrow, and as such is hard to distinguish from it. When this threatens to sabotage your programs, you can usually resolve it by ignoring key events that have shift pressed.

多くのブラウザにおいて、単一のキーコードがキーボードの単一の物理的なキーに結びついている。Operaブラウザでは、しかしながら、シフトキーが押されているか否かで異なるキーコードが生成される。悪いことには、これらのシフトが押された時のコードには他のキーで使われているものと同じであるものもある -- シフト-9、プログラムでは、多くのキーボードでは括弧をタイプするときに使うが、下矢印と同じコードになり、かつその識別は困難である。これはプログラムをサボタージュするよう脅かし、シフトが押された時のキー・イベントを無視するというのが通常の解決になる。

..  To find out whether the shift, control, or alt key was held during a key or mouse event, you can look at the shiftKey, ctrlKey, and altKey properties of the event object.

キーやマウスのイベントにおいて、シフト、コントロール、altキーが押されているか判別するには、eventオブジェクトのshiftKey、ctrlKey、altKeyプロパティを見る。

..  For keypress events, you will want to know which character was typed. The event object will have a charCode property, which, if you are lucky, contains the Unicode number corresponding to the character that was typed, which can be converted to a 1-character string by using String.fromCharCode. Unfortunately, some browsers do not define this property, or define it as 0, and store the character code in the keyCode property instead.

keypressイベントでは、タイプされた文字を知ることができる。eventオブジェクトはcharCodeプロパティを持ち、これは、もしラッキーなら、タイプされた文字のUnicode数値を持ち、String.fromCharCodeで1文字の文字列に変換できる。残念ながら、あるブラウザはこのプロパティを定義していない、あるいは0として定義していて、代わりに文字コードをkeyCodeプロパティに格納している。

.. code-block:: javascript

    function printCharacter(event) {
      event = event || window.event;
      var charCode = event.charCode;
      if (charCode == undefined || charCode === 0)
        charCode = event.keyCode;
      print("Character '", String.fromCharCode(charCode), "'");
    }

    registerEventHandler($("textfield"), "keypress", printCharacter);
    unregisterEventHandler($("textfield"), "keypress", printCharacter);

..  An event handler can 'stop' the event it is handling. There are two different ways to do this. You can prevent the event from bubbling up to parent nodes and the handlers defined on those, and you can prevent the browser from taking the standard action associated with such an event. It should be noted that browsers do not always follow this ― preventing the default behaviour for the pressing of certain 'hotkeys' will, on many browsers, not actually keep the browser from executing the normal effect of these keys.

イベント・ハンドラはイベントのハンドリングを'停止'することができる。これには2つの方法がある。ハンドラが定義されている親ノードへイベントが伝播するのを防止することもできるし、そのようなイベントに結びついた標準的なアクションをブラウザが取るのを防止することもできる。ブラウザは常にこれに従わないことに注意しよう -- 決まった'ホットキー'を押したときのデフォルトの振る舞いを抑止すると、多くのブラウザでは、これらのキーの通常の効果の実行も実際には維持されなくなる。

..  On most browsers, stopping event bubbling is done with the stopPropagation method of the event object, and preventing default behaviour is done with the preventDefault method. For Internet Explorer, this is done by setting the cancelBubble property of this object to true, and the returnValue property to false, respectively.

多くのブラウザで、イベントの伝播の停止はeventオブジェクトのstopPropagationメソッドで、デフォルトの振る舞いの帽子はpreventDefaultメソッドで行われる。InternetExplorerでは、これらはそれぞれ、そのオブジェクトのcancelBubbleプロパティが真に、そしてreturnValueプロパティを偽に設定することで行われる。

..  And that was the last of the long list of incompatibilities that we will discuss in this chapter. Which means that we can finally write the event normaliser function and move on to more interesting things.

そして非互換の長いリストについてはこの章で論じる。これはとうとうイベントを正常化する関数を書くことができ、より面白い話題に移れるということを意味する。

.. code-block:: javascript

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

..  A stop method is added, which cancels both the bubbling and the default action of the event. Some browsers already provide this, in which case we leave it as it is.

stopメソッドが追加された。これはイベントの伝播とデフォルトのアクションの両方をキャンセルする。あるブラウザは既にこれを提供していて、その場合は、それをそのまま残している。

..  Next we can write convenient wrappers for registerEventHandler and unregisterEventHandler:

次はregisterEventHandlerとunregisterEventHandlerの便利なラッパーを書ける。

.. code-block:: javascript

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

..  The new addHandler function wraps the handler function it is given in a new function, so it can take care of normalising the event objects. It returns an object that can be given to removeHandler when we want to remove this specific handler. Try typing a 'q' in the text field.

新しいaddHandler関数は、eventオブジェクトの正常化の面倒を見る新しい関数に、与えられたハンドラ関数をラップする。この個別のハンドラを取り除きたいときにはremoveHandlerに与えることでオブジェクトを戻す。'q'をテキスト・フィールドにタイプしてみよう。

.. code-block:: javascript

    removeHandler(blockQ);

..  Armed with addHandler and the dom function from the last chapter, we are ready for more challenging feats of document-manipulation. As an exercise, we will implement the game known as Sokoban. This is something of a classic, but you may not have seen it before. The rules are this: There is a grid, made up of walls, empty space, and one or more 'exits'. On this grid, there are a number of crates or stones, and a little dude that the player controls. This dude can be moved horizontally and vertically into empty squares, and can push the boulders around, provided that there is empty space behind them. The goal of the game is to move a given number of boulders into the exits.

addHandlerと前章のdom関数を身に付け、文書操作のよりチャレンジングな事柄への準備ができた。練習として、倉庫番として知られるゲームを実装してみよう。これは古典とも言うべきものだが、あなたはこれまで知らなかったかもしれない。ルールはこうだ：壁によって作られたマス目があって、空いてるスペースだったり、1つかそれ以上の'出口'がある。このマス目に、木箱や石がいくつもあって、プレイヤーが動かせるのは小さい男だけ。男は水平と垂直の方向が空いていれば移動することができ、周りの岩を押して、その背後が空であれば運ぶことができる。ゲームのゴールは与えられた数の岩を出口まで運ぶことである。

..  Just like the terraria from chapter 8, a Sokoban level can be represented as text. The variable sokobanLevels, in the example_events.html window, contains an array of level objects. Each level has a property field, containing a textual representation of the level, and a property boulders, indicating the amount of boulders that must be expelled to finish the level.

8章の飼育器のように、倉庫番のレベルもテキストで表現できる。example_events.htmlウインドウの、変数sokobanLevelsはlevelオブジェクトの配列を含み、それぞれのlevelはfieldプロパティを持ち、これにそのレベルのテキスト表現とbouldersプロパティを入れ、bouldersプロパティはそのレベルで追い出さねばならない岩の数を示す。

.. code-block:: javascript

    show(sokobanLevels.length);
    show(sokobanLevels[1].boulders);
    forEach(sokobanLevels[1].field, print);

..  In such a level, the # characters are walls, spaces are empty squares, 0 characters are used for for boulders, an @ for the starting location of the player, and a * for the exit.

このようなレベルで、#文字は壁、スペースは空の場所、0文字は岩があるところ、@はプレイヤーの開始位置、*は出口である。

..  But, when playing the game, we do not want to be looking at this textual representation. Instead, we will put a table into the document. I made small style-sheet (sokoban.css, if you are curious what it looks like) to give the cells of this table a fixed square size, and added it to the example document. Each of the cells in this table will get a background image, representing the type of the square (empty, wall, or exit). To show the location of the player and the boulders, images are added to these table cells, and moved to different cells as appropriate.

しかし、ゲームをプレイするとき、このテキスト表現の見かけは変えたくない。代わりに、文書にテーブルを入れよう。小さなスタイルシート（もし中身を見たければ、sokoban.cssがそれだ）でテーブルのセルに正方形のサイズを設定し、例を文書に追加した。テーブルのそれぞれのセルは背景画像を得て、その四角のタイプ（空、壁、出口）を表現する。プレイヤーと岩の場所を見せるには、画像をこのテーブルのセルに追加し、違う妥当なセルに動かす。

..  It would be possible to use this table as the main representation of our data ― when we want to look whether there is a wall in a given square, we just inspect the background of the appropriate table cell, and to find the player, we just search for the image node with the correct src property. In some cases, this approach is practical, but for this program I chose to keep a separate data structure for the grid, because it makes things much more straightforward.

このテーブルをデータの主な表現として使うこともできた -- 与えられた資格に壁があるかどうか見たいときは、ただ、テーブルの正しいセルの背景を見て、プレイヤーを探すには、正しいsrcプロパティを持つimgノードを探す。場合によっては、このアプローチが実用的だが、このプログラムではマス目のデータ構造を分離して持つことを選び、それはその方が素直だからである。

..  This data structure is a two-dimensional grid of objects, representing the squares of the playing field. Each of the objects must store the type of background it has and whether there is a boulder or player present in that cell. It should also contain a reference to the table cell that is used to display it in the document, to make it easy to move images in and out of this table cell.

このデータ構造はオブジェクトの2次元のマス目で、プレイング・フィールドの四角の表現である。それぞれのオブジェクトはその場所の背景の種類と岩やプレイヤーがいるかどうかを示す。それは文書の表示のためのテーブルのセルへの参照も含み、画像を動かしたり、このテーブルセルを出し入れするのを簡単にする。

..  That gives us two kinds of objects ― one to hold the grid of the playing field, and one to represent the individual cells in this grid. If we want the game to also do things like moving the next level at the appropriate moment, and being able to reset the current level when you mess up, we will also need a 'controller' object, which creates or removes the field objects at the appropriate moment. For convenience, we will be using the prototype approach outlined at the end of chapter 8, so object types are just prototypes, and the create method, rather than the new operator, is used to make new objects.

2種類のオブジェクトが与えられる -- 1つはプレイング・フィールドのマス目を格納し、1つはマス目上の個々のセルを表現する。もし、ふさわしい時にゲームを次のレベルに勧めたいと思ったり、散らかった現在のレベルをリセットしたい時のために、'コントローラ'のオブジェクトも必要で、フィールド・オブジェクトをふさわしい時に作ったり取り除いたりする。利便性のために、8章の最後で要点を示したプロトタイプのアプローチを使い、オブジェクト型をプロトタイプとし、new演算子よりcreateメソッドを新しいオブジェクトを作るのに使おう。

..  Let us start with the objects representing the squares of the game's field. They are responsible for setting the background of their cells correctly, and adding images as appropriate. The img/sokoban/ directory contains a set of images, based on another ancient game, which will be used to visualise the game. For a start, the Square prototype could look like this.

ゲームフィールドの四角を表現するオブジェクトから始めよう。セルに設定された背景を正しく応答し、ふさわしい画像を追加する。img/sokobanには、他の古いゲームに基づいた画像のセットが入っていて、ゲームを視覚化するのに使える。手始めに、Squareプロトタイプはこのようになった。：

.. code-block:: javascript

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

..  The character argument to the constructor will be used to transform characters from the level blueprints into actual Square objects. To set the background of the cells, style-sheet classes are used (defined in sokoban.css), which are assigned to the td elements' className property.

コンストラクタのcharacter引数はレベルの青写真を文字に変形して実際のSquareオブジェクトに入れるのに使う。セルの背景を設定するには、スタイルシートの（sokoban.cssで定義された）classを使い、それらはtd要素のclassNameプロパティに割り当てられている。

..  The methods like hasPlayer and isEmpty are a way to 'isolate' the code that uses objects of this type from the internals of the objects. They are not strictly necessary in this case, but they will make the other code look better.

hasPlayerとisEmptyのようなメソッドは、この型のオブジェクトを使うコードをオブジェクトの内部から分離する手段だ。この場合には厳密には必要なものでは無いが、他のコードは良いものになるだろう。

Ex. 13.2

..  Add methods moveContent and clearContent to the Square prototype. The first one takes another Square object as an argument, and moves the content of the this square into the argument by updating the content properties and moving the image node associated with this content. This will be used to move boulders and players around the grid. It may assume the square is not currently empty. clearContent removes the content from the square without moving it anywhere. Note that the content property for empty squares contains null.

moveContentとclearContentメソッドをSquareプロトタイプに追加せよ。1つめはもう一つのSquareオブジェクトを引数とし、引数のsquareのcontentプロパティを更新し、このcontentに結びついたimgノードを移動することで、このsquareの中身を移動する。これは岩とプレイヤーを周囲のマスに移動するのに使う。四角は現在空でないことを仮定して良い。clearContentはsquareからcontentをどこにも移動せずに取り除く。空のマスのcontentプロパティはnullでないことに注意。

..  The removeElement function we defined in chapter 12 is available in this chapter too, for your node-removing convenience. You may assume that the images are the only child nodes of the table cells, and can thus be reached through, for example, this.tableCell.lastChild.

12章で定義されたremoveElement関数はこの章でも利用可能で、ノードの削除に便利だ。画像はテーブルのセルの子ノードのみであり、this.tableCsll.lastChildで到達できると仮定して良い。

.. code-block:: javascript

    Square.moveContent = function(target) {
      target.content = this.content;
      this.content = null;
      target.tableCell.appendChild(this.tableCell.lastChild);
    };
    Square.clearContent = function() {
      this.content = null;
      removeElement(this.tableCell.lastChild);
    };

..  The next object type will be called SokobanField. Its constructor is given an object from the sokobanLevels array, and is responsible for building both a table of DOM nodes and a grid of Square objects. This object will also take care of the details of moving the player and boulders around, through a move method that is given an argument indicating which way the player wants to move.

次のオブジェクト型はSokobanFieldという。そのコンストラクタはsokobanLevels配列を与えられ、DOMノードとSquareオブジェクトのマス目の両方を組み立てて返す。このオブジェクトはプレイヤーと周りの岩の動きの詳細も扱い、moveメソッドはプレイヤーが移動したい方向を示す引数を与えられる。

..  To identify the individual squares, and to indicate directions, we will again use the Point object type from chapter 8, which, as you might remember, has an add method.

個々のsquareの識別し、方向を示すために、8章のPointオブジェクトを再び使おう。覚えているかもしれないが、これはaddメソッドを持っている。

..  The base of the field prototype looks like this:

fieldプロトタイプの基礎はこのようになる。：

.. code-block:: javascript

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

..  The constructor goes over the lines and characters in the level, and stores the Square objects in the squares property. When it encounters the square with the player, it saves this position as playerPos, so that we can easily find the square with the player later on. getSquare is used to find a Square object corresponding to a certain x,y position on the field. Note that it doesn't take the edges of the field into account ― to avoid writing some boring code, we assume that the field is properly walled off, making it impossible to walk out of it.

コンストラクタはレベルの線と文字に立ち寄って、squaresプロパティにSquareオブジェクトを格納する。プレイヤーがいるsquareに遭ったら、この位置をplayerPosに保存しておき、後で楽にプレイヤーのいるsquareを探せるようにする。getSquareはフィールドの特定のx、y位置に結びついたSquareオブジェクトを探すのに使う。フィールドの端を勘定に入れないことに注意 -- 退屈なコードを書くのを避けるため、フィールドは適切に壁に隔てられ、そこから抜け出すのは不可能であると仮定しておこう。。

..  The word "class" in the dom call that makes the table node is quoted as a string. This is necessary because class is a 'reserved word' in JavaScript, and may not be used as a variable or property name.

tableノードを作るdomの中の"class"という語は文字列として引用される。これは、classがJavaScriptの'予約語'であるため、変数名やプロパティ名として使えないために必要だ。

..  The amount of boulders that have to be cleared to win the level (this may be less than the total amount of boulders on the level) is stored in bouldersToGo. Whenever a boulder is brought to the exit, we can subtract 1 from this, and see whether the game is won yet. To show the player how he is doing, we will have to show this amount somehow. For this purpose, a div element with text is used. div nodes are containers without inherent markup. The score text can be updated with the updateScore method. The won method will be used by the controller object to determine when the game is over, so the player can move on to the next level.

レベルをクリアするために片付けるべき岩の数（そのレベルの岩の数の合計より少ないかもしれない）がboulderToGoである。岩が出口に運ばれたら、これから1を引いて、ゲームがもうクリアされたかをチェックする。プレイヤーに既にやっつけた数を見せるため、この数を何らかの方法で表示する。この目的のため、テキストのdiv要素を使う。divノードは固有のマークアップのないコンテナである。得点のテキストはupdateScoreメソッドで更新できる。wonメソッドはコントローラ・オブジェクトによりゲームが終了したか、プレイヤーが次のレベルに勧めるかのの判定に使われる。

..  If we want to actually see the playing field and the score, we will have to insert them into the document somehow. That is what the place method is for. We'll also add a remove method to make it easy to remove a field when we are done with it.

もし実際にプレイング・フィールドと得点を見空ければ、何ら可能方法で文書にこれらを挿入しなければならない。このためにplaceメソッドがある。ゲームが終わった時にフィールドを簡単に取り除くためにremoveメソッドも追加しよう。

.. code-block:: javascript

    SokobanField.place = function(where) {
      where.appendChild(this.score);
      where.appendChild(this.table);
    };
    SokobanField.remove = function() {
      removeElement(this.score);
      removeElement(this.table);
    };

    testField.place(document.body);

..  If all went well, you should see a Sokoban field now.

もし全部うまくいっていたら、Sokobanフィールドを今見ているはずだ。

Ex. 13.3

..  But this field doesn't do very much yet. Add a method called move. It takes a Point object specifying the move as argument (for example -1,0 to move left), and takes care of moving the game elements in the correct way.

しかし、このフィールドはまだ上手く動かない。moveメソッドを追加せよ。動きを指示するPointオブジェクト（例えば、-1,0なら左に動く）を引数に取り、ゲームの要素の動きを正しい方法で扱う。

..  The correct way is this: The playerPos property can be used to determine where the player is trying to move. If there is a boulder here, look at the square behind this boulder. When there is an exit there, remove the boulder and update the score. When there is empty space there, move the boulder into it. Next, try to move the player. If the square he is trying to move into is not empty, ignore the move.

正しい方法とはこうだ：playerPosプロパティはプレイヤーが動こうとしている場所を判別する。もしそこに岩があれば、この岩の背後を見る。そこが出口であれば、岩は取り除かれて得点が更新される。そこが空のスペースであれば、そこに岩が動く。次に、プレイヤーを動かしてみる。もし、プレイヤーが動こうとしているsquareが空でなければ、移動は無視される。

.. code-block:: javascript

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

..  By taking care of boulders first, the move code can work the same way when the player is moving normally and when he is pushing a boulder. Note how the square behind the boulder is found by adding the direction to the playerPos twice. Test it by moving left two squares:

最初に岩を扱うことで、moveのコードはプレイヤーが普通に動く場合でも岩を押している時でも同様に動く。岩の背後のsquareをdirectionをplayerPosに2回加えることで見つけていることに注意。左に2マス進む場合でテストしよう。

.. code-block:: javascript

    testField.move(new Point(-1, 0));
    testField.move(new Point(-1, 0));

..  If that worked, we moved a boulder into a place from which we can't get it out anymore, so we'd better throw this field away.

もしこれが動けば、もう取り出せない場所に岩を動かしたので、このフィールドを放棄しよう。

.. code-block:: javascript

    testField.remove();

..  All the 'game logic' has been taken care of now, and we just need a controller to make it playable. The controller will be an object type called SokobanGame, which is responsible for the following things:

全ての'ゲーム・ロジック'は今のように扱われ、プレイ可能にするためにコントローラが必要だ。コントローラはSokobanGameという型のオブジェクトで、下記のように応答する。

..  Preparing a place where the game field can be placed.
..  Building and removing SokobanField objects.
..  Capturing key events and calling the move method on current field with the correct argument.
..  Keeping track of the current level number and moving to the next level when a level is won.
..  Adding buttons to reset the current level or the whole game (back to level 0).

* ゲーム・フィールドが位置すべき場所の準備
* SokobanFieldオブジェクトの組み立てと削除
* キー・イベントを捕まえて、正しい引数で現在のフィールドのmoveメソッドの呼び出し
* 現在のレベル数の保持と、そのレベルがクリアされたら次のレベルに移動
* 現在のレベルまたはゲーム全体（0レベルに戻る）をリセットするボタンの追加

..  We start again with an unfinished prototype.

再び、未完成のプロトタイプから始めよう。

.. code-block:: javascript

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

..  The constructor builds a div element to hold the field, along with two buttons and a title. Note how method is used to attach methods on the this object to events.

コンストラクタはフィールドとともに2つのボタンとタイトルを保持するdivエレメントを組み立てる。このオブジェクトのイベントにメソッドを結びつけているメソッドがどのように使われているかに注意。

..  We can put a Sokoban game into our document like this:

文書にSokobanゲームを入れるにはこのようにする。：

.. code-block:: javascript

    var sokoban = SokobanGame.create(document.body);

Ex. 13.4

..  All that is left to do now is filling in the key event handler. Replace the keyDown method of the prototype with one that detects presses of the arrow keys and, when it finds them, moves the player in the correct direction. The following Dictionary will probably come in handy:

今残っているのはキー・イベントのハンドラを埋めることだ。プロトタイプのkeyDownメソッドを矢印キーの押下の検知に入れ替えて、もしそれが見つかったら、プレイヤーを正しい方向に動かそう。下記の辞書がおそらく手っ取り早い。：

.. code-block:: javascript

    var arrowKeyCodes = new Dictionary({
      37: new Point(-1, 0), // left
      38: new Point(0, -1), // up
      39: new Point(1, 0),  // right
      40: new Point(0, 1)   // down
    });

..  After an arrow key has been handled, check this.field.won() to find out if that was the winning move. If the player won, use alert to show a message, and go to the next level. If there is no next level (check sokobanLevels.length), restart the game instead.

矢印キーのハンドルの後、this.field.won()をチェックしてこの動きでクリアか判定しよう。もしプレイヤーがクリアしていたら、メッセージを表示するアラートを使い、次のレベルに移動する。もし、次のレベルがもう無ければ（sokobanLevels.lengthでチェック）、代わりにゲームを再スタートしよう。
..  It is probably wise to stop the events for key presses after handling them, otherwise pressing arrow-up and arrow-down will scroll your window, which is rather annoying.

キー押下をハンドルしたらイベントを止めておくのが、おそらく賢明で、そうしなければ上矢印や下矢印を押した時にウインドウがスクロールして、ゲームの邪魔になるだろう。

.. code-block:: javascript

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

..  It has to be noted that capturing keys like this ― adding a handler to the document and stopping the events that you are looking for ― is not very nice when there are other elements in the document. For example, try moving the cursor around in the text field at the top of the document. ― It won't work, you'll only move the little man in the Sokoban game. If a game like this were to be used in a real site, it is probably best to put it in a frame or window of its own, so that it only grabs events aimed at its own window.

キーをこのように捕まえることに注意しなければならない -- イベントを監視するために文書にハンドラを追加したりイベントを停止するのはあまりよくない -- 文書に他の要素がある時には。例えば、文書の上のテキスト・フィールドにカーソルを移動してみよう。 -- これは動かず、Sokobanゲームの小男だけが動くだろう。もし本物のサイトで、このようなゲームがあれば、それ自身のフレームかウインドウに入れて、そのウインドウ自身のイベントだけを掴むようにするのが、おそらくベストだ。

Ex. 13.5

..  When brought to the exit, the boulders vanish rather abrubtly. By modifying the Square.clearContent method, try to show a 'falling' animation for boulders that are about to be removed. Make them grow smaller for a moment before, and then disappear. You can use style.width = "50%", and similarly for style.height, to make an image appear, for example, half as big as it usually is.

出口まで運ばれた時、岩はむしろ突然に消える。Square.clearContentメソッドを変更して、取り除かれた岩が'落ちる'アニメーションを表示してみよう。先に岩が一瞬小さくなり、それから消える。例えば、通常の半分の大きさであれば、style.width = "50%"と同様にstyle.heightを使って、その画像を表示できる。

..  We can use setInterval to handle the timing of the animation. Note that the method makes sure to clear the interval after it is done. If you don't do that, it will continue wasting your computer's time until the page is closed.

アニメーションのタイミングを操るのにはsetIntervalを使える。終わった後はメソッドを確実にクリアしなければならないことに注意。もしこれがそうしないと、パージが閉じられるまで、コンピュータの処理時間が浪費され続ける。

.. code-block:: javascript

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

..  Now, if you have a few hours to waste, try finishing all levels.

今、もし無駄遣いできる時間があれば、全部レベルのクリアしてみよう。

..  Other event types that can be useful are focus and blur, which are fired on elements that can be 'focused', such as form inputs. focus, obviously, happens when you put the focus on the element, for example by clicking on it. blur is JavaScript-speak for 'unfocus', and is fired when the focus leaves the element.

便利な他のイベント型にはfocusとblurがある。フォームのinputのようなこれは要素が'フォーカス'を得た時に発火する。フォーカスは、明らかに、要素にフォーカスした時に起こり、例えば、それをクリックした時だ。blurはJavaScript語では'フォーカスを失う'ということで、その要素からフォーカスが放れた時に発火する。

.. code-block:: javascript

    addHandler($("textfield"), "focus", function(event) {
      event.target.style.backgroundColor = "yellow";
    });
    addHandler($("textfield"), "blur", function(event) {
      event.target.style.backgroundColor = "";
    });

..  Another event related to form inputs is change. This is fired when the content of the input has changed... except that for some inputs, such as text inputs, it does not fire until the element is unfocused.

フォームのinputに関するもう一つのイベントにはchangeがある。これはinputの内容が変更された時に発火する...text inputのような例外のinputもあり、その場合は要素がフォーカスを失うまで発火されない。、

.. code-block:: javascript

    addHandler($("textfield"), "change", function(event) {
      print("Content of text field changed to '",
            event.target.value, "'.");
    });

..  You can type all you want, the event will only fire when you click outside of the input, press tab, or unfocus it in some other way.

打ちたいだけタイプできて、inputの外をクリックしたり、タブを押したり、他の方法でフォーカスを失った時だけイベントが発火する。

..  Forms also have a submit event, which is fired when they submit. It can be stopped to prevent the submit from taking place. This gives us a much better way to do the form validation we saw in the previous chapter. You just register a submit handler, which stops the event when the content of the form is not valid. That way, when the user does not have JavaScript enabled, the form will still work, it just won't have instant validation.

フォームにはsubmitイベントもあり、それがサブミットされた時に発火する。その場所が取れるまでサブミットを抑止するために止めることができる。これで前章で見たフォームのバリデーションより良い方法が採れる。submitのハンドラを登録し、フォームの内容が正しくないうちは、それを止めるのである。その方法では、ユーザーがJavaScriptを有効にしなかったときに、即時のバリデーションだけ行われずに、フォームはまだ動くだろう。

..  Window objects have a load event that fires when the document is fully loaded, which can be useful if your script needs to do some kind of initialisation that has to wait until the whole document is present. For example, the scripts on the pages for this book go over the current chapter to hide solutions to exercises. You can't do that when the exercises are not loaded yet. There is also an unload event, firing when the user leaves the document, but this is not properly supported by all browsers.

windowオブジェクトは文書が完全にロードされた時に発火するloadイベントを持ち、もしスクリプトが何らかの種類の初期化のために、完全に表示されるまで待つことを必要としているときに有益だ。例えば、この本の現在の章ページの演習の解答を隠すスクリプト。まだロードされていないうちは演習を見ることができない。unloadイベントもあって、文書からユーザーが離れた時に発火するが、全てのブラウザで正しくサポートされているものではない。

..  Most of the time it is best to leave the laying out of a document to the browser, but there are effects that can only be produced by having a piece of JavaScript set the exact sizes of some nodes in a document. When you do this, make sure you also listen for resize events on the window, and re-calculate the sizes of your element every time the window is resized.

多くの場合、文書のレイアウトはブラウザに任せるのがベストであるが、JavaScriptの部品で作られた文書のノードだけが正確なサイズを設定できるという効果もある。これをやるなら、ウインドウのresizeイベントを監視し、ウインドウがリサイズされる度に確実にその要素のサイズを再計算すること。

..  Finally, I have to tell you something about event handlers that you would rather not know. The Internet Explorer browser (which means, at the time of writing, the browser used by a majority of web-surfers) has a bug that causes values to not be cleaned up as normal: Even when they are no longer used, they stay in the machine's memory. This is known as a memory leak, and, once enough memory has been leaked, will seriously slow down a computer.

最後に、イベントハンドラについてあなたが知らないだろうことを言わなければならない。InternetExplorerブラウザ（これを書いた時点では、最も多数のウェブ・サーファーに使用されているブラウザである）には、値が正常にクリアされないというバグがある：使われなくなっても、PCのメモリに残ったままになる。これはメモリ・リークとして知られていて、そして、一旦メモリがリークしすぎると、コンピューターは深刻に遅くなる。

..  When does this leaking occur? Due to a deficiency in Internet Explorer's garbage collector, the system whose purpose it is to reclaim unused values, when you have a DOM node that, through one of its properties or in a more indirect way, refers to a normal JavaScript object, and this object, in turn, refers back to that DOM node, both objects will not be collected. This has something to do with the fact that DOM nodes and other JavaScript objects are collected by different systems ― the system that cleans up DOM nodes will take care to leave any nodes that are still referenced by JavaScript objects, and vice versa for the system that collects normal JavaScript values.

このリークはいつ起こるのか？InternetExplorerのガベージ・コレクタの不具合による。このシステムは使わなくなった値を再利用する目的のもので、DOMノードが、そのプロパティの中の1つか、あるいはより間接的な手段で、正常なJavaScriptオブジェクトを参照していて、一方、そのオブジェクトからもDOMノードが参照し返されているとき、両方のオブジェクトが回収されないのだ。これはDOMノードとJavaScriptオブジェクトが異なるシステムで回収されるということによる -- DOMノードを掃除するシステムはJavaScriptオブジェクトから参照されているノードを消さないようにするし、反対に通常のJavaScript値の回収もそのようになっている。

..  As the above description shows, the problem is not specifically related to event handlers. This code, for example, creates a bit of un-collectable memory:

上記の説明のように、問題はイベント・ハンドラに関するものに留まらない。このコードは、例えば、ほんの少し回収不能なメモリを作る。：

.. code-block:: javascript

    var jsObject = {link: document.body};
    document.body.linkBack = jsObject;

..  Even after such an Internet Explorer browser goes to a different page, it will still hold on to the document.body shown here. The reason this bug is often associated with event handlers is that it is extremely easy to make such circular links when registering a handler. The DOM node keeps references to its handlers, and the handler, most of the time, has a reference to the DOM node. Even when this reference is not intentionally made, JavaScript's scoping rules tend to add it implicitly. Consider this function:

InetnetExplorerのようなブラウザは別のページに移動しても、ここに表示したdocument.bodyをまだ保持している。このバグの理由はイベント・ハンドラにしばしば結びついて、ハンドラの登録時に非常に簡単に循環したリンクを作る。DOMノードはそのハンドラへの参照を持っていて、ハンドラは、多くの場合、DOMノードを参照を持っている。この参照が故意によって作られなかった時でも、JavaScriptのスコーピング・ルールは暗黙に追加する傾向がある。この関数を考えよう。：

.. code-block:: javascript

    function addAlerter(element) {
      addHandler(element, "click", function() {
        alert("Alert! ALERT!");
      });
    }

..  The anonymous function that is created by the addAlerter function can 'see' the element variable. It doesn't use it, but that does not matter ― just because it can see it, it will have a reference to it. By registering this function as an event handler on that same element object, we have created a circle.

addHandler関数によって作られた関数はelement変数を'見る'ことができる。それが使われないことは、問題にならない -- ただ見ることができるからで、それへの参照がもたれる。この関数を同じ要素のイベントハンドラとして登録することで、循環を作ることができる。

..  There are three ways to deal with this problem. The first approach, a very popular one, is to ignore it. Most scripts will only leak a little bit, so it takes a long time and a lot of pages before the problems become noticeable. And, when the problems are so subtle, who's going to hold you responsible? Programmers given to this approach will often searingly denounce Microsoft for their shoddy programming, and state that the problem is not their fault, so they shouldn't be fixing it.

この問題を扱う手段が3つある。1つめのアプローチは、とても人気のある者で、それを無視するということだ。多くのスクリプトは少ししかリークしない、長い間、多くのページを扱って、初めて問題が注意を要するものになる。加えて、問題がとても微妙なのであれば、誰がその責任をあなたに問うだろうか？このアプローチを与えられたプログラマーはしばしばマイクロソフトのそのショボいプログラミングをひどく非難し、問題は彼らの失敗にはないという状態なので、それを自分たちで直そうとはしない。

..  Such reasoning is not entirely without merit, of course. But when half your users are having problems with the web-pages you make, it is hard to deny that there is a practical problem. Which is why people working on 'serious' sites usually make an attempt not to leak any memory. Which brings us to the second approach: Painstakingly making sure that no circular references between DOM objects and regular objects are created. This means, for example, rewriting the above handler like this:

そのような理由には全く価値がない、もちろん。しかし半分のユーザーがあなたの作ったウェブページで問題にあったら、それが実際的な問題であることを否定するのは難しい。これが'真面目な'サイトで働く人々が、通常はいかなるメモリもリークしないように試みる理由である。これが2つめのアプローチをもたらした：骨惜しみせず、DOMオブジェクトと正規のオブジェクトの間の循環が作られないように確実な仕事をするということだ。これは、例えば、上記のハンドラをこのように書き換えるという意味だ。：

.. code-block:: javascript

    function addAlerter(element) {
      addHandler(element, "click", function() {
        alert("Alert! ALERT!");
      });
      element = null;
    }

..  Now the element variable no longer points at the DOM node, and the handler will not leak. This approach is viable, but requires the programmer to really pay attention.

今element変数はDOMノードを指しておらず、ハンドラはメモリリークしない。このアプローチは実行可能だが、プログラマに本当に注意を払うことを要求する。

..  The third solution, finally, is to not worry too much about creating leaky structures, but to make sure to clean them up when you are done with them. This means unregistering any event handlers when they are no longer needed, and registering an onunload event to unregister the handlers that are needed until the page is unloaded. It is possible to extend an event-registering system, like our addHandler function, to automatically do this. When taking this approach, you must keep in mind that event handlers are not the only possible source of memory leaks ― adding properties to DOM node objects can cause similar problems.

3つめの解決法、最後に、リークするような構造を作ることをあまり気にしない、しかし、それができる時には確実に掃除する。これは、必要でなくなったらイベント・ハンドラの登録を外し、onunloadイベントに、ページがアンロードされたら必要でなくなったハンドラの登録を外す処理を登録するということだ。addHandler関数のように、イベント登録システムを拡張することは可能で、これを自動化する。このアプローチを取った時は、イベント・ハンドラだけがメモリ・リークを起こしうるソースであるとは考えないこと -- DOMノードオブジェクトにプロパティを追加すれば同じような問題が引き起こされるかもしれない。
