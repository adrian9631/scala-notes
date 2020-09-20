# scala-notes

### 宝藏资料 ( 学习timeline: 2020.9.12 -- 神秘时间 )  
- scala入门 http://twitter.github.io/scala_school/zh_cn/

### 语法区别点 ( 和 python 稍微对比 )  
- 循环 （ 枚举用 to 或者 until，<- 代表 in，循环 mainbody 必须加 {} ） 
```python3
# python3
for i in range(10):
    print(i) # mainbody
```

```scala
// scala
for (i <- 0 to 9){
    println(i) // mainbody
}
// or 
for (i <- 0 until 10){
    println(i) // mainbody
}
```
- 声明 （ 注意 val 和 var 关键字区别, 指定类型后置且可选 ）
```scala
// scala
val i = 10 // immutable variable
var i = 10 // mutable variable
val i:Int = 10 // type:Int
var i:Double = 10 // type:Double, output 10.0
```
- 函数声明 ( 注意函数参数列表必须声明类型，返回类型则可选，return 可隐藏 )
```python3
# python3
def example_method(a, b):
    return a + b
```

```scala
// scala
def example_method(a:Int, b:Int):Int = {
    a+b
}
```
- 数组声明和使用
```
```

- 递归 (注意 tail recursion 是利用积累参数到底直接返回， 普通 recursion 需要连续出栈到最开始那层)
```scala
//Example 1
// recursion way
def logTen(n: Int): Int = {
    if (n/10 < 1) { 0 }
    else {logTen(n/10) + 1} 
}
// tail recursion way
import scala.annotation.tailrec
def logTenTail(n: Int): Int = { 
    @tailrec def Helper(n: Int, acc: Int): Int = {
    if (n/10 < 1) {acc}
    else {Helper(n/10, acc+1)}
    }
    Helper(n, 0)
}

//Example 2
// recursion way
def shuffle(s: String): String = {
    val n = s.length()
    if (n <= 1) { s }
    else {
        val secondHalf = s.substring(n/2, n)
        val shuffledHalf = shuffle(secondHalf)
        val firstHalf = s.substring(0, n/2)
        return  shuffledHalf +  firstHalf 
    }
}

// tail recursion way
def tailRecursiveShuffle(s: String, acc: String = ""): String = {
    val n = s.length()
    if (s.length() == 1) { return s + acc}
    else {
        val firstHalf = s.substring(0, n/2)
        val secondHalf = s.substring(n/2, n)
        tailRecursiveShuffle(secondHalf, firstHalf + acc)    
    }
}
```

- 函数式编程 
( 这里三个概念，一注意 case class 样本类特别用于模式匹配；二注意 trait 和 extends；三注意 match case 模式匹配写法类似于 C 中的 switch )  
```scala
sealed trait Regex

// Rule 
case class Toregex(s: String) extends Regex
case class Concat(a: Regex, b: Regex) extends Regex
case class Or(a: Regex, b: Regex) extends Regex
case class And(a: Regex, b: Regex) extends Regex

// Finalfunction
val finalfunction = {
    Concat( Or(Toregex("Hello"),Toregex("World")), And(Toregex("Function"),Toregex("Programmming")) ) 
}

// Definition 
def munge(r: Regex): String = r match {
    case Toregex(st) => "|start|" + st
    case Concat(r1, r2) => munge(r1) + "|concat|" + munge(r2)
    case Or(r1, r2) => munge(r1) + "|Or|" + munge(r2)
    case And(r1, r2) => munge(r1) + "|And|" + munge(r2)
}

munge(finalfunction)
```
- tips: To trait, or not to trait?  
 https://www.artima.com/pins1ed/traits.html#12.7    
 https://stackoverflow.com/questions/1991042/what-is-the-advantage-of-using-abstract-classes-instead-of-traits  
 
- 柯西函数化 (注意 “_” 通配符, 可先应用部分参数，剩下用通配符进行参数获取)
```scala
def multiply(m: Int)(n: Int): Int = m * n
//output: multiply: (m: Int)(n: Int)Int

multiply(2)(3)
//output: res0: Int = 6

val timesTwo = multiply(2) _
//output: timesTwo: (Int) => Int = <function1>
timesTwo(3)
//output: res1: Int = 6

```

- recursion & match case (必须考虑所有匹配的情况，可在复杂参数前加 xxx@ 将参数重新命名为 xxx)

定义
```scala
sealed trait NList
case object Nil extends NList
case class Cons(n: Int, l: NList) extends NList 
```

返回第N个元素
```scala
def getNthElement(lst: NList, n: Int): Int = {
    lst match {
        case Nil => throw new IllegalArgumentException("Nil")
        case _ if n < 0 => throw new IllegalArgumentException("n < 0")
        case Cons(n1, Nil) if n != 0 => throw new IllegalArgumentException("n >= length of list")
        case Cons(n1, _) if n == 0 => n1
        case Cons(n1, next@Cons(n2, _)) => getNthElement(next, n-1)
    }
}
```

判断是否为Fibonacci数列
```scala
def isFibonacci(lst: NList): Boolean = { 
    lst match {
        case Cons(i, Cons(j, Nil)) => true
        case Cons(i, next@Cons(j, Cons(k, _))) if i+j == k => isFibonacci(next)
        case Cons(i, Cons(j, Cons(k, _))) if i+j != k => false
    }
}
```

用函数f对数列进行筛选，符合保留，不符合剔除
```scala
def filterNumList(lst: NList, f:Int => Boolean): NumList = { 
    lst match {
        case Nil => Nil
        case Cons(n1, Nil) if f(n1) => Cons(n1, Nil)
        case Cons(n1, Nil) if !f(n1) => Nil
        case Cons(n1, next@Cons(n2, _)) if f(n1) => Cons(n1, filterNumList(next, f))
        case Cons(n1, next@Cons(n2, _)) if !f(n1) => filterNumList(next, f)
    }
}
```
