=====================================================
HTTP requests
=====================================================
..  Chapter 14:
..  HTTP requests

..  As mentioned in chapter 11, communication on the World Wide Web happens over the HTTP protocol. A simple request might look like this:

11章で言ったように、ワールド・ワイド・ウェブでのコミュニケーションはHTTPプロトコルの上で起こる。単純なリクエストは以下のようになる。：
::

    GET /files/fruit.txt HTTP/1.1
    Host: eloquentjavascript.net
    User-Agent: The Imaginary Browser

..  Which asks for the file files/fruit.txt from the server at eloquentjavascript.net. In addition, it specifies that this request uses version 1.1 of the HTTP protocol ― version 1.0 is also still in use, and works slightly differently. The Host and User-Agent lines follow a pattern: They start with a word that identifies the information they contain, followed by a colon and the actual information. These are called 'headers'. The User-Agent header tells the server which browser (or other kind of program) is being used to make the request. Other kinds of headers are often sent along, for example to state the types of documents that the client can understand, or the language that it prefers.

ファイルfiles/fruit.txtがどこにあるかをeloquentjavascript.netのサーバーに尋ねる。加えて、このリクエストにHTTPプロトコルのバージョン1.1を使うことを指定している。-- バージョン1.0はまだ使われており、少々異なる働きをする。HostとUser-Agentの行はパターンに従う：それが含んでいる情報の識別から初めて、コロンと実際の情報が続く。これらは'ヘッダー'と呼ばれる。User-Agentヘッダーは、サーバーにこのリクエストを作るのにどのブラウザ（または他の種類のプログラム）が使われているかを知らせる。他の種類のヘッダーもしばしば一緒に送られ、例えば、クライアントが理解できる文書の種類や、好んでいる言語といった状態が送られる。

..  When given the above request, the server might send the following response:

上記の状態を与えられた時、サーバーは下記のようなレスポンスを返すかもしれない。：
::

    HTTP/1.1 200 OK
    Last-Modified: Mon, 23 Jul 2007 08:41:56 GMT
    Content-Length: 24
    Content-Type: text/plain

    apples, oranges, bananas

..  The first line indicates again the version of the HTTP protocol, followed by the status of the request. In this case the status code is 200, meaning 'OK, nothing out of the ordinary happened, I am sending you the file'. This is followed by a few headers, indicating (in this case) the last time the file was modified, its length, and its type (plain text). After the headers you get a blank line, followed by the file itself.

1行めはまたHTTPプロトコルのバージョンを示し、リクエストのステータスが続く。この場合、ステータスコードは200で、'OK'で、おかしなことが何も起こらなかったので、ファイルを送るという意味だ。これに何行かのヘッダーが続き、（この場合は）ファイルが最後に更新された日時、その長さ、その型（プレーンテキスト）が示されている。ヘッダーの後に空白の行が続き、ファイルそれ自体が続く。

..  Apart from requests starting with GET, which indicates the client just wants to fetch a document, the word POST can also be used to indicate some information will be sent along with the request, which the server is expected to process in some way.1

クライアントが文書の読み出しを希望することを示す、GETで始まるリクエストの他に、送るリクエストの情報に従って、サーバーに何らかの処理をして欲しいことを示す、POSTという語もある。

..  When you click a link, submit a form, or in some other way encourage your browser to go to a new page, it will do an HTTP request and immediately unload the old page to show the newly loaded document. In typical situations, this is just what you want ― it is how the web traditionally works. Sometimes, however, a JavaScript program wants to communicate with the server without re-loading the page. The 'Load' button in the console, for example, can load files without leaving the page.

リンクをクリックしたり、フォームをサブミットしたり、他の何らかの手段でブラウザが新しいページに行く時、HTTPリクエストが行われ、新しくロードされた文書を表示するために、直ちに古いページがアンロードされる。典型的な状況としては、これは望まれた時 -- 伝統的なウェブの動きかただ。時には、しかしながら、JavaScriptプログラムは、ページを再ロードすることなしにサーバーとコミュニケートしたがる。コンソールの'Load'ボタンは、例えば、ページから離れることなしにファイルをロードできる。

