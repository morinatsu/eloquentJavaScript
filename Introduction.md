---
layout: default
title: 1. 導入
---

1. 導入
=======

パーソナルコンピューターが最初に導入されたとき、その多くはシンプルなプログラミング言語を備えていて、通常それはBASICの変種だった。コンピューターとのやりとりはこの言語に統合され、そのため全てのコンピューターユーザーは好むと好まざるに係わらず、それに触れざるを得なかった。現在コンピューターは豊富で安価なものとなり、典型的なユーザーはマウスクリック以上のことをしなくなった。多くの人々にとって、それは良いことだ。しかし、我々のような、テクノロジーいじりに自然に惹かれるような者にとっては、コンピューターを使う毎日からプログラミングが取り除かれるということは、何らかの障壁が設けられたようなものだ。

幸運なことに、ワールドワイドウェブの開発の影響で、全てのコンピューターがモダンなWebブラウザを備え、JavaScriptによるプログラミング環境を持つことになった。今日ではユーザーは技術的な細部に飽きさせられることない。それはうまく隠されたままだ。しかしWebページはアクセス可能になり、プログラミングの学習のプラットフォームとして使うことができるようになった。

それをこの本で行おうとしている。

* * *


**私は学習意欲のない者、彼ら自身を解説することに気を払わない者を啓蒙しようとは思わない。もし四角形の1つの角を見せても彼らが残りの3つを私に返さないとしたら、私は再びそのポイントを示すことはしない。**
-- 孔子

JavaScriptの解説に加えて、この本ではプログラミングの基本原則を紹介しようとしている。プログラミングは明らかに難しい。基礎的なルールは、多くの間、単純でクリアなこととされている。しかし、プログラムはこの基本的なルールに従って作られていても、それ自身のルールや、それ自身の複雑さを持ち込むために、複雑になりがちである。このためプログラミングが単純だったり予測可能であることはまれだ。この分野における父であると見られているDonald Knuthはそれは **アート** であると言った。

この本から何か得ようとするなら、受動的に読む以上のことが必要となる。鋭くあること、演習を解くために努力すること、そして理解が深まったと確認できるまで続けること。

* * *

**コンピュータープログラマーは彼だけが責任を負う宇宙の創造者だ。仮想的で限界の無い複雑さの宇宙はコンピュータープログラムという形で創造される。**
-- Joseph Weizenbaum, コンピューターパワーと人間の理性


プログラムは多くの物からなる。プログラマーによってタイプされたテキストの一片、それはコンピューターにあることを行わせる力であったり、コンピュータメモリの中のデータであったり、同じメモリで行われる動作を制御するするものであったりする。精巧に組み合わされた機械時計の歯車は、よい時計職人によるものであれば、多くの年月にわたって時間を示し続ける。同じように組み合わされたプログラムの要素は、もしプログラマーが彼自身の行うことを知っていればクラッシュすることなしに動き続ける。

コンピューターはこれらの実体のない機械のホストとして動くように作られたものだ。コンピュータ自体は愚鈍にまっすぐなことを行うことしかできない。これらのことを非常に速いスピードで行うほうが便利だというのがその理由だ。プログラムは単純な動作を精巧に組み合わせることで、とても複雑なことを行うことを可能にする。

我々の中のある者にとっては、コンピュータープログラムを書くと言うことは魅惑的なゲームだ。プログラムは思考の建築物だ。費用をかけずに建てることができ、重くなく、我々のタイプする手によって簡単に大きくできる。もし我々がいなくなれば、そのサイズと複雑さはコントロールから外れて大きくなり、それを操作する者を混乱させる。これはプログラミングの主要な問題だ。多くの今日のソフトウェアがクラッシュし、失敗し、へまをしがちな理由である。

プログラムが働くとき、それは美しい。プログラミングの芸術は複雑さをコントロールする技術にある。偉大なプログラムは複雑さを抑えられ、単純にされたものだ。

* * *

今日、多くのプログラマーは、彼らのプログラムのよく理解された技術の小さいセットを使うだけで、この複雑さはうまく管理されていると信じている。彼らはプログラムがあるべき姿についての厳格なルールを作り、これらのルールを破る者を悪いプログラマーであると熱心に非難する。

プログラミングの豊かさへの、この敵意はなんだ！何かをまっすぐに予測可能にするために豊かさを削ぐことは、すばらしくかつ美しいもの全てをタブーに置き換えることだ。プログラミング技術の展望は大きく、その多様性という魅力があり、いまだ大きな未踏である。あなたが注意深く進むため、あなたについてのあなたの知恵を保つというだけの意味しかないことで、罠と仕掛けをちりばめ、経験のないプログラマーを全ての種類の恐ろしい過ちに導くことになることは確かだ。常に新しい挑戦者であり、新しい場所への冒険であるかのように学びなさい。冒険し続けることをやめたプログラマーは確実に、澱んで、楽しみを忘れ、プログラムする意志を失い（、そして管理者になってしまう）。

