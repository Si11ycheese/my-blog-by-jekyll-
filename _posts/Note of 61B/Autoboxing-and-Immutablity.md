## Autoboxing

### Autoboxing and Unboxing

![wrapper classes](https://joshhug.gitbooks.io/hug61b/content/assets/wrapper_classes.png)

Java可以在primitive和wrapper类型之间自动转换

### Caveats

1. 数组不是autoboxes或者auto-unboxed
2. Autoboxing和unboxing会对性能产生影响，依赖这种方式要比回避转换问题的代码缓慢许多
3. wrapper类型用更多的内存，在大多数电脑上，不禁要花费64 bit来保存对象，还要花费 64 bit来存储类似动态类型之类的

## Immutability

用**final**关键词可以使变量变成不可变状态

### Advantages

  1.可以更轻松调试，因为属性不变

  2.可以使得对象有具体的行为特征

### Disadvantages

 为了改变属性，需要创建一个新的对象

### Caveats

- 用**final**不会使引用指向的对象变成不可变，只会使得引用指针本身不可指向其他对象，而对象本身可变。

- 用**Reflection API**可以改变对象，甚至对private变量也起作用！

