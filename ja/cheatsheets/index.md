---
layout: cheatsheet
istranslation: true
title: Scalacheat
by: Kenji Ohtsuka
about: Thanks to <a href="http://brenocon.com/">Brendan O'Connor</a>. このチートシートは Scala 構文 のクイックリファレンスとして作成されました。 Licensed by Brendan O'Connor under a CC-BY-SA 3.0 license.
language: ja
---

###### Contributed by {{ page.by }}

|                                                                                                          |                 |
| ------                                                                                                   | ------          |
|  <span id="variables" class="h2">変数</span>                                                             |                 |
|  `var x = 5`                                                                                             |  変数           |
|  <span class="label success">Good</span> `val x = 5`<br> <span class="label important">Bad</span> `x=6`  |  定数           |
|  `var x: Double = 5`                                                                                     |  明示的な型     |
|  <span id="functions" class="h2">関数</span>                                                             |                 |
|  <span class="label success">Good</span> `def f(x: Int) = { x*x }`<br> <span class="label important">Bad</span> `def f(x: Int)   { x*x }` |  関数定義<br> 隠れたエラー: = を書かないと Unit を返す処理になります。; 大惨事の原因になります。 |
|  <span class="label success">Good</span> `def f(x: Any) = println(x)`<br> <span class="label important">Bad</span> `def f(x) = println(x)` |  関数定義 <br> シンタックスエラー: すべての引数に型指定が必要です。 |
|  `type R = Double`                                                                                       |  型エイリアス   |
|  `def f(x: R)` vs.<br> `def f(x: => R)`                                                                  |  値による呼び出し <br> 名前による呼び出し(遅延評価パラメータ) |
|  `(x:R) => x*x`                                                                                          |  無名関数                                                      |
|  `(1 to 5).map(_*2)` vs.<br> `(1 to 5).reduceLeft( _+_ )`                                                |  無名関数: アンダースコアは位置に応じて引数が代入されます。   |
|  `(1 to 5).map( x => x*x )`                                                                              |  無名関数: 引数を2回使用する場合は名前をつけます。             |
|  <span class="label success">Good</span> `(1 to 5).map(2*)`<br> <span class="label important">Bad</span> `(1 to 5).map(*2)` |  無名関数: 演算子の右側を省略する方法。 わかりやすく書きたいなら `2*_` と書くのがよいでしょう。 |
|  `(1 to 5).map { val x=_*2; println(x); x }`                                                             |  無名関数: ブロックスタイルでは最後の式の結果が戻り値になります。 |
|  `(1 to 5) filter {_%2 == 0} map {_*2}`                                                                  |  無名関数: パイプラインスタイル (括弧でも同様) 。                 |
|  `def compose(g:R=>R, h:R=>R) = (x:R) => g(h(x))` <br> `val f = compose({_*2}, {_-1})`                   |  無名関数: 複数のブロックを渡す場合は外側の括弧が必要です。 |
|  `val zscore = (mean:R, sd:R) => (x:R) => (x-mean)/sd`                                                   |  カリー化の明示的記法       |
|  `def zscore(mean:R, sd:R) = (x:R) => (x-mean)/sd`                                                       |  カリー化の明示的記法       |
|  `def zscore(mean:R, sd:R)(x:R) = (x-mean)/sd`                                                           |  カリー化の簡易記法、ただし |
|  `val normer = zscore(7, 0.4)_`                                                                          |  簡易記法を使用する場合、部分関数を取得するには末尾にアンダースコアが必要です。 |
|  `def mapmake[T](g:T=>T)(seq: List[T]) = seq.map(g)`                                                     |  ジェネリック型                     |
|  `5.+(3); 5 + 3` <br> `(1 to 5) map (_*2)`                                                               |  中置記法                           |
|  `def sum(args: Int*) = args.reduceLeft(_+_)`                                                            |  可変長引数                         |
|  <span id="packages" class="h2">パッケージ</span>                                                        |                                     |
|  `import scala.collection._`                                                                             |  ワイルドカードでインポートします。 |
|  `import scala.collection.Vector` <br> `import scala.collection.{Vector, Sequence}`                      |  選択してインポートします。         |
|  `import scala.collection.{Vector => Vec28}`                                                             |  別名でインポートします。           |
|  `import java.util.{Date => _, _}`                                                                       |  Date を除いて java.util のすべてをインポートします。 |
|  `package pkg` _at start of file_ <br> `package pkg { ... }`                                             |  パッケージ宣言                     |
|  <span id="data_structures" class="h2">データ構造</span>                                                 |                                     |
|  `(1,2,3)`                                                                                               |  タプルリテラル (`Tuple3`)          |
|  `var (x,y,z) = (1,2,3)`                                                                                 |  構造化代入: タプルはパターンマッチにより展開して代入されます。 |
|  <span class="label important">Bad</span>`var x,y,z = (1,2,3)`                                           |  隠れたエラー: 各変数にタプル全体が代入されます。               |
|  `var xs = List(1,2,3)`                                                                                  |  リスト (イミュータブル)            |
|  `xs(2)`                                                                                                 |  括弧を使って添字を書きます。 ([slides](http://www.slideshare.net/Odersky/fosdem-2009-1013261/27)) |
|  `1 :: List(2,3)`                                                                                        |  先頭に要素を追加             |
|  `1 to 5` _次と同じ_ `1 until 6` <br> `1 to 10 by 2`                                                     |  Range の簡易記法             |
|  `()` _(中身のない括弧)_                                                                                 |  Unit 型 の唯一のメンバ (C/Java でいう void) 。 |
|  <span id="control_constructs" class="h2">制御構造</span>                                                |                               |
|  `if (check) happy else sad`                                                                             |  条件分岐                     |
|  `if (check) happy` _次と同じ_ <br> `if (check) happy else ()`                                           |  条件分岐の省略形             |
|  `while (x < 5) { println(x); x += 1}`                                                                   |  while ループ                 |
|  `do { println(x); x += 1} while (x < 5)`                                                                |  do while ループ              |
|  `import scala.util.control.Breaks._`<br>`breakable {`<br>`    for (x <- xs) {`<br>`        if (Math.random < 0.1) break`<br>`    }`<br>`}`|  break ([slides](http://www.slideshare.net/Odersky/fosdem-2009-1013261/21)) |
|  `for (x <- xs if x%2 == 0) yield x*10` _次と同じ_ <br>`xs.filter(_%2 == 0).map(_*10)`                   |  for 内包表記: filter/map             |
|  `for ((x,y) <- xs zip ys) yield x*y` _次と同じ_ <br>`(xs zip ys) map { case (x,y) => x*y }`             |  for 内包表記: 構造化代入             |
|  `for (x <- xs; y <- ys) yield x*y` _次と同じ_ <br>`xs flatMap {x => ys map {y => x*y}}`                 |  for 内包表記: 直積                   |
|  `for (x <- xs; y <- ys) {`<br>    `println("%d/%d = %.1f".format(x,y, x*y))`<br>`}`                     |  for 内包表記: 命令型の記述<br>[sprintf-style](http://java.sun.com/javase/6/docs/api/java/util/Formatter.html#syntax) |
|  `for (i <- 1 to 5) {`<br>    `println(i)`<br>`}`                                                        |  for 内包表記: 上限を含んだイテレート |
|  `for (i <- 1 until 5) {`<br>    `println(i)`<br>`}`                                                     |  for 内包表記: 上限を除いたイテレート |
|  <span id="pattern_matching" class="h2">パターンマッチング</span>                                        |                                       |
|  <span class="label success">Good</span> `(xs zip ys) map { case (x,y) => x*y }`<br> <span class="label important">Bad</span> `(xs zip ys) map( (x,y) => x*y )` |  case をパターンマッチのために関数の引数で使っています。 |
|  <span class="label important">Bad</span><br>`val v42 = 42`<br>`Some(3) match {`<br>`  case Some(v42) => println("42")`<br>`    case _ => println("Not 42")`<br>`}` |  "v42" は任意の Int の値とマッチする変数名として解釈され、 "42" が表示されます。 |
|  <span class="label success">Good</span><br>`val v42 = 42`<br>`Some(3) match {`<br>``    case Some(`v42`) => println("42")``<br>`case _ => println("Not 42")`<br>`}`  | バッククオートで囲んだ "\`v42\`" は既に存在する `v42` として解釈され、 "Not 42" が表示されます。 |
|  <span class="label success">Good</span><br>`val UppercaseVal = 42`<br>`Some(3) match {`<br>`  case Some(UppercaseVal) => println("42")`<br>`    case _ => println("Not 42")`<br>`}` |  大文字から始まる `UppercaseVal` は既に存在する定数として解釈され、新しい変数としては扱われません。 これにより `UppercaseVal` は `3` とは異なる値と判断され、 "Not 42" が表示されます。 |
|  <span id="object_orientation" class="h2">オブジェクト指向</span>                                        |                                 |
|  `class C(x: R)` _次と同じ_ <br>`class C(private val x: R)`<br>`var c = new C(4)`                         |  コンストラクタの引数 - private |
|  `class C(val x: R)`<br>`var c = new C(4)`<br>`c.x`                                                      |  コンストラクタの引数 - public  |
|  `class C(var x: R) {`<br>`assert(x > 0, "positive please")`<br>`var y = x`<br>`val readonly = 5`<br>`private var secret = 1`<br>`def this = this(42)`<br>`}`|<br>コンストラクタはクラスの body 部分 です。<br>public メンバ の宣言<br>読取可能・書込不可なメンバの宣言<br>private メンバ の宣言<br>代替コンストラクタ |
|  `new{ ... }`                                                                                            |  無名クラス                     |
|  `abstract class D { ... }`                                                                              |  抽象クラスの定義 (生成不可)    |
|  `class C extends D { ... }`                                                                             |  継承クラスの定義               |
|  `class D(var x: R)`<br>`class C(x: R) extends D(x)`                                                     |  継承とコンストラクタのパラメータ (要望: 自動的にパラメータを引き継げるようになってほしい)
|  `object O extends D { ... }`                                                                            |  シングルトンオブジェクトの定義 (モジュールライク) |
|  `trait T { ... }`<br>`class C extends T { ... }`<br>`class C extends D with T { ... }`                  |  トレイト<br>実装を持ったインターフェースで、コンストラクタのパラメータを持つことができません。 [mixin-able]({{ site.baseurl }}/tutorials/tour/mixin-class-composition.html).
|  `trait T1; trait T2`<br>`class C extends T1 with T2`<br>`class C extends D with T1 with T2`             |  複数のトレイトを組み合わせられます。              |
|  `class C extends D { override def f = ...}`	                                                           |  override をつけてオーバライドします。             |
|  `new java.io.File("f")`                   	                                                             |  オブジェクトの作成                                |
|  <span class="label important">Bad</span> `new List[Int]`<br> <span class="label success">Good</span> `List(1,2,3)` |  型のエラー: 抽象型のオブジェクトは生成できません。<br>習慣的に型を書かない方法をとります。 |
|  `classOf[String]`                                                                                       |  クラスの情報取得                                  |
|  `x.isInstanceOf[String]`                                                                                |  型のチェック (実行時)                             |
|  `x.asInstanceOf[String]`                                                                                |  型のキャスト (実行時)                             |
|  `x: String`                                                                                             |  型帰属 (コンパイル時)                         |
