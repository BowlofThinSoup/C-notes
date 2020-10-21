# C#学习

[toc]

### Obiect类型

#### Object

object(System.Object)是所有类型的终极父类

所有类型都可以向上转换为object

简单例子：

```csharp
public class Stack
{
    int position;
    object[] date = new object[10];
    public void Push (object obj) { date[position++] = obj;}
    public object Pop()           { return data[--position];}
}
```

```csharp
Stack stack = new Stack();
stack.Push("sausage");
string s = (string) stack.pop();//downcast ,so explicit id needed
Console.WriteLine(s);//sausage
```



object是引用类型

但值类型可以转换为object，反之亦然（类型统一）

```csharp
stack.Push(3);
int three = (int) stack.Pop();
```

在值类型和object之间转换的时候，CLR必须执行一些特殊的工作，以弥合值类型和引用类型之间语义上的差异，这个过程就叫**装箱**和**拆箱**

##### Boxing装箱

装箱就是把值类型的值转化为引用类型实例的动作

目标引用类型可以是object，也可以是某个接口

```csharp
int x = 9;
object obj = x; //box the int
```

##### Unboxing拆箱

拆箱刚好相反，把那个对象转换为原来的值类型

```csharp
int y = (int)obj; //unbox the int
```

拆箱需要显式的转换

运行时会检查这个值类型和object对象的真实类型是否匹配

如果不适合就抛出InvalidCastExpression

```csharp
object obj = 9;      //9 is inferred to be of type int
long x = (long) obj  //InvalidCastExpression
```

```csharp
object obj = 9; 
long x = (int) obj 
```

```csharp
object obj = 3.5;           //3.5 is inferred to be of type double
int x = (int)(double) obj;  //x is now 3
```



装箱对于类型统一是非常重要的，但是系统不够完美

数组和泛型只支持引用转换，不支持装箱

```csharp
object[] a1 = new string[3]; //Legal
object[] a2 = new int[3];    //Error
```

##### 装箱拆箱的复制

装箱会把值类型的实例复制到一个新的对象

拆箱会把这个对象的内容再复制给一个值类型的实例

```csharp
int i = 3;
object boxed = i;
i = 5;
Console.WriteLine(boxed);  //3
```

##### 静态和运行时类型检查

C#的程序既会做静态的类型检查（编译时）也会做运行时的类型检查（CLR）

静态检查：不运行程序的情况下，让编译器保证你程序的正确性

```csharp
int x = "5";👈//静态检查报错
```

运行时检查：由CLR执行，发生在向下的引用转换或者拆箱的时候

```csharp
object y = "5";
int z = (int)y;   //Runtime error,downcast failed
```

运行时检查之所以可行是因为：每一个在heap（堆）上的对象内部都存储了一个类型的token，这个token（令牌）可以通过调用object的GetType（）方法来获取



##### GetType()方法与typeof操作符

所有C#的类型在运行时都是以System.Type的实例来展现的

有两种方式可以获取System.Type对象：

- 在实例上调用GetType()方法
- 在类型名上使用typeof操作符

GetType()是在运行时被算出的

typeof是在编译时被算出的（静态）（当涉及到泛型类型参数时，它是由JIT编译器来解析的）



##### System.Type

System.Type属性有：类型的名称，Asembly，基类等等

```csharp
using system
public  class Point{ public int X,Y;}
class Test
{
    static void main()
    {
        Point p = new Point();
        Console.WriteLine(p.GetType().Name);//Point
        Console.WriteLine(typeof(Point).Name);//Point
        Console.WriteLine(p.GetType() == typeof(Point));//True
        Console.WriteLine(p.X.GetType().Name);//int32
        Console.WriteLine(p.Y.GetType().FullName);//System.int32
    }
}
```



##### ToString()方法

ToString()方法会返回一个类型实例的默认文本表示

所有的内置类型都重写了该方法

```csharp
int x = 1;
string s = x.ToString();  //s is "1"
```

可以在自定义的类型上重写ToString()方法

如果不重写该方法，那么就会返回该类型的名称

```csharp
public class Panda
{
    public string Name;
    public override string ToString() => Name;
}
···
Panda p = new panda {Name = "Petey"};
Console.WriteLine(p);//Petey
```



当调用一个被重写的object成员的时候，例如在值类型上直接调用ToString()方法，这时候就不会发生装箱操作

凡是如果进行了转换，那么装箱操作就会发生

```csharp
int x = 1;
string s1 = x.ToString();//Calling on nonboxed value
object box = x;
string s2 = box.ToString();//Calling on boxed value
```



##### Object 成员列表

```csharp
public class Object
{
    public Object();
    public extern Type GetType();
    
    public virtual bool Equals(Object obj);
    public static bool Equals(Object objA,Object objB);
    public static bool ReferenceEquals(Object objA,Object objB);
    
    public virtual int GetHashCode();
    
    public virtual string ToString();
    
    protected virtual void Finalize();
    protected extern object MemberwiseClone();
}
```



