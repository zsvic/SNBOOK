
# 1 基础概念

## 1.1 JavaBean与POJO

JavaBean的诞生是为了支持可视化组件编程，它是通过对普通Java类制定一些规范，使其满足组件的特性，比如良好的封装与合理的接口以及优良的复用性。具体规范如下：
- 要有一个无参的构造方法：方便自动实例化
- 字段使用get/set方法对外暴露，这样字段和方法就可以使用统一的方式对外暴露
- 实现序列化接口：方便存储与复制

虽然Java的可视化并不成功，但是其JavaBean却在服务端流行起来，也算是无心插柳。随后，企业级JavaBean(EJB)被提出，用来提供企业级业务的开发。JavaBean的成功在于其简单的规范，而EJB过于繁重并且代码侵入性极高，使得其难以使用，因此引起了一场关于JavaBean的变革。

POJO(Plain Ordinary Java Object)就是一个普通的简单Java对象，这个名称的出现是为了区别与传统JavaBean和EJB，它不需要实现任何接口或继承特殊的类，也不需要强制遵循特殊的规范。

Spring主要操作的对象就是POJO，它可以让简单的POJO实现传统JavaBean以及EJB的功能，即一个可复用的、封装好的、接口易使用的组件，同时Spring也将这些POJO组件称为bean。

> 可以让POJO遵循一些规范，使其变成特定功能的对象，比如传统JavaBean，VO，DTO等等。

## 1.2 IOC/DI(控制反转/依赖注入)
IOC(控制反转)与DI(依赖注入)是两面一体的，通常可以认为是同一个过程。

在讨论控制反转或依赖注入时，首先要讨论类复用中`组合`的弊端。组合是指在一个类中创建其他类的对象，从而实现类的复用，这种做法带来了强依赖关系，这种耦合性会产生许多问题：
- 难以测试：在测试一个类时，需要保证内部所有依赖对象的可靠性，才能进一步测试该类。
- 不易修改：当修改一个类时，难以确定该类的对象在其他类中是否会产生意想不到的影响。

为了解决这个问题，控制反转(IOC)被提出，这个过程是指类不再需要自己创建它所依赖的对象，而是交给一个专门的第三方去集中创建并管理这些对象，然后通过类的构造方法或GET/SET方法讲它依赖的对象注入到该类中，这个注入的过程称为依赖注入(DI)。

以下比较组合与依赖注入的区别：
```java

//组合
class A1{
    public B b = new B();
    public A(){}
}

//依赖注入1
Class A2{
    public B b;
    public A(B b){
        this.b = b;
    }
}

//依赖注入2
Class A3{
    public AbstractB b;
    public A(AbstractB b){
        this.b = b;
    }
}
```
以上实例中，通过组合的方式，对A1进行测试时，出现问题时无法确认是A1本身代码出现问题，还是其依赖对象b出现问题，而通过依赖注入的A2，A3，在测试时可以构造一个完全正确的对象b进行测试，通过控制变量，很容易对A2，A3进行测试。

同时，在A3中，可以使用抽象类或接口进行声明，在依赖注入的时候可以更容易实现多态，比如可以传入不同类型的对象实现不同的操作；而A1只能使用对象b的方法，如果A1要进行扩展，需要依赖B的兄弟类(实现共同接口)，只能在A1中进行源代码扩展，不利于开闭原则。


## 1.3 AOP(面向切面的编程)
AOP主要的目的是将一些散布在应用各处的、实现同一功能的代码抽离出来集成为一个可重用的组件，这个类型的组件在应用中被称为一个切面，通过在应用的业务代码中配置切入点，讲切面的功能织入到业务代码中。


# 2 Spring核心

## 2.1 Spring容器
Spring容器负责管理Bean的生命周期，它负责创建并存储对象、装配对象以及销毁对象。Spring实现容器的基本方式是采用工厂模式，其BeanFactory是最简单容器，提供基本的DI支持。除此之外，Spring还基于BeanFactory提供了功能更强大的应用上下文(ApplicationContext)容器来支持复杂业务。

> Context(上下文)可以理解为环境变量。应用中获取到一个Context，即可获得该Context内存储的相关信息，当这个Context被多个类、模块、线程读取时，即实现了信息共享。

### 2.1.1 应用上下文(ApplicationContext)

Spring内置了许多应用上下文，可以通过加载配置文件来创建Bean。配置文件类型分为JavaConfig和XML，根据加载不同类型配置文件，应用上下文分为如下几种：
- 加载JavaConfig的应用上下文
    - AnnotationConfigApplicationContext
    - AnnotationConfigWebApplicationContext
- 加载XML的应用上下文
    - ClassPathXmlApplicationContext
    - FileSystemXmlApplicationContext
    - XmlWebApplicationContext

### 2.1.2 Bean生命周期


## 2.2 Bean装配

## 2.3 AOP配置

# 3 Sping Web
