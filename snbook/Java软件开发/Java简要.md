# 1 基础语法

## 1.1 常见内置类型

Java中变量存储的内容可以分为两类，一个为基本类型，一个为引用类型。


### 1.1.1 基本类型

每种基本类型所占存储空间的长度都是固定的。基本类型的种类和长度如下：
- 整数：byte 1字节，short 2字节，int 4字节，long 8字节
- 浮点型：float 4字节，double 8字节
- 字符型：char 2字节
- boolen：根据Jvm实现确定

基本类型初始化
```java
int a = 1;
```

### 1.1.2 数组
数组是一个特殊的内置类型，当创建一个数组对象时，返回一个引用。数组对象的类型为 [+type，例如：
```java
//一维数组，a的类型为：[int
int[] a = new int[n];
//二维数组，b的类型为：[[int
int[][] b = new int[n][m];
```

### 1.1.3 字符串
Java提供的String类，内部是由一个final的char数组来存储字符串的。

#### 字符串常量与String堆内对象
字符串常量和new出来的String对象虽然都是String类的实例，并且是不可变的，但是他们还是有很大区别的，字符串常量在编译时就放入常量池，String对象在程序运行时才从堆中分配。
```java
String a = "abc";
String b = new String("abc");
String c = "a" + "bc";
String d = "a" + new String("bc");
// a == c != b != d
```
上述a，b，c中
- a在编译时直接分配内存空间，存入常量池
- b在运行的时候在堆中建立String对象
- c在编译的时候进行优化，自动合并成 "abc"，与a为同一个对象
- d在运行时，实际是隐含使用StringBuider进行字符串连接，返回一个String对象（在堆中）

#### StringBuilder与StringBuffer
String对象都是不可变的，这是为了减少String运算产生大量的中间变量，在字符串连接等操作时会有效率问题，因此Java提供了StringBuilder来增强字符串的操作。

StringBuilder内部使用一个char数组来存储字符串，并且长度可以动态增长，可以使用append方法连接字符串，使用toString方法返回一个String对象。

StringBuffer和StringBuilder的功能一样，但是它是线程安全的，效率比StringBuilder要低。

#### 正则表达式

pass


## 1.2 操作符

操作符通常接受一个或多个参数（操作数），并生成一个新值。

### 赋值操作符

赋值操作使用‘=’，赋值大体分为两类：
- 基本类型赋值
- 引用类型赋值

基本类型赋值是进行内容复制操作，如下操作中，首先将常数4复制到变量a的内存空间中，然后再将a的内容进行复制到b的内存空间，a和b是两个独立的变量，它们的更改并不会影响对方。
```java
a = 4;
b = a;
```

引用类型赋值的本质也是进行内容复制，但复制的是对象的引用，这样通常会使两个变量指向同一个对象，那么以他们为句柄进行修改对象时，是会相互影响的，例如以下变量c和d指向同一对象。
```java
c = new Object();
d = c;
```
### 算数运算符

算数运算符通常只能作用于基本类型（除了‘+’重载可以对字符串进行连接），算数运算符有以下几类：

- 基础：加号（+），减号（-），乘号（*），整除（/)，取模（%）
- 一元加（+），一元减（-）
- 简化符号：+= ，-=，*=，/=，%=

> 基础运算符对基本类型运算时，会自动进行类型扩展；一元运算符改变数值正负；**简化符号在赋值时会自动强制转换成目标类型**

```java
short a = 1;
a = a+1; //编译不通过，a+1自动扩展类型为int型，赋值时类型不正确。
a += 1; //a+1后的值，在赋值时会强制转换成short，等价于 a=(short)(a + 1);
```
### 递增和递减

递增（++）和递减（--）可以非常方便地进行线性单位运算，它可以分为两类：
- 前缀式（++a）
- 后缀式（a--）

