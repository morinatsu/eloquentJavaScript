---
layout: default
title: 14. HTTPリクエスト
---

HTTPリクエスト
=====================================================

11章で言ったように、ワールド・ワイド・ウェブでのコミュニケーションはHTTPプロトコルの上で起こる。単純なリクエストは以下のようになる。：

```
GET /files/fruit.txt HTTP/1.1
Host: eloquentjavascript.net
User-Agent: The Imaginary Browser
```

ファイルfiles/fruit.txtがどこにあるかをeloquentjavascript.netのサーバーに尋ねる。加えて、このリクエストにHTTPプロトコルのバージョン1.1を使うことを指定している。-- バージョン1.0はまだ使われており、少々異なる働きをする。HostとUser-Agentの行はパターンに従う：それが含んでいる情報の識別から初めて、コロンと実際の情報が続く。これらは'ヘッダー'と呼ばれる。User-Agentヘッダーは、サーバーにこのリクエストを作るのにどのブラウザ（または他の種類のプログラム）が使われているかを知らせる。他の種類のヘッダーもしばしば一緒に送られ、例えば、クライアントが理解できる文書の種類や、好んでいる言語といった状態が送られる。

上記の状態を与えられた時、サーバーは下記のようなレスポンスを返すかもしれない。：

```
HTTP/1.1 200 OK
Last-Modified: Mon, 23 Jul 2007 08:41:56 GMT
Content-Length: 24
Content-Type: text/plain

apples, oranges, bananas
```

1行めはまたHTTPプロトコルのバージョンを示し、リクエストのステータスが続く。この場合、ステータスコードは200で、'OK'で、おかしなことが何も起こらなかったので、ファイルを送るという意味だ。これに何行かのヘッダーが続き、（この場合は）ファイルが最後に更新された日時、その長さ、その型（プレーンテキスト）が示されている。ヘッダーの後に空白の行が続き、ファイルそれ自体が続く。

クライアントが文書の読み出しを希望することを示す、GETで始まるリクエストの他に、送るリクエストの情報に従って、サーバーに何らかの処理をして欲しいことを示す、POSTという語もある。

リンクをクリックしたり、フォームをサブミットしたり、他の何らかの手段でブラウザが新しいページに行く時、HTTPリクエストが行われ、新しくロードされた文書を表示するために、直ちに古いページがアンロードされる。典型的な状況としては、これは望まれた時 -- 伝統的なウェブの動きかただ。時には、しかしながら、JavaScriptプログラムは、ページを再ロードすることなしにサーバーとコミュニケートしたがる。コンソールの'Load'ボタンは、例えば、ページから離れることなしにファイルをロードできる。

このようなことができるには、JavaScriptプログラムはHTTPリクエストそれ自体を作らなければならない。現代のブラウザはこれへのインターフェースを提供する。新しいウインドウを開く時のように、このインターフェースにはいくつかの制限が課されている。スクリプトが恐ろしいことをしないように、現在いるページのドメインにしかHTTPリクエストを送れないようになっている。

HTTPリクエストを作るためにオブジェクトを使うことができ、多くのブラウザで、new XMLHttpRequest()を使うことでオブジェクトが作られる。InternetExplorerの古いバージョンが、これらのオブジェクトの元の発明者で、new ActiveXObject("Msxml2.XMLHTTP")または、もっと古いバージョンでは、new ActiveXObject("Microsoft.XMLHTTP")が要求される。ActiveXObjectはInternetExplorerと様々なブラウザ・アドオンとのインターフェースである。既に非互換性ラッパーを書いているが、それをもう一度やろう。：

```javascript
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
```

ラッパーは3つの方法でオブジェクトを作成を試みて、tryとcatchを使って、それらの中の1つの失敗を検知する。もしどの手段も動かなかったら、古いブラウザである場合か厳密なセキュリティ設定をされている場合なので、raiseでエラーにする。

なぜこのオブジェクトはXML HTTPリクエストと呼ばれているのだろうか？これはちょっとミスリードしそうな名前だ。XMLはテキストのデータを格納する手段で、HTMLのようにタグと属性を使うが、より構造化されていてフレキシブルである -- 自分自身のデータの種類を格納し、XMLタグの自分自身の型を定義できる。これらのHTTPリクエストのオブジェクトは、XML文書の取り出しを扱うための、ある組み込みの機能を持っていて、それが名前にXMLがついている理由だ。それらは他の文書の対応も扱うことができ、私の経験ではしばしばXMLでないリクエストにも使われる。

HTTPオブジェクトが手に入ったので、上記の例のようなリクエストを作るのに使ってみよう。

