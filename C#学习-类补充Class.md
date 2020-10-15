# C#学习
## 类class（补充）😁
***
**参考书目**：<div align=center>
![C# 7.0 in a Nutshell](https://img01.haolizi.net/2019/07/28/b3/a/b/b3ab2ef41ea3e2bebcb8e43c48a27616.png) </div> 
  
Class：最常见的一种**引用类型**，基本结构如下：  
```c++
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
```c++
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
- 字段的**初始化在构造函数之前进行**

###### 同时声明多个字段
```c++
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
类型内部签名必须统一
签名：方法名，参数类型（**包含参数的顺序**）和**参数名称及返回类型无关**
例如：
```c++
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
上述两个方法签名重复，虽然返回类型不一致，但是签名与返回类型无关

如果更换参数的位置，签名就不一致了，如下：
```c++
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
虽然函数名一致，但是参数位置不一样，函数签名也不同

###### **expression-bodied** 方法
```c++
int foo(int x)
{ 
    return x * 2;
}
```
可以写作：
```c++
int foo(int x) => x * 2;
```
void返回类型也可以使用bodied写法，直接将语句写上便可
```c++
void foo(int x) => console.writeline(x);
```
相当于将大括号与return去掉
值得注意的是这种写法**只适用于单表达式**的写法

##### Overload重载
重载：类型里的方法可以进行重载（允许多个同名方法的存在），只要这些方法的签名不同即可
```C++
void foo(int x){···}
void foo(double x){···}
void foo(int x,double y){···}
void foo(float x,int y){···}
```
与方法名命名规则类似，参考方法签名内容
值得注意的是，如下的重载是错误的：
```c++
void foo(int[] x){···}
void foo(params int[] x){···}
```
因为签名中不包括params修饰符

参数的**传递方式**也是签名的一部分
```c++
void foo(int x){···}
void foo(ref int x){···}
```
上例是重载，这两个函数的传递方法是不一致的，可以重载
但是如下：
```c++
void foo(ref👈 int x){···}
void foo(out👈 int x){···}
```
就不属于重载，这两种类型都是按引用传递，所以编译时会判定签名一致，报错

##### 本地方法
```c++
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
- 本地方法不可以使用static修饰符