### Java基础--装箱拆箱，包装类和常量池

自动装箱与拆箱（Autoboxing and unboxing）
***

___---  为何要进入封装类？有何好处？___

    不满足面向对象的开发思想
    可以定义成员变量，成员方法，方便类型转换

    Java基本类型的包装类都实现了常量池技术，超出范围仍然会创建对象
***


```java
Integer a = 10;//自动装箱 将基本数据类型转为包装器类型
int b = a;//自动拆箱 将包装器类型转换为基本数据类型
```
    在执行第一句代码时，系统调用了
    Integer a = Integer.valueOf(10);
    第二句时则调用了
    int b = a.intValue();

```java
public static Integer valueOf(int i){
    //不在这个区间时，就--创建--一个新的对象
    //在这个区间内就去--引用--同一个对象
    return i>=128||i<-128 ? new Integer(i):SMALL_VALUES[i+128]; 
}
```
```java
//两个Integer的构造函数
private final int value;
public Integer(int value){
    this.value = value;
}
public Integer(String string) throws NumberFormatException {
    this(parseInt(string));
}
```
    装箱的过程会创建对应的对象，会消耗一定的内存，影响性能
关于intValue函数
```java
public int intValue(){
    return value;
}
```

关于valueOf函数，存在两个归类
- Integer派别： Integer,Short,Byte,Character,Long这几个类的valueOf方法实现是类似的
- Double派别： Double和Float的valueOf方法实现是类似的。每次都返回不同的对象

```java
//Double中的vauleOf
public static Double valueOf(double d){
    return new Double(d);
}
```
    其中的原因是因为，拿int举例
    在（-128，127]之间只有固定的256个值，所以可以预先创建一个大小为256的SAMALL_VALUES，所以值在该范围内时，就可以返回事先创建好的对象

    但是对于Double,这个范围区间内有无数个值，无法进行这样的操作

一些注意事项：

    1 当一个基础数据类型与封装类进行==,+,-,*,/运算时,会将封装类进行拆箱，对基础数据进行运算
    2 小心空指针  
    Integer a = null;
```java
Integer num1 = 100;
int num2 = 200;
Long num3 = 300l;
System.out.print(num3.equals(num1 + num2));//False

@override
public boolean equals(Object o){
    return (o instanceof Long) && (((Long) o).value == value);
}
```
    2 上述代码需满足两个条件才为true
    - 类型相同  - 内容相同
```java
int num1 = 100;
int num2 = 200;
long num3 = 300;
System.out.print("num3 = (num1 + num2)");//true
```
***
    总结:
    1 需要知道什么时间进行拆箱与装箱
    2 装箱操作会创建对象，频繁进行该操作会消耗内存，影响性能，所以尽量避免该操作
    3 equals(Object o)因为原equals方法中的参数类型时封装类型，所传入的参数是原始数据类型，会对其自动装箱，反之，会对其进行拆箱
    4 当两种不同类型进行 == 比较时，包装器类需要拆箱，同种类型进行比较时，会自动拆箱装箱
