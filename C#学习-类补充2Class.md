# C#学习
## 类class（补充2）😃
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
