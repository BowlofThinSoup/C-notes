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
public interface IUndoable       	   {void Undo();}
public interface IRedoable : IUndoable{ void Redo();}
```

IRedoable继承了IUndoable的所有成员

