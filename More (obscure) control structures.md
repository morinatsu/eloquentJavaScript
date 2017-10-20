---
layout: default
title: A1. より多くの制御構造
---

より多くの(紛らわしい)制御構造
=====================================================

2章では、while、for、breakのようないくつかの制御文を紹介した。物事をシンプルに保つために、その他の、私個人の経験上で、あまり有益でなかったものは捨て置いた。この付録では簡潔にこれら残りの制御文を説明しよう。

1つめに、doがある。doはwhileのように動くが、ループの本体を0回ないしより多く実行する代わりに、1回ないしより多く実行する。doループはこのようになる。：

```javascript
do {
  var answer = prompt("Say 'moo'.", "");
  print("You said '", answer, "'.");
} while (answer != "moo");
```

条件はループが1度実行された後でのみチェックされることを強調するため、ループの本体の後に書かれている。

次に、continueがある。これはbreakに密接な関係があり、ある場所で使われる。breakはループから抜け出しプログラムの実行をループの後に進めるが、continueはループの次の繰り返しにジャンプする。

```javascript
for (var i = 0; i < 10; i++) {
  if (i % 3 != 0)
    continue;
  print(i, " is divisible by three.");
}
```

同様の効果は、通常はifを使うことで得られるが、continueの方が良さそうな場合もある。

あるループがもう1つのループの内側にあるとき、break文やcontinue文は内側のループにのみ作用する。時には外側のループに脱出したいと思うこともあるだろう。個々のループを参照するためには、ループ文にラベルを付けることができる。ラベルは名前（変数名として妥当な名前）で、コロン（:）がその後に続く。

```javascript
outer: for (var sideA = 1; sideA < 10; sideA++) {
  inner: for (var sideB = 1; sideB < 10; sideB++) {
    var hypotenuse = Math.sqrt(sideA * sideA + sideB * sideB);
    if (hypotenuse % 1 == 0) {
      print("A right triangle with straight sides of length ",
            sideA, " and ", sideB, " has a hypotenuse of ",
            hypotenuse, ".");
      break outer;
    }
  }
}
```

次に、ある値に従って、実行するコードを選択することに使われるswitchという構造がある。とても便利だが、これに使われるJavaScriptの文法（C言語から取られた）はややこしくて醜いので、私は通常、代わりにif文を連鎖させることを好んでいる。

```javascript
function weatherAdvice(weather) {
  switch(weather) {
    case "rainy":
      print("Remember to bring an umbrella.");
      break;
    case "sunny":
      print("Dress lightly.");
    case "cloudy":
      print("Go outside.");
      break;
    default:
      print("Unknown weather type: ", weather);
      break;
  }
}

weatherAdvice("sunny");
```

switchで開かれたブロックの内側には、caseのラベルをいくつも書ける。プログラムはswitchで与えられた値に結びついたラベルか、あるいはマッチする値がなければdefaultにジャンプする。それから、そこにある文を実行が始まり、以降の他のラベルに、break文にたどり着くまで続ける。例えば"sunny"のような、ある場合には、caseの間でコードを共有することもできる（晴れと曇りの両方の天気で、外出を勧めている）。多くの場合、これは多くの醜いbreak文の追加か、あるいはその追加を忘れることで問題を起こすことになる。

ループと似て、switch文にはラベルを付けることができる。

最後に、withという名のキーワードがある。私は本物のプログラムの中で実際にこれを使ったことがないが、しかし他の人が使っているのを見た事はあり、これが何なのか知っていることは有益だ。withを使ったコードはこのようになる。：

```javascript
var scope = "outside";
var object = {name: "Ignatius", scope: "inside"};
with(object) {
  print("Name == ", name, ", scope == ", scope);
  name = "Raoul";
  var newVariable = 49;
}
show(object.name);
show(newVariable);
```

ブロックの中で、withに与えられたオブジェクトのプロパティは変数のように動く。新しく導入された変数はプロパティのようには、このオブジェクトに追加されない。この構造の背景にあるアイデアは、このオブジェクトのたくさんのプロパティを使うメソッドでは有益かもしれないと仮定する。そのようなメソッドをwith(this) {...}で始められ、そうすればthisをその後の全ての場所にいちいち書かなくても良くなる。
