---
layout: default
title: A2. バイナリー・ヒープ
---

バイナリー・ヒープ
=====================================================

7章において、小さな要素を素早く見つけられるように、オブジェクトのコレクションを格納する方法としてバイナリー・ヒープを紹介した。約束したように、この付録ではこのデータ構造の背景を詳細に説明しよう。

解決する必要のあった問題をもう一度考えよう。大量の小さなオブジェクトでA*アルゴリズムは作られ、'オープン・リスト'でこれらを保持し続ける必要があった。このリストから頻繁に小さい要素を取り除いてもいた。単純なアプローチは全てのオブジェクトを1つの配列にいれ、必要に応じて最小の要素を探すことだっただろう。しかし、多くの時間がなければ、これはできない。ソートされていない配列から最小の要素を見つけるには配列全体を見て回り、それぞれの要素をチェックする必要がある。

次の解決はもちろん、配列をソートすることになるだろう。JavaScriptの配列はすばらしいsortメソッドを持っていて、大変な仕事をするのに使える。残念ながら、要素をとりのぞくごとに毎回配列全体を再ソートすることは、ソートされていない配列から最小の値を探すより大きな仕事だ。使えるトリックはある。配列全体を再ソートする代わりに、新しい値を配列の正しい場所に挿入する、そんなような。配列が最初にソートされていれば、ソートされたままになる。これは既にバイナリー・ヒープが使っているアプローチに近くなるが、しかし配列の真ん中に値を挿入することは、その場所より後の全ての要素の移動を必要とする。これではまだ遅すぎる。

もう一つのアプローチは配列を全く使わないで、連結されたオブジェクトのセットに値を格納する。この単純な形は全てのオブジェクトに値と、2つ(あるいは、より少ない)の他のオブジェクトへのリンクをを保持させるものだ。ルート（根）のオブジェクトが1つあり、最小の値を保持し、他の全てのオブジェクトへのアクセスに使われる。リンクは常により大きな値を持つオブジェクトを指し、その完全な姿はこのようなものになる。：

![tree]({{ "/assets/img/tree.png" | prepend:site.baseurl }})

このような構造は通常、ツリー（木）と呼ばれる。それは枝分かれがあるからである。今、最小の値が必要な時は、頂点の要素を取り、頂点の要素の子の1つが -- 低い方の値を持つものが -- 新しい頂点になるように、ツリーを再配置するだけだ。新しい要素を挿入する時は、新しい要素より小さいものが見つかるまでツリーを降りて、そこに挿入する。これはソートされた配列を探すより大変楽だ、しかし大量のオブジェクトを作るのが遅くなるという欠点もある。

バイナリー・ヒープは、それから、ソートされた配列として使えるが、ツリーの上側の部分だけがソートされる。オブジェクトの代わりに、この画像が示すように、配列上の位置がツリーの形に使われる。：

![heap]({{ "/assets/img/heap.png" | prepend:site.baseurl }})

配列の要素1はツリーのルートで、要素2と3がその子になる。全般には、要素Xは子として X * 2 と X * 2 + 1 を持つ。なぜこの構造が'heap'（山）と呼ばれるかわかるだろう。この配列は1から始まり、JavaScriptの配列は0から始まることに注意。ヒープは常に最小の要素が1の位置にあり、配列の位置Xにある全ての要素について、 X / 2（切り捨て）の要素の方が確実に小さい。

最小の要素を見つけることは今や1の位置にある要素を取り出すだけになった。しかし、この要素が取り除かれたら、ヒープは配列に穴が残ったままにはしておけない。これを実行するため、配列の最後の要素を取って開始地点に移動し、それから2と3の位置にある、子の要素と比較する。大きいようなら、その1つと入れ替えて、新しい位置のその要素とその子の比較のプロセスを、その子の方が大きいか、または子がいない位置に来るまで繰り返していく。

```
[2, 3, 5, 4, 8, 7, 6]
2を取って、6を先頭に持ってくる。
[6, 3, 5, 4, 8, 7]
6はその子である3より大きいので、入れ替える。
[3, 6, 5, 4, 8, 7]
今の6の子は4と8（位置4と位置5）である。
4より大きいので、再度入れ替える。
[3, 4, 5, 6, 8, 7]
6は位置4にあり、子を持たない。ヒープの順序は正しくなった。
```

同様に、要素がヒープに追加された時は、配列の最後に入れて、新しいノードより小さい親が見つかるまで、その親との入れ替え続けることによって浮かび上がってくるようにする。