..  To be able to do things like that, the JavaScript program must make the HTTP request itself. Contemporary browsers provide an interface for this. As with opening new windows, this interface is subject to some restrictions. To prevent a script from doing anything scary, it is only allowed to make HTTP requests to the domain that the current page came from.

このようなことができるには、JavaScriptプログラムはHTTPリクエストそれ自体を作らなければならない。現代のブラウザはこれへのインターフェースを提供する。新しいウインドウを開く時のように、このインターフェースにはいくつかの制限が課されている。スクリプトが恐ろしいことをしないように、現在いるページのドメインにしかHTTPリクエストを送れないようになっている。

..  An object used to make an HTTP request can, on most browsers, be created by doing new XMLHttpRequest(). Older versions of Internet Explorer, which originally invented these objects, require you to do new ActiveXObject("Msxml2.XMLHTTP") or, on even older versions, new ActiveXObject("Microsoft.XMLHTTP"). ActiveXObject is Internet Explorer's interface to various kinds of browser add-ons. We are already used to writing incompatibility-wrappers by now, so let us do so again:

HTTPリクエストを作るためにオブジェクトを使うことができ、多くのブラウザで、new XMLHttpRequest()を使うことでオブジェクトが作られる。InternetExplorerの古いバージョンが、これらのオブジェクトの元の発明者で、new ActiveXObject("Msxml2.XMLHTTP")または、もっと古いバージョンでは、new ActiveXObject("Microsoft.XMLHTTP")が要求される。ActiveXObjectはInternetExplorerと様々なブラウザ・アドオンとのインターフェースである。既に非互換性ラッパーを書いているが、それをもう一度やろう。：

.. code-block:: javascript

    function makeHttpObject() {
      try {return new XMLHttpRequest();}
      catch (error) {}
      try {return new ActiveXObject("Msxml2.XMLHTTP");}
      catch (error) {}
      try {return new ActiveXObject("Microsoft.XMLHTTP");}
      catch (error) {}

      throw new Error("Could not create HTTP request object.");
    }

    show(typeof(makeHttpObject()));

..  The wrapper tries to create the object in all three ways, using try and catch to detect which ones fail. If none of the ways work, which might be the case on older browsers or browsers with strict security settings, it raises and error.

ラッパーは3つの方法でオブジェクトを作成を試みて、tryとcatchを使って、それらの中の1つの失敗を検知する。もしどの手段も動かなかったら、古いブラウザである場合か厳密なセキュリティ設定をされている場合なので、raiseでエラーにする。

..  Now why is this object called an XML HTTP request? This is a bit of a misleading name. XML is a way to store textual data. It uses tags and attributes like HTML, but is more structured and flexible ― to store your own kinds of data, you may define your own types of XML tags. These HTTP request objects have some built-in functionality for dealing with retrieved XML documents, which is why they have XML in their name. They can also handle other types of documents, though, and in my experience they are used just as often for non-XML requests.

なぜこのオブジェクトはXML HTTPリクエストと呼ばれているのだろうか？これはちょっとミスリードしそうな名前だ。XMLはテキストのデータを格納する手段で、HTMLのようにタグと属性を使うが、より構造化されていてフレキシブルである -- 自分自身のデータの種類を格納し、XMLタグの自分自身の型を定義できる。これらのHTTPリクエストのオブジェクトは、XML文書の取り出しを扱うための、ある組み込みの機能を持っていて、それが名前にXMLがついている理由だ。それらは他の文書の対応も扱うことができ、私の経験ではしばしばXMLでないリクエストにも使われる。

..  Now that we have our HTTP object, we can use it to make a request similar the example shown above.

HTTPオブジェクトが手に入ったので、上記の例のようなリクエストを作るのに使ってみよう。

.. code-block:: javascript

    var request = makeHttpObject();
    request.open("GET", "files/fruit.txt", false);
    request.send(null);
    print(request.responseText);

..  The open method is used to configure a request. In this case we choose to make a GET request for our fruit.txt file. The URL given here is relative, it does not contain the http:// part or a server name, which means it will look for the file on the server that the current document came from. The third parameter, false, will be discussed in a moment. After open has been called, the actual request can be made with the send method. When the request is a POST request, the data to be sent to the server (as a string) can be passed to this method. For GET requests, one should just pass null.

