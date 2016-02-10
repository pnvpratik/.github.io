---
layout:     post
title:      Value type
date:       2015-12-25
summary:    Value types.
categories: MVC6 Localization 
---

This looks like a amateur question , but don't be surprised if you get the answer wrong.
There is more to understand in value types and reference types than saying


Value types are data types which are stored on the stack while Reference types are stored on the Heap.

While this is a very popular statement which is used to differentiate value types and reference types , it is not entirely true.

In .NET primitive data types like int , double , float are value types and are generally stored on stack. The important word here is generally. It is not necessary that these primitive data types will always be stored on stack. E.g:

```csharp
Class OnHeap
{
       int i;
}
```

a object of above class will be stored on the heap.The variable i in for that object instance will also go on the heap.

Another very popularly distinction that you will find on the internet says "Classes are reference types and struct's are value types"

Does this statement hold good in .NET ? Yes and No
Yes because : Classes are reference types in .NET i.e. they are always stored on the heap.
No because : Although structs are value types , they are not always stored on the stack. Indeed sometimes structs are also stored on the heap. The important differentiate to understand as explained in Eric Lippert's Blog  is
Quote
the most relevant fact about value types is not the implementation detail of how they are allocated, but rather the by-design semantic meaning of “value type”, namely that they are always copied “by value”. If the relevant thing was their allocation details then we’d have called them “heap types” and “stack types”
What he is essentially trying to say is don't try to categorize value types by where they are stored , but categorize them by there design semantic meaning i.e. When you copy value types , they are always copied by value. Even if they are stored on the heap.


To understand the true meaning of what Eric is trying to say we need to have a detailed look at some of the value types and reference types in .NET

```csharp
int i;

Form f;
```
In .NET int is a primitive data type and is a value type, the MSDN documentation mentions that int is a built in type for System.Int32. If we have a look at the implementation System.Int32 in .NET using reflector, it looks something like this

```csharp
[Serializable, StructLayout(LayoutKind.Sequential), ComVisible(true)]
public struct Int32 : IComparable, IFormattable, IConvertible, IComparable, IEquatable
{
public const int MaxValue = 0x7fffffff;
public const int MinValue = -2147483648;
internal int m_value;
public int CompareTo(object value);
public int CompareTo(int value);
public override bool Equals(object obj);
public bool Equals(int obj);
public override int GetHashCode();
public override string ToString();
public string ToString(string format);
public string ToString(IFormatProvider provider);
public string ToString(string format, IFormatProvider provider);
public static int Parse(string s);
public static int Parse(string s, NumberStyles style);
public static int Parse(string s, IFormatProvider provider);
public static int Parse(string s, NumberStyles style, IFormatProvider provider);
public static bool TryParse(string s, out int result);
public static bool TryParse(string s, NumberStyles style, IFormatProvider provider, out int result);
public TypeCode GetTypeCode();
```
The important thing to notice here is Int32 is actually a struct, this is the key to making Int32 a value type. In .NET structs are always value types ( i.e they are copied by value). If you check the documentation for structs carefully on MSDN you will discover an interesting facts.
Structs cannot inherit from other structs, but the definition of struct shows that it itself is derived from System.ValueType which is inturn derived from System.Object.

This throws up a few questions :

How can struct inherit from a class ?
How can a object of Int32 struct which is derived from System.ValueType and System.Object and implements multiple level of inheritance be a value type ?

The answer although unsatisfactory from the design and consistency point of view is simple.
The class System.ValueType is treated as a special class, whenever the compile finds System.ValueType in the inheritance hierarchy of the object , it knows that this object is a value type and tries to accommodate the variables used in the Stack. It also does special optimization to make sure that the object/variable is stored in the stack. ( although this is not guaranteed )
The copy by value effect is achieved because System.ValueType overrides the Equals and Copy methods of the object class to create its own deep copy implementation.
The compiler also makes exception and allows struct to inherit from System.ValueType even though a normal class is not allowed to inherit from it.
The compiler shows the following error.

'DummyClass' cannot derive from special class 'System.ValueType' 

## Conclusion:


In summary one can say that structs are value types and classes are reference types in .NET.
Another important conclusion to remember is that when we say a particular object is value types it does not mean that the object is stored in stack. It just means that the object is always copied by value.
