#Typedefs

在 Dart 中，方法是对象，就像字符串和数字也是对象。typedef ,又被称作函数类型别名，让你可以为函数类型命名，并且该命名可以在声明字段和返回类型的时候使用。当一种函数类型被分配给一个变量的时候，typedef 会记录原本的类型信息。  

考虑下面的代码，哪一个没有使用 typedef。

```
class SortedCollection {
  Function compare;

  SortedCollection(int f(Object a, Object b)) {
    compare = f;
  }
}

 // Initial, broken implementation.
 int sort(Object a, Object b) => 0;

main() {
  SortedCollection coll = new SortedCollection(sort);

  // All we know is that compare is a function,
  // but what type of function?
  assert(coll.compare is Function);
}

```

当  `f` 分配到 `compare` 的时候类型信息丢失了。`f`的类型是 `(Object, Object)` → `int`(→ 意味着返回的)，然而`compare` 的类型是方法。如果我们使用显式的名字更改代码并保留类型信息，则开发者和工具都可以使用这些信息。

```
typedef int Compare(Object a, Object b);

class SortedCollection {
  Compare compare;

  SortedCollection(this.compare);
}

 // Initial, broken implementation.
 int sort(Object a, Object b) => 0;

main() {
  SortedCollection coll = new SortedCollection(sort);
  assert(coll.compare is Function);
  assert(coll.compare is Compare);
}

```  

**请注意** 目前 typedefs 仅限于函数类型，我们期望这一点能有所改变。

因为 typedefs 是简单的别名，所以它提供了一种方法来检查任何函数的类型。比如： 

```
typedef int Compare(int a, int b);

int sort(int a, int b) => a - b;

main() {
  assert(sort is Compare); // True!
}
```