openメソッドはリクエストを構成する。この場合、fruit.txtファイルに対するGETリクエストを選ぶ。与えられたURLは相対URLで、http://やサーバ名の部分を含まず、これは現在の文書と同じサーバーからファイルを探すという意味である。3つめのパラメータ、falseについては、後で論じよう。openが呼んだ後、実際のリクエストをsendメソッドで送る。リクエストがPOSTリクエストの時は、サーバに送るデータ（文字列として）がこのメソッドに渡される。GETリクエストでは、nullを渡す。

..  After the request has been made, the responseText property of the request object contains the content of the retrieved document. The headers that the server sent back can be inspected with the getResponseHeader and getAllResponseHeaders functions. The first looks up a specific header, the second gives us a string containing all the headers. These can occasionally be useful to get some extra information about the document.

リクエストを作った後で、requestオブジェクトのresponseTextプロパティに取り出された文書の内容が入る。サーバから送り返されてきたヘッダーはgetResponseHeaderとgetAllResponseHeader関数で監視する。1つめは個別のヘッダーを、2つめは全てのヘッダーを含んだ文字列を返す。これらは時折、文書についての拡張情報を得るのに役に立つ。

.. code-block:: javascript

    print(request.getAllResponseHeaders());
    show(request.getResponseHeader("Date"));

..  If, for some reason, you want to add headers to the request that is sent to the server, you can do so with the setRequestHeader method. This takes two strings as arguments, the name and the value of the header.

もし、何らかの理由で、サーバーに送るリクエストにヘッダーを追加したいなら、setRequestHeaderメソッドでそれができる。これは引数として2つの文字列を取り、ヘッダーの名前と値である。

..  The response code, which was 200 in the example, can be found under the status property. When something went wrong, this cryptic code will indicate it. For example, 404 means the file you asked for did not exist. The statusText contains a slightly less cryptic description of the status.

レスポンスのコードは、例では200であったが、statusプロパティで見ることができる。何か問題があったら、この暗号のようなコードが問題を示す。例えば、404は要求されたファイルが存在しなかったという意味である。statusTextにはもう少し暗号っぽくないステータスの説明が含まれている。

.. code-block:: javascript

    show(request.status);
    show(request.statusText);

..  When you want to check whether a request succeeded, comparing the status to 200 is usually enough. In theory, the server might in some situations return the code 304 to indicate that the older version of the document, which the browser has stored in its 'cache', is still up to date. But it seems that browsers shield you from this by setting the status to 200 even when it is 304. Also, if you are doing a request over a non-HTTP protocol2, such as FTP, the status will not be usable because the protocol does not use HTTP status codes.

リクエストが成功したかどうかチェックしたい時は、ステータスが200か否か比較するだけで普通は十分だ。理屈の上では、サーバーはある状況では、文書のバージョンが古いことを示す、コード304を返すかもしれない。これはブラウザが'キャッシュ'を持っていて、それがまだ最新であるということだ。しかしブラウザは、ステータスが304であったときは、それに200を設定してこの値を隠すようだ。また、FTPのような、HTTP以外のプロトコルでリクエストを行った時は、プロトコルはHTTPステータスコードを使わないから、statusは使えないだろう。

..  When a request is done as in the example above, the call to the send method does not return until the request is finished. This is convenient, because it means the responseText is available after the call to send, and we can start using it immediately. There is a problem, though. When the server is slow, or the file is big, doing a request might take quite a while. As long as this is happening, the program is waiting, which causes the whole browser to wait. Until the program finishes, the user can not do anything, not even scroll the page. Pages that run on a local network, which is fast and reliable, might get away with doing requests like this. Pages on the big great unreliable Internet, on the other hand, should not.

