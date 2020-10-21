# C#学习

### Class类补充

[TOC]

#### 常量

- 一个值不可以改变的静态字段

- 在编译时值就已经确定下来了

- 任何使用常量的地方，编译器都会把这个常量替换为它的值

- 使用const关键字声明，声明的同时必须使用具体的值来对其初始化 

```csharp
public class Test
{
    public const string Message = "Hello World";
}
```

##### 常量与静态只读字段

常量比静态只读字段更严格，体现在下面两个方面：

- 可以使用的类型，常量更少一点
- 字段初始化语义上，常量必须定义时初始化

常量的值是在进行编译的时候进行估算的，编译时就确定了

下面的例子中演示了常量的编译过程

```csharp
public static double Circumference(double radius)
{
    return 2 * System.Math.PI * radius;
}
```

编译后：

```csharp
public static double Circumference(double radius)
{
    return 6.2831853071795862 * radius;
}
```

注意：

当值有可能改变，并且需要暴露给其他Assembly的时候，静态只读字段是相对比较好的选择

```csharp
public const decimal ProgramVersion = 2.3;
```

如果Y Assembly 引用了X  Assembly并且使用了这个常量，那么编译的时候，2.3这个值就会固化在 Y Assembly里。这意味着如果后来X重新编译，这个常量变成了2.4，如果Y不重新编译的话，Y将仍旧使用2.3这个值，直到Y被重新编译，它的值才会变成2.4。静态只读字段可以避免这个问题的发生。

##### 本地常量

方法里可以有本地的常量

```csharp
static void Main()
{
    const double twoPI = 2 * System.Math.PI；👈
}
```

#### 静态构造函数

- 静态构造函数，每个**类型**执行一次
- 非静态构造函数，每个**实例**执行一次
- **一个类型只能定义一个静态构造函数**，且有以下条件：
  - 必须无参
  - 方法名与函数名一致

```csharp
class Test
{
    static Test(){ Console.Writeline("Type Initialized");}
}
```

- 在类型使用之前的一瞬间，编译器会自动调用类型的静态构造函数：
  - 实例化一个类型
  - 访问类型的一个静态成员

- 只允许使用unsafe 和 extern 修饰符

  

  注意：

  如果静态构造函数抛出了未处理异常，那么这个类型在该程序的剩余生命周期内将无法使用

##### 初始化顺序

- 静态字段的初始化器在静态构造函数被执行之前的一瞬间运行

- 如果一个类没有静态构造函数，那么静态字段初始化器在类型被使用之前的一瞬间执行，或者更早，在运行时突然进行

- 静态字段初始化顺序和他们声明的顺序一致

```csharp
class Foo
{
    public static int x = y;//0
    public static int y = 3;//3
}
```

```csharp
class program
{
    static void main(){ Console.Writeline(Foo.X);}//3(静态字段初始化在类型使用前)
}
class Foo
{
    public static Foo Instance = new Foo();
    public static int X = 3;
    foo(){ Console.Writeline(X);}//0(第一个字段调用，第二个字段才赋值)
}
```



##### 静态类

- 类也可以是静态的

- 其成员必须全是静态的

- 不可以有子类

例如：System.Console,System.Math



#### Finalizer终结器（析构函数）

- finalizer是Class专有的一种方法

- 在GC回收未引用对象的内存之前运行

- 其实就是**对object的Finalizer()方法重写**的一种语法

```csharp
class Class1
{
    ~class1()👈
    {
        ···
    }
}
```

expressioon-bodied形式

```csharp
 ~class1() => Console.Writeline("Finalizer");
```



#### Partial type局部类型

允许一个类的定义分布在多个地方（文件）

经典应用：一个类的一部分是自动生成的，另一部分需要手写代码

- 每个分布类都必须使用partial来声明

- 下面例子就会报错

  ```csharp
  partial class PaymentForm{}
  class PaymentForm{}
  ```

- 每个分布类的成员不能冲突，不能有同样参数的构造函数

- 各分布类完全靠编译器进行解析：每个分布类在编译时必须可用，且在同一个Assembly里

- 如果有父类，可以在一个或多个分布类上指明，但必须一致

- 每个分布类可以独立的实现不同的接口

- 编译器无法保证各个分布类的字段的初始化顺序

  

#### Partial method局部方法

partial类型可以有partial method

自动生成的分布类里可以有partial method，通常作为“钩子”使用，在另一部分的partial method里，我们可以对这个方法进行自定义。

```csharp
partial class PaymentForm //in auto-gengrated file
{
    ···
    partial void ValidatePayment(decimal amount);👈
}

partial class PaymentForm//in hand-authored fle
{
    ···
    partial void ValidatePayment(decimal amount)👈
    {
        if (amount > 100)//自定义函数体
            ···    
    }
}
```

partial Method由两部分构成：定义和实现

- 定义部分通常是生成的
- 实现部分通常是手动编写的
- 如果partial Method只有定义，没有实现，那么编译的时候该方法定义就没有了，调用该方法的代码也没有了。这就允许自动生成的代码可以自由的提供钩子，不用担心代码**膨胀**[^1]
- partial Method必须是void，并且隐式private的

#### Nameof操作符

nameof操作符会返回任何符号（类型，成员，变量……）的名字（string）

优点是有利于重构

```csharp
int count = 123;
string name = nameof (count);//name is "count"
```



1 代码膨胀是什么 