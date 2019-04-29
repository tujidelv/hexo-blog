---
title: Java8新特性
date: 2019-03-30 15:04:29
categories:
- 开发语言
tags:
- java
---

# Java8 新特性

## 目录

- [简介](#简介)
- [正篇](#正篇)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

- 整理下 Java8 的所有新特性。 
    - `Lambda表达式`
    - `Stream API`
    - 全新时间日期 API
    - ConcurrentHashMap
    - MetaSpace
- Java8 的新特性对 java 的影响。
    - 更安全(Optional)
    - 更效率(HashMap,HashSet,ConcurrentHashMap,MetaSpace)
    - 简化书写(Lambda,Stream API)

## 正篇

### `Lambda 表达式`

```
一、简介：
    Lambda表达式相当于匿名内部类的更简化的写法，并且可以像数据一样进行传递。
    它需要函数式接口的支持。
二、基础语法：
    Java8中引入了一个新的操作符 "->"，该操作符被称为箭头操作符或Lambda操作符，箭头操作符将Lambda表达式拆分成两部分：
        左侧：指定了Lambda表达式需要的所有参数 
        右侧：指定了Lambda体，即Lambda表达式要执行的功能。
    上联：左右遇一括号省
    下联：左侧推断类型省
    横批：能省则省
```
```
语法格式一：无参数，无返回值，Lambda体只需一条语句
    Runnable r1 = () -> System.err.println("Hello Lambda!r1");
    r1.run();
语法格式二：有一个参数，无返回值
    Consumer<String> con = (arg) -> System.err.println("hello " + arg);
	con.accept("consumer!");
    
    若只有一个参数，参数的小括号可以省略不写
        Consumer<String> con = arg -> System.err.println("hello " + arg);
语法格式三：有两个以上的参数，有返回值，并且Lambda体中有多条语句
    Comparator<Integer> com = (x, y) -> {
        System.out.println("函数式接口");
        return Integer.compare(x, y);
    };
    
    若Lambda体中只有一条语句，return和大括号都可以省略不写
        Comparator<Integer> com = (x, y) -> Integer.compare(x, y);
语法格式四：Lambda表达式的参数列表的数据类型可以省略不写，因为JVM编译器根据程序的上下文推断出参数的数据类型，即“类型推断”
    Comparator<Integer> com = (Integer x, Integer y) -> Integer.compare(x, y);
    
    例如：
        String[] str = {"aaa","bbb","ccc"};
        List<String> list = new ArrayList<>();
```

### `函数式接口`

```
一、简介
    1. 只包含一个抽象方法的接口，称为函数式接口。 
    2. 可以通过Lambda表达式来创建该接口的对象。（若Lambda表达式抛出一个受检异常，那么该异常需要在目标接口的抽象方法上进行声明）。
    3. 可以在任意函数式接口上使用@FunctionalInterface注解，可以检查它是否是一个函数式接口，同时javadoc也会包含一条声明，说明这个接口是一个函数式接口。
二、自定义函数式接口
    @FunctionalInterface
    public interface MyFun<T>{
        public T getValue(T t);
    }
    public String toUpperString(MyFun<String> mf, String str){
        return mf.getValue(str);
    }
    @Test
    public void test(){
        String newStr = toUpperString((x) -> str.toUpperCase, "abcdeF");
    }
```
```
Java8内置四大核心函数式接口
    Consumer<T> 消费型接口
        包含方法： void accept(T t)
    Supplier<T> 供给型接口
        包含方法：T get();
    Function<T, R> 函数型接口
        包含方法：R apply(T t);
    Predicate<T> 断言型接口
        包含方法：boolean test(T t)
-------------------
@Test
public void test1(){
    happy(10000, (m) -> System.out.println("团建每次消费：" + m + "元"));
} 
public void happy(double money, Consumer<Double> con){
    con.accept(money);
}
@Test
public void test2(){
    String newStr = strHandler("\t\t\t 函数型接口   ", (str) -> str.trim());
    System.out.println(newStr);
}
public String strHandler(String str, Function<String, String> fun){
    return fun.apply(str);
}
...
```

### `方法引用与构造器引用`

```
方法引用：若Lambda体中的功能，已经有方法提供了实现，可以使用方法引用(可以将方法引用理解为Lambda表达式的另外一种表现形式)
    对象::实例方法
    类::静态方法
    类::实例方法
注意：
    ①方法引用所引用的方法的参数列表与返回值类型，需要与函数式接口中抽象方法的参数列表和返回值类型保持一致！
    ②若Lambda的参数列表的第一个参数，是实例方法的调用者，第二个参数(或无参)是实例方法的参数时，格式： ClassName::MethodName
------------------
Consumer<String> con = (x) -> System.out.println(x);
    等价于：Consumer<String> con = System.out::println;
BinaryOperator<Double> bo = (x,y) -> Math.pow(x,y);
    等价于：BinaryOperator<Double> bo = Math::pow;
BiPredicate<String,String> bp = (x,y) -> x.equals(y);
    等价于：BinaryOperator<Double> bo = String::equals;
```
```
构造器引用：构造器的参数列表，需要与函数式接口中参数列表保持一致！
    ClassName::new 
------------------
Function<Integer,MyClass> fun = (x) -> new MyClass(x);
    等价于：Function<Integer,MyClass> fun = Myclass::new;
```
```
数组引用：
    type[] :: new
------------------
Function<Integer,String[]> fun = (x) -> new String[x];
    等价于：Function<Integer,String[]> fun = String[]::new;
```

###  `Stream API`

### `接口中的默认方法与静态方法`

### `新时间日期 API`

### `其他新特性`


## 参考链接

## 结束语

- 未完待续...
