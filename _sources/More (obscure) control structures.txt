=====================================================
More (obscure) control structures
=====================================================
..  Appendix 1:
..  More (obscure) control structures

..  In chapter 2, a number of control statements were introduced, such as while, for, and break. To keep things simple, I left out some others, which, in my experience, are a lot less useful. This appendix briefly describes these missing control statements.

2章では、while、for、breakのようないくつかの制御文を紹介した。物事をシンプルに保つために、その他の、私個人の経験上で、あまり有益でなかったものは捨て置いた。この付録では簡潔にこれら残りの制御文を説明しよう。

..  First, there is do. do works like while, but instead of executing the loop body zero or more times, it executes it one or more times. A do loop looks like this:

1つめに、doがある。doはwhileのように動くが、ループの本体を0回ないしより多く実行する代わりに、1回ないしより多く実行する。doループはこのようになる。：

.. code-block:: javascript

    do {
      var answer = prompt("Say 'moo'.", "");
      print("You said '", answer, "'.");
    } while (answer != "moo");

..  To emphasise the fact that the condition is only checked after the loop has run once, it is written at the end of the loop's body.

条件はループが1度実行された後でのみチェックされることを強調するため、ループの本体の後に書かれている。

..  Next, there is continue. This one is closely related to break, and can be used in the same places. While break jumps out of a loop and causes the program to proceed after the loop, continue jumps to the next iteration of the loop.

次に、continueがある。これはbreakに密接な関係があり、ある場所で使われる。breakはループから抜け出しプログラムの実行をループの後に進めるが、continueはループの次の繰り返しにジャンプする。

.. code-block:: javascript

    for (var i = 0; i < 10; i++) {
      if (i % 3 != 0)
        continue;
      print(i, " is divisible by three.");
    }

..  A similar effect can usually be produced using just if, but there are cases where continue looks nicer.

同様の効果は、通常はifを使うことで得られるが、continueの方が良さそうな場合もある。

..  When there is a loop sitting inside another loop, a break or continue statement will affect only the inner loop. Sometimes you want to jump out of the outer loop. To be able to refer to a specific loop, loop statements can be labelled. A label is a name (any valid variable name will do), followed by a colon (:).

あるループがもう1つのループの内側にあるとき、break文やcontinue文は内側のループにのみ作用する。時には外側のループに脱出したいと思うこともあるだろう。個々のループを参照するためには、ループ文にラベルを付けることができる。ラベルは名前（変数名として妥当な名前）で、コロン（:）がその後に続く。

.. code-block:: javascript

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

..  Next, there is a construct called switch which can be used to choose which code to execute based on some value. This is a very useful thing to do, but the syntax JavaScript uses for this (which it took from the C programming language) is so clumsy and ugly that I usually prefer to use a chain of if statements instead.

次に、ある値に従って、実行するコードを選択することに使われるswitchという構造がある。とても便利だが、これに使われるJavaScriptの文法（C言語から取られた）はややこしくて醜いので、私は通常、代わりにif文を連鎖させることを好んでいる。

.. code-block:: javascript

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

..  Inside the block opened by switch, you can write a number of case labels. The program will jump to the label that corresponds to the value that switch was given, or to default if no matching value is found. Then it start executing statements there, and continues past other labels, until it reaches a break statement. In some cases, such as the "sunny" case in the example, this can be used to share some code between cases (it recommends going outside for both sunny and cloudy weather). Most of the time, this just adds a lot of ugly break statements, or causes problems when you forget to add one.

switchで開かれたブロックの内側には、caseのラベルをいくつも書ける。プログラムはswitchで与えられた値に結びついたラベルか、あるいはマッチする値がなければdefaultにジャンプする。それから、そこにある文を実行が始まり、以降の他のラベルに、break文にたどり着くまで続ける。例えば"sunny"のような、ある場合には、caseの間でコードを共有することもできる（晴れと曇りの両方の天気で、外出を勧めている）。多くの場合、これは多くの醜いbreak文の追加か、あるいはその追加を忘れることで問題を起こすことになる。

..  Like loops, switch statements can be given a label.

ループと似て、switch文にはラベルを付けることができる。

..  Finally, there is a keyword named with. I've never actually used this in a real program, but I have seen other people use it, so it is useful to know what it is. Code using with looks like this:

最後に、withという名のキーワードがある。私は本物のプログラムの中で実際にこれを使ったことがないが、しかし他の人が使っているのを見た事はあり、これが何なのか知っていることは有益だ。withを使ったコードはこのようになる。：

.. code-block:: javascript

    var scope = "outside";
    var object = {name: "Ignatius", scope: "inside"};
    with(object) {
      print("Name == ", name, ", scope == ", scope);
      name = "Raoul";
      var newVariable = 49;
    }
    show(object.name);
    show(newVariable);

..  Inside the block, the properties of the object given to with act as variables. Newly introduced variables are not added as properties to this object though. I assume the idea behind this construct was that it could be useful in methods that make lots of use of the properties of their object. You could start such a method with with(this) {...}, and not have to write this all the time after that.

ブロックの中で、withに与えられたオブジェクトのプロパティは変数のように動く。新しく導入された変数はプロパティのようには、このオブジェクトに追加されない。この構造の背景にあるアイデアは、このオブジェクトのたくさんのプロパティを使うメソッドでは有益かもしれないと仮定する。そのようなメソッドをwith(this) {...}で始められ、そうすればthisをその後の全ての場所にいちいち書かなくても良くなる。