私の係わるところでは、プログラムの評価基準はそれが正しいかと定義する。効率、明瞭さ、サイズもまた重要だが、互いに対立するそれらをどのようにバランスさせるかは常に価値判断の問題であり、価値判断はそれぞれのプログラマーが自分自身で行わなければならないものだ。経験則は便利だが、それを破ることを恐れるべきではない。

* * *

事の始め、コンピューティングの誕生のとき、プログラミング言語がなかったとき、プログラムはこのようなものであった。

```
00110001 00000000 00000000
00110001 00000001 00000001
00110011 00000001 00000010
01010001 00001011 00000010
00100010 00000010 00001000
01000011 00000001 00000000
01000001 00000001 00000001
00010000 00000010 00000000
01100010 00000000 00000000
```

これは1から10までの数を足し合わせ、その結果(1 + 2 + ... + 10 = 55)をプリントするプログラムである。とても単純な種類のコンピューター上で走らせることができた。初期のコンピューターをプログラムするには、スイッチの巨大な配列を正しい位置にセットしたり、カードの束のパンチ穴をコンピュータに読み込ませる必要があった。あなたはこれは退屈で、ミスしやすい処理であるというイメージを持つかもしれない。単純なプログラムを書くのにも大変な利口さと修練が必要となり、これらを組み合わせることは想像を絶する。

もちろん、これらの不可解なビット（上記の1と0のことを全般的にそう呼ぶ）のパターンを手作業で入力することはプログラマーが強力な魔法使いであるかのように強い感覚を与える。仕事のやりがいといったような、何か価値のあるものがある。

このプログラムの各行は1つの命令を含んでいる。日本語で書けばこのようになる。

```
1. メモリの位置0に数値0を保存する
2. メモリの位置1に数値1を保存する
3. メモリの位置1にある値をメモリの位置2に保存する
4. メモリの位置2にある値から11を引く
5. メモリの位置2にある値が0であれば、命令9から続ける
6. メモリの位置1にある値をメモリの位置0に足す
7. メモリの位置1にある値に1を足す
8. 命令3から続ける
9. メモリの位置0にある値を出力する
```

これは先ほどの2進数のスープよりは読みやすいが、まだ意地悪だ。命令やメモリを示す数値を名前に置き換えれば助けになるかもしれない。

```
'合計'に0をセットする
'カウント'に1をセットする
|ループ|
    '比較'に'カウント'をセットする
    '比較'から11を引く
    もし'比較'が0ならば|終了|から続ける
    'カウント'を'合計'に足す
    'カウント'に1を足す
    |ループ|から続ける
|終了|
'合計'を出力する
```

ここまでくれば、プログラムがどのように動くのか見るのがあなたにとっても難しすぎるということはないだろうね。最初の2行は2つのメモリの位置に開始時の値を与えている：`合計`はプログラムの結果を作るために使われ、`カウント`は我々が今見ている値を追いつづける。`比較`を用いている行はおそらく一番奇異なものだ。プログラムが求めているのは、もう実行を止めていいかどうか判定するために、`カウント`が11と等しいかどうか見ることだ。機械はとても原始的なので、数値がゼロであるかどうかテストすることしかできず、判定（ジャンプ）はそれに基づく。そこで`比較`と名付けたメモリの位置を`カウント - 11`の値を計算するために使い、この値を元に判定する。次の2行は`カウント`を結果に足して、`カウント`を1増やすということを、プログラムが`カウント`がまだ11になってないと判断する間、毎回続ける。

同じプログラムをJavaScriptで書くとこうなる:

```javascript
var total = 0, count = 1;
while (count <= 10) {
    total += count;
    count += 1;
}
print(total);
```

これは我々に幾つかの進歩をもたらす。最も重要なのは、我々はプログラム後戻りさせるための特別なジャンプを書く必要がなく、もうそれを強いられることもなくなったということだ。魔法の言葉、`while`がその面倒を見てくれる。それはその下の行を、与えられた条件：`count <= 10`（`count`が`10`より小さいか等しいという意味）が続く限り実行し続ける。見たところ、一時的な値を作り出してゼロと比較する必要はなくなったようだ。これは愚かで些細なことであり、そしてこのような我々にとっては愚かで些細であることの面倒を見ることがプログラミング言語の力なのである。