```javascript
var request = makeHttpObject();
request.open("GET", "files/fruit.txt", false);
request.send(null);
print(request.responseText);
```

openメソッドはリクエストを構成する。この場合、fruit.txtファイルに対するGETリクエストを選ぶ。与えられたURLは相対URLで、http://やサーバ名の部分を含まず、これは現在の文書と同じサーバーからファイルを探すという意味である。3つめのパラメータ、falseについては、後で論じよう。openが呼んだ後、実際のリクエストをsendメソッドで送る。リクエストがPOSTリクエストの時は、サーバに送るデータ（文字列として）がこのメソッドに渡される。GETリクエストでは、nullを渡す。

リクエストを作った後で、requestオブジェクトのresponseTextプロパティに取り出された文書の内容が入る。サーバから送り返されてきたヘッダーはgetResponseHeaderとgetAllResponseHeader関数で監視する。1つめは個別のヘッダーを、2つめは全てのヘッダーを含んだ文字列を返す。これらは時折、文書についての拡張情報を得るのに役に立つ。

```javascript
print(request.getAllResponseHeaders());
show(request.getResponseHeader("Date"));
```

もし、何らかの理由で、サーバーに送るリクエストにヘッダーを追加したいなら、setRequestHeaderメソッドでそれができる。これは引数として2つの文字列を取り、ヘッダーの名前と値である。

レスポンスのコードは、例では200であったが、statusプロパティで見ることができる。何か問題があったら、この暗号のようなコードが問題を示す。例えば、404は要求されたファイルが存在しなかったという意味である。statusTextにはもう少し暗号っぽくないステータスの説明が含まれている。

```javascript
show(request.status);
show(request.statusText);
```

リクエストが成功したかどうかチェックしたい時は、ステータスが200か否か比較するだけで普通は十分だ。理屈の上では、サーバーはある状況では、文書のバージョンが古いことを示す、コード304を返すかもしれない。これはブラウザが'キャッシュ'を持っていて、それがまだ最新であるということだ。しかしブラウザは、ステータスが304であったときは、それに200を設定してこの値を隠すようだ。また、FTPのような、HTTP以外のプロトコルでリクエストを行った時は、プロトコルはHTTPステータスコードを使わないから、statusは使えないだろう。

上記のようなリクエストが行われた時、sendメソッドの呼び出しはリクエストが終わるまで返ってこない。これは便利だ、なぜなら、responseTextはsendの呼び出しの後で利用でき、それを即使い始めることができるということだからだ。それでも、問題はある。サーバーが遅かったり、あるいはファイルが大きければ、リクエストがとても長くかかる。これが起こっている間、プログラムは待ち続け、ブラウザ全体が待つことになってしまう。プログラムが終わるまで、ユーザーはページをスクロールする以外のことができなくなってしまう。ページは早くて確実なローカルのネットワーク上で実行されるので、このようなリクエストから逃れうるが、他方、大きくてひどく不確実なインターネットではそうはいかない。

openの3つめの引数がtrueのとき、リクエストは'非同期'であると設定される。これはsendは何も返さず、バックグラウンドでリクエストが行われるという意味だ。

```javascript
request.open("GET", "files/fruit.xml", true);
request.send(null);
show(request.responseText);
```

しばらく待って、そして...

```javascript
print(request.responseText);
```

'しばらく待つ'はsetTimeoutやそのような何かで実装できるが、もっといい方法がある。requestオブジェクトは、その状態を示すreadyStateプロパティを持っているのだ。文書が完全にロードされたらこれが4になり、それより小さい値もある。このステータスの変化に反応するため、オブジェクトのonreadystatechangeプロパティに関数を設定できる。この関数はステータスが変化する度に呼び出される。

```javascript
request.open("GET", "files/fruit.xml", true);
request.send(null);
request.onreadystatechange = function() {
  if (request.readyState == 4)
    show(request.responseText.length);
};
```

requestオブジェクトから取り出されたファイルがXML文書だった時、requestのresponseXMLプロパティはこの文書の複写を持つ。この複写は、styleやinnerHTMLのようなHTML固有の機能を除けば、12章で論じたDOMオブジェクトのように動く。responseXMLは、そのdocumentElementプロパティがXML文書の外側のタグを参照するdocumentオブジェクトをくれる。

```javascript
var catalog = request.responseXML.documentElement;
show(catalog.childNodes.length);
```

このようなXML文書はサーバーと構造化された情報を交換するのに用いられる。この形態 -- タグはその中に他のタグを含み -- しばしば、単純なテキストよりもトリッキーなものを格納するのにとても適している。DOMインターフェースは情報を展開するにはむしろややこしく、XML文書はひどく冗長だ：fruit.xml文書はたくさんのことが書かれているように見えるが、'リンゴは赤い、オレンジはオレンジ色、バナナは黄色'としか書かれていない。