上記のようなリクエストが行われた時、sendメソッドの呼び出しはリクエストが終わるまで返ってこない。これは便利だ、なぜなら、responseTextはsendの呼び出しの後で利用でき、それを即使い始めることができるということだからだ。それでも、問題はある。サーバーが遅かったり、あるいはファイルが大きければ、リクエストがとても長くかかる。これが起こっている間、プログラムは待ち続け、ブラウザ全体が待つことになってしまう。プログラムが終わるまで、ユーザーはページをスクロールする以外のことができなくなってしまう。ページは早くて確実なローカルのネットワーク上で実行されるので、このようなリクエストから逃れうるが、他方、大きくてひどく不確実なインターネットではそうはいかない。

..  When the third argument to open is true, the request is set to be 'asynchronous'. This means that send will return right away, while the request happens in the background.

openの3つめの引数がtrueのとき、リクエストは'非同期'であると設定される。これはsendは何も返さず、バックグラウンドでリクエストが行われるという意味だ。

.. code-block:: javascript

    request.open("GET", "files/fruit.xml", true);
    request.send(null);
    show(request.responseText);

..  But wait a moment, and...

しばらく待って、そして...

.. code-block:: javascript

    print(request.responseText);

..  'Waiting a moment' could be implemented with setTimeout or something like that, but there is a better way. A request object has a readyState property, indicating the state it is in. This will become 4 when the document has been fully loaded, and have a smaller value before that3. To react to changes in this status, you can set the onreadystatechange property of the object to a function. This function will be called every time the state changes.

'しばらく待つ'はsetTimeoutやそのような何かで実装できるが、もっといい方法がある。requestオブジェクトは、その状態を示すreadyStateプロパティを持っているのだ。文書が完全にロードされたらこれが4になり、それより小さい値もある。このステータスの変化に反応するため、オブジェクトのonreadystatechangeプロパティに関数を設定できる。この関数はステータスが変化する度に呼び出される。

.. code-block:: javascript

    request.open("GET", "files/fruit.xml", true);
    request.send(null);
    request.onreadystatechange = function() {
      if (request.readyState == 4)
        show(request.responseText.length);
    };

..  When the file retrieved by the request object is an XML document, the request's responseXML property will hold a representation of this document. This representation works like the DOM objects discussed in chapter 12, except that it doesn't have HTML-specific functionality, such as style or innerHTML. responseXML gives us a document object, whose documentElement property refers to the outer tag of the XML document.

requestオブジェクトから取り出されたファイルがXML文書だった時、requestのresponseXMLプロパティはこの文書の複写を持つ。この複写は、styleやinnerHTMLのようなHTML固有の機能を除けば、12章で論じたDOMオブジェクトのように動く。responseXMLは、そのdocumentElementプロパティがXML文書の外側のタグを参照するdocumentオブジェクトをくれる。

.. code-block:: javascript

    var catalog = request.responseXML.documentElement;
    show(catalog.childNodes.length);

..  Such XML documents can be used to exchange structured information with the server. Their form ― tags contained inside other tags ― is often very suitable to store things that would be tricky to represent as simple flat text. The DOM interface is rather clumsy for extracting information though, and XML documents are notoriously wordy: The fruit.xml document looks like a lot, but all it says is 'apples are red, oranges are orange, and bananas are yellow'.

このようなXML文書はサーバーと構造化された情報を交換するのに用いられる。この形態 -- タグはその中に他のタグを含み --しばしば、単純なテキストよりもトリッキーなものを格納するのにとても適している。DOMインターフェースは情報を展開するにはむしろややこしく、XML文書はひどく冗長だ：fruit.xml文書はたくさんのことが書かれているように見えるが、'リンゴは赤い、オレンジはオレンジ色、バナナは黄色'としか書かれていない。

..  As an alternative to XML, JavaScript programmers have come up with something called JSON. This uses the basic notation of JavaScript values to represent 'hierarchical' information in a more minimalist way. A JSON document is a file containing a single JavaScript object or array, which in turn contains any number of other objects, arrays, strings, numbers, booleans, or null values. For an example, look at fruit.json:

XMLの代替として、JavaScriptプログラマはJSONと呼ばれるものを思いついた。これは、JavaScriptの値の基本的な表記を、'階層的な'情報を表現するための、より小さいのやりかたとして用いる。JSON文書は、単一のJavaScriptオブジェクトや配列を含むファイルであるが、反対に、任意の数の他のオブジェクト、配列、文字列、数値、真偽値、あるいはnullの値をその中に含めることができる。例えば、fruit.jsonを見て欲しい：

