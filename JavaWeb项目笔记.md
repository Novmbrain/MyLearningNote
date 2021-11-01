# JavaWeb项目笔记

## 第五阶段图书模块

### BeanUtils.populate方法使用

在执行BeanUtils.populate之后，会把map封装成User对象。要注意的是，UserBean类中的字段名必须和html中的name属性值相同，不然在BeanUtils.populate执行之后，Bean对象的字段中会出现NULL数据。

该方法的函数原型为:BeanUtils.populate( Object bean, Map properties )。这个方法会遍历map<key,value>中的key,如果bean中有这个属性，就把这个key对应的value值赋给bean的属性。

### BookServlet 程序中添加 add 方法

```java
protected void add(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
    // 1、获取请求的参数==封装成为 Book 对象
    Book book = WebUtils.copyParamToBean(req.getParameterMap(),new Book());
    // 2、调用 BookService.addBook()保存图书
    bookService.addBook(book);
    // 3、跳到图书列表页面
    // /manager/bookServlet?action=list
    // req.getRequestDispatcher("/manager/bookServlet?action=list").forward(req, resp);
    resp.sendRedirect(req.getContextPath() + "/manager/bookServlet?action=list");
}

当用户提交完请求，浏览器会记录下最后一次请求的全部信息。当用户按下功能键 F5，就会发起浏览器记录的最后一次请求。所以在完成添加图书，跳转回图书管理页面后，用户按下F5功能键仍然能够向其中添加图书。
    
若这个地方使用请求转发，而是要使用请求重定向。这样使得跳回图书管理页面的那个请求成为最后一个请求。用户按下F5也只会调用跳回图书管理页面
```

```java
在 web 中 / 斜杠 是一种绝对路径。
    
/ 斜杠 如果被浏览器解析，得到的地址是：http://ip:port/
<a href="/">斜杠</a>
/ 斜杠 如果被服务器解析，得到的地址是：http://ip:port/工程路径
1、<url-pattern>/servlet1</url-pattern>
2、servletContext.getRealPath(“/”);
3、request.getRequestDispatcher(“/”);
特殊情况： response.sendRediect(“/”); 把斜杠发送给浏览器解析。得到 http://ip:port/
```

### request.sendRedirect()

为了实现请求重定向，HttpServletResponse 接口定义了一个 sendRedirect() 方法，该方法用于生成 302 响应码和 Location 响应头，从而通知客户端重新访问 Location 响应头中指定的 URL，sendRedirect() 方法的完整语法如下所示：

public void sendRedirect(java.lang.String location) throws java.io.IOException

在上述方法代码中，参数 location 可以使用相对 URL，Web 服务器会自动将相对 URL 翻译成绝对 URL，再生成 Location 头字段

### Servlet查询数据并填充页面

当从页面A跳转到页面B，同时在页面B需要到数据库查询一些内容并显示。

这时不要直接从页面A跳转到页面B，因为MVC的限制，页面无法直接访问数据库。所以应该先跳转到对应的Servlet，由Servlet完成查询工作并返回需要显示的内容