前缀式通常是将运算的结果作为返回值，后缀式是临时保存运算前的值，再进行运算，并且将临时保存的值作为返回值。
> 上述过程通常表现为：前缀式(++a)表示先做运算，再进行赋值；后缀式(a--)表示先赋值，再进行运算。
> 当有编译器优化的时候，会通过调整执行顺序，使得后缀式不必临时保存变量，可以让前缀式和后缀式的效率一样，但是有些情况并不能总是优化，所以如果在不涉及赋值的情况下，优先使用前缀式。

### 关系操作符

- 基本类型：等于（==），大于（>），小于（<）
- 对象比较：equals()

> Java内置对象大都重写了equals方法，可以很方便使用equals比较对象内容，但是自建对象继承Object对象时，其equals方法默认比较引用。

### 其他操作符

- 逻辑操作符：&&，||，!
- 常量标志：F(float)，D(double)，L(long)，0X(16进制)，0(8进制)
- 位操作符：&(位与)，|(位或)，^(异或)，~(非)
- 位移操作符：<<(左移，低位补0)，>>(右移，符号位扩展，高位补0)，>>>(无符号右移，不管正负，符号位插0)
- 三元操作符：A ? B : C; (A为真，返回B，否则返回C)
  

## 1.3 控制执行
控制执行流程大体分为 **判断**，**循环**，**跳出**，**返回**。




# 2 面向对象

## 2.1 封装：类与对象
类表示具有同一特征事物的集合，它通常由属性（字段），操作（方法）组成。对象是类中具体的某一个实例。

### 2.1.1 类的定义
类使用Class关键字定义
```java
public class MyClass{
    public Type Feild;
    public MyClass(){}
    public Type Func(){}
}
```
### 2.1.2 对象初始化
对象的创建与初始化主要是通过构造器来完成，使用构造器的途径通常有两个：
- 使用new关键字与构造器创建并初始化对象
- 使用反射机制获取到类的构造器实例，通过构造器实例创建并初始化对象。

```java
//new
MyClass mc = new MyClass();

//反射
Class  clazz=Class.forName("MyClass");   
MyClass mc = clazz.newInstance();//无参

Constructor  c=clazz.getConstructor(String.class,int.class);
MyClass mc = c.newInstance("String",1); //有参 
```


### 2.1.3 访问权限
Java中访问权限有四种，Public，Protected，Default，Private。
- 类访问权限：类的访问权限只有两种：Public，Default。
    - Public表示任何位置可以使用
    - Default为缺省值，表示包访问权限
- 类中成员访问权限：
    - Public：表示类外的所有地方都可以访问
    - Protected：表示包内以及包外子类可以访问
    - Default：默认的包访问权限
    - Private：仅该类内访问

### 2.1.4 内部类
将一个类的定义放到另一个类的内部，这个内部类可以访问外部类的所有成员，外部类需要使用内部类对象来访问内部类(类似于成员方法)。

#### 内部类的初始化
内部类创建对象时，会自动包含一个引用指向一个外部类对象，这也是为什么内部类可以访问外部类成员的原因。其创建和初始化都要依赖于外部类(非Static内部类)。
- 在外部类中直接创建内部类对象，内部类对象自动包含外部类的this引用
- 在其他地方创建内部类对象需要使用外部类对象使用```.new```完成创建，这时自动包含该外部类的引用。

```java

public class OuterClass
    //外部类直接创建内部类对象
    InnerClass ic = new InnerClass();
    public class InnerClass{}
}
//其他地方创建内部类时，需要依赖外部类
OuterClass oc = new OuterClass();
OuterClass.InnerClass ic = oc.new InnerClass();
```
#### 内部类的访问权限
内部类的访问权限不同于一般的类，而和类中成员相似，可以拥有四种访问权限，当内部类为private时，只有外部类能够访问，这样可以彻底隐藏内部类。

### 2.1.5 匿名类




## 2.2 继承：继承与组合




## 2.3 抽象：抽象类与接口

## 2.4 多态：重写与重载


# 3 高级特性

