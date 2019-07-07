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

```
一、简介：
    Stream是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。
    通过操作Stream API,可以非常高效且方便对数据源进行查找、过滤和映射数据等操作。类似于写sql一样。
二、注意：
    Stream 自己不会存储元素。
    Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream。
    Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。
三、操作步骤：
    1.创建 Stream
        通过一个数据源（如：集合、数组），来获取一个流
    2.中间操作
        一个中间操作链，对数据源的数据进行处理 
    3.终止操作(终端操作)
        通过一个终止操作，来执行中间操作链，并产生结果
```
- ***创建 Stream***

```
1.使用Collection集合提供的stream()和parallelStream()
     default Stream<E> stream() : 返回一个串行(顺序)流
     default Stream<E> parallelStream() : 返回一个并行流
2.使用Arrays类中的静态方法stream()获取数组流
     static <T> Stream<T> stream(T[] array): 返回一个流
3.使用Stream类中的静态方法of(),通过显示值创建一个流,它可以接收任意数量的参数。
     public static<T> Stream<T> of(T... values) : 返回一个流
4.使用Stream类中的静态方法iterate()或者generate()创建无限流
    迭代：public static<T> Stream<T> iterate(final T seed, final UnaryOperator<T> f) 
        eg：Stream<Integer> steram = Stream.iterate(0,(x) -> x+2);
    生成：public static<T> Stream<T> generate(Supplier<T> s)
        eg：Stream<Integer> steram = Stream.generate(() -> Math.random());
```

- ***中间操作***

```
多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止操作，否则中间操作不会执行任何的处理！而在终止操作时一次性全部处理，称为“惰性求值”。
------------------
1.筛选与切片
    filter(Predicate p)：接收Lambda，从流中排除某些元素。
    limit(long maxSize)：截断流，使其元素不超过给定数量。
    skip(long n)：跳过元素，返回一个扔掉了前n个元素的流。若流中元素不足n个，则返回一个空流。与limit(n)互补。
    distinct()：筛选，通过流所生成元素的hashCode()和equals()去除重复元素。
2.映射(提取)
    map(Function f)：接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。
        mapToDouble(ToDoubleFunction f)：接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的DoubleStream。 
        mapToInt(ToIntFunction f)：接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的IntStream。 
        mapToLong(ToLongFunction f)：接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的LongStream。
    flatMap(Function f)：接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流。
        注：map与flatMap类似于集合中的add和addAll之间的关系。
3.排序
    sorted()：产生一个新流，其中按自然顺序排序。
    sorted(Comparator comp)：产生一个新流，其中按比较器顺序排序。
```

- ***终止操作***

```
终端操作会从流的流水线生成结果。其结果可以是任何不是流的值，例如：List、Integer，甚至是void。
------------------
1.查找与匹配
    allMatch(Predicate p)：检查是否匹配所有元素。
    anyMatch(Predicate p)：检查是否至少匹配一个元素。
    noneMatch(Predicate p)：检查是否没有匹配所有元素。
    findFirst()：返回第一个元素。
    findAny()：返回当前流中的任意元素。
    count()：返回流中元素总数。
    max(Comparator c)：返回流中最大值。
    min(Comparator c)：返回流中最小值。
    forEach(Consumer c)：内部迭代(使用Collection接口需要用户去做迭代，称为外部迭代。相反，Stream API使用内部迭代——它帮你把迭代做了)。
2.归约
    reduce(T iden, BinaryOperator b)：可以将流中元素反复结合起来，得到一个值。返回T 
    reduce(BinaryOperator b)：可以将流中元素反复结合起来，得到一个值。 返回 Optional<T> 
    注：map和reduce的连接通常称为map-reduce模式，因Google用它来进行网络搜索而出名。
3.收集
    collect(Collector c)：将流转换为其他形式。接收一个Collector接口的实现，用于给Stream中元素做汇总的方法。
    注：Collector接口中方法的实现决定了如何对流执行收集操作(如收集到List、Set、Map)。但是Collectors实用类提供了很多静态方法，可以方便地创建常见收集器实例，具体方法与实例如下表：
        List<T> toList  把流中元素收集到List
            List<Employee> emps= list.stream().collect(Collectors.toList()); 
        Set<T> toSet  把流中元素收集到Set
            Set<Employee> emps= list.stream().collect(Collectors.toSet()); 
        Collection<T> toCollection  把流中元素收集到创建的集合
            Collection<Employee> emps =list.stream().collect(Collectors.toCollection(ArrayList::new));
        Long counting  计算流中元素的个数
            long count = list.stream().collect(Collectors.counting()); 
        Integer summingInt  对流中元素的整数属性求和
            int total=list.stream().collect(Collectors.summingInt(Employee::getSalary)); 
        Double averagingInt  计算流中元素Integer属性的平均值 
            double avg= list.stream().collect(Collectors.averagingInt(Employee::getSalary));
        IntSummaryStatistics summarizingInt  收集流中Integer属性的统计值。如：平均值 
            IntSummaryStatistics iss= list.stream().collect(Collectors.summarizingInt(Employee::getSalary));
        String joining  连接流中每个字符串 
            String str= list.stream().map(Employee::getName).collect(Collectors.joining());
        Optional<T> maxBy  根据比较器选择最大值 
            Optional<Emp> max= list.stream().collect(Collectors.maxBy(comparingInt(Employee::getSalary))); 
        Optional<T> minBy  根据比较器选择最小值 
            Optional<Emp> min = list.stream().collect(Collectors.minBy(comparingInt(Employee::getSalary))); 
        归约产生的类型 reducing  从一个作为累加器的初始值开始，利用BinaryOperator与流中元素逐个结合，从而归约成单个值 
            int total=list.stream().collect(Collectors.reducing(0, Employee::getSalar, Integer::sum)); 
        转换函数返回的类型 collectingAndThen  包裹另一个收集器，对其结果转换函数 
            int how= list.stream().collect(Collectors.collectingAndThen(Collectors.toList(), List::size)); 
        Map<K, List<T>> groupingBy  根据某属性值对流分组，属 性为K，结果为V 
            Map<Emp.Status, List<Emp>> map= list.stream().collect(Collectors.groupingBy(Employee::getStatus)); 
        Map<Boolean, List<T>> partitioningBy  根据true或false进行分区 
            Map<Boolean,List<Emp>> vd= list.stream().collect(Collectors.partitioningBy(Employee::getManage));
```


### `接口中的默认方法与静态方法`

### `新时间日期 API`

### `其他新特性`


## 参考链接

## 结束语

- 未完待续...
