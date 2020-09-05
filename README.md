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
- 函数声明 (注意函数参数列表必须声明类型，返回类型则可选，return可隐藏)
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