```
[3, 4, 5, 6, 8, 7]
要素2を再度追加し、後ろから始める。
[3, 4, 5, 6, 8, 7, 2]
2は位置7、その親は位置3にあり、親は5である。
5は2より大きいので入れ替える。
[3, 4, 2, 6, 8, 7, 5]
位置3の親は位置1である。もう一度入れ替える。
[2, 4, 3, 6, 8, 7, 5]
位置1より前の要素はないので、これで終わり。
```

要素の追加でも挿入でも配列の全ての要素と比較する必要がないことに注意。実際、配列が大きくなるに従って、親と子との間でのジャンプは大きくなるが、この利点は多くの要素を持つ時に特に大きくなる。

ここにバイナリー・ヒープの実装の完全なコードを示す。2つ注意すべき事があって、ヒープに入った値を要素を直接比較する代わりに、関数(scoreFunction)をまずそれらに適用し、直接比較できないオブジェクトを格納することができるようにした。

また、JavaScriptの配列は0から始まり、親/子の計算は1から始まるシステムを使うので、計算がおかしくなるところを直した場所がいくつかある。

```javascript
function BinaryHeap(scoreFunction){
  this.content = [];
  this.scoreFunction = scoreFunction;
}

BinaryHeap.prototype = {
  push: function(element) {
    // Add the new element to the end of the array.
    this.content.push(element);
    // Allow it to bubble up.
    this.bubbleUp(this.content.length - 1);
  },

  pop: function() {
    // Store the first element so we can return it later.
    var result = this.content[0];
    // Get the element at the end of the array.
    var end = this.content.pop();
    // If there are any elements left, put the end element at the
    // start, and let it sink down.
    if (this.content.length > 0) {
      this.content[0] = end;
      this.sinkDown(0);
    }
    return result;
  },

  remove: function(node) {
    var len = this.content.length;
    // To remove a value, we must search through the array to find
    // it.
    for (var i = 0; i < len; i++) {
      if (this.content[i] == node) {
        // When it is found, the process seen in 'pop' is repeated
        // to fill up the hole.
        var end = this.content.pop();
        if (i != len - 1) {
          this.content[i] = end;
          if (this.scoreFunction(end) < this.scoreFunction(node))
            this.bubbleUp(i);
          else
            this.sinkDown(i);
        }
        return;
      }
    }
    throw new Error("Node not found.");
  },

  size: function() {
    return this.content.length;
  },

  bubbleUp: function(n) {
    // Fetch the element that has to be moved.
    var element = this.content[n];
    // When at 0, an element can not go up any further.
    while (n > 0) {
      // Compute the parent element's index, and fetch it.
      var parentN = Math.floor((n + 1) / 2) - 1,
          parent = this.content[parentN];
      // Swap the elements if the parent is greater.
      if (this.scoreFunction(element) < this.scoreFunction(parent)) {
        this.content[parentN] = element;
        this.content[n] = parent;
        // Update 'n' to continue at the new position.
        n = parentN;
      }
      // Found a parent that is less, no need to move it further.
      else {
        break;
      }
    }
  },

  sinkDown: function(n) {
    // Look up the target element and its score.
    var length = this.content.length,
        element = this.content[n],
        elemScore = this.scoreFunction(element);

    while(true) {
      // Compute the indices of the child elements.
      var child2N = (n + 1) * 2, child1N = child2N - 1;
      // This is used to store the new position of the element,
      // if any.
      var swap = null;
      // If the first child exists (is inside the array)...
      if (child1N < length) {
        // Look it up and compute its score.
        var child1 = this.content[child1N],
            child1Score = this.scoreFunction(child1);
        // If the score is less than our element's, we need to swap.
        if (child1Score < elemScore)
          swap = child1N;
      }
      // Do the same checks for the other child.
      if (child2N < length) {
        var child2 = this.content[child2N],
            child2Score = this.scoreFunction(child2);
        if (child2Score < (swap == null ? elemScore : child1Score))
          swap = child2N;
      }

      // If the element needs to be moved, swap it, and continue.
      if (swap != null) {
        this.content[n] = this.content[swap];
        this.content[swap] = element;
        n = swap;
      }
      // Otherwise, we are done.
      else {
        break;
      }
    }
  }
};
```

そして単純なテストは...

```javascript
var heap = new BinaryHeap(function(x){return x;});
forEach([10, 3, 4, 8, 2, 9, 7, 1, 2, 6, 5],
        method(heap, "push"));

heap.remove(2);
while (heap.size() > 0)
  print(heap.pop());
```
