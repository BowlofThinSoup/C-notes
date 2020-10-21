# C#学习

### Inheritance继承（补充）

[toc]

#### 抽象类和抽象成员

- 使用abstract声明的类是抽象类
- 抽象类不可以被实例化，只有其具体的子类才可以实例化
- 抽象类可以定义抽象成员
- 抽象成员和Virtual成员很像，但是不提供具体的实现。子类必须提供实现，除非子类也是抽象的

定义如下抽象类：

```csharp
public abstract class Asset
{
    public abstract decimal NetValue { get;}
}
```

其子类继承后：

```csharp
public class Stock : Asset
{
    public long ShareOwned;
    public decimal CurrentPrice;
    public override deciaml NetValue => CurrentPrice * SharesOwned;👈
    //Override like a virtual method
}
```

##### 隐藏被继承的成员

父类和子类可以定义相同的成员

```csharp
public class A  { public int Counter = 1;}
public class B ： A  { public int Counter = 2;}
```

class B 中的Counter字段就隐藏了A里面的Counter字段（通常是偶然发生的）。例如子类添加某个字段后，父类也添加了相同的一个字段。

编译器会发出警告，警告变量被隐藏



编译时按照如下规则进行解析

- 编译时对A的引用会绑定到A.Counter
- 编译时对B的引用会绑定到B.Counter

结合上面的例子，下面展示了Manin()函数调用的输出情况：

```csharp
class program
{
    static void Main()
    {
        A a = new A();
        System.Console.WriteLine(a.Counter);//1
		B b = new B();
        System.Console.WriteLine(b.Counter);//2
        A c = new B();//upcast
        System.Console.WriteLine(c.Counter);//1,此时是对A的引用
    }
}
```

变量类型是什么，就会调用相应的字段



如果想故意隐藏父类的成员，可以在子类的成员前面加上**new**修饰符

这里的==new修饰符仅仅会抑制编译器的警告而已==

```csharp
public class A  { public int Counter = 1;}
public class B ： A  { public new 👈 int Counter = 2;}
```



##### New和Override

通过下面例子的输出比较new和override差别

```csharp
public class BseClass
{
    public virtual void Foo() {Console.WriteLine("BaseClass.Foo");}
}
public class Overrider : BaseClass
{
    public override void Foo() {Console.WriteLine("Overrider.Foo");}
}
public class Hider: BseClass
{
    public new void Foo() {Console.WriteLine("Hider.Foo");}
}
```

调用时：

```csharp
Override over = new Overrider();
BaseClass b1 = over;
over.foo();   //Overrider.Foo
b1.Foo();     //Overrider.Foo

Hider h = new Hider();
BaseClass b2 = h;
h.Foo();     //Hider.Foo
b2.Foo();    //BaseClass.Foo
```



#### Sealed 密封

针对重写的成员，可以使用sealed关键字将其“密封”起来，防止它被其子类重写

```csharp
public sealed override decimal Liability { get { return Mortgage;}}
```

也可以sealed类本身，就隐式的sealed所有的virtual函数



#### Base关键字

base和this略像，base主要用于

- 从子类访问父类里被重写的函数
- 调用父类的构造函数

```csharp
public class House : Asset
{
    ···
    public overridedecimal liability => base.liability + Mortgage;
}
```

这种写法可以保证，访问的一定是Asset的Liability属性，无论该属性是被重写还是被隐藏



#### 构造函数和继承

子类必须声明自己的构造函数

从子类可以访问父类的构造函数，但是不是自动继承的

子类必须重新定义他想要暴露的构造函数（得自己写，但是可以调用父类的）

- 调用父类构造函数需要使用base关键字
- 父类的构造函数肯定先执行



##### 隐式调用无参的构造函数

如果子类构造函数里没有使用base关键字，那么父类的无参构造函数会被隐式调用

```csharp
public class BaseClass
{
    public int x;
    public BaseClass(){ x = 1;}
}
public class SubClass : BaseClass
{
    public SubClass() { Console.WriteLine(x);} //1
}
```

如果父类没有无参构造函数，那么子类就必须在构造函数里使用base关键字

##### 构造函数和字段初始化顺序

对象被实例化时，初始化动作按照如下顺序进行：

- 从子类到父类
  - 字段被初始化
  - 父类构造函数的参数被算出
- 从父类到子类
  - 构造函数体被执行

```csharp
public class B
{
    int x = 1;                  //Executes 3rd
    public B (int x)
    {
        ···                     //Executes 4th
    }
}
public class D ：B
{
    int y = 1;                  //Executes 1st
    public D (int X)
        :base (x+1)             //Executes 2nd(注意只执行base这一句)
    {
        ···                     //Executes 5th
    }
}
```



#### 重载和解析

```csharp
static void Foo(Asset a){}
static void Foo(House b){}
```

重载方法被调用时，更具体的类型拥有更高的优先级

```csharp
House h = new House();
Foo(h);  //calls Foo(House)
```

调用哪一个重载方法是在编译时就确定下来的

```csharp
Asset a = new House();
Foo(a);  //calls Foo(Asset)
```

