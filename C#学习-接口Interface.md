# C#学习

[toc]

### Interface接口类型

#### 接口定义

- 接口与class类似，但是它只为其成员提供了规格，而==**没有提供具体的实现**==

- 接口的成员都是**隐式抽象**的

- 一个class或者struct可以实现多个接口

例如：

```csharp
public interface IEnumerator
{
    bool MoveNext();
    object Current{ get;}
    void Reset();
}
```



#### 接口的实现

接口的成员都是隐式public的，==不可以声明访问修饰符==

实现接口对它的**所有成员**进行public的实现

实现上例的接口：

```csharp
internal class Countdown : IEnumerator
{
    int count = 11;
    public bool MoveNext() => count -->0;
    public object Current => count;
    public void Reset(){ throw new NotSupportedException();}
}
```



#### 对象与接口的转换

可以隐式把一个对象转化成它实现的接口

```csharp
IEnumerator e = new Countdown();
while (e.MoveNext())
    Console.Write(e.Current);   
```

虽然Countdown是一个internal的class，但是可以通过把它的实例转化成IEnumerator接口来公共访问它的成员



#### 接口的扩展

接口可以继承其他接口

```csharp
public interface IUndoable       	   { void Undo();}
public interface IRedoable : IUndoable { void Redo();}
```

IRedoable继承了IUndoable的所有成员

 #### 显式的接口实现

实现多个接口的时候可能会造成成员签名的冲突

```csharp
interface I1 { void Foo();}
interface I2 { int Foo();}
```

通过显式实现接口可以解决这个问题，所谓显式就是写成`*<接口名称>.方法*`的形式

```csharp
punlic class Weight : I1,I2
{
    public void Foo()
    {
        Console.WriteLine("Widge's implementation of I1.Foo");
    }
    
    int I2.Foo()👈
    {
        Console.WriteLine("Widge's implementation of I2.Foo");
        return 42;
    }
}
```

本例中，想要调用相应实现的接口方法，只能把其**实例转化成相应的接口**才行

```csharp
Widge w = new Widge();
w.Foo();           //Widge's implementation of I1.Foo
((I1)w).Foo();     //Widge's implementation of I1.Foo
((I2)w).Foo();     //Widge's implementation of I2.Foo
```

另一个显示实现接口成员的理由是==故意隐藏那些对于类型来说不常用的成员==



如果方法和返回类型完全一样，那么只需要实现一次就可以了

```csharp
public interface IFoo { void Do();}
public interface IBar { void Do();}
public class : IFoo,IBar
{
    public void Do() => Console.WriteLine("Parent");👈
}
```



#### Virtual的实现接口成员

隐式实现的接口成员默认是**sealed**的

如果向要进行重写的话，必须==在基类中把成员标记为virtual或者abstract==

```csharp
public interface IUndoable { void Undo();}

public class TextBox : IUndoable
{
    public virtual void Undo() => Console.WriteLine("TextBox.Undo");
}

public class RichTextBox : TextBox
{
    public override void Undo() => Console.WriteLine("RichTextBox.Undo");
}
```



无论是转化为**基类**还是转化为**接口**来调用接口的成员，==调用的都是子类的实现==：

```csharp
RichTextBox r = new RichTextBox();
r.Undo();//RichTextBox.Undo
((IUndoable)r).Undo();//RichTextBox.Undo（转化为接口实现）
((TextBox)r).Undo();//RichTextBox.Undo（转化为父类实现）
```

==显式实现的接口成员**不可以被标记为virtual**，也不可以通过寻常方式来重写，但是可以对其进行**重新实现**。==



##### 在子类中重新实现接口

子类可以重新实现父类已经实现的接口成员

重新实现会“劫持”成员的实现（通过转化为接口然后调用），无论在基类中该成员是否是virtual的，无论该成员是显式的还是隐式的实现（最好还是显式地实现）

```csharp
public interface IUndoable { void Undo();}

public class TextBox : IUndoable
{
     void IUndoable.Undo() => Console.WriteLine("TextBox.Undo");
}

public class RichTextBox : TextBox,IUndoable👈
{
    public void Undo() => Console.WriteLine("RichTextBox.Undo");
}
```





#### 重新实现接口的替代方案

即使是显式实现的接口，接口的重新实现也可能有一些问题：

- 子类无法调用基类的方法
- 基类的开发人员没有预见到方法会被重新实现，并且可能不允许潜在的后果