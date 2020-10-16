# C#学习
## 类class（补充）😁
***
[Toc]
**参考书目**：<div align=center>
![C# 7.0 in a Nutshell](https://img01.haolizi.net/2019/07/28/b3/a/b/b3ab2ef41ea3e2bebcb8e43c48a27616.png) </div> 
  
Class：最常见的一种**引用类型**，基本结构如下：  
```csharp
    Class ClassName
    {
        Class Menber//类成员
    }
```  
Class关键字之前可以加1-2个关键字，用来限定访问或标记  
ClassName之后可以加以下几种修饰符
- `<T>`，表示泛型
- `：父类名`，表示继承父类
- `：接口名`，表示实现的接口
ClassMenber表示类成员，有以下几种
- Field字段
- Method方法
- Property属性
- Indexer索引器
- Event事件
- Constructor构造器
- Overlord重载操作符
- finallizer终结器
- 嵌套类型


##### Field字段
Field：是Class或Struct的成员，是一个**变量**
```csharp
Class ClassName
    {
        string name;👈
        public int age = 10;👈
    }
```
上述👈所指都是字段，
常见字段修饰符
- 与访问有关：public,protected,privide,static,readonly
- 与继承有关：new，unsafe
- 与线程有关：volunteer

###### readonly修饰符
- 可以防止字段在构造之后被改变
- 只能在声明的时候赋值，或者在构造函数里赋值

###### 字段初始化  
- 字段可选初始化
- 未初始化的字段有一个**默认值**（int:0;char:\0;引用类型:null）
- ==字段的**初始化在构造函数之前进行**==

###### 同时声明多个字段
```csharp
    readonly int age = 8,👈
                 height = 178;
```
同时声明多个字段时，用逗号（,）隔开  

##### Method方法
Method：由一些语句组成，会执行某**个**动作（一个方法最好只做一件事）
参数：方法可以接受一些输入的数据，称为方法的参数，由方法调用者提供
返回类型：方法执行完可能会返回一些数据，返回类型表示返回数据的数据类型；当什么都不返回时，返回类型就是void，表示什么都不返回
引用类型：ref/out可以决定引用的类型

###### 方法的签名
==类型内部签名必须统一==
签名：***方法名***，***参数类型***（**包含参数的顺序**）和**参数名称及返回类型无关**
例如：
```csharp
    calss test
    {
        static void main(string[],int a)
        {       ☝

        }
        static int main(string[],int a)
        {       ☝
            
        }
    }
```
上述两个方法签名重复，虽然返回类型不一致，但是签名**与返回类型无关**

如果更换参数的位置，签名就不一致了，如下：
```csharp
    calss test
    {
        static void main(int a,string[])
        {                 ☝

        }
        static void main(string[],int a)
        {                         ☝
            
        }
    }
```
虽然函数名一致，但是参数**位置不一样**，函数签名也不同

######  **expression-bodied** 方法
```csharp
    int foo(int x)
    { 
        return x * 2;
    }
```
可以写作：
```csharp
    int foo(int x) => x * 2;
```
void返回类型也可以使用bodied写法，直接将语句写上便可
```csharp
    void foo(int x) => console.writeline(x);
```
==相当于将大括号与return去掉并合并写作=>==
值得注意的是这种写法**只适用于单表达式**的写法

##### Overload重载
重载：类型里的方法可以进行重载（允许多个同名方法的存在），只要这些方法的签名不同即可
```csharp
    void foo(int x){···}
    void foo(double x){···}
    void foo(int x,double y){···}
    void foo(float x,int y){···}
```
与方法名命名规则类似，参考方法签名内容
值得注意的是，如下的重载是错误的：
```csharp
    void foo(int[] x){···}
    void foo(params int[] x){···}
```
因为==签名中不包括params修饰符==

==参数的**传递方式**也是签名的一部分==
```csharp
    void foo(int x){···}
    void foo(ref int x){···}
```
上例是重载，这两个函数的传递方法是不一致的，可以重载
但是如下：
```csharp
    void foo(ref👈 int x){···}
    void foo(out👈 int x){···}
```
就不属于重载，这两种类型都是按引用传递，所以编译时会判定签名一致，报错

##### 本地方法
```csharp
    void WriteCubes()
    {
        console.writeline(cube(3));
        console.writeline(cube(4));
        console.writeline(cube(5));

        👉int Cube (int value) => value*value*value;👈
    }
```
本质上本地方法就是方法内嵌套方法
- 但是上例中`Cube()`方法只对外部一层方法`WriteCubes()`可见
- 本地方法可以访问外部一层的本地参数
- 本地方法还可以存在于构造函数之中，本地方法也可以嵌套本地方法
- ==本地方法不可以使用static修饰符==

#### 构造函数
作用：运行class或struct初始化代码  
与方法类似，方法名与类型一致，返回类型也与类型一致，但是不写返回类型
例如：
```csharp
    public class student
    {
        string name;//define field
        public student(string n)👈//define constructor
        {
            name = n;//initialization code(set u field)
        }
    }
```
在main()函数中调用时
```csharp
student xiaoming = new student("xiaoming");
```
返回类型就是student，将**对实例的引用**赋给了变量`xiaoming`

从C#7开始，**单个语句**的构造函数可以**写成expression-bodied**成员形式，如下：
```csharp
    public class student
    {
        string name;//define field
        public student(string n) => name = n;👈
    }
```

##### 构造函数的重载
class和struct可以重载构造函数，意味着可以有多个构造函数  
调用构造函数可以使用**this**标识符，如例：
```csharp
    public class student
    {
        int Age;
        string Name;//define field
        public student(string name) => Name = name;
        public student(string name,int age):this(name)👈
        {
            Age = age
        }
    }
```
当同一类型下构造函数A调用构造函数B的时候，B先执行(==被调用的先执行==)
上例中👈所指的构造函数调用了其上一行的构造函数，于是应该先执行上一行  

##### 表达式传递
==可以把**表达式传递给另一个构造函数**==，但表达式本身**不能使用this引用**，因为此时对象还未初始化，所以任何方法的调用都会失败，但是可以使用static方法
如下例：
```csharp
    public class student
    {
        int Age;
        float Weight;
        string Name;
        public student(string name) => Name = name;

        public student(string name,int age):this(name)
        {
            Age = age
        }

        public student(string name,float weight):this(name,this.getAge())
        {                                                      👆
            Weight = weight
        }

        public int getAge()
        {
            return 22;
        }
    }
```
上式中👆所指位置会报错，因为此时**对象未初始化**，无法调用`getAge()`方法
注意，如果此时将调用的方法改为**static**，那么就可以成功调用
```csharp
    public class student
    {
        int Age;
        float Weight;
        string Name;
        public student(string name) => Name = name;

        public student(string name,int age):this(name)
        {
            Age = age
        }

        public student(string name,float weight):this(name,student.getAge())
        {                                                         👆
            Weight = weight
        }

        public 🌓static🌗 int getAge()
        {
            return 22;
        }
    }
```
因为此时`getAge()`不再是实例方法，而是类型方法（注意👆发生变化）

##### 无参构造函数
对于class，==如果没有定义任何构造函数，那么C#编译器会自动生成一个**无参的public构造函数**==
但是==如果已经定义了构造函数，则不会生成无参构造函数==
（*在敲代码时输入：ctor并Tab可以生成无参构造函数的模板*） 

##### 构造函数和字段初始化顺序
- **字段初始化在构造函数执行之前**  
- 字段**按照声明的先后顺序**进行初始化  

##### 非public构造函数
构造函数**可以是非public的**
```csharp
    public class student
    {
        string Name;
        student(string name) => Name = name;

        public static student CreateStudent()
        {
            return new student();
        }
    }
```
==类中默认不加修饰符的构造函数是private==，外部无法调用其生成实例，但是可以通过**静态方法**生成实例
在`main()`函数中可以通过如下生成实例：
```csharp
    public void main()
    {
        var xiaoming = student.CreateStudent();
    }

```

##### Deconstructor
c#7引入，作用与构造函数相反，会将字段反赋值给变量
使用要求：**方法名必须是Deconstruct**,有一个或多个out参数  
```csharp

    public class student
    {
        int Age;
        string Name;//define field
        public student(string name) => Name = name;
        public student(string name,int age):this(name)
        {
            Age = age
        }

        public void Deconstruct(out string name,out int age)👈
        {
            name = Name;
            age = Age;
        }
    }
```
main()函数中调用：
```csharp
    public void main()
    {
       student xiaoming = new student();
       string name;
       int age;
       xiaoming.Deconstruct(out name,out age);👈
       //(string name,int age) = xiaoming;
       console.writeline(name+" "+age);
    }
```
调用Deconstruct将对象姓名和年龄两个属性赋值给变量`name`和`age`  

###### Deconstructor重载
如下，可以重载成为不同的数据类型：
```csharp

    public class student
    {
        int Age;
        string Name;//define field
        public student(string name) => Name = name;
        public student(string name,int age):this(name)
        {
            Age = age
        }

        public void Deconstruct(out string name,out int age)👈
        {
            name = Name;
            age = Age;
        }

        public void Deconstruct(out string name)👈
        {
            name = Name;
        }

    }
```

###### Deconstruct可以是**扩展方法**[^1]
```csharp

    public class student
    {
        int Age;
        string Name;//define field
        public student(string name) => Name = name;
        public student(string name,int age):this(name)
        {
            Age = age
        }
    }
    public static class Extisions
    {
        public static void Deconstract(this student xiaoming,out string name,out int age)👈
        {
            name = xiaoming.name;
            age = xiaoming.age;
        }
    }
```
main()函数中调用时：
```csharp
    xiaoming.Deconstruct(out var name,out var age);
```
[^1]:关于扩展方法后续会学习