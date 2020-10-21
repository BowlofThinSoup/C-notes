# C#学习  

[toc]



## 类Class
***
#### ❓什么是类 
**最常使用的引用类型**
当你定义一个类时，你定义了一个**数据类型的蓝图**。这实际上并没有定义任何的数据，但它定义了类的名称意味着什么，也就是说，类的对象由什么组成及在这个对象上可执行什么操作。==对象是类的实例。构成类的**方法**和**变量**称为类的成员==。  
#### 类的形式
```json
    <access specifier> class  class_name
    {
        // member variables
        <access specifier> <data type> variable1;
        <access specifier> <data type> variable2;
        ...
        <access specifier> <data type> variableN;
        // member methods
        <access specifier> <return type> method1(parameter_list)
        {
            // method body
        }
        <access specifier> <return type> method2(parameter_list)
        {
            // method body
        }
        ...
        <access specifier> <return type> methodN(parameter_list)
        {
            // method body
        }
    }
```
- 访问标识符 \<access specifier> 指定了对类及其成员的**访问规则**。如果没有指定，则使用默认的访问标识符。==类的默认访问标识符是 internal，**成员**的默认访问标识符是 **private**==。
- 数据类型 \<data type> 指定了变量的类型，返回类型 \<return type> 指定了返回的方法返回的数据类型。
- 如果要访问类的成员，你要使用点（**.**）运算符。
- 点运算符链接了对象的名称和成员的名称  