最後に、もし我々が、範囲に含まれる数のコレクションを作り、数のコレクションの合計を計算する`range`や`sum`といった便利な操作を使えれば、このプログラムのように書けたであろう:

```javascript
print(sum(range(1, 10)));
```

この文章の方針として、今後は、同じプログラムは長く書かれていれば短く、読みにくければ読みやすいものにしていこう。プログラムの最初のバージョンは非常に不明瞭で、最後のものはほとんど英語と変わらない：`1`から`10`までの`range`の数の`sum`を`print`せよ。（後の章で`sum`や`range`のようなものの作りかたを見よう）

良いプログラミング言語はより抽象的な手段を提供することで、プログラマーが彼自身が書きたいことを書くことを助ける。興味を引かない細部を隠し、（`while`構造のような）便利な組み立て用のブロックを提供し、そして、多くの場合に、プログラマーが自分自身で（`sum`や`range`操作のような）組み立て用の部品を加えることを可能にする。

* * *

JavaScriptは、まさしくそのような言語であり、現在、ワールドワイドウェブ上のページで全ての種類の高度で危険を伴う事柄にもっとも使われている。[ある人々](http://steve-yegge.blogspot.com/2007/02/next-big-language.html) はJavaScriptの次のバージョンが他の仕事についても重要な言語になるだろうと主張している。私はそうなると確信してはいないが、もしプログラミングに興味があるなら、JavaScriptは学ぶには良い言語である。たとえ結局ウェブのプログラミングをしないのであっても、私はこの本で見せる驚くべきプログラムはいつもあなたとともにあり、あなたの所に通い、他の言語であなたが書くプログラムに影響するだろう。

JavaScript**恐ろしいものである**とも言われている。そして、それらの多くが真実である。私が最初にJavaScriptで何かを書く必要とした際にも、私はすぐにJavaScriptを嫌うようになった。私がタイプしたことのほとんどがエラーにならずそのまま受け入れられ、しかも私が意図と全く違うように解釈される。私がしたいことへの道筋が繋がらないことが多くあり、これが現実の問題となった：JavaScriptはそれが許す限り途方もなく自由である。この設計の背後にあるアイデアはJavaScriptでのプログラミングを初心者にとって簡単なものにしようというものだ。実際には、システムがあなたにそれを指摘しなくなったため、あなたがプログラムの問題箇所を見つけることを困難なものにした。

しかしながら、言語の柔軟さはアドバンテージでもある。より厳格な言語では不可能なたくさんのテクニックの余地があり、それらはJavaScriptの短所のいくつかを克服するのに使えるかもしれない。正しく学び、それらを動かすことができるようになったら、私は**このような**言語を学んだと言えるだろう。

* * *

名前が示すのとは正反対に、JavaScriptはJavaと名付けられた言語にほとんど似ていない。同じような名前になっているのは良き判断よりむしろマーケティング上の考慮によるものだ。1995年に、JavaScriptがNetscapeに導入されたとき、Java言語はマーケット上重要であり、人気も上昇していた。見たところ、誰かがこの市場に乗ってみるのがいいアイデアだと考えたのだ。今では我々はこの名前にハメられている。

JavaScriptに関係あるのはECMAScriptと呼ばれているものだ。Netscape以外のブラウザがJavaScript、あるいはそれに似たもののサポートを始めたとき、言語がどのように動作するかの正確な説明がドキュメントには書かれた。組織化と標準化の後、このドキュメントに説明されている言語はECMAScriptと呼ばれた。

ECMAScriptは汎用のプログラミング言語と説明され、インターネットブラウザへのこの言語の統合については何も言われなかった。JavaScriptはECMAScriptにインターネットページとブラウザウインドウを取り扱う拡張ツールを加えたものである。

ECMAScriptのドキュメントにはこの言語の他のソフトウェアでの使用についても説明されていた。最も重要なのは、Flashで使われるActionScriptもECMAScriptが土台になっていることだ（正確には標準に従っていないにも係わらず）。Flashは動くものやたくさんの音をウェブページに追加するシステムである。JavaScriptを知ることはもしあなたがFlashムービーの制作を学ぶことになったとしても害にはならないだろう。

JavaScriptはまだ進化している。この本が出た後、ここで書かれたバージョンの互換を持ちつつ、我々自身で組み込みメソッドのようなものを書くのに使える幾つかの機能が追加されたECMAScript5がリリースされた。最新世代のブラウザはこの拡張されたバージョンのJavaScriptを提供する。2011年に'ECMAScript harmony'という、もっと根本的な言語の拡張があり、それは標準化のプロセスにある。新しいバージョンがこの本で学ぶあなたを時代遅れにすると嘆く必要はない。一つには、今行われている拡張で、この本の中に書かれているほとんどのことはそのまま残る。

* * *

この本の多くの章には実にたくさんのコードが含まれる [^1] 。私の経験では、読み書きすることがプログラムの学習での重要な位置を占める。これらの例を一見して分らなくても、注意深く読み理解すること。初めは時間がかかり混乱するかもしれないが、じきに自分のものにできるだろう。演習も同じ事だ。正しく動く回答を書けるようになるまでは理解できたとは思わないこと。

[^1] ‘コード’とはプログラムを作る要素である。プログラムの全ての部品は、単一行であろうと全体であろうと‘コード’と呼ばれる。

ウェブの仕組みとして、人々はウェブページとしてJavaScriptプログラムを見ることが可能である。それは物事がどうやってなされているか知るには良い。多くのウェブプログラマーが’プロフェッショナルな’プログラマーではないため、またはJavaScriptのプログラミング、単体テストを学んでいないため、あなたが見ることができるコードの多くはとても低い品質のものである。醜く正しくないコードから学べばあなた自身のコードにも醜さと混乱が伝染するから、何から学ぶかに注意を払うべきだ。

* * *

例とあなた自身が書いたコードの両方のプログラムを、あなたが試すことができるよう、コンソールと呼ばれるものを本書に用意している [^2] 。もしあなたがモダンなグラフィック機能を備えたブラウザ（InternetExplorer6以降、Firefox1.5以降、Opera9以降、Safari3以降）であれば、本書の各ページの下にバーが表示される。右端の矢印をクリックするとコンソールを開くことができる。

[^2] （訳注）現時点では翻訳文にコンソールおよび著者へのメッセージ送信の両機能を組み込めていません。本家のコンソールは[こちら](http://eloquentjavascript.net/paper.html) 。

コンソールはこれらの重要な要素を含む。出力ウインドウというのがあり、ここにはエラーメッセージやプログラムがプリントアウトしたものを見るのに使われる。その下にあなたがJavaScriptをタイプできる行がある。数値をタイプしエンターキーを押せばあなたがタイプしたものが実行される。何か意味のあるテキストを入力すれば出力ウインドウに表示される。ここで`wrong`とタイプしてエンターを押してみよう！出力ウインドウにはエラーメッセージが表示される。上下の矢印キーで以前タイプした命令を遡ることができる。

もっと大きな複数行にわたるようなコードをしばらくの間試したいなら、右のフィールドを使える。'Run'ボタンはこのフィールドに書かれたコードを実行するのに使う。複数のプログラムを同時に開くこともできる。'New'ボタンと'Load'ボタンを使い新しいプログラム（空あるいはウェブ上のファイル）を加えよう。1つ以上のプログラムを開いているとき、'Run'ボタンの次のメニューは表示されるものを選ぶのに使われる。'Close'ボタンは、あなたの期待通り、プログラムを閉じる。

本書のプログラム例の右上には常に小さな矢印があり、プログラムを実行するのに使う。先ほど我々が見たこの例。:

```javascript
var total = 0, count = 1;
while (count <= 10) {
    total += count;
    count += 1;
}
print(total);
```

矢印をクリックし実行しよう。他のボタンは、コンソールにこのプログラムをロードする。ためらうことなくプログラムを変更しその結果を試してみよう。最悪、無限ループを作ってしまうことがある。無限ループとは例えばカウントの変数に`1`の代わりに`0`を足したときのように、`while`の条件が永遠に`false`にならないことだ。このときプログラムは永遠に走り続ける。

幸運にも、ブラウザはその中で走っているプログラムを監視し続ける。終了までに特に長くかかるものがあれば、ブラウザはあなたに実行を中断しないかと尋ねるだろう。

* * *

後の章で、我々は多くのブロックを持つプログラム例を作るだろう。しばしば、それらの一つ一つを実行しプログラムを動かしたくなるだろう。注意していれば、コードブロックの矢印が実行後に紫色になる。章を読みながら現われ、何か新しいものを'define'する全てのコードのブロックを試していこう。（その意味は次の章で見られる）

それは、もちろん、一気に章を読みすすめることができないかもしれないということである。読み進める途中で、章の最初のコードは上手く動かないかもしれない。シフトキーを押している間、コードのブロック上の'run'矢印は、それが動く前の全てのブロックも同じように実行し、章の途中から始めた時などは、最初にコードを実行するときシフトを押せば、全て期待通りに動くだろう。

* * *

最後に、左上の隅にある小さい顔は、著者である私にメッセージを送るのに使える。もし、コメントがあったり、途方もなく道に迷ったり、スペルミスなどを見つけたときは教えて欲しい。ページから離れることなくメッセージを送れるから、あなたの読解が妨げられることもないだろう。