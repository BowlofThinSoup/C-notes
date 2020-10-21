# C#学习
## 类class（补充2）😃

[toc]

***
##### Object initializers对象初始化器
对象任何**可访问**的**字段/属性**在**构建之后**，可通过对象初始化器直接为其进行设定值
例如：

```csharp
    public class student
    {
        int Age;
        string Name;
        float Weight;
        public student() {}
        public student(string name,int age)
        {
            Name = name;
            Age = age;
        }

    }
```
使用对象初始化器对其进行初始化可以如下声明：
```csharp
//无参的构造函数
student xiaoming = new student { Name = xiaoming, Age = 22, weight = 60 };
```
上式中无参构造函数初始化可以不写（），有参数的构造函数初始化时如下：
```csharp
//有参数的构造函数
student xiaoming = new student("xiaoming",22) { weight = 60 };
```
初始化时编译器生成的代码：
```csharp
    student temp1 = new student();
    temp1.Name = "xiaoming";
    temp1.Age = 22;
    temp1.Weight = 60;
    student xiaoming = temp1;👈

    student temp2 = new student("xiaoming",22);
    temp2.Weight = 60;
    student xiaoming = temp2;👈
```
使用临时变量temp
目的是确保，如果在初始化过程中出现异常，那么不会以一个初始化到一半的对象来结尾

##### 可选参数
如果不使用初始化器，上式中也可以使用可选参数,如下：
```csharp
    public class student
    {
        int Age;
        string Name;
        float Weight;
        public student(string name,int age = 22,float weight = 60)👈
        {
            Name = name;
            Age = age;
            Weight = weight;
        }
    }
```
对比可选参数时的初始化声明：
```csharp
student xiaoming = new student(name:"xiaoming",age:22);
```
###### 可选参数优缺点
优点：可以让student类的字段/属性只读
缺点：每个可选参数的值都被嵌入到calling site(调用栈),C#会把构造函数的调用翻译成：
```csharp
student xiaoming = new student("xiaoming",22,60👈);
```
上方的60并没有选择初始化，但是依然用默认值初始化，这可能导致一些问题
1.当从另一个程序集实例化student类时，如果再为student添加一个可选参数，那么除非重新编译，否则将继续调用旧的构造函数，运行时报错
2.当改变一个可选参数的值的时候，别的调用者将继续使用旧的值直到被重新编译

##### This引用
this引用指的是实例的本身

例如：

```csharp
public class panda
{
    public Panda Mate;
    public void Marry(Panda partner)
    {
        Mate = partner;
        partner.Mate = this;
    }
}
```

上面的例子中将this类的实例赋给了参数的一个属性，属性叫Mate，this就代表这个实例



this引用可以将字段与本地变量或者参数分开

只有class/struct的**非静态成员**才可以使用this

```csharp
public class Test
{
    string name;
    public Test (string name){ this.name = name}
}
```

上述类中字段叫name，方法参数也叫name，this.name就是指字段，不带this的就是本地变量或参数



##### Properties属性

从外在看，属性和字段很像，但从内部看，**属性含有逻辑**，就像方法一样

属性的声明和字段的声明很像，但是多了一个**get set**块

属性的get/set：

- get/set代表属性的访问器
- get访问器会在属性被**读取**的时候运行。但必须返回一个该属性类型的值
- set访问器会在属性被**赋值**的时候运行，有一个隐式的该类型参数value，通常会把value赋给一个私有字段

###### 属性和字段的区别：

尽管属性的访问方式与字段的访问方式相同，但不同之处在于，**属性赋予了实现者对获取和赋值的完全控制权**。这种控制允许实现者选择任意所需的内部表示，不向属性的使用者公开其内部实现细节。

```csharp
public class Stock
{
    decimal currentPrice;
    public decimal CurrentPrice
    {
        get { return currentPrice;}
        set { currentPrice = value;}
    }
}
```

get/set中可以**添加判断语句**之类的，也可以**做一些计算**



###### 只读和计算的属性

- 如果属性只有get访问器，那么它是只读的

- 如果只有set访问器，那么它是只写的（很少这么用）

- 属性通常拥有一个专用的backing field（幕后字段），这个幕后字段用来存储数据

  ```csharp
  decimal currentPrice,sharesOwned;
  public decimal Worth👈//只读属性，由两个值计算出来
  {
      get { return currentPrice * sharesOwned;}
  }
  ```

  

###### Expression-bodied属性

C#6开始可以使用Expression-bodied形式来表示只读属性

```csharp
public decimal Worth => currentPrice * sharesOwned;👈
```



C#7允许访问器也使用该形式

```csharp
public decimal Worth
{
    get => currentPrice * sharesOwned;
    set => sharesOwned = value/currentPrice;
}
```
###### 自动属性

- 属性最常见的一种实践就是：getter和setter只是对private field进行简单直接的读写

- 自动属性就告诉编译器来提供这种实现

```csharp
public class Stock
{
    ···
    public decimal CurrentPrice{ get; set;}👈
}
```

- 编译器会自动生成一个私有的幕后字段，其名称不可引用（由编译器生成）

- **set访问器也可以是private 或 protected**

###### 属性初始化器

从C#6开始，可以为自动属性添加属性初始化器

```csharp
 public decimal CurrentPrice {get;set;} = 123;
```

只读的自动属性也可以使用（只读自动属性也可以在构造函数里被赋值）

```csharp
 public decimal CurrentPrice {get;} = 123;
```

######  get/set的访问性

get和set访问器可以拥有不同的访问级别

典型用法：public get,interal/private set

```csharp
public class foo
{
    private decimal x;
    public 👈 deciaml X
    {
        get {return x;}
        private 👈 set {x = Math.Round(Value,2);}
    }
}
```

注意，属性的访问级别更宽松一些，访问器的访问级别更严一些

###### CLR的属性实现

C#的属性访问器内部会编译成get_XXX和set_XXX

简单的非Virtual属性访问器会被JIT编译器进行**内联（inline）**操作,这会**消除属性访问与字段访问之间的性能差异**。内联是一种优化技术，它会**把方法调用换成直接实现方法体**。 

##### Indexer索引器

索引器提供了一种可以访问封装了列表值或者字典值的class/struct的元素的一种自然语法

```csharp
string s = "hello";
Console.Writeline(s[0]);//'h'
```

语法很像使用数组时的语法，但是这里的索引参数可以是任何类型的

索引器和属性拥有相同的修饰符

可以按照下列方式使用null条件操作符：(可空类型)

```csharp
string s = null;
Console.Writeline(s?[0]);👈
```

###### 索引器实现

需要定义一个this属性，并用中括号指定参数

```csharp
class Sentence
{
    string[] words = "The quick brown fox".Split();
    public string this [int wordnum]
    {
        get { return words [wordnum];}
        set { words[wordnum] = value;}
    }
}
```

使用索引器

```csharp
Sentence s = new Sentence();
Console.Writeline(S[3]);//fox
s[3] = "kangaroo";
Console.Writeline(S[3]);//kangaroo

```

###### 多个索引器

一个类型可以有多个索引器，他们的参数类型可以不同

一个索引器可以有多个参数

###### 只读索引器

如果不写set访问器，那么这个索引器就是**只读**的

C#6以后，也可以使用expression-bodied语法

```csharp
public string this [int wordnum] => words [wordnum];
```

###### CLR的索引器的实现

索引器在内部会编译成get_Item和set_Item方法

```csharp
public string get_Item(int wordNum){}
public void set_Item(int wordNum,string value){}
```