#### 类的例子 
```csharp
    using System;
    namespace BoxApplication
    {
        class Box
        {
        public double length;   // 长度
        public double breadth;  // 宽度
        public double height;   // 高度
        }
        class Boxtester
        {
            static void Main(string[] args)
            {
                Box Box1 = new Box();        // 声明 Box1，类型为 Box
                Box Box2 = new Box();        // 声明 Box2，类型为 Box
                double volume = 0.0;         // 体积

                // Box1 详述
                Box1.height = 5.0;
                Box1.length = 6.0;
                Box1.breadth = 7.0;

                // Box2 详述
                Box2.height = 10.0;
                Box2.length = 12.0;
                Box2.breadth = 13.0;
            
                // Box1 的体积
                volume = Box1.height * Box1.length * Box1.breadth;
                Console.WriteLine("Box1 的体积： {0}",  volume);

                // Box2 的体积
                volume = Box2.height * Box2.length * Box2.breadth;
                Console.WriteLine("Box2 的体积： {0}", volume);
                Console.ReadKey();
            }
        }
    }
```
上述例子中，定义了盒子类Box，它拥有三个成员变量：length，breadth和height
```C #
    public double length;   // 长度
    public double breadth;  // 宽度
    public double height;   // 高度
```
分别定义盒子的长宽高，三个变量访问标识符都是public，表示都是公开成员，外部也可以调用。  
随后在main()函数中创建类的实例，分别是盒子1(Box1)和盒子2(Box2)  
```C #
    Box Box1 = new Box();        // 声明 Box1，类型为 Box
    Box Box2 = new Box();        // 声明 Box2，类型为 Box
```
声明一个double类型的变量用来存放体积  
```C #
    double volume = 0.0;    
```
通过.操作符分别访问两个对象的成员变量并赋值，即分别给两个盒子的长宽高赋值  
```C #
    // Box1 详述
    Box1.height = 5.0;
    Box1.length = 6.0;
    Box1.breadth = 7.0;

    // Box2 详述
    Box2.height = 10.0;
    Box2.length = 12.0;
    Box2.breadth = 13.0;   
```
最后同样通过.操作符访问盒子的长宽高并将乘积赋予之前定义的体积，最后再输出  
```C++
    // Box1 的体积
    volume = Box1.height * Box1.length * Box1.breadth;
    Console.WriteLine("Box1 的体积： {0}",  volume);

    // Box2 的体积
    volume = Box2.height * Box2.length * Box2.breadth;
    Console.WriteLine("Box2 的体积： {0}", volume); 
```
#### 🎁类的封装 
##### 什么是封装  
- *封装，即**隐藏对象的属性和实现细节，仅对外公开接口，控制在程序中属性的读和修改的访问级别**；将抽象得到的数据和行为（或功能）相结合，形成一个有机的整体，也就是将数据与操作数据的源代码进行有机的结合，形成“类”，其中数据和函数都是类的成员*  
##### 成员函数和成员变量：👨‍👩‍👦
- **成员函数**是一个在类定义中有它的定义或原型的函数，就像其他变量一样。作为类的一个成员，**它能在类的任何对象上操作，且能访问该对象的类的所有成员**。
- **成员变量**是对象的属性（从设计角度），**且它们保持私有来实现封装**。这些变量只能使用公共成员函数来访问。
##### 封装示例  
```c++
    using System;
    namespace BoxApplication
    {
        class Box
        {
        private double length;   // 长度
        private double breadth;  // 宽度
        private double height;   // 高度
        public void setLength( double len )
        {
                length = len;
        }

        public void setBreadth( double bre )
        {
                breadth = bre;
        }

        public void setHeight( double hei )
        {
                height = hei;
        }
        public double getVolume()
        {
            return length * breadth * height;
        }
        }
        class Boxtester
        {
            static void Main(string[] args)
            {
                Box Box1 = new Box();        // 声明 Box1，类型为 Box
                Box Box2 = new Box();        // 声明 Box2，类型为 Box
                double volume;               // 体积


                // Box1 详述
                Box1.setLength(6.0);
                Box1.setBreadth(7.0);
                Box1.setHeight(5.0);

                // Box2 详述
                Box2.setLength(12.0);
                Box2.setBreadth(13.0);
                Box2.setHeight(10.0);
        
                // Box1 的体积
                volume = Box1.getVolume();
                Console.WriteLine("Box1 的体积： {0}" ,volume);

                // Box2 的体积
                volume = Box2.getVolume();
                Console.WriteLine("Box2 的体积： {0}", volume);
            
                Console.ReadKey();
            }
        }
    }
```
与上面的例子类似，依然是定义盒子类，但是在`例1`基础上实现了封装，三个成员变量length，breadth和height的访问标识符都是private,表示仅有类的成员函数可以调用，外部将不能通过.标识符访问，从而实现了对外的不透明。  
        &emsp;&emsp;**`private`** `double length;   // 长度`
        &emsp;&emsp;**`private`** `double breadth;  // 宽度`
        &emsp;&emsp;**`private`** `double height;   // 高度`
与此同时，新增加了三个成员函数用来给外部访问以设置对象的属性  
```C++
    public void setLength( double len )
    {
            length = len;
    }

    public void setBreadth( double bre )
    {
            breadth = bre;
    }

    public void setHeight( double hei )
    {
            height = hei;
    }
```
可以看到三个函数访问标识符都是public，表示外部可以访问。函数将传入的参数赋值给私有成员变量，以此实现实例化。
另外为了体现封装性，将上一例的求体积也封装为类的一个方法
```c++
        public double getVolume()
    {
        return length * breadth * height;
    }

```
在`main()`函数中调用时，通过成员函数实例化两个对象，将长宽高数值作为参数传入。
```c++
    // Box1 详述
    Box1.setLength(6.0);
    Box1.setBreadth(7.0);
    Box1.setHeight(5.0);

    // Box2 详述
    Box2.setLength(12.0);
    Box2.setBreadth(13.0);
    Box2.setHeight(10.0);
```
最后的体积也通过调用Box类的`getVolume()`方法实现。 
```c++
    // Box1 的体积
    volume = Box1.getVolume();
    Console.WriteLine("Box1 的体积： {0}" ,volume);

    // Box2 的体积
    volume = Box2.getVolume();
    Console.WriteLine("Box2 的体积： {0}", volume);
```
#### 🛴构造函数
##### 什么是构造函数
- *构造函数 ，是一种特殊的方法。主要用来在创建对象时初始化对象， 即为对象成员变量赋初始值，总与new运算符一起使用在创建对象的语句中。特别的一个类可以有多个构造函数 ，可根据其参数个数的不同或参数类型的不同来区分它们 即构造函数的重载。*  
- 类的构造函数 是类的一个特殊的成员函数，当创建类的新对象时执行。  
- 构造函数的名称与类的名称完全相同，它没有任何返回类型。
##### 为什么需要构造函数
- 参考c++构造函数❗
>在我们过去学习C语言编程的时候，我们通常生成的变量都是放在栈区里（auto存储类型）。然而，真正处理实际问题的程序却常常将变量或数组生成在堆区里。
假设我们定义了一个对象obj，此对象有一个占存储很大的成员member，将来要放很大的一篇文章，则我们不希望这个字符数组出现在栈区，而是希望它出现在堆区，如下图所示，<div align=center>
![(图3-1.png)](https://img-my.csdn.net/uploads/201210/21/1350784197_1207.png)</div>
为了实现我们的愿望，我们不能把成员member定义成数组，而是把它定义成**指针数据成员**，让它指向堆区里的一个占存储很大的字符数组。这样，虽然这个字符数组不是对象的成员，但是，我们能通过指针member访问到它，与这个字符数组是成员的效果是一样的。
据上图（图3‑1）我们看到，**有两个东西需要初始化，一个是指针成员member，另一个是堆里的这个数组**。这两个东西只能用一条语句生成并初始化，然而这是有问题的。
在C语言里是用等号来表示初始化的，现在我们试着用两个等号来初始化这两个东西：
`char* member=new char[10000]= {“large-size string”};`
<u>上面的语句中第二个等号语法上是有问题的，本来等号的左边只能是变量名或数组名，但是这个等号的左边却是一个表达式。</u>💢
C++提供了这样一种解决途径。它提供了一种成员函数，在初始化对象的时候，会自动地调用它。我们称这样的函数为**构造函数**。在这样的函数里，我们写几个赋值语句：
`number=new char[9000];`
`strcpy(number, “large-size string”);`
上述问题就解决了。🉑
现在还有一个问题。当上图（图3‑1）对象obj的生命结束时，obj就要从栈区被释放，然而，<u>指针成员member所指向的堆里的东西不会自动地因此而被释放</u>，这是不符合我们的愿望的。C++提供了这样一种成员函数，在对象被释放的时候，会自动地调用它，我们称这个函数为**析构函数**。我们只要把delete写到这个函数里，上述问题就解决了。
[参考CSDN大佬原文](https://blog.csdn.net/u013565071/article/details/78267440)  

当没有构造函数时：  
倘若在类的声明中没有显式地提供实例构造函数，在这种情况下编译器会提供一个**隐式的默认构造函数**，它具有以下特点：
**1.不带参数**
**2.方法体为空**
==但是如果你声明了任何构造函数，那么编译器就不会把该类定义为默认构造函数==。
如下例：👇
```c++
    class Class2
    {
        public Class2(int Value)  {...}  //构造函数0
        public Class2(String Value) {...}  //构造函数1
    }

    class Program
    {
        static void Main()
        {
            Class2 a = new Class2();   👈//错误！没有无参数的构造函数
            ...
        }
    }
```
在以上的代码中至少有一个显式定义的构造函数，编译器不会创建任何额外的构造函数，在 `Main()`中如果试图用不带参数的构造函数创建新的实例，因为**没有无参数的构造函数**，所以编译器就会产生一条错误信息。



##### 构造函数实例
```c++
    using System;
    namespace LineApplication
    {
    class Line
    {
        private double length;   // 线条的长度
        public Line()👈
        {
            Console.WriteLine("对象已创建");
        }

        public void setLength( double len )
        {
            length = len;
        }
        public double getLength()
        {
            return length;
        }

        static void Main(string[] args)
        {
            Line line = new Line();    
            // 设置线条长度
            line.setLength(6.0);
            Console.WriteLine("线条的长度： {0}", line.getLength());
            Console.ReadKey();
        }
    }
    }
```
上例中构造函数部分：
```c++
    public Line()
    {
        Console.WriteLine("对象已创建");
    }
```
可以发现，与成员函数不同在于构造函数**没有返回值类型**，且**函数名称与类名一致**。  

##### 参数化构造函数
默认的构造函数没有任何参数。但是如果你需要一个**带有参数的构造函数**可以有参数，这种构造函数叫做参数化构造函数。通过参数化构造函数可以在创建对象的同时给对象赋初始值。
```c++
    public Line(double len)  // 参数化构造函数
        {
            Console.WriteLine("对象已创建，length = {0}", len);
            length = len;
        }
```
这样声明时可以使用参数初始化对象,在初始化时使用如下语句：
`Line line = new Line(10.0);`  
可以将line的长度初始化为10。

#### 🐷析构函数
##### 什么是析构函数
*析构函数(destructor) 与构造函数相反，当对象结束其生命周期，如对象所在的函数已调用完毕时，系统自动执行析构函数。析构函数往往用来做“清理善后” 的工作（例如在建立对象时用new开辟了一片内存空间，delete会自动调用析构函数后释放内存）。*
析构函数 是类的一个特殊的成员函数，当类的对象**超出范围时执行[^1]**。
析构函数的名称是在类的名称前加上一个**波浪形（~）作为前缀**，它**不返回值，也不带任何参数**。
析构函数用于在结束程序（比如关闭文件、释放内存等）之前**释放资源**。析构函数**不能继承或重载**。  
```c++
    ~Line() //析构函数
        {
            Console.WriteLine("对象已删除");
        }
```
#### 静态成员
我们可以使用 static 关键字把类成员定义为静态的。当我们声明一个类成员为静态时，意味着无论有多少个类的对象被创建，**只会有一个该静态成员的副本**。

关键字 static 意味着类中只有一个该成员的实例。静态变量用于**定义常量**，因为它们的值可以通过直接调用类而不需要创建类的实例来获取。<u>静态变量可在成员函数或类的定义外部进行初始化。你也可以在类的定义内部初始化静态变量</u>。
##### 静态成员变量：
见下例：👇
```c++
    using System;
    namespace StaticVarApplication
    {
        class StaticVar
        {
        public static int num;👈
            public void count()
            {
                num++;
            }
            public int getNum()
            {
                return num;
            }
        }
        class StaticTester
        {
            static void Main(string[] args)
            {
                StaticVar s1 = new StaticVar();
                StaticVar s2 = new StaticVar();
                s1.count();
                s1.count();
                s1.count();
                s2.count();
                s2.count();
                s2.count();        
                Console.WriteLine("s1 的变量 num： {0}", s1.getNum());
                Console.WriteLine("s2 的变量 num： {0}", s2.getNum());
                Console.ReadKey();
            }
        }
    }
```
上述`StaticVar`类中定义了一个静态变量`num`可以通过`count()`方法调用以实现自增，在`main()`函数里实例化两个对象s1和s2,轮流调用`count()`方法并输出。
~~如果不是静态成员~~输出结果应该是:
```c++
s1 的变量 num： 3
s2 的变量 num： 3
```
这里的输出是：
```c++
s1 的变量 num： 6
s2 的变量 num： 6
```
可以看出两个实例调用的是同一个静态成员，并没有两个副本。
##### 静态成员函数：
也可以把一个成员函数声明为 static。这样的函数**只能访问静态变量**。==静态函数在对象被创建之前就已经存在==。
见下例：👇
```c++
    class StaticVar
    {
       public static int num;
        public void count()
        {
            num++;
        }
        public static int getNum()👈
        {
            return num;
        }
    }
```
此处静态成员函数只能访问num这个静态变量，而不能访问别的。

[^1]:对象超出范围怎么理解
