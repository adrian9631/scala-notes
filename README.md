# scala-notes

语法区别点 ( 和 python 稍微对比 )  
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
```scala
```
