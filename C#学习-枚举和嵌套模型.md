# C#学习



## 枚举与嵌套类型

[toc]

### 枚举

#### 什么是枚举

枚举是一个特殊的**值类型**，它可以让你指定一组命名的数值常量

```csharp
public enum BorderSide {Left,Right,Top,Buttom}
BorderSide topSide = BorderSide.top;
bool isTop = (topSide == BorderSide.top);  //True
```

#### 枚举的底层原理

每个枚举都对应一个底层的**整型数值**(Enum.GetUnderlyingType())，默认情况下：

- 是**int类型**
- 0，1，2……==会按照枚举成员的声明顺序自动赋值==

也可以**指定其他类型**作为枚举的整数类型，例如byte：

```csharp
public enum BorderSide : byte { Left,Right,Top,Bottom}
```

也可以单独指定枚举成员对应的整数值

```csharp
public enum BorderSide : byte { Left=1,Right=2,Top=3,Bottom=4}
```

也可以只指定其中某些成员的数值，未被赋值的成员将接着它前面已赋值成员的值递增

#### 枚举的转换

枚举的值可以和其底层的数值相互转换

```csharp
int i = (int) BorderSide.Left;
BorderSide side = (BorderSide) i;
bool LeftOrRight = (int)side <= 2;
```



#### 0

在枚举表达式里，0数值会被编译器特殊对待，它不需要显式的转换

```csharp
BorderSide b = 0;
if (b == 0) ···
```

因为枚举的第一个成员通常被当作“默认值”，它的默认值就是0

组合枚举里，0没有标志（flags）



#### 组合枚举(Flags enum)

可以对枚举成员进行组合

为了避免歧义，枚举成员的需要显式地赋值，典型的就是使用2的乘幂

```csharp
[flags]
public enum BorderSides { None=0,Left=1,Right=2,Top=4,bottom=8 }
```

Flags enum**可以使用位操作符**，例如|和&

```csharp
BorderSides leftRight = BorderSides.left | BorderSides.Right;

if ((leftRight & BorderSides.left) != 0)
    Console.WriteLine("Includes Left");   //Includes Left

string formatted = leftRight.ToString();  //"Left,Right"

BorderSides s = BorderSide.left;
s |= BorderSide.Right;
Console.WriteLine(s == leftRight);//True

s ^= BorderSides.Right;          //Toggles BorderSides.Right
Console.WriteLine(s);            //Left
```



#### Flags属性

按照约定，如果枚举成员可组合的话，flags属性就应该应用在枚举类型上

如果声明了这样的枚举却没有使用flags属性，你任然可以组合枚举的成员，但是调用枚举实例的ToString()方法时，输出的将是一个数值而不是一组名称

按约定，可组合枚举的名称应该是复数的

在声明可组合枚举的时候，可以直接使用组合的枚举成员作为成员

```csharp
[flags]
public enum BorderSides
{
    None=0,
    Left=1,Right=2,Top=4,bottom=8,
    leftRight = left | Right,👈
    TopButtom = Top | Buttom,👈
    All = leftRight | TopButtom👈
}
```

#### 枚举支持的操作符

其中按位的，比较的，算术的操作符返回的都是处理底层值后得到的结果

加法操作符只允许一个枚举和一个**整型数值相加**，两个枚举相加是不可以的



#### 类型安全的问题

```csharp
public enum BorderSide { Left,Right,Top,Bottom }
BorderSide b = (BorderSide) 12345;
Console.WriteLine(b);                     //12345
orderSide b = BorderSide.Buttom;
b++;                                      //no Error
```

检查枚举类型合理性：Enum.IsDefined()静态方法

```csharp
BorderSide b = (BorderSide) 12345;
Console.WriteLine(Enum.IsDefined(typeof(BorderSide),side));    //False
```



### 嵌套类型

嵌套类型就是**声明在另一个类型作用范围内**的类型

```csharp
public class TopLevel
{
    public class Nested { }               //Nested class
    public enum Color { Red,Blue,Tan}     //Nested enum
}
```

#### 嵌套类型的特性

- 可访问封闭类型的私有成员，以及任何封闭类型[^1]能访问的东西

- 可以使用所有的访问修饰符来声明，不仅仅是public和internal

- 嵌套类型的默认访问级别是private而不是internal

- 从封闭类型外边访问嵌套类型需要使用到封闭类型的名称

使用例子：

```csharp
TopLevel.Color.color = TopLevel.Color.Red;👈
```

```csharp
public class TopLevel
{
    static int x;
    class Nested
    {
        static void Foo() { Console.WriteLine(TopLevel.x👈);}
    }
}
```



[^1]:封闭类型：针对嵌套类型而言外层的类型