.. code-block:: javascript

    request.open("GET", "files/fruit.json", true);
    request.send(null);
    request.onreadystatechange = function() {
      if (request.readyState == 4)
        print(request.responseText);
    };

..  Such a piece of text can be converted to a normal JavaScript value by using the eval function. Parentheses should be added around it before calling eval, because otherwise JavaScript might interpret an object (enclosed by braces) as a block of code, and produce an error.

このようなテキストの部品は、eval関数を使うことで通常のJavaScriptの値に変換することができる。evalを呼び出す前に、その周りに括弧を追加すべきで、そうしなければJavaScriptはオブジェクト（中括弧で囲まれた）をコードのブロックとして解釈し、エラーになるかもしれない。

.. code-block:: javascript

    function evalJSON(json) {
      return eval("(" + json + ")");
    }
    var fruit = evalJSON(request.responseText);
    show(fruit);

..  When running eval on a piece of text, you have to keep in mind that this means you let the piece of text run whichever code it wants. Since JavaScript only allows us to make requests to our own domain, you will usually know exactly what kind of text you are getting, and this is not a problem. In other situations, it might be unsafe.

テキストでevalを実行する時、このテキストが、実行したいコードか否かにかかわらず実行されることを念頭に置くこと。JavaScriptは自分自身のドメインへのリクエストしか許していないから、普通は取得するテキストがどんなものかを正しく知っているだろうし、これは問題にならない。他の状況では、これは安全でないことがある。

Ex. 14.1

..  Write a function called serializeJSON which, when given a JavaScript value, produces a string with the value's JSON representation. Simple values like numbers and booleans can be simply given to the String function to convert them to a string. Objects and arrays can be handled by recursion.

JavaScriptの値が与えられたら、JSON表記で値を表現する文字列を作る、serializeJSONという関数を書け。数値と真偽値のような単純な値は単純にString関数で文字列に変換できる。オブジェクトや配列は再帰を使って処理できる。

..  Recognizing arrays can be tricky, since its type is "object". You can use instanceof Array, but that only works for arrays that were created in your own window ― others will use the Array prototype from other windows, and instanceof will return false. A cheap trick is to convert the constructor property to a string, and see whether that contains "function Array".

配列を見分けるのはトリッキーかもしれない、その型が"object"になるからである。instanceof Arrayを使ってもいいが、これは自分自身で開いたウインドウの中で作られた配列でしか動かない -- 他のものは、他のウィンドウからArrayプロトタイプを使っても、instanceofは偽を返すだろう。安っぽいトリックとしてはconstructorプロパティを文字列に変換して、中に"function Array"が入っているか見ることだ。

..  When converting a string, you have to take care to escape special characters inside it. If you use double-quotes around the string, the characters to escape are \", \\, \f, \b, \n, \t, \r, and \v4.

文字列に変換する時、中の特殊文字をエスケープするよう注意しなければならない。もし、文字列を囲むのに二重引用符を使っていれば、文字のエスケープは\\"になるし、\\\、\\f、\\b、\\n、\\t、\\r、\\vとなっていく。

