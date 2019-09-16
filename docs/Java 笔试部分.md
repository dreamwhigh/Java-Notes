# 笔试

## 关键字

### static

static用来修饰类或类的成员，这时不需要创建实例就能访问（而且不能实例化），在被调用的时候自动实例化，且在内存中产生一个实例。当含有静态成员的非静态类实例化出对象后，这些对象公用这些静态成员，通过类名或对象名都能访问它们。



## 继承

### 定义

抽象方法没有方法体，只有声明。

```
abstract class xy
{
    abstract sum (int x, int y) { }//false
    abstract sum (int x, int y)；//true
}
```



## 异常

<div align="center">
    <img src="<div align="center">
    <img src="https://github.com/dreamwhigh/Java-Notes/blob/master/docs/pics/异常分类.png?raw=true" width="400px">
</div>
</div>





1. 粉红色的是受检查的异常(checked exceptions),其必须被 try{}catch语句块所捕获,或者在方法签名里通过throws子句声明.受检查的异常必须在编译时被捕捉处理,命名为 Checked Exception 是因为Java编译器要进行检查,Java虚拟机也要进行检查,以确保这个规则得到遵守.

2. 绿色的异常是运行时异常(runtime exceptions),需要程序员自己分析代码决定是否捕获和处理,比如 空指针,被0除...

3. 而声明为Error的，则属于严重错误，如系统崩溃、虚拟机错误、动态链接失败等，这些错误无法恢复或者不可能捕捉，将导致应用程序中断，Error不需要捕捉