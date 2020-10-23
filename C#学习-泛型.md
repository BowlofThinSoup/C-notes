# C#学习

### <span id = "泛型"> 泛型 </span>

[toc]

#### 泛型的作用

C#中想要复用代码，有两种方式：**继承和泛型**

- 继承  —> 基类

- 泛型 —> 带有“(类型)占位符"的”模板“



#### Generic Types 泛型类型

泛型会声明**类型参数**—==泛型的消费者需要提供类型参数(argument)来把占位符类型填充上==

```csharp
public class Stack<T>
{
    int position;
    T[] data = new T[100];
    public void push(T obj) => data[position++] = obj;
    public T Pop()          => data[--position];
}
```

```csharp
var stack = new Stack<int>();
stack.Push(5);
stack.Push(10);
int x = stack.Pop();   //x is 10
int y = stack.Pop();   //x is 5
```

```csharp
public class ###
{
    int position;
    int[] data = new int[100];
    public void Push (int obj) => data[position++] = obj;
    public int Pop()           => data[--position];
}
```



#### Open type & Closed Type

开放类型：Stack<T> Open Type

封闭类型：Stack<int>Closed Type

==在运行时，所有的泛型类型实例都是封闭的（占位符类型已被填充）==

```csharp
var stack = new Stack<T>();//Illegal 占位符没有被填充
```

```csharp
public class Stack<T>
{
    ···
    public Stack<T> Clone()
    {
        Stack<T> clone = new Stack<T>();     //legal
        ···
    }
}
```



#### 为什么泛型会出现

为了写出不同类型的代码

如果不能使用泛型，那么如果想要实现不同类型的代码，就需要对不同类型的代码都重新实现一遍

或者使用Object类型，如下：

```csharp
public class ObjectStack
{
    int position;
    Object data = new Object[100];
    public void push(Object obj) => data[position++] = obj;
    public Object Pop()          => data[--position];
}
```

使用Object缺点如下：

需要**装箱和向下转换**，这种转换在编译时无法进行检查

```csharp
stack.Push("s");            //Wrong type,but no error!
int i = (int)stack.Pop();   //Downcast - runtime error
```



#### 泛型方法

泛型方法在方法的签名内也可以声明类型参数

```csharp
static void Swap<T> (ref T a,ref T b)
{
    T temp = a;
    a = b;
    b = temp;
}
```

```csharp
int x = 5;
int y = 10;
Swap (ref x, ref ,y);
```

<div align = center>👇</div>

如果害怕不能自动判断出类型，可以在前面注明类型

```csharp
Swap<int> (ref x, ref y);
```



在泛型类型里面的方法，除非也引入了**类型参数**（type parameters），否则是不会归为泛型方法的

==只有类型和方法可以引入类型参数==，属性，索引器，事件，字段，构造函数，操作符等都不可以声明类型参数。但是他们==可以使用他们所在的泛型类型的**类型参数**==

```csharp
public T this [int index] => data [index];
```

```csharp
public Stack<T>() { }    //Illegal
```



#### 声明类型参数

在声明class、struct、interface、delegate的时候可以引入参数类型（Type parameters）

其他的例如属性，就不可以引入参数类型，但是可以使用参数类型

```csharp
public struct Nullable<T>
{
    publicc T Value { get;}
}
```



泛型类型/泛型方法==可以有多个类型参数==：

```csharp
class Dictionary<TKey, TValue>{……}👈
```

```csharp
Dictionary<int,string>myDic = new Dictionary<int,string>();
```

```csharp
var myDIC = new Dictionary<int,string>();
```



==泛型类型/泛型方法**可以被重载**，条件是**参数类型的个数不同**==：

```csharp
class A       {}
class A<T>    {}
class A<T1,T2>{}
```

按约定，泛型类型/泛型方法如果有一个类型参数，那么就叫T

当使用多个类型参数的时候，==每个类型参数都使用**T**作为前缀==，随后跟着具有**描述性**的一个名字。



#### Typeof与未绑定的泛型类型

==开放的泛型类型在编译后就变成了封闭的泛型类型==

但是如果作为Type对象，那么未绑定的泛型类型在运行时是可以存在的（只能通过typeof操作符来实现）

```csharp
class A<T>    {}
class A<T1,T2>{}
···
Type a1 = typeof(A<>);//unbound type (notice no type arguments)
Type a2 = typeof(A<,>);//Use commas to indicate mulyipe type args.
```

```csharp
Type a3 = typeof( A<int,int>);
```

```csharp
class B<T> { void X() { Type t = typeof(T);}}
```



#### 泛型的默认值

使用default关键字来获取泛型类型参数的默认值

```csharp
static void Zap<T>(T[] array)
{
    for (int i = 0;i < array.length;i++)
        array[i] = default(T);
}
```

[回到开头](#泛型)