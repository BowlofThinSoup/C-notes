# C#学习

## Struct与访问修饰符

[toc]

### Struct

#### Struct与Class区别

struct和class差不多，但是有一些不同：

- struct是**值类型**，class是**引用类型**
- struct不支持继承 (除了隐式地继承了object，也就是System.ValueType)



#### Struct的成员

class能有的成员，struct也可以有，但是以下几个不行：

- 无参构造函数
- 字段初始化器
- 终结器
- virtual或protected成员



#### Struct的构建

struct有一个无参数的构造函数，但是==不能对其重写==。它会对字段进行**按位归零**操作

当定义struct构造函数的时候，必须==**显式地为每个字段赋值**==

不可以有字段初始化器

```csharp
public struct Point
{
    int x,y;
    public Point (int x,int y){ this.x = x,this.y = y};
}
···
Point p1 = new Point();//p1.x and p1.y will be 0
Ponit p2 = new Point(1,1);//p1.x and p1.y will be 1
```

有三个错误的例子：

```csharp
public struct Point
{
    int x = 1;//Illegal:field initializer
    int y;
    public Point(){}//Illegal:parameterless constructor
    public Point(int x){ this.x = x;}//Illegal:must assign field y
}
```

错误点：

1. 不能直接对字段x初始化

2. 不能定义无参构造函数
3. 自定义构造函数不能只对x一个值赋值



### 访问修饰符

#### 5个访问修饰符

- public：完全可访问。==enum和interface的**成员**默认都是这个级别==
- internal：当前assembly或朋友assembly可访问，==非嵌套**类**的默认访问级别==
- private：本类可访问。==class和struct的**成员**默认访问级别==
- protected：本类或其子类可以访问
- protected internal：联合了protected和internal的访问级别

例子：

```csharp
class Class{}         //Class is internal(default)
public class Class2{}
```

```csharp
class ClassA{ int x;} //x is private(default)
class ClassB{internal int x;}
```

```csharp
class BaseClass
{
    void Foo()  {}     //Foo is private(default)
    protected void Bar() {}
}
class Subclass : BaseClass
{
    void Test1(){Foo();}//Error:cannot access Foo
    void Test2(){Bar();}//OK
}
```



#### 友元Assembly

通过添加System.Runtime.CompilerServices.InternalsVisibleTo这个Assembly的属性，并指定朋友Assembly的名字，就可以把internal的成员函数暴露给朋友Assembly

```csharp
[assembly:InternalsVisibleTo("friend")]
```

如果朋友Assembly有Strong name，那么就必须指定其完整的160字节的public key

```csharp
[assembly:InternalsVisibleTo("Strongfriend,PublicKey = 0024f000048c···")]
```

==单元测试时经常使用朋友Assembly==



#### 类型限制成员的访问级别

```csharp
class C{public void Foo(){}}
```

内部访问级别是public，外部是internal，内部比外部要大，但其实内部最大访问范围也不会超过外层



##### 访问修饰符的限制

当重写父类的函数时，重写后的函数和被重写的函数的**访问级别必须一致**

```csharp
class BaseClass               { protected virtual void Foo(){}}
class Subclass1 : BaseClass   { protected override void Foo(){}}//OK
class Subclass2 : BaseClass   { public 👈 override void Foo(){}}//Error
```

==有一个例外：当其它Assembly重写protected internal的方法时，重写后的方法必须是protected==