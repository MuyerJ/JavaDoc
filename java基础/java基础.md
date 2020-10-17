# Java基础

### jdk8新特性

#### 1.函数式接口

- Consumer<T>

```xml
interface Consumer<T>
    void accept(T t)
    default Consumer<T> andThen(Consumer<? super T> after)
==================================
-接受一个泛型参数,然后调用一个accept方法，对这个参数进行一系列操作
-andThen(T)：
传入的参数是一个其他的Consumer接口
这个Consumer参数接口的泛型要和本接口的泛型一致
执行过程是：先执行本接口，再执行另外一个Consumer接口

==================================
例子：
Consumer<Integer> consumer = x->{
    System.out.println("inital value="+x);
    x = x + 2;
    System.out.println("step1:+2,value="+x);
    x = x + 5;
    System.out.println("step2:+5,value="+x);
};
consumer.accept(5);

```