.. code-block:: javascript

    function serializeJSON(value) {
      function isArray(value) {
        return /^\s*function Array/.test(String(value.constructor));
      }

      function serializeArray(value) {
        return "[" + map(serializeJSON, value).join(", ") + "]";
      }
      function serializeObject(value) {
        var properties = [];
        forEachIn(value, function(name, value) {
          properties.push(serializeString(name) + ": " +
                          serializeJSON(value));
        });
        return "{" + properties.join(", ") + "}";
      }
      function serializeString(value) {
        var special =
          {"\"": "\\\"", "\\": "\\\\", "\f": "\\f", "\b": "\\b",
           "\n": "\\n", "\t": "\\t", "\r": "\\r", "\v": "\\v"};
        var escaped = value.replace(/[\"\\\f\b\n\t\r\v]/g,
                                    function(c) {return special[c];});
        return "\"" + escaped + "\"";
      }

      var type = typeof value;
      if (type == "object" && isArray(value))
        return serializeArray(value);
      else if (type == "object")
        return serializeObject(value);
      else if (type == "string")
        return serializeString(value);
      else
        return String(value);
    }

    print(serializeJSON(fruit));

..  The trick used in serializeString is similar to what we saw in the escapeHTML function in chapter 10. It uses an object to look up the correct replacements for each of the characters. Some of them, such as "\\\\", look quite weird because of the need to put two backslashes for every backslash in the resulting string.

serializeStringで使ったトリックは10章のescapeHTML関数で見せたものと同様のものだ。文字それぞれの正しい置換文字を見つけるためにオブジェクトを使っている。"\\\\\\\\"のような、それらはひどく恐ろしげに見えるが、結果の文字列の中で、バックスラッシュ毎に2つのバックスラッシュを置換するために必要なものだ。

..  Also note that the names of properties are quoted as strings. For some of them, this is not necessary, but for property names with spaces and other strange things in them it is, so the code just takes the easy way out and quotes everything.

プロパティの名前は文字列として引用符を付けることにも注意。中には、必要ない場合もある、しかしプロパティの中に空白や他のおかしなものが混じっていたら、コードは簡単に道を外れて全てが引用符の中に入ってしまう。

..  When making lots of requests, we do, of course, not want to repeat the whole open, send, onreadystatechange ritual every time. A very simple wrapper could look like this:

たくさんのリクエストを行うとき、我々は、もちろん、open、send、onreadeystatechangeといった全ての儀式をいちいち繰り返したくはない。このような、とても単純なラッパーを作れる。：

.. code-block:: javascript

    function simpleHttpRequest(url, success, failure) {
      var request = makeHttpObject();
      request.open("GET", url, true);
      request.send(null);
      request.onreadystatechange = function() {
        if (request.readyState == 4) {
          if (request.status == 200)
            success(request.responseText);
          else if (failure)
            failure(request.status, request.statusText);
        }
      };
    }

    simpleHttpRequest("files/fruit.txt", print);

..  The function retrieves the url it is given, and calls the function it is given as a second argument with the content. When a third argument is given, this is used to indicate failure ― a non-200 status code.

関数は与えられたURLを取り出して、2つめの引数として与えられた関数を内容とともに呼び出す。3つめの引数が与えられたとき、これは失敗が示された時 -- ステータス・コードが200以外の時に使われる

..  To be able to do more complex requests, the function could be made to accept extra parameters to specify the method (GET or POST), an optional string to post as data, a way to add extra headers, and so on. When you have so many arguments, you'd probably want to pass them as an arguments-object as seen in chapter 9.

より複雑なリクエストをできるようにするために、関数が、メソッド（GETかPOST）の指定、POSTする時のオプションのデータ、拡張のヘッダーの追加、その他のような拡張のパラメータを受け付けるようにできるだろう。引数が多くなりすぎた時には、9章で見たような引数オブジェクトを渡すようにしたいと思うだろう。

..  Some websites make use of intensive communication between the programs running on the client and the programs running on the server. For such systems, it can be practical to think of some HTTP requests as calls to functions that run on the server. The client makes request to URLs that identify the functions, giving the arguments as URL parameters or POST data. The server then calls the function, and puts the result into JSON or XML document that it sends back. If you write a few convenient support functions, this can make calling server-side functions almost as easy as calling client-side ones... except, of course, that you do not get their results instantly.

あるウェブサイトはクライアントで実行されるプログラムとサーバで実行されるプログラムの間で激しいコミュニケーションに使われる。そのようなシステムでは、あるHTTPリクエストはサーバで実行される関数の呼び出しであると考えることが実用的であり得る。クライアントはURLを関数の識別としてリクエストを行い、URLのパラメータやPOSTのデータとして引数を与える。サーバは関数を呼び出して、送り返すJSONやXML文書に結果を入れる。いくつか便利なサポート関数を書けば、これはクライアント側と同じくらい簡単にサーバ側の関数を呼び出せるようになる...もちろん、即その結果を受け取ることはできないという点を除いて。