XMLの代替として、JavaScriptプログラマはJSONと呼ばれるものを思いついた。これは、JavaScriptの値の基本的な表記を、'階層的な'情報を表現するための、より小さいのやりかたとして用いる。JSON文書は、単一のJavaScriptオブジェクトや配列を含むファイルであるが、反対に、任意の数の他のオブジェクト、配列、文字列、数値、真偽値、あるいはnullの値をその中に含めることができる。例えば、fruit.jsonを見て欲しい：

```javascript
request.open("GET", "files/fruit.json", true);
request.send(null);
request.onreadystatechange = function() {
  if (request.readyState == 4)
    print(request.responseText);
};
```

このようなテキストの部品は、eval関数を使うことで通常のJavaScriptの値に変換することができる。evalを呼び出す前に、その周りに括弧を追加すべきで、そうしなければJavaScriptはオブジェクト（中括弧で囲まれた）をコードのブロックとして解釈し、エラーになるかもしれない。

```javascript
function evalJSON(json) {
  return eval("(" + json + ")");
}
var fruit = evalJSON(request.responseText);
show(fruit);
```

テキストでevalを実行する時、このテキストが、実行したいコードか否かにかかわらず実行されることを念頭に置くこと。JavaScriptは自分自身のドメインへのリクエストしか許していないから、普通は取得するテキストがどんなものかを正しく知っているだろうし、これは問題にならない。他の状況では、これは安全でないことがある。

### <a name="Ex14-1">[演習 14.1]</a>

JavaScriptの値が与えられたら、JSON表記で値を表現する文字列を作る、serializeJSONという関数を書け。数値と真偽値のような単純な値は単純にString関数で文字列に変換できる。オブジェクトや配列は再帰を使って処理できる。

配列を見分けるのはトリッキーかもしれない、その型が"object"になるからである。instanceof Arrayを使ってもいいが、これは自分自身で開いたウインドウの中で作られた配列でしか動かない -- 他のものは、他のウィンドウからArrayプロトタイプを使っても、instanceofは偽を返すだろう。安っぽいトリックとしてはconstructorプロパティを文字列に変換して、中に"function Array"が入っているか見ることだ。

文字列に変換する時、中の特殊文字をエスケープするよう注意しなければならない。もし、文字列を囲むのに二重引用符を使っていれば、文字のエスケープは\\"になるし、\\\、\\f、\\b、\\n、\\t、\\r、\\vとなっていく。

```javascript
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
```

serializeStringで使ったトリックは10章のescapeHTML関数で見せたものと同様のものだ。文字それぞれの正しい置換文字を見つけるためにオブジェクトを使っている。"\\\\\\\\"のような、それらはひどく恐ろしげに見えるが、結果の文字列の中で、バックスラッシュ毎に2つのバックスラッシュを置換するために必要なものだ。

プロパティの名前は文字列として引用符を付けることにも注意。中には、必要ない場合もある、しかしプロパティの中に空白や他のおかしなものが混じっていたら、コードは簡単に道を外れて全てが引用符の中に入ってしまう。

たくさんのリクエストを行うとき、我々は、もちろん、open、send、onreadeystatechangeといった全ての儀式をいちいち繰り返したくはない。このような、とても単純なラッパーを作れる。：

```javascript
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
```

関数は与えられたURLを取り出して、2つめの引数として与えられた関数を内容とともに呼び出す。3つめの引数が与えられたとき、これは失敗が示された時 -- ステータス・コードが200以外の時に使われる

より複雑なリクエストをできるようにするために、関数が、メソッド（GETかPOST）の指定、POSTする時のオプションのデータ、拡張のヘッダーの追加、その他のような拡張のパラメータを受け付けるようにできるだろう。引数が多くなりすぎた時には、9章で見たような引数オブジェクトを渡すようにしたいと思うだろう。

あるウェブサイトはクライアントで実行されるプログラムとサーバで実行されるプログラムの間で激しいコミュニケーションに使われる。そのようなシステムでは、あるHTTPリクエストはサーバで実行される関数の呼び出しであると考えることが実用的であり得る。クライアントはURLを関数の識別としてリクエストを行い、URLのパラメータやPOSTのデータとして引数を与える。サーバは関数を呼び出して、送り返すJSONやXML文書に結果を入れる。いくつか便利なサポート関数を書けば、これはクライアント側と同じくらい簡単にサーバ側の関数を呼び出せるようになる...もちろん、即その結果を受け取ることはできないという点を除いて。
