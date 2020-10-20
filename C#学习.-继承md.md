## C#学习

### 继承

#### 继承👑

- 一个类可以继承另一个类

- 被继承的类被称为父类或基类，继承的叫做子类

- 继承的类可以重用被继承的类的功能

- C#里，一个类只能继承于一个类，但是这个类可以被多个类继承

实例：

定义一个父类Asset

```csharp
public class Asset
{
    public string Name;
}
```

两个子类Stock和ouse分别继承自父类Asset

```csharp
public class Stock : Asset
{
    public long ShareOwned;
}
public class House : Asset
{
    public decimal Mortgage;
}
```

主函数里调用时

```csharp
Stock msft = new Stock { Name = "MSFT",ShareOwned = 1000};
Console.Writeline(msft.Name);//MSFT
Console.Writeline(msft.ShareOwned);//1000
House mansion = new House { Name = "Mansion",Mortgage = 250000};
Console.Writeline(Mansion.Name);//Mansion
Console.Writeline(Mansion.Mortgage);//250000
```

可以看到Name属性是父类定义的，子类一样可以调用



#### 多态🦋

==引用是多态的，类型为X的变量**可以引用其子类的对象**==

比如有一个父类的方法：

```csharp
public static void Display(Asset asset)
{
    System.Console.WiteLine(asset.Name);
}
```

现在可以将两个子类的对象作为参数传进这个方法

```csharp
Stock msft = new Stock···;
House mansion = new House···;
Display (msft);)👈
Display (mansion);)👈
```

因为子类具有父类的全部功能特性，所以参数可以是子类

但是反过来就不行

```csharp
static void main()
{
    Display(new Asset());👈//will not accpt Asset
}
public static void Display(House house))
{
    System.Console.WiteLine(house.Mortgage);
}
```



#### 引用转换🔄

一个对象的引用可以隐式地转换到对其父类的引用（向上转换）

想转换到子类的引用则需要显示转换（向下转换）

引用转换：创建一个新的引用，它也指向同一个对象 

##### Upcast向上转换

从子类的引用创建父类的引用

```csharp
Stock msft = new Stock();
Asset a = msft;👈
```

变量a依然指向同一个Stock对象（maft也指向它）

```csharp
Console.WriteLine(a == msft);//True
```

尽管变量a和msft指向同一个对象，但是a的**可视范围更小**一点

```csharp
Console.WriteLine(a.Name);//OK
Console.WriteLine(a.ShareOwned);//Error
```



##### Downcast 向下转换

从父类的引用创建子类的引用

```csharp
Stock msft = new Stock();
Asset a = msft;//Upcast
Stock s = (Stock)a;👈//Downcast
Console.WriteLine(s.ShareOwned);//<No Error>
Console.WriteLine(s == a);//True
Console.WriteLine(s == msft);//True
```

向下转换和向上转换一样，==只涉及到引用，底层的对象不会受到影响==

需要**显示转换**，因为可能会失败

```csharp
House h = new House();
Asset a = h;//Upcast always succeed
Stock s = (Stock)a;//downcast fails:a is not a Stock
```

如果向下转换失败，那么会抛出InvalidCastExpression（属于运行时类型检查）



##### AS操作符 

as操作符会执行向下转换，如果转换失败，**不会抛出异常**，值会变为null

```csharp
Asset a = new Asset();
Stock s = a as Stock;  //s is null;no exception thrown
```

as操作符无法做自定义转换

```csharp
long x = 3 as long; //compile-time error
```

