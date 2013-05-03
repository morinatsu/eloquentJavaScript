=====================================================
Webプログラミング: 短期集中コース
=====================================================
..  Chapter 11:
..  Web programming: A crash course

..  You are probably reading this in a web browser, so you are likely to be at least a little familiar with the World Wide Web. This chapter contains a quick, superficial introduction to the various elements that make the web work, and the way they relate to JavaScript. The three after this one are more practical, and show some of the ways JavaScript can be used to inspect and change a web-page.

あなたはおそらくこれをウェブブラウザで読んでいるだろうし、ワールドワイドウェブについてもちょっとは親しんでいるだろう。この章には、ウェブを動かすための様々な要素と、それらをJavaScriptに結びつける手段の、手早く、表面的な紹介が含まれる。この3章後ではもっと実用的な、JavaScriptでウェブページを点検し、変更するいくつかの手段を見せよう。

.. |hr| raw:: html

   <hr>

|hr|

..  The Internet is, basically, just a computer network spanning most of the world. Computer networks make it possible for computers to send each other messages. The techniques that underlie networking are an interesting subject, but not the subject of this book. All you have to know is that, typically, one computer, which we will call the server, is waiting for other computers to start talking to it. Once another computer, the client, opens communications with this server, they will exchange whatever it is that needs to be exchanged using some specific language, a protocol.

インターネットは、基本的には、ただ世界中に広がったコンピューター・ネットワークである。コンピューター・ネットワークはコンピューター同士でのメッセージのやりとりを可能にする。ネットワークの下に横たわるテクニックは面白い課題だが、この本の課題では無い。知らなければならないのは、典型的な、サーバーと呼ばれる1台のコンピュータが他のコンピューターから話しかけられるのを待っているということだ。1度他のコンピューター、クライアント、がこのサーバーとコミュニケーションを開いたら、特定の言葉、プロトコルを使って交換すべきものを交換するだろう。

..  The Internet is used to carry messages for many different protocols. There are protocols for chatting, protocols for file sharing, protocols used by malicious software to control the computer of the poor schmuck who installed it, and so on. The protocol that is of interest to us is that used by the World Wide Web. It is called HTTP, which stands for Hyper Text Transfer Protocol, and is used to retrieve web-pages and the files associated with them.

インターネットは、\ *多く*\ の異なるプロトコルのメッセージを運ぶために使われる。チャットのためのプロトコル、ファイル共有のためのプロトコル、悪意をもったソフトウェアが、それをインストールされた間抜けなコンピュータを制御するために使われるプロトコル、その他。もっとも我々の興味を引くプロトコルはワールドワイドウェブに使われるものだ。それはHTTPと呼ばれ、Hyper Text Transfer Protocolを意味し、ウェブページとそれらに結びついたファイルを取り出すのに使われる。

..  In HTTP communication, the server is the computer on which the web-page is stored. The client is the computer, such as yours, which asks the server for a page, so that it can display it. Asking for a page like this is called an 'HTTP request'.

HTTPのコミュニケーションにおいて、サーバーはウェブページが格納されたコンピューターである。クライアントは、あなたのもののように、サーバーにページを要求し、表示するコンピューターだ。このようなページの要求は'HTTPリクエスト'と呼ばれる。

|hr|

..  Web-pages and other files that are accessible though the Internet are identified by URLs, which is an abbreviation of Universal Resource Locators. A URL looks like this:

インターネットを通じてアクセスできるウェブページと他のファイルはURLによって識別され、これはUniveral Resource Locatorsの略である。URLはこのようなものだ。：
::

    http://acc6.its.brooklyn.cuny.edu/~phalsall/texts/taote-v3.html

..  It is composed of three parts. The start, http://, indicates that this URL uses the HTTP protocol. There are some other protocols, such as FTP (File Transfer Protocol), which also make use of URLs. The next part, acc6.its.brooklyn.cuny.edu, names the server on which this page can be found. The end of the URL, /~phalsal/texts/taote-v3.html, names a specific file on this server.

これは3つの部分により作られている。最初は、\ ``http://``\ 、このURLがHTTPプロトコルを使うことを示す。他にもFTP(File Transfer Protocol)といったようなプロトコルがあり、それらもURLを使う。次の部分、\ ``acc6.its.brooklyn.cuny.edu``\ はこのページが見つかるであろうサーバーの名前だ。URLの最後の、\ ``/~phalsal/texts/taote-v3.html``\ はこのサーバー上の個別のファイルの名前だ。

..  Most of the time, the World Wide Web is accessed using a browser. After typing a URL or clicking a link, the browser makes the appropriate HTTP request to the appropriate server. If all goes well, the server responds by sending a file back to the browser, who shows it to the user in one way or another.

長い間、ワールドワイドウェブはブラウザーを使ってアクセスされてきた。URLをタイプするかリンクをクリックされたら、ブラウザは妥当なHTTPリクエストを妥当なサーバーに送る。もし全てがうまくいったら、サーバーはブラウザにファイルを送り返すことで応答し、ブラウザは1つまたは他の手段でユーザーにそれを見せる。

..  When, as in the example, the retrieved file is an HTML document, it will be displayed as a web-page. We briefly discussed HTML in chapter 6, where we saw that it could refer to image files. In chapter 9, we found that HTML pages can also contain <script> tags to load files of JavaScript code. When showing an HTML document, a browser will fetch all these extra files from their servers, so it can add them to the document.

この例の場合、取り出されたファイルはHTMLドキュメントであり、ウェブページとして表示される。\ :doc:`6章</Functional Programming>`\ でHTMLについて少々論じ、イメージファイルを参照できることを知った。\ :doc:`9章</Modularity>`\ ではHTMLページは、JavaScriptコードのファイルをロードするために\ ``<script>``\ タグを含むことを知った。HTML文書を見せるとき、ブラウザはサーバーからこれらの拡張ファイルを取り出し、文書に追加することができる。

|hr|

..  Although a URL is supposed to point at a file, it is possible for a web-server to do something more complicated than just looking up a file and sending it to the client. ― It can process this file in some way first, or maybe there is no file at all, but only a program that, given an URL, has some way of generating the relevant document for it.

例えURLが1つのファイルを指していることになっていても、ウェブサーバーにはファイルを見つけてそれをクライアントに送るより複雑なことをすることが可能である。　― 1つには、このファイルを何らかの手段で処理できるし、あるいは結局ファイルは無くて、URLを与えられ、それに関する文書を何らかの方法で生成するプログラムだけがあるのかもしれない。

..  Programs that transform or generate documents on a server are a popular way to make web-pages less static. When a file is just a file, it is always the same, but when there is a program that builds it every time it is requested, it could be made to look different for each person, based on things like whether this person has logged in or specified certain preferences. This can also make managing the content of web-pages much easier ― instead of adding a new HTML file whenever something new is put on a website, a new document is added to some central storage, and the program knows where to find it and how to show it to clients.

サーバー上の文書を変形または生成するプログラムはウェブページを静的でないものにする人気のある手段だ。ファイルがただのファイルであればそれは常に同じものであるが、リクエストの度にそれを毎回組み立てるプログラムがあれば、例えば、その人がログインしているかどうか、または決まった設定を指定しているかどうかといったことで、それぞれの人に異なる見栄えを作ることができる。これでウェブページの内容の管理が簡単になる ― ウェブサイトに新しいものを追加する度に新しいHTMLファイルを追加するよりも。新しい文書は中央の記憶装置に追加され、プログラムはそれがどこに見つかるか、クライアントにどう見せるか知っているのだ。

..  This kind of web programming is called server-side programming. It affects the document before it is sent to the user. In some cases, it is also practical to have a program that runs after the page has been sent, when the user is looking at it. This is called client-side programming, because the program runs on the client computer. Client-side web programming is what JavaScript was invented for.

この種類のウェブプログラミングはサーバーサイドプログラミングと呼ばれる。これは文書がユーザーに送られる前に作用する。ある場合には、ページが送られた\ *後*\ 、ユーザーがそれを見る前にプログラムを動かした方が実用的である。クライアントコンピューター上でプログラムが動くため、こちらはクライアントサイドプログラミングと呼ばれる。クライアントサイドウェブプログラミングのためにJavaScriptは発明された。

|hr|

..  Running programs client-side has an inherent problem. You can never really know in advance what kinds of programs the page you are visiting is going to run. If it can send information from your computer to others, damage something, or infiltrate your system, surfing the web would be a rather hazardous activity.

クライアントサイドでプログラムを動かすには固有の問題がある。訪れているページの上でどんな種類のプログラムが動くかを本当に前もって知ることはできないだろう。それはあなたのコンピュータの情報を他に送るかもしれず、何かにダメージを与えるかもしれず、システムに侵入するかもしれず、ウェブをサーフィンすることはむしろ危険な行動になった。

..  To solve this dilemma, browsers severely limit the things a JavaScript program may do. It is not allowed to look at your files, or to modify anything not related to the web-page it came with. Isolating a programming environment like this is called sand-boxing. Allowing the programs enough room to be useful, and at the same time restricting them enough to prevent them from doing harm is not an easy thing to do. Every few months, some JavaScript programmer comes up with a new way to circumvent the limitations and do something harmful or privacy-invading. The people responsible for the browsers respond by modifying their programs to make this trick impossible, and all is well again ― until the next problem is discovered.

このジレンマを解決するため、ブラウザはJavaScriptが行えることを厳しく制限している。あなたのファイルを読んだり、そのウェブページに関係しない何物をも変更することはできない。このようにプログラミング環境を分離することはサンドボクシングと呼ばれる。プログラムが有益なことをするのに必要な場所を与え、かつ同時にそれらが何かを傷つけることを防ぐのは簡単なことでは無い。数ヶ月毎に、JavaScriptプログラマーは制限を回避する新しい方法を発見して何かを傷つけたりプライバシーを侵害したりする。ブラウザに責任を負っている人々は、このトリックを不可能にするため、それらのプログラムを変更し、すべては再び良くなる ― 次の問題が発見されるまでは。

|hr|

..  One of the first JavaScript tricks that became widely used is the open method of the window object. It takes a URL as an argument, and will open a new window showing that URL.

最初に広く使われるようになったJavaScriptのトリックの1つは\ ``window``\ オブジェクトの\ ``open``\ メソッドだ。URLを引数として取り、新しいウインドウを開いてそのURLを見せる。

.. code-block:: javascript

    var perry = window.open("http://www.pbfcomics.com");

..  Unless you turned off pop-up blocking in chapter 6, there's a chance that this new window is blocked. There is a good reason pop-up blockers exist. Web-programmers, especially those trying to get people to pay attention to advertisements, have abused the poor window.open method so much that by now, most users hate it with a passion. It has its place though, and in this book we will be using it to show some example pages. As a general rule, your scripts should not open any new windows unless the user asked for them.

:doc:`6章</Functional Programming>`\ でポップアップブロックをオフにしていれば、この新しいウインドウがブロックされるチャンスがある。ポップアップブロッカーが存在すべき理由があるのだ。ウェブプログラマー、特に、広告に人々の注意を払わせようとする者達は、今のような貧弱な\ ``window.open``\ メソッドを悪用し、多くの人々に激怒される。とは言いつつ、この本でも例のページを見せるのにそれを使っている。一般的なルールとしては、スクリプトはユーザーに確認することなしに新しいウインドウを開くべきではないということだ。

..  Note that, because open (just like setTimeout and company) is a method on the window object, the window. part can be left off. When a function is called 'normally', it is called as a method on the top-level object, which is what window is. Personally, I think open sounds a bit generic, so I'll usually type window.open, which makes it clear that it is a window that is being opened.

注意して欲しいのは、\ ``open``\ （\ ``setTimeout``\ とその仲間）は\ ``window``\ オブジェクトのメソッドであるため、\ ``window.``\ の部分は切り離せないということだ。関数が'通常通り'に呼び出された場合、トップレベルのメソッドとして呼び出され、ウィンドウもそうなる。個人的には、\ ``open``\ は少々一般的すぎるように聞こえ、普通に\ ``window.open``\ とタイプするのは、それは開かれたウインドウであるとした方が明確になったと思う。

..  The value returned by window.open is a new window. This is the global object for the script running in that window, and contains all the standard things like the Object constructor and the Math object. But if you try to look at them, most browsers will (probably) not let you...

``window.open``\ の戻り値は新しいウインドウである。これはそのウインドウでスクリプトが走っている間のグローバルなオブジェクトであり、\ ``Object``\ のコンストラクタや\ ``Math``\ オブジェクトのような、全ての標準的なものを含んでいる。しかしそれらを見ようとしても、多くのブラウザは（おそらく）そうさせないだろう...。

.. code-block:: javascript

    show(perry.Math);

..  This is part of the sand-boxing that I mentioned earlier. Pages opened by your browser might show information that is meant only for you, for example on sites where you logged in, and thus it would be bad if any random script could go and read them. The exception to this rule is pages opened on the same domain: When a script running on a page from eloquentjavascript.net opens another page on that same domain, it can do everything it wants to this page.

これは先ほど述べたサンドボクシングの一部である。ブラウザで開かれたページは、例えばログインしたサイトのような、あなたにだけ意味のある情報を見せ、もし無作為のスクリプトが実行されてそれを読んだりできたとしたらそれはまずいことだ。このルールの例外は、同じドメインで開かれたページだ：\ ``eloquentjavascript.net``\ のページでスクリプトが実行されているとき、開かれた同じドメインの他のページも、このページに望む全てのことができる。

..  An opened window can be closed with its close method. If you didn't already close it yourself...

開かれたウインドウは\ ``close``\ メソッドで閉じることができる。もしあなたが自分で既に閉じていなければ...

.. code-block:: javascript

    perry.close();

..  Other kinds of sub-documents, such as frames (documents-within-a-document), are also windows from the perspective of a JavaScript program, and have their own JavaScript environment. In fact, the environment that you have access to in the console belongs to a small invisible frame hidden somewhere on this page ― this way, it is slightly harder for you to accidentally mess up the whole page.

他のサブ文書としては、フレーム（文書の中の文書）のようなものがあり、これもJavaScriptプログラムからはウインドウのように見え、それ自身のJavaScript環境を持つ。実際、コンソールとしてアクセスしている環境はこのページの小さな隠されたフレームとして存在し ― この方法で、ページ全体を偶然に壊すことはやや難しい。

|hr|

..  Every window object has a document property, which contains an object representing the document shown in that window. This object contains, for example, a property location, with information about the URL of the document.

全ての\ ``window``\ オブジェクトは\ ``document``\ プロパティを持ち、それは、そのウインドウに表示されている文書を表現するオブジェクトを含んでいる。このオブジェクトは、例えば、\ ``location``\ プロパティ、文書のURLに関する情報を含んでいる。

.. code-block:: javascript

    show(document.location.href);

..  Setting document.location.href to a new URL can be used to make the browser load another document. Another application of the document object is its write method. This method, when given a string argument, writes some HTML to the document. When it is used on a fully loaded document, it will replace the whole document by the given HTML, which is usually not what you want. The idea is to have a script call it while the document is being loaded, in which case the written HTML will be inserted into the document at the place of the script tag that triggered it. This is a simple way to add some dynamic elements to a page. For example, here is a trivially simple document showing the current time.

``document.location.href``\ に新しいURLをセットするとブラウザに他の文書がロードされる。\ ``document``\ オブジェクトの他の利用としてはその\ ``write``\ メソッドがある。このメソッドは、文字列の引数を与えられたとき、文書にHTMLとして書き出す。完全にロードされた文書に対して使われたとき、それを望んでいなくても、通常は与えられたHTMLで文書全体が置き換えられる。このアイデアは、文書がロードされている間にスクリプトの呼び出しを行い、そのスクリプトにより書き出されたHTMLを文書の\ ``script``\ タグのあった場所に挿入するためにある。これはページに動的に要素を追加する単純な手段である。例えば、現在の時刻を表示するちょっとした文書をここに示す。

.. code-block:: javascript

    print(timeWriter);
    var time = viewHTML(timeWriter);
    time.close();

..  Often, the techniques shown in chapter 12 provide a cleaner and more versatile way to modify the document, but occasionally, document.write is the nicest, simplest way to do something.

:doc:`12章</The Document-Object Model>`\ では、より明瞭で融通の利く、文書を更新するためのテクニックを見せるが、しかし、しばしば、\ ``document.write``\ は最も良く、最も単純な手段である。

|hr|

..  Another popular application of JavaScript in web pages centers around forms. In case you are not quite sure what the role of 'forms' is, let me give a quick summary.

ウェブでのJavaScriptの利用としてもう一つ人気の高いのはフォームの処理だ。この場合の'フォーム'の役割を完全には分ってないだろうから、ここで手っ取り早く要約しよう。

..  A basic HTTP request is a simple request for a file. When this file is not really a passive file, but a server-side program, it can become useful to include information other than a filename in the request. For this purpose, HTTP requests are allowed to contain additional 'parameters'. Here is an example:

基本的なHTTPリクエストはファイルの単純なリクエストだ。このファイルが本当は受動的なファイルではなく、サーバー側のプログラムであるとき、リクエストでファイル名よりも有益な情報を含めるようにすることができる。この目的のため、HTTPリクエストには'パラメータ'を追加することができるようになっている。例を示そう。：
::

    http://www.google.com/search?q=aztec%20empire

..  After the filename (/search), the URL continues with a question mark, after which the parameters follow. This request has one parameter, called q (for 'query', presumably), whose value is aztec empire. The %20 part corresponds to a space. There are a number of characters that can not occur in these values, such as spaces, ampersands, or question marks. These are 'escaped' by replacing them with a % followed by their numerical value1, which serves the same purpose as the backslashes used in strings and regular expressions, but is even more unreadable.

ファイル名（\ ``/search``\ ）の後に、URLは疑問符で続けられており、その後にパラメータが続いている。このリクエストはパラメータは1つ持っており、\ ``q``\ （たぶん'query'だろう）と呼ばれていて、その値は\ ``aztec empire``\ である。\ ``%20``\ の部分は空白に一致する。空白、アンパサンド、疑問符のような、URL中にあってはならない値の文字コードである。これらは\ ``%``\ とその文字コードの値\ [#f1]_\ で置換されることによりエスケープされ、それはサーバが文字列や正規表現の中で、読みにくくなるにもかかわらず、バックスラッシュを使うのと同じ目的である。

..  JavaScript provides functions encodeURIComponent and decodeURIComponent to add these codes to strings and remove them again.

JavaScriptはこれらのコードを文字列に追加し、あるいは取り除くために\ ``encodeURIComponent``\ と\ ``decodeURIComponent``\ 関数を提供している。

.. code-block:: javascript

    var encoded = encodeURIComponent("aztec empire");
    show(encoded);
    show(decodeURIComponent(encoded));

..  When a request contains more than one parameter, they are separated by ampersands, as in...

リクエストに1つより多いパラメータが含まれるとき、それらはアンパサンドで分割される、このように...
::

    http://www.google.com/search?q=aztec%20empire&lang=nl

|hr|

..  A form, basically, is a way to make it easy for browser-users to create such parameterised URLs. It contains a number of fields, such as input boxes for text, checkboxes that can be 'checked' and 'unchecked', or thingies that allow you to choose from a given set of values. It also usually contains a 'submit' button and, invisible to the user, an 'action' URL to which it should be sent. When the submit button is clicked, or enter is pressed, the information that was entered in the fields is added to this action URL as parameters, and the browser will request this URL.

フォームは、基本的には、ブラウザのユーザーにこのようなパラメータ付きのURLを作らせる簡単な手段だ。テキストのためのインプットボックスや、チェックされたか否かのチェックボックス、値のセットから選択するためのアレなど、いくつものフィールドを含む。通常は、'サブミット'ボタンと、ユーザーには見えない、情報を送る'アクション'のURLも含まれている。サブミットボタンがクリックされるか、エンターが押されるかしたら、フィールドに入力された情報は、このアクションのURLにパラメータとして追加され、ブラウザはこのURLをリクエストする。

..  Here is the HTML for a simple form:

ここに単純なフォームを含むHTMLがある。：

.. code-block:: html

    <form name="userinfo" method="get" action="info.html">
      <p>Please give us your information, so that we can send
      you spam.</p>
      <p>Name: <input type="text" name="name"/></p>
      <p>E-Mail: <input type="text" name="email"/></p>
      <p>Sex: <select name="sex">
                <option>Male</option>
                <option>Female</option>
                <option>Other</option>
              </select></p>
      <p><input name="send" type="submit" value="Send!"/></p>
    </form>

..  The name of the form can be used to access it with JavaScript, as we shall see in a moment. The names of the fields determine the names of the HTTP parameters that are used to store their values. Sending this form might produce a URL like this:

見て分るとおり、フォームの名前でJavaScriptからアクセスすることができる。フィールドの名前でそれらを格納するHTTPパラメータの名前が決まる。このフォームを送ることで、このようなURLが作られる。：
::

    http://planetspam.com/info.html?name=Ted&email=ted@zork.com&sex=Male

..  There are quite a few other tags and properties that can be used in forms, but in this book we will stick with simple ones, so that we can concentrate on JavaScript.

他にもいくつかのタグやプロパティをフォームで使えるが、JavaScriptに集中できるよう、この本では単純なものに留めておこう。

|hr|

..  The method="get" property of the example form shown above indicates that this form should encode the values it is given as URL parameters, as shown before. There is an alternative method for sending parameters, which is called post. An HTTP request using the post method contains, in addition to a URL, a block of data. A form using the post method puts the values of its parameters in this data block instead of in the URL.

上記の例のフォームの\ ``method="get"``\ プロパティはこのフォームが値を上で見たようなURLのパラメータにエンコードすることを示す。\ ``post``\ という、パラメータを送る代わりのメソッドもある。\ ``post``\ メソッドを含むHTTPリクエストは、URLに加えて、データのブロックを含む。\ ``post``\ メソッドを使うフォームはURLの代わりにこのデータブロックにパラメータの値を入れる。

..  When sending big chunks of data, the get method will result in URLs that are a mile wide, so post is usually more convenient. But the difference between the two methods is not just a question of convenience. Traditionally, get requests are used for requests that just ask the server for some document, while post requests are used to take an action that changes something on the server. For example, getting a list of recent messages on an Internet forum would be a get request, while adding a new message would be a post request. There is a good reason why most pages follow this distinction ― programs that automatically explore the web, such as those used by search engines, will generally only make get requests. If changes to a site can be made by get requests, these well-meaning 'crawlers' could do all kinds of damage.

``get``\ メソッドで結果のURLが長くなりすぎるような大きなデータの塊を送るときに、\ ``post``\ は通常、より便利である。しかしこの2つのメソッドの違いはただ利便性の問題のみではない。伝統的に、\ ``get``\ リクエストはサーバから文書を求めるためのリクエストであり、\ ``post``\ はサーバー上で何かを変更するためのアクションを起こすリクエストである。例えば、インターネットのフォーラムの過去のメッセージのをリストを得るのは\ ``get``\ リクエストで、新しいメッセージを追加するのは\ ``post``\ リクエストである。この区別に紙面を割く理由がある ― サーチエンジンで使われるような、ウェブを自動的に探索するプログラムは、一般的には\ ``get``\ リクエストのみを使うだろう。もし\ ``get``\ リクエストでサイトを更新できたら、'クロウラー'はあらゆる種類のダメージを与えることを行うかもしれない。

|hr|

..  When the browser is displaying a page containing a form, JavaScript programs can inspect and modify the values that are entered in the form's fields. This opens up possibilities for all kinds of tricks, such as checking values before they are sent to the server, or automatically filling in certain fields.

ブラウザがフォームを含むページを表示しているとき、JavaScriptはフォームのフィールドに入力された値を監視したり変更したりできる。これが、サーバーに送られる前の値をチェックしたり、決まったフィールドを自動的に埋めたりといった、あらゆる種類のトリックを可能にする。

..  The form shown above can be found in the file example_getinfo.html. Open it.

上記で見たフォームを\ ``example_getinfo.html``\ というファイルに入れた。開いてみよう。

.. code-block:: javascript

    var form = window.open("example_getinfo.html");

..  When a URL does not contain a server name, is called a relative URL. Relative URLs are interpreted by the browser to refer to files on the same server as the current document. Unless they start with a slash, the path (or directory) of the current document is also retained, and the given path is appended to it.

サーバー名が含まれないURLは、相対URLと呼ばれる。相対URLはブラウザによって、現在の文書と同じサーバーのファイルを参照しているものと解釈される。スラッシュで始まらない限り、現在の文書のパス（またはディレクトリ）も引き継がれ、与えられたパスがそれに追加される。

..  We will be adding a validity check to the form, so that it only submits if the name field is not left empty and the e-mail field contains something that looks like a valid e-mail address. Because we no longer want the form to submit immediately when the 'Send!' button is pressed, its type property has been changed from "submit" to "button", which turns it into a regular button with no effect. ― Chapter 13 will show a much better way of doing this, but for now, we use the naive method.

nameフィールドに空のものが残っておらず、e-mailフィールドが正しい電子メールアドレスのようなものを含むようになったときのみサブミットできるように、フォームに正当性チェックを追加しよう。明示的に'Send!'ボタンを押すまでフォームをサブミットされたくないから、そのtypeプロパティを\ ``"submit"``\ から何の効果も無いただのボタンである\ ``"button"``\ に変更しておこう。 ― \ :doc:`13章</Browser Events>`\ でこれを行うもっと良い方法を見せるが、今は、ネイティブなメソッドを使おう。

|hr|

..  To be able to work with the newly opened window (if you closed it, re-open it first), we 'attach' the console to it, like this:

新しく開いたウィンドウ（もし閉じてしまっていたら、まずそれを再度開こう）を働かせるには、このようにコンソールにそれを'attach'させよう。：

.. code-block:: javascript

    attach(form);

..  After doing this, the code run from the console will be run in the given window. To verify that we are indeed working with the correct window, we can look at the document's location and title properties.

このようにした後は、コンソールで実行されたコードは与えられたウインドウで走る。正しいウインドウで動いていることは、文書の\ ``location``\ と\ ``title``\ プロパティで確認できる。

.. code-block:: javascript

    print(document.location.href);
    print(document.title);

..  Because we have entered a new environment, previously defined variables, such as form, are no longer present.

新しい環境に入ったため、\ ``form``\ のような、今まで定義した変数はもう存在していない。

.. code-block:: javascript

    show(form);

..  To get back to our starting environment, we can use the detach function (without arguments). But first, we have to add that validation system to the form.

最初の環境に戻るには、\ ``detach``\ 関数（引数無し）を使う。しかし、まずは、フォームに正当性システムを追加しなければならない。

|hr|

..  Every HTML tag shown in a document has a JavaScript object associated with it. These objects can be used to inspect and manipulate almost every aspect of the document. In this chapter, we will work with the objects for forms and form fields, chapter 12 talks about these objects in more detail.

文書の中の全てのHTMLタグはJavaScriptオブジェクトと関連づけられる。これらのオブジェクトは文書のほとんど全ての外観を監視し操作するのに使える。この章ではフォームとフォームフィールドのオブジェクトを扱うが、\ :doc:`12章</The Document-Object Model>`\ ではこれらのオブジェクトの詳細について語る。

..  The document object has a property named forms, which contains links to all the forms in the document, by name. Our form has a property name="userinfo", so it can be found under the property userinfo.

``document``\ オブジェクトは\ ``forms``\ という名のプロパティを持ち、文書の全てのフォームへの名前によるリンクを含む。我々のフォームは\ ``name="userinfo"``\ というプロパティを持っているので、\ ``userinfo``\ プロパティの下に見つかる。

.. code-block:: javascript

    var userForm = document.forms.userinfo;
    print(userForm.method);
    print(userForm.action);

..  In this case, the properties method and action that were given to the HTML form tag are also present as properties of the JavaScript object. This is often the case, but not always: Some HTML properties are spelled differently in JavaScript, others are not present at all. Chapter 12 will show a way to get at all properties.

この場合、HTMLの\ ``from``\ タグで与えられる\ ``mthod``\ と\ ``action``\ プロパティはJavaScriptオブジェクトのプロパティとしても表現される。このような場合はしばしばあるが、常にでは無い：いくつかのHTMLプロパティはJavaScriptでは違うスペルであり、他は全く存在していない。\ :doc:`12章</The Document-Object Model>`\ で全てのプロパティを得る手段を見せよう。

..  The object for the form tag has a property elements, which refers to an object containing the fields of the form, by name.

``form``\ タグのオブジェクトは\ ``elements``\ プロパティを持ち、それはformのフィールドに含むオブジェクトを名前で参照する。

.. code-block:: javascript

    var nameField = userForm.elements.name;
    nameField.value = "Eugène";

..  Text-input objects have a value property, which can be used to read and change their content. If you look at the form window after running the above code, you'll see that the name has been filled in.

テキストインプットオブジェクトは\ ``value``\ プロパティを持ち、それはその内容を読んだり変更したりするのに使われる。上記のコードの実行後、フォームウインドウを見れば、埋められた名前を見ることができるだろう。

|hr|

演習 11.1

..  Being able to read the values of the form fields makes it possible to write a function validInfo, which takes a form object as its argument and returns a boolean value: true when the name field is not empty and the email field contains something that looks like an e-mail address, false otherwise. Write this function.

フォームフィールドの値を読めるようになれば、\ ``validInfo``\ 関数を書くことができるようになる。引数としてフォームオブジェクトを取り、真偽値の値を返す：\ ``name``\ フィールドに空のものが無く、\ ``email``\ フィールドに電子メールアドレスのようなものが含まれていれば\ ``true``\ 、そうでなければ\ ``false``\ となる。この関数を書け。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    function validInfo(form) {
      return form.elements.name.value != "" &&
        /^.+@.+\.\w{2,3}$/.test(form.elements.email.value);
    }

    show(validInfo(document.forms.userinfo));

..  You did think to use a regular expression for the e-mail check, didn't you?

電子メールアドレスのチェックには正規表現を使うことを考えたよね？

|hr|

..  All we have to do now is determine what happens when people click the 'Send!' button. At the moment, it does not do anything at all. This can be remedied by setting its onclick property.

やるべきことのうち、残っているのは、人が'Send!'ボタンをクリックしたときの挙動だ。あっという間で、何もすることは無い。\ ``onclick``\ プロパティを設定するだけだ。

.. code-block:: javascript

    userForm.elements.send.onclick = function() {
      alert("Click.");
    };

..  Just like the actions given to setInterval and setTimeout (chapter 8), the value stored in an onclick (or similar) property can be either a function or a string of JavaScript code. In this case, we give it a function that opens an alert window. Try clicking it.

ちょうど\ ``setInterval``\ や\ ``setTimeout``\ に与えられたアクション（\ :doc:`8章</Object-oriented Programming>`\ ）のように、\ ``onclick``\ （または同様の）プロパティに、関数やJavaScriptコードの文字列を値として格納できる。この場合、警告ウインドウを開く関数を与える。クリックしてみよう。

|hr|

演習 11.2

..  Finish the form validator by giving the button's onclick property a new value ― a function that checks the form, submits when it is valid, or pops up a warning message when it is not. It will be useful to know that form objects have a submit method that takes no parameters and submits the form.

ボタンの\ ``onclick``\ プロパティに新しい値 ― フォームをチェックする関数を与えてフォームのバリデーターを完成させよ。正しければサブミットし、またはそうでなければ警告のメッセージをポップアップする。formオブジェクトが、パラメータを持たず、フォームをサブミットする、\ ``submit``\ メソッドを持っていることを知っていると役立つだろう。

..  [show solution]

[解答を見る]

.. code-block:: javascript

    userForm.elements.send.onclick = function() {
      if (validInfo(userForm))
        userForm.submit();
      else
        alert("Give us a name and a valid e-mail address!");
    };

|hr|

..  Another trick related to form inputs, as well as other things that can be 'selected', such as buttons and links, is the focus method. When you know for sure that a user will want to start typing in a certain text field as soon as he enters the page, you can have your script start by placing the cursor in it, so he won't have to click it or select it in some other way.

フォームのインプットだけでなく、ボタンやリンクのような'選択可能なもの'に関するもう一つのトリックとして、\ ``focus``\ メソッドがある。ユーザーがページに入ったとき、決まったフィールドからタイプを始めたいだろうことが確実に分っているとき、彼にクリックや他の手段で選択させるのではなく、スクリプトでカーソルをそこに移動させることができる。

.. code-block:: javascript

    userForm.elements.name.focus();

..  Because the form sits in another window, it may not be obvious that something was selected, depending on the browser you are using. Some pages also automatically make the cursor jump to the next field when it looks like you finished filling in one field ― for example, when you type a zip code. This should not be overdone ― it makes the page behave in a way the user does not expect. If he is used to pressing tab to move the cursor manually, or mistyped the last character and wants to remove it, such magic cursor-jumping is very annoying.

フォームが他のウインドウにあるため、何かが選択されたかは明らかでなく、使われているブラウザに依存する。あるページはそのフィールドが完全に埋まったように見えるとき、自動的にカーソルを次のフィールドに移動させる ― 例えば、郵便番号をタイプするときだ。これはやり過ぎてはいけない ― これはページをユーザーの期待しないように振る舞わせる。もし手動でカーソルを移動しようとタブを押したり、最後の文字をミスタイプして削除したいと思っていたとしたら、このような手品めいたカーソル移動はとても邪魔だ。

|hr|

.. code-block:: javascript

    detach();

..  Test the validator. When you enter valid information and click the button, the form should submit. If the console was still attached to it, this will cause it to detach itself, because the page reloads and the JavaScript environment is replaced by a new one.

バリデーターをテストしよう。正しい情報を入力してボタンをクリックしたとき、フォームはサブミットされるだろう。もしコンソールがまだ接続されていたら、ページがリロードされてJavaScript環境が新しいものに置き換わるため、デタッチされる。

..  If you haven't closed the form window yet, this will close it.

もしフォームウインドウをまだ閉じていなければ、これで閉じよう。

.. code-block:: javascript

    form.close();

|hr|

..  The above may look easy, but let me assure you, client-side web programming is no walk in the park. It can, at times, be a very painful ordeal. Why? Because programs that are supposed to run on the client computer generally have to work for all popular browsers. Each of these browsers tends to work slightly different. To make things worse, each of them contains a unique set of problems. Do not assume that a program is bug-free just because it was made by a multi-billion dollar company. So it is up to us, the web-programmer, to rigorously test our programs, figure out what goes wrong, and find ways to work around it.

上記は簡単なように見えるだろうが、私は保証しよう、クライアントサイド・ウェブ・プログラミングは公園の散歩ではないと。それは、時には、とても痛い体験となる。なぜか？なぜなら、クライアント・コンピュータ上で実行されることを想定されたプログラムは、一般的に、全ての有名なブラウザで動かねばならないからだ。これらのブラウザのそれぞれがちょっとずつ異なるように動く。悪いことには、これらそれぞれが固有の問題のセットを抱えている。プログラムが、数百万ドルの会社によって作られているからというだけで、バグと無縁であると想定しないこと。我々、ウェブ・プログラマーにとって、プログラムを厳格にテストすることが、間違いを防止することで、それを動かす道を見つけることだ。

..  Some of you might think "I will just report any problems/bugs I find to the browser manufacturers, and they will certainly solve fix them immediately". These people are in for a major disappointment. The most recent version of Internet Explorer, the browser that is still used by some seventy percent of web-surfers (and that every web-developer likes to rag on) still contains bugs that have been known for over five years. Serious bugs, too.

「私は見つけた問題やバグをブラウザの作成者にただ報告するだけにしよう、そうすれば直ちに彼らがそれを確かに解決してくれるだろう。」そう考える者もいるかもしれない。これらの人々は大きく落胆することになる。InternetExplorerの過去のバージョンの多くは、未だウェブ・サーファーの70%に使われている（そして全てのウェブ・ディベロッパーからけなされがちな）ブラウザであるが、5年以上前から知られているバグが未だに残っている。重大なバグさえも。

..  But do not let that discourage you. With the right kind of obsessive-compulsive mindset, such problems provide wonderful challenges. And for those of us who do not like wasting our time, being careful and avoiding the obscure corners of the browser's functionality will generally prevent you from running into too much trouble.

しかしガッカリしないように。極端に強迫的な種類の考え方の持ち主にとっては、このような問題はすばらしいチャレンジの提供である。そして時間の浪費を好まぬ我々のために、注意深く、そしてブラウザの機能の暗い隅を回避して、一般的にあなたが多すぎるトラブルの中に飛び込まないよう守ってくれる。

|hr|

..  Bugs aside, the by-design differences in interface between browsers still make for an interesting challenge. The current situation looks something like this: On the one hand, there are all the 'small' browsers: Firefox, Safari, and Opera are the most important ones, but there are more. These browsers all make a reasonable effort to adhere to a set of standards that have been developed, or are being developed, by the W3C, an organisation that tries to make the Web a less confusing place by defining standard interfaces for things like this. On the other hand, there is Internet Explorer, Microsoft's browser, which rose to dominance in a time when many of these standards did not really exist yet, and hasn't made much effort to adjust itself to what other people are doing.

バグはおいても、ブラウザ間の、設計によるインターフェースの違いは面白いチャレンジを作る。現在の状況はこのように見える：一方で、全ての'小さい'ブラウザがある：Firefox、Safari、Operaが最も重要なものだが、他にももっとある。これらのブラウザは全て、このようなものの標準インターフェースを定義することにより、ウェブの混乱をより少なくしようとしている組織であるW3Cにより、開発された、または開発中の標準に忠実であるために合理的に努力している。もう一方で、MicrosoftのブラウザであるInternetExplorerがあり、これらの標準がまだ存在しなかったころの、一時は優位に立っていて、他の人々が従っている標準に合わせるための努力を行っていない。

..  In some areas, such as the way the content of an HTML document can be approached from JavaScript (chapter 12), the standards are based on the method that Internet Explorer invented, and things work more or less the same on all browsers. In other areas, such as the way events (mouse-clicks, key-presses, and such) are handled (chapter 13), Internet Explorer works radically different from other browsers.

ある領域、HTML文書の内容をJavaScriptからアプローチできるようにする方法（\ :doc:`12章</The Document-Object Model>`\ ）のようなところでは、標準はInternetExplorerが発明した方法をもとにしていて、全てのブラウザで多かれ少なかれ同じように動く。他の領域では、イベント（マウスクリック、キー押下など）のハンドリング（\ :doc:`13章</Browser Events>`\ ）のようなところでは、InternetExplorerは他のブラウザと激しく異なる。

..  For a long time, owing partially to the cluelessness of the average JavaScript developer, and partially to the fact that browser incompatibilities were much worse when browsers like Internet Explorer version 4 or 5 and old versions of Netscape were still common, the usual way to deal with such differences was to detect which browser the user was running, and litter code with alternate solutions for each browser ― if this is Internet Explorer, do this, if this is Netscape, do that, and if this is other browser that we didn't think of, just hope for the best. You can imagine how hideous, confusing, and long such programs were.

長い間、部分的には、平均的なJavaScript開発者の無知により、そして部分的にはブラウザの非互換性という事実により、InternetExplorerのバージョン4、5と古いバージョンのNetscapeのようなブラウザがまだ一般的であった時はもっと酷く、ユーザーが動かしているブラウザを判定して、そのような相違点を扱うのが普通のやり方だった。そして、それぞれのブラウザ向けに代替の解決を行うためにコードが散乱した ― もしInternetExplorerならこうする、Netscapeならああする、考慮していない、他のブラウザであれば、それがベストであることを望むだけ。いかに恐ろしく、混乱した、そして長いプログラムであるかが想像できるだろう。

..  Many sites would also just refuse to load when opened in a browser that was 'not supported'. This caused a few of the minor browsers to swallow their pride and pretend they were Internet Explorer, just so they would be allowed to load such pages. The properties of the navigator object contain information about the browser that a page was loaded in, but because of such lying this information is not particularly reliable. See what yours says2:

多くのサイトもまた、ブラウザで開かれたときに'サポートされてません'と、ロードすることをただ拒まれる。これはいくつかのマイナーなブラウザに、そのプライドを飲み込み、ただそのようなページをロードできるようにするために、InternetExplorerであるかのように見せかけさせた。\ ``navigator``\ オブジェクトのプロパティは、ページを読み込んだブラウザについての情報を含むが、しかしこの情報にウソがあるため、とても信頼できるものではない。あなたのブラウザが表示するものを見てみよう。\ [#f2]_\ ：

.. code-block:: javascript

    forEachIn(navigator, function(name, value) {
      print(name, " = ", value);
    });

..  A better approach is to try and 'isolate' our programs from differences in browsers. If you need, for example, to find out more about an event, such as the clicks we handled by setting the onclick property of our send button, you have to look at the top-level object called event on Internet Explorer, but you have to use the first argument passed to the event-handling function on other browsers. To handle this, and a number of other differences related to events, one can write a helper function for attaching events to things, which takes care of all the plumbing and allows the event-handling functions themselves to be the same for all browsers. In chapter 13 we will write such a function.

より良いアプローチは試してみてることと、ブラウザの違いからプログラムを'隔離する'ことだ。もし必要なら、例えば、送信ボタンの\ ``onclick``\ プロパティを設定してクリックをハンドルするような、イベントについて、もっと探し出したいなら、\ ``event``\ というInternetExplorerのトップレベルのオブジェクトを見なければならない、しかし他のブラウザでは最初の引数はイベントハンドリングの関数の受け渡しに使わなければならない。これと、イベントに関する違いの数々をハンドリングするための、イベントと物を結びつける、全ての配管の面倒を見てイベントハンドリング関数それ自体を全てのブラウザで同じ物にするヘルパー関数を書けるだろう。\ :doc:`13章</Browser Events>`\ でそのような関数を書こう。

..  (Note: The browser quirks mentioned in the following chapters refer to the state of affairs in early 2007, and might no longer be accurate on some points.)

.. note::
   ブラウザのごまかしについての以降の章での言及は2007年初頭での状況に基づいていて、あるポイントについてはもう正しくないかもしれない。

|hr|

..  These chapters will only give a somewhat superficial introduction to the subject of browser interfaces. They are not the main subject of this book, and they are complex enough to fill a thick book on their own. When you understand the basics of these interfaces (and understand something about HTML), it is not too hard to look for specific information online. The interface documentation for the Firefox and Internet Explorer browsers are a good place to start.

これらの章はブラウザのインターフェースの課題についての表面的な紹介のようなものだけを与える。それらはこの本の主題ではなく、そしてこの薄い本自体に書くには複雑すぎる。これらのインターフェースの基本を理解したら（そしてHTMLについて理解したら）、オンラインの個々の情報を探すのは難しくないだろう。\ `Firefox <http://www.mozilla.org/docs/dom/domref/dom_shortTOC.html>`_\ と\ `InternetExplorer <http://msdn2.microsoft.com/library/yek4tbz0.aspx>`_\ のインターフェースの文書が手始めには良い。

..  The information in the next chapters will not deal with the quirks of 'previous-generation' browsers. They deal with Internet Explorer 6, Firefox 1.5, Opera 9, Safari 3, or any more recent versions of the same browsers. Most of it will also probably be relevant to modern but obscure browsers such as Konqueror, but this has not been extensively checked. Fortunately, these previous-generation browsers have pretty much died out, and are hardly used anymore.

次の章の情報は'前世代の'ブラウザのごまかしを扱わない。InternetExplorer 6、Firefox 1.5、Opera 9、Safari 3、またはそれらのブラウザのそれ以降のバージョンを扱う。その多くはおそらく現代的だがKonquerorのようなよく分らないブラウザもあり、これは広範囲にはチェックされていない。幸運にも、これらの前世代のブラウザはほとんど死滅していて、もう余り使われることはない。

..  There is, however, a group of web-users that will still use a browser without JavaScript. A large part of this group consists of people using a regular graphical browser, but with JavaScript disabled for security reasons. Then there are people using textual browsers, or browsers for blind people. When working on a 'serious' site, it is often a good idea to start with a plain HTML system that works, and then add non-essential tricks and conveniences with JavaScript.

しかしながら、JavaScriptのないウェブユーザーのグループがまだ存在する。このグループの大部分は正規のグラフィカルなブラウザを使うが、セキュリティ上の理由からJavaScriptをオフにしている人々だ。それからテキストのブラウザを使っている人々もいるし、目の見えない人々のためのブラウザもある。'真面目な'サイトで働くなら、プレーンなHTMLシステムで動くものから始めて、それからJavaScriptで本質的でないトリックと利便性を追加するのが良いアイデアだ。

.. The value a character gets is decided by the ASCII standard, which assigns the numbers 0 to 127 to a set of letters and symbols used by the Latin alphabet. This standard is a precursor of the Unicode standard mentioned in <a href="chapter2.html">chapter 2</a>.
.. Some browsers seem to hide the properties of the <code>navigator</code> object, in which case this will print nothing.
  
.. [#f1] 文字の値はASCII標準によって決まり、0から127までの数値にアルファベットの文字と記号の集合が割り当てられている。この標準は\ :doc:`2章</Basic JavaScript>`\ で触れたUnicode標準の先駆けである。
.. [#f2] あるブラウザは\ ``navigator``\ オブジェクトのプロパティを隠しているようで、この場合は何もプリントされない。