## 3.1 类型信息与反射
类型信息与反射
## 3.2 泛型
泛型简单的说就是类型参数化，它可以支持运行时才确定对象的类型，比如构造方法的形参的类型在类的初始化化时才确定，同时也可以保障类型安全。它主要的目的之一就是指定容器所持有什么类型的对象。

使用：在类定义的时候，在类名后使用尖括号作为类型参数，
```java
public class A<T>{
    public T a;
    public A(T a){
        this.a = a;
    }
}

A<String> a = new A<String>("a");
```


## 3.3 注解
注解是一种特殊的接口，是在代码中添加额外信息的一种形式化方法，通过注解处理器对这些信息进行处理和利用。

没有注解时，这些额外的信息通常是通过配置文件来配置的，配置文件虽然降低了代码的耦合性，但是也使得代码编写不统一。注解的好处就是以Java语句来表达这些额外信息，使得代码编写得到统一，减少额外配置，并且能够在编译期对这些信息进行类型检查。

### 3.3.1 注解使用
注解使用@符号作为前缀，使用方法与其他修饰符一样可以修饰类，方法，字段，变量等。

#### 常用内置注解
- @Override：表示重写
- @Deprecated：表示弃用，仍然可以，但是不推荐使用，在新的版本中有新的字段，方法或类可以进行替换。
- @SuppressWarnings：关闭警告信息

### 3.3.2 自定义注解
自定义注解通常分为两部分，一个是使用@interface对注解进行定义，另一个是在注解使用时，需要编写注解处理器来处理注解以及注解中的元素。

#### 注解定义

注解的定义非常简单，如下所示，与接口的定义有点相似。

```java
@Target(ElementType.METHOD)
public @interface MyAnnotation{
    public int id() default 1;
}
```
上述注解定义中，涉及两点：注解元素，元注解。
- 注解元素：注解的元素使用方法来定义，与接口中的方法几乎一样，唯一的区别在于注解元素可以使用default来设置默认值。注解元素可用的类型如下：
    - 基本类型
    - String
    - Class
    - enum
    - Annotation
- 元注解：简单的说就是注解的注解，它可以对注解进行配置。元注解有以下四个：
    - @Target：表示该注解可用在什么地方（字段，方法，类等等）
    - @Retention：表示注解用在什么级别（SOURCE（源代码级别，被编译器丢弃），CLASS（字节码级别，被JVM丢弃），RUNTIME（运行时级别，注解始终存在，可以用反射处理））
    - @Documented
    - @Inherited：允许子类继承父类注解

> 注解会编译成class文件
> 注解的元素一定要有个“非空”的确定值



#### 注解处理器

如果没有注解处理器，那么注解的将不会起到任何作用，因此在自定义注解中，编写注解处理器是必要的。
注解处理器通常使用反射机制、外部工具apt来实现。

- **简单反射机制**：当注解的Retention级别为RUNTIME时，可以直接使用反射来获取对象的方法集合(元素为Method类型的对象)，通过遍历Method对象，获取到每个方法对象的注解，然后针对该注解进行处理。（同样方式可以处理类，字段上的注解）

```java

public class classType{
    @AnnotationType(id = 1)
    public void function(){}
}

public class AnnotationProcess{
    classType c = new classType();
    for(Method m : c.getDeclaredMethods()){
        AnnotationType at = m.getAnnotation(AnnotationType.class);
        if(at != null){
            at.id();
        }
    }
}

```
- **注解处理工具apt**：apt是直接操作源文件，将源文件的注解进行处理，若在此过程生成新的源文件，则进行新一轮的处理，所有源文件的注解处理完毕后，再一起进行编译。
  
使用apt，需要继承```javax.annotation.processing.AbstractProcessor```类，并重写其中的```process```方法
```java

public class MyAnnotationProcessor extends AbstractProcessor {

	@Override
	public boolean process(Set<? extends TypeElement> annotations,
			RoundEnvironment roundEnv) {
		// TODO Auto-generated method stub
		return false;
	}

}
```


## 3.4 并发

## 3.5 IO管理



