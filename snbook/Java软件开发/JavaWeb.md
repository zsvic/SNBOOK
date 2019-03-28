# 1 Servlet规范

## 1.1 简述
Servlet)是J2EE中WEB开发的一个重要组成部分，它规范了Java应用程序如何对外提供`请求-应答`类的服务。

> 虽然Servlet可以响应任何类型的请求，但以下只讨论基于HTTP协议的请求和应答。

## 1.2 Servlet接口

所有的Servlet都需要实现javax.servlet.Servlet接口，Servlet接口具体如下：
```java
Interface Servlet{
    void init();
    void service(ServletRequest req, ServletResponse res);
    ServletConfig getServletConfig();
    String getServletInfo();
    void destroy(); 
}
```
`javax.servlet.http.HttpServlet`类是Servlet接口的一个重要实现，它内部定义了大量与Http协议相关的方法，例如`doGet`,`doPost`等等，这些方法将在service方法中被调用。

## 1.3 Servlet生命周期
从Servlet接口可以看出，Servlet生命周期主要有`init`、`service`、`destroy`三个阶段。
- init：Servlet容器加载Servlet类，并创建一个实例，然后执行init方法进行初始化，这个过程只执行一次。
- service：Servlet容器每次处理一个请求时，需要生成`ServletRequest`对象和`ServletResponse`对象，然后调用service方法进行业务处理。
- destroy：当Servlet容器关闭或Servlet容器回收Servlet实例时，执行destroy方法。

> `ServletRequest`对象封装了Http的请求信息，service方法进行业务处理后，将结果信息存储到`ServletResponse`对象，WEB服务器最后会解析ServletResponse对象并响应WEB客户端。
  
## 1.4 Servlet容器

Servlet容器的作用就是管理Servlet的生命周期，并且使用Servlet对外响应请求。

当Servlet实例不存在时，需要加载该Servlet类，然后创建实例并进行初始化；然后根据请求信息，创建一对Request和Response对象作为service方法的参数，最后将Response作为返回结果。

### 1.4.1 Tomcat

Tomcat是一个小巧的Java WEB服务器，它包含连接器和Servlet容器两大部分：连接器负责接收客户端的请求，容器负责装载Servlet进行业务处理。
> 以下仅介绍部分原理

#### A 连接器
Tomcat连接器主要分为两个部分：
- Connector：实现了Runnable接口,使用一个单独的线程来监听客户端的请求，每收到一个请求，便将该请求的socket交给Processor处理，然后继续监听客户端请求。
- Processor：同样实现了Runnable接口，使用一个单独的线程来处理一个请求，它依据请求的socket生成Request和Response对象，然后通过ServletProcessor调用具体的Servlet容器。

#### B 容器
Servlet容器用来管理Servlet生命周期，它必须要实现`org.apache.catalina.Container`接口，Servlet容器实例需要传递给Processor，这样Processor才能调用Servlet容器。

Tomcat的Servlet容器是一个具有层级的结构：
- Wrapper：最低层的容器，装入并管理一个Servlet实例。
- Context：表示web应用程序，可以包含多个Wrapper。
- Host：表示一个虚拟主机，可以包含多个Context。
- Engine：表示整个Servlet容器。

> 以上四个容器都是继承`Container`接口，连接器可以调用任何一种容器，例如调用Context容器后，根据请求进一步调用其中的Wrapper容器，然后再调用Wrapper载入的Servlet实例。

##### B.1 Warpper接口
Warpper实现类的实例是要管理一个基础的Servlet对象的，Warpper接口中重要的方法有两个，`load`和`allocate`，load负责载入并初始化Servlet类，allocate负责分配一个已经初始化的Servlet实例，然后就可以调用该实例的service方法。

##### B.2 Context接口
一个Context通常代表一个应用程序，其中可以包括若干Wrapper，它使用映射器作为辅助来确定一个请求对应的Wrapper。

映射器类必须实现`org.apache.catalina.Mapper`接口，一个映射器将请求地址与Wrapper容器相关联，这样连接器或上级容器调用Context时，会搜索映射器来查找Wrapper。

#### C Session管理器
Tomcat使用一个Session管理器组件来管理Session对象，一个Session管理器必须与一个Context容器相关联，当一个请求到来时，Session管理器需要返回一个有效的Session对象，Servlet实例就可以通过Request接口实例的getSession方法获得一个Session对象。


# 2 JSP规范
一个JSP页面是一个文本文档，它描述了如何根据一个请求对象来创建一个响应对象。JSP文档通常被编译成一个Servlet，然后被Servlet容器加载并提供服务。


# 3 SpringMVC框架

# 4 JSF框架
