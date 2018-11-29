---
title: SpringBoot 简介
tags:
  - SpringBoot
  - 学习笔记
categories: 学习笔记
keywords: '简介,入门'
date: 2018-11-28 22:24:04
url_suffix: springboot
---
### 前言
作为一个SpringBoot新人，在学习中有诸多疑惑，慢慢梳理出来、但求拨云见日。
### SpringBoot和Spring
Spring是容器，是管理对象生命周期的容器。开发的时候，由Spring容器来处理对象的生命周期。Spring容器能够帮助我们来管理生命周期的前提是我们需要对Spring进行配置。而SpringBoot的出现，简化了这种XML的配置过程，这也是SpringBoot的一大优势。当然，SpringBoot也不单单仅有这一个优势，更多的优势可以自行百度一下。
### HelloWorld
SpringBoot自带了java web服务器，引入SpringBoot后，在main方法中注册Application启动类，即可运行。页面输出HelloWorld，需要配置一个@Controller，用来响应浏览器请求。
### 控制器
@Controller 用来处理http的请求，@RestController用来返回Json数据格式，@RequestMapping用来配置Url映射。可以使用GetMapping来简化Get操作。
{%codeblock Controller  lang:java %}
public class HelloController {
    @RequestMapping(value = "/hello",method = RequestMethod.GET)//等价于@GetMapping(value="/hello")
    @ResponseBody
    public String sayHello() {
        return "Hello，Spring Boot！";
    }
}{%endcodeblock%}<!-- more-->
当注解写成@RestController的时候，可以直接返回一个序列化的JSON对象。下面的代码将返回一个JSON格式的CMB对象。
{%codeblock RestController  lang:java %}
@RestController
@RequestMapping("/config")
public class ConfigController {   
    @RequestMapping("/hr")
    public CMB currentUser() {
        return CMBUtils.getCurrentCMB();
    }
}{%endcodeblock%}
Controller可以返回字符串，可以返回序JSON列化后的对象，那要怎么返回HTML页面呢？可以使用前端页面模板。
### thymeleaf
首先在配置文件中引入thymeleaf的包，然后再Controller中编写响应请求代码，返回html的页面名字。其中，ModelMap是用来与Html页面进行数据交互的。
{%codeblock Controller  lang:java %}
@RequestMapping(method = RequestMethod.GET)
public String getBookList(ModelMap map) {
    map.addAttribute("bookList",bookService.findAll());
    return "bookList";
}{%endcodeblock%}
bookList.html的页面部分内容如下所示：
{%codeblock bookList.html  lang:html %}
<tr th:each="book : ${bookList}">
  <th scope="row" th:text="${book.id}"></th>
  <td><a th:href="@{/book/update/{bookId}(bookId=${book.id})}" th:text="${book.name}"></a></td>
  <td th:text="${book.writer}"></td>
  <td th:text="${book.introduction}"></td>
  <td><a class="btn btn-danger" th:href="@{/book/delete/{bookId}(bookId=${book.id})}">删除</a></td>
</tr>{%endcodeblock%}
### 数据库操作
SpringBoot可以方便的连接多种数据库，比如MySql，ElasticSearch等。在学习过程中梳理出了以下几个需要了解的点：
* ORM框架主流的有hibernate以及mybatis，区别在于前者无须写sql，通过对象操作数据库，后者通过编写更加灵活的sql语句来操作数据库。
* ORM是JPA规范中的一个思想体现，JPA规范包含了ORM。
* JPA、spring-data-jpa、hibernate三者之间的关系，JPA是Java Persistence API缩写，是JAVA持久化规范，spring-data-jpa对具体的JPA实现做了封装，默认采用hibernate实现，能够方便大家在不同的ORM框架之间进行切换而不需要更改代码。

操作步骤：1、引入spring-data-jpa；2、编写Entity类；3、编写Repository类；4、调用方法
{%codeblock Book.java  lang:java %}
@Entity
public class Book implements Serializable {
    @Id
    @GeneratedValue
    private Long id;
    private String name;
    private String writer; 
   ....get set
   ....
{%endcodeblock%}
{%codeblock BookRepository.java  lang:java %}
/**
 * Book 数据持久层操作接口
 */
public interface BookRepository extends JpaRepository<Book, Long> {
}
{%endcodeblock%}
{%codeblock BookServiceImpl.java lang:java %}
/**
 * Book 业务层实现
 */
@Service
public class BookServiceImpl implements BookService {

 @Autowired
 BookRepository bookRepository;

 @Override
 public List<Book> findAll() {
        return bookRepository.findAll();
 }
......
{%endcodeblock%}
从上面代码中可以看到，在业务服务层只需要引入BookRepository后，就可以操作数据库中相关数据，很方便。已经具备了搭建一套对外服务的功能，包括响应请求、操作数据、返回数据。
### 代码结构
规范的SpringBoot工程分层结构应该包括如下项：{% asset_img 1.jpg 工程结构 %}
### 应用监控
sprig-boot-starter-actuator
### SpringCloud 






