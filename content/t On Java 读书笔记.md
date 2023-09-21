
#临时

对于较大的数字，可以使用下划线分割，方便阅读。

当创建一个内部类时，这个内部类的对象会隐含一个链接，指向用于创建该对象的外围对象。通过该链接，无需任何特殊条件，内部类对象就可以访外围对象的成员。此外，内部类拥有对外围对象所有元素的访问权。

在内部类中，this指向的是内部类，所以要获取外部对象时，要写`外部对象的类名.this`。

当需要向上转型为基类，特别是接口时，内部类就更有吸引力了。因为内部类对外部而言是不可见的、不可用的，这便于隐藏实现。外部获得的只是一个指向基类或接口的引用。

```java
public interface Destination {
    String readLabel();
}

class Parcel4 {
    private class PDestination implements Destination {
        private String label;
        private PDestination(String whereTo) {
            label = whereTo;
        }
        @Override
        public String readLabel1() {
            return label;
        }
    }
}
```
接口的实现被隐藏了。

使用内部类的两大理由：
1. 要实现某种接口，以便创建和返回一个引用。内部类完善了多重继承问题的解决方案。如果一个类必须以某种形式继承两个类，那么可以由内部类继承一个，外部类继承另外一个。
2. 要解决一个复杂问题，在自己的解决方案中创建了一个类来辅助，但是不想让辅助类公开。

嵌套类指用static修饰的静态内部类。

创建栈建议使用`ArrayDeque`，而不是`Stack`和`LinkedList`。
不使用`Stack`的原因：Stack 继承 Vector，Vector 线程安全，效率低，Stack的实现兼容了Vector的很多无用方法。
不使用`LinkedList`的原因：和`ArrayList`与`LinkedList`的选取理由类似，使用数组避免了频繁地创建和销毁链表节点，可以利用CPU cache的连续性，

HashMap是为快速访问设计的，TreeMap将键以有序的方式保存。LinkedHashMap按照元素的插入顺序来保存元素，但通过哈希提供了快速访问的能力。 

绝大多数的函数式接口都在`java.util.function`包中定义了，不需要自己写

高阶函数是一个能接受函数为参数或能把函数当返回值的函数。

函数组合指将多个函数结合使用，以创建新的函数。`java.util.function`中支持函数组合的方法有：
- andThen(argument)：先执行原始操作，再执行参数操作
- compose(argument)：先执行参数操作，再执行原始操作
- and(argument)：对原始谓词和参数谓词执行短路逻辑与计算
- or(argument)：对原始谓词和参数谓词执行短路逻辑或计算
- negate()：所得谓词为该谓词的逻辑取反

柯里化指将一个接受多个参数的函数转变为一系列只接受一个参数的函数：
```java
Function<String, Function<String, String>> sum = a -> b -> a + b;
Function<String, String> hi = sum.apply("Hi ");
// Hi Ho
System.out.println(hi.apply("Ho"));
```