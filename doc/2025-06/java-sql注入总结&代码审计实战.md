#  java-sql注入总结&代码审计实战   
 渗透安全团队   2025-06-05 03:53  
  
****  
# 前言  
  
我们要先明确，java是强类型语言，实战我想我们也遇到不少尝试sql注入但是遇到java强制类型要求，参数只能是integer类型的java报错，这种是不存在sql注入的，所以只有变量是String类型的时候，才会存在sql注入。  
  
现在就来看看常规的可以执行sql语句的程序：  
# JDBC  
  
具体的介绍就不说了，直接进入正题吧！  
### 1、jdbc的java.sql.Statement  
  
Statement是Java JDBC下执行SQL语句的一种原生方式，执行语句时需要通过拼接来执行。若拼接的语句没有经过过滤，将出现SQL注入漏洞  
```
public static void main(String[] args) {         // 数据库连接信息         String url = "jdbc:mysql://localhost:3306/security";         String username = "root";         String password = "root";         Connection connection = null;         Statement statement = null;         ResultSet resultSet = null;         try {             // 加载数据库驱动             Class.forName("com.mysql.cj.jdbc.Driver");             // 获取数据库连接             connection = DriverManager.getConnection(url, username, password);             // 创建 Statement 对象             statement = connection.createStatement();             // 执行 SQL 查询             String sql = "select * from users where id=1";  //这里是String             resultSet = statement.executeQuery(sql);             // 处理查询结果             while (resultSet.next()) {                 // 假设表中有 id 和 name 两列                 int id = resultSet.getInt("id");                 String name = resultSet.getString("username");                 String pass = resultSet.getString("password");                 System.out.println("ID: " + id + ", Name: " + name + ", Password: "+pass);             }         } catch (Exception e) {             e.printStackTrace();         } finally {             // 关闭资源             try {                 if (resultSet != null) resultSet.close();                 if (statement != null) statement.close();                 if (connection != null) connection.close();             } catch (Exception e) {                 e.printStackTrace();             }         }     }
```  
  
String sql = "select * from users where id=1";，此处没有进行任何过滤，就会存在sql注入漏洞，此处只是形式，真实情况下，id值是我们可控的！看看预编译的情况！  
### 2、jdbc的PreparedStatement  
  
PreparedStatement使用了预编译的方式，是statement的升级版，更加安全、效率也更高。能有效防止sql注入  
```
public static void main(String[] args) {         // 数据库连接信息         String url = "jdbc:mysql://localhost:3306/security";         String username = "root";         String password = "root";         Connection connection = null;         PreparedStatement preparedStatement = null;         ResultSet resultSet = null;         try {             // 加载数据库驱动             Class.forName("com.mysql.cj.jdbc.Driver");             // 获取数据库连接             connection = DriverManager.getConnection(url, username, password);             // 创建预编译的 SQL 语句             String sql = "SELECT * FROM users WHERE id = ?";             preparedStatement = connection.prepareStatement(sql);             // 设置参数             preparedStatement.setInt(1, 1);             //如果是int型则用setInt方法，如果是string型则用setString方法             // 执行查询             resultSet = preparedStatement.executeQuery();             // 处理查询结果             while (resultSet.next()) {                 // 假设表中有 id 和 name 两列                 int id = resultSet.getInt("id");                 String name = resultSet.getString("username");                 String pass = resultSet.getString("password");                 System.out.println("ID: " + id + ", Name: " + name + ", Password: "+pass);             }             System.out.println(preparedStatement);         } catch (Exception e) {             e.printStackTrace();         } finally {             // 关闭资源             try {                 if (resultSet != null) resultSet.close();                 if (preparedStatement != null) preparedStatement.close();                 if (connection != null) connection.close();             } catch (Exception e) {                 e.printStackTrace();             }         }     }
```  
  
使用占位符？，并且进行预编译，锁定了语法树和查询结构。找到无法跳出user表的束缚，导致了无法进行sql注入！  
### ORM简介  
  
ORM 是一种**编程技术**  
，用于**在面向对象的编程语言（如 Java、Python）和关系型数据库（如 MySQL、PostgreSQL）之间进行数据映射**  
。它的主要目的是**让开发者使用面向对象的方式操作数据库，而不是直接写 SQL**  
。也就是说它不是直接与数据库尽心交互，而是通过操作对象间接操作数据库。  
  
下面的Mybatis、Hibernate就具有ORM框架的属性。  
# Mybatis下的sql注入  
  
MyBatis 是对 JDBC 的进一步封装与优化，它属于 **半自动化的 ORM 框架**  
，相比 Hibernate 灵活性更强，保留了 SQL 的可控性与性能优势。  
### 工作原理简述：  
- • 开发者在 **Mapper 文件（XML）中提前定义 SQL 语句**  
。  
  
- • 在 Java 正文中，只需调用 Mapper 接口方法，框架会自动完成 SQL 执行。  
  
- • MyBatis 本质上是对 **JDBC 的封装与升级版**  
，简化了数据库操作流程。  
  
直接步入mybtis的sql查询，根据上面的原理简述，我们的sql语句在Mapper的xml文档中，所以下面的代码也是Mapper文件中的。  
### 安全用法（#{}）：  
```
<select id="getUserById" resultType="User">     SELECT * FROM users WHERE id = #{id} </select>
```  
  
生成的 SQL：  
  
SELECT * FROM users WHERE id = ?  
  
参数通过 JDBC 绑定，无法注入。  
### 危险用法（${}）  
```
<select id="getUsersBySort" resultType="User">     SELECT * FROM users ORDER BY ${sortColumn} </select>
```  
  
如果 sortColumn 是用户输入，传入：  
  
sortColumn=id desc; DROP TABLE users --  
  
最终拼接为：  
  
SELECT * FROM users ORDER BY id desc; DROP TABLE users --  
  
造成严重 SQL 注入漏洞，其利用形同最简单的sql注入  
  
下面我们来看看常见的肯能存在漏洞的几种情况  
### 基础漏洞代码  
  
预编译语法错误导致sql注入，预编译虽然能够防止sql注入，但是如果程序员没有养成良好的习惯，也会存在sql注入的风险  
```
String sql = "SELECT * FROM users WHERE id = ?";             String username1="null";             sql = sql + " and username like '%" + username1 + "%'";
```  
  
如果我的username1传参为user%' or '1'='1'# ，此时语句就变成了  
  
SELECT * FROM users WHERE id = 1 and username like '%user%' or '1'='1'#%’  
  
返回结果也会是全部信息  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJl2zvciccwjaF3YJSo7TERBGibT3VZxb8XsibNVpT1G4n2CVWy0EibsFoVQ/640?wx_fmt=png&from=appmsg "")  
  
下面就看一看需要用到${}的情况  
### Order by 注入  
  
还有就是order by注入（order by 字段名）不能使用预编译的写法，因为order by语句后面必须要跟字段名（username、id这种）或者字段列数（1、2这种），字段列数肯定是可以预编译的，因为它是Integer类型，但是如果要采用字段名进行排序，就会出现问题，因为setString(1,username)后会自动给usernname套一个引号，而引号会让数据库认为这是一个字符串，而不是字段名，导致order by语句失效，字段列数就不会导致无法使用预编译，如下就是使用预编译的情况下的order by情况：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJnncLTh1c2jXYIt94AgHhFWZic5ZOfX6U9Igq4ichmYWTRN9zCeZEaaWA/640?wx_fmt=png&from=appmsg "")  
  
可以看到并没有使用username进行排序  
  
我换为username所在的字段列数2试试，  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJA9tiaaMKxWa2aF0MKHVPESfPCia2u8tKsEuF2oer9XrdSAhaHPmPLgIg/640?wx_fmt=png&from=appmsg "")  
  
这就成功了。所以要么是字段列数+预编译进行排序，要么就是字段名+不使用预编译的方式，  
  
这里就也看看存在order by注入的漏洞代码  
```
String sort = "username";         String sql = "select * from users order by " + sort;         System.out.println(sql);         try (Connection connection = DriverManager.getConnection(url, username, password);              Statement statement = connection.createStatement();              ResultSet resultSet = statement.executeQuery(sql)) {             while (resultSet.next()) {                 // 处理结果集                 String name = resultSet.getString("username");                 System.out.println(name);             }         } catch (SQLException e) {             e.printStackTrace();         }
```  
  
sort就是注入点：sort=”EXTRACTVALUE(1, CONCAT(0x7e, (SELECT DATABASE()), 0x7e))”  
  
**简单利用**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJ941LmlmzCz2GQMFauiaT4dZwypkHj60uHRLsdicR3ltvABgr695GejJw/640?wx_fmt=png&from=appmsg "")  
### Like模糊查询  
  
select * from Book where name LIKE '%${id}%'，语法不会报错，但是会存在sql注入  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJ4YclFqp5rPv1nxFE8w2xjuX2UqZdIZuuXhOz89dp5jiatjrLOmUmmxg/640?wx_fmt=png&from=appmsg "")  
  
select * from Book where name LIKE '%#{id}%' ，语法会报错  
  
为什么？  
  
因为#{}相当于jdbc中的预编译，相当于语句为select * from Book where name LIKE '%?%'  
  
%?%，mysql是无法识别语句的，无法识别语法，所以会报错  
### 模糊查询变式  
  
对于模糊查询不代表就不能使用#{}了，下面就是可以使用几种变式来实现预编译  
  
1、concat函数拼接： SELECT * FROM student WHERE name LIKE CONCAT('%', #{stuName}, '%')  
  
2、mysql语法糖：SELECT * FROM student WHERE name LIKE "%"#{stuName}"%"  
  
这就很好的规避了之前无法预编译的问题  
### in查询  
  
我们现在先明确in查询有何妙用，它主要实现的功能是把原本只查询的id这个字段替换为了一个数组，来实现多次查询，从而避免查询语句的冗余，达到多次查询的结果。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJvstUZbrHkgGicY0ibzOGsWAxIQwgDHuMShlNfol9olj3gENZFoxDKq1A/640?wx_fmt=png&from=appmsg "")  
  
**2. 多值查询in之后的参数**  
  
Select * from news where id in (#{id})  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJA2icbzuia652t4MpgrZL7H4NnAPWibl0RxKlB6pFa1hyliaQQkEuqKZRibg/640?wx_fmt=png&from=appmsg "")  
  
这样的SQL语句虽然能执行但得不到预期结果，于是研发人员将SQL查询语句修改如下：  
  
Select * from news where id in (${id})  
  
修改后如下  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJphWASLfAxdGqXH54VwKscJck7icRETbkgeJUHAibib9svHjDex227OqOA/640?wx_fmt=png&from=appmsg "")  
  
所以就无法被预编译保护，存在sql注入  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJgibRKH46qL7tfOBE1MrgEmHlhGbkhAicztkb4rHllvPw0kWp55NhuqFA/640?wx_fmt=png&from=appmsg "")  
### in查询变式  
  
采用list数组传入，而不是通过字符串，这个方式就成功避免了无法预编译的问题  
```
public static void main(String[] args) {         BookDaoin bookDao = new BookDaoin();         List<Integer> ids = Arrays.asList(1, 2);         List<Book> books = bookDao.getBooksByIds(ids);         books.forEach(System.out::println);     }          xml文件         <select id="selectBookByIds" parameterType="list" resultType="com.mybatis.dao.Book">         SELECT * FROM Book WHERE id IN         <foreach collection="list" item="id" open="(" separator="," close=")">             #{id}         </foreach>     </select>
```  
### 实战正则  
  
${  
### 实战一下  
  
这是xx医院管理系统，其中用了Spring boot、mybatis框架  
  
我们通过检索mapper文件的${ ，来找sql注入，找到如下结果  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJ6UOdtYvzHpWMq8YVUO7Zjh9uc35InubsMoHkOchbWhkh16MsVc1g7w/640?wx_fmt=png&from=appmsg "")  
  
因为用了Spring boot，我们就顺从其框架逻辑，主要一般会分成三层：Controller、Server、Dao  
  
Controller → Service → DAO → Database  
- • Controller：处理 HTTP 请求（如接收前端参数）。  
  
- • Service：处理业务逻辑（如校验参数、事务管理）。  
  
- • DAO：直接操作数据库（如执行 SQL）。  
  
所以我们就是使用从下往上去追踪getOption  
  
Dao层  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJ1FAflVsFbCwPabJaia74ibfFBV4gjG8PuDvfDpwIB3ADjXIkYcIBribCg/640?wx_fmt=png&from=appmsg "")  
  
Server层  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJqEbDprmicapsNPamkXoEWCC7B2JnLESksAnHTA3cIGnWEpqFkQ6LwHw/640?wx_fmt=png&from=appmsg "")  
  
Controller层  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJ1ZibC49Wyc5OIclWeGmibFYxzgPR9YJmGgowUVurKACddicANN55kQpSQ/640?wx_fmt=png&from=appmsg "")  
  
看重要部分  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJRzEXibycEUkvHia9nPSiaVWMb0ecY0O6xhDW8FFzBDmeL4b47FicvNSRicQ/640?wx_fmt=png&from=appmsg "")  
  
mapper文档中，${}包裹的column是用columnName以路由的形式传入  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJqHtsUqzW0OZx3SViabo6hz7ILW12Zd8yIE18KXyVN4aoKlNQ2T9tXiag/640?wx_fmt=png&from=appmsg "")  
  
此处也没什么别的过滤，简单构造一下payload:  
```
http://10.19.241.45:8081/yiyaoguanlixitong/option/users/(select%20database()) 最终的sql语句为： SELECT distinct (select database()) FROM users WHERE (select database()) IS NOT NULL AND (select database()) != ''
```  
  
我们直接去web端是验证一下  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJLOTg1U0QICcPMHHekudlq9dUhibgSzFwRUEIG6dkYlzdPaEQouIwQ4w/640?wx_fmt=png&from=appmsg "")  
# JPA & Hibernate下的sql注入  
### JPA & Hibernate简介  
  
**Hibernate**  
 是一个功能强大的 ORM 框架，可以把 Java 对象和数据库表自动进行映射。**JPA（Java Persistence API）**  
 是 Java 官方定义的 **ORM 标准接口**  
，它不是真正的 ORM 实现，而是一组 **规范（接口）**  
。JPA 就像 JDBC 的接口，而 Hibernate 是 JPA 的实现类。  
### HQL注入  
  
因为其高度封装性，我们看不到语句，更看到参数值。难道他就不存在sql注入了吗，那是不可能的。其实他存在HQL注入，HQL(Hibernate Query Language) 是面向对象的查询语言, **它和 SQL 查询语言有些相似。HQL查询并不直接发送给数据库，而是由hibernate引擎对查询进行解析并解释，然后将其转换为SQL**  
。因为不是直接与数据库进行交互，所以此处使用单引号是无法判断是否存在sql语句的。那我们如何判断呢？  
  
最基础的漏洞代码：  
```
public static void main(String[] args) {     Configuration configuration = new Configuration();     Configuration configure = configuration.configure("hibernate.cfg.xml");     SessionFactory sessionFactory = configure.buildSessionFactory();     Session session = sessionFactory.openSession();     try {         // 模拟用户输入（可能来自控制台、网页等）         String userInput = "2 or 1=1";  // ⚠️ 注入点         // 不安全：字符串拼接构造 HQL         String hql = "from Book where id = " + userInput;         Query<Book> query = session.createQuery(hql, Book.class);         Book book = query.uniqueResult();         if (book != null) {             System.out.println(book.toString());         } else {             System.out.println("未找到对应的书籍记录");         }     } catch (Exception e) {         e.printStackTrace();     } finally {         if (session != null) {             session.close();         }         if (sessionFactory != null) {             sessionFactory.close();         }     }
```  
  
这里通过 2 or 1=1 来判断是否存在注入，同时还要确保2是已存在的字段，那我们不知道存在字段呢，那只能使用枚举爆破了。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJI2OWFqmr73n8R1tTnBAzYpM7wbIPKmZmEHtnZOudndEUVYDqcMqByg/640?wx_fmt=png&from=appmsg "")  
  
看起来报错了，但是是存在sql注入的，只是说查询函数返回的数据限制为1。这里返回三条所以报错了。  
  
我们还可以使用更高级的**模糊查询**  
，String userInput = "0 or name like 'A%'"，这个name是怎么来的，也是通过枚举、爆破、猜解的。依次遍历字符表 A-Z, a-z, 0-9，可以逐步枚举出字段值。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcr3wEP1V2n7Eh3DklJ4ibohJmzgBY9QyKamostq2RsLL4SxLz0Of94SCE8G17YicJcmOW39sJlo88QA/640?wx_fmt=png&from=appmsg "")  
### HQL 限制说明  
- • 不支持 UNION SELECT；  
  
- • 不能像原生 SQL 那样使用注入查看系统表（如 information_schema）；  
  
- • 你只能基于对象模型注入，所以字段名要提前知道；  
  
- • 不支持 - 注释（但某些实现允许 /* comment */）；  
  
### 防注入写法  
```
public static void main(String[] args) {         Configuration configuration = new Configuration();         Configuration configure = configuration.configure("hibernate.cfg.xml");         SessionFactory sessionFactory = configure.buildSessionFactory();         Session session = sessionFactory.openSession();         try {             // 模拟用户输入（可能来自控制台、网页等）             String userInput = "1";  // 示例输入             Long bookId = Long.parseLong(userInput);             // ----------------------------             // ✅ 方式一：HQL + 参数绑定（推荐）             // ----------------------------             String hql = "from Book where id = :id";  //绑定参数，book不是表是实体类名，:id 是占位符             Query<Book> query = session.createQuery(hql, Book.class);  //创建一个 HQL 查询             query.setParameter("id", bookId);  // 设置参数值             Book book1 = query.uniqueResult(); // 执行查询并获取结果             printResult("HQL + 参数绑定", book1); // 打印结果             // ----------------------------             // ✅ 方式二：Criteria API 查询             // ----------------------------             CriteriaBuilder cb = session.getCriteriaBuilder(); // 创建 CriteriaBuilder             CriteriaQuery<Book> cq = cb.createQuery(Book.class); // 创建 CriteriaQuery             Root<Book> root = cq.from(Book.class); // 创建 Root             cq.select(root).where(cb.equal(root.get("id"), bookId)); // 添加查询条件             Book book2 = session.createQuery(cq).uniqueResult(); // 执行查询并获取结果             printResult("Criteria API", book2); // 打印结果             // ----------------------------             // ✅ 方式三：session.get() 主键查             // ----------------------------             Book book3 = session.get(Book.class, bookId); // 使用 session.get() 主键查询             printResult("session.get()", book3); // 打印结果         } catch (Exception e) {             e.printStackTrace();         } finally {             if (session != null) session.close();             if (sessionFactory != null) sessionFactory.close();         }     }     private static void printResult(String method, Book book) {         System.out.println("\n[查询方式: " + method + "]");         if (book != null) {             System.out.println(book.toString());         } else {             System.out.println("未找到对应的书籍记录");         }     }
```  
  
HQL + 参数绑定：  
  
适用于：需要使用 HQL 编写复杂查询语句时优点：语义清晰、灵活度高、支持复杂筛选和排序防注入机制：通过 :参数名 + setParameter() 实现安全绑定  
  
Criteria API 查询：  
  
适用于：构建动态、多条件、复杂查询时，完全对象化操作优点：类型安全、可编程构建、免拼接防注入机制：值通过 API 构造器设置，无字符串拼接风险  
  
session.get() 主键查询：  
  
适用于：只根据主键查数据（id）时优点：语法最简单，效率高防注入机制：get() 方法内部安全绑定主键参数  
  
由于hibernate的sql注入确实及其少见，没有什么比较好的案例，我们了解其原理，以后遇到了翻翻笔记再实战即可  
# 总结  
  
对于java的sql注入，着重体现在框架、模块的注入，我们的代码审计的关注点也要根据对应搭建的框架进行特定化挖掘。  
  
   
  
申明：本公众号所分享内容仅用于网络安全技术讨论，切勿用于违法途径，  
  
所有渗透都需获取授权，违者后果自行承担，与本号及作者无关，请谨记守法.  
  
****  
  
**★**  
  
**付费圈子**  
  
  
**欢 迎 加 入 星 球 ！**  
  
**代码审计+免杀+渗透学习资源+各种资料文档+各种工具+付费会员**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/pLGTianTzSu7XRhTMZOBAqXehvREhD5ThABGJdRialUx3dQWwO7fclsicyiajicKfvXV4kHs38nkwFxUSckVF2nYlibA/640?wx_fmt=gif&random=0.4447566002908574&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
  
**进成员内部群**  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/pPVXCo8Wd8AQHAyOTgM5sLrvP6qiboXljGWG0uOdvcNR8Qw5QJLxSVrbFds2j7MxExOz1ozb9ZoYwR68leoLdAg/640?wx_fmt=jpeg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/pLGTianTzSu7XRhTMZOBAqXehvREhD5ThABGJdRialUx3dQWwO7fclsicyiajicKfvXV4kHs38nkwFxUSckVF2nYlibA/640?wx_fmt=gif&random=0.09738205945672873&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
  
**星球的最近主题和星球内部工具一些展示******  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/pPVXCo8Wd8Doq0iczyRiaBfhTQyfzqSGuia4lfHfazabEKr2EDe7sGVoxUhLrNRA4FbI1yef6IkWdmzxvZrTiaJncg/640?wx_fmt=jpeg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/pPVXCo8Wd8BmE6FAA8Bq7H9GZIRt1xYZpmYNWxrrzolt71FtX5HyM03H0cxkiaYelv7ZSajLtibEdBXUpCibdItXw/640?wx_fmt=png&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/pPVXCo8Wd8ADSxxicsBmvhX9yBIPibyJTWnDpqropKaIKtZQE3B9ZpgttJuibibCht1jXkNY7tUhLxJRdU6gibnrn0w/640?wx_fmt=png&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/pPVXCo8Wd8DKZcqe8mOKY1OQN5yfOaD5MpGk0JkyWcDKZvqqTWL0YKO6fmC56kSpcKicxEjK0cCu8fG3mLFLeEg/640?wx_fmt=png&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/pPVXCo8Wd8DAc8LkYEjnluf7oQaBR9CR7oAqnjIIbLZqCxwQtBk833sLbiagicscEic0LSVfOnbianSv11PxzJdcicQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/pPVXCo8Wd8B96heXWOIseicx7lYZcN8KRN8xTiaOibRiaHVP4weL4mxd0gyaWSuTIVJhBRdBmWXjibmcfes6qR1w49w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1&tp=wxpic "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/pPVXCo8Wd8DAc8LkYEjnluf7oQaBR9CRBgpPoexbIY7eBAnR7sWS1BlBAQX51QhcOOOz06Ct2x1cMD25nA6mJQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/pPVXCo8Wd8AqNwoQuOBy9yePOpO5Kr6aHIxj7d0ibfAuPx9fAempAoH9JfIgX4nKzCwDyhQzPrRIx4upyw5yT4Q/640?wx_fmt=png&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
****  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/pLGTianTzSu7XRhTMZOBAqXehvREhD5ThABGJdRialUx3dQWwO7fclsicyiajicKfvXV4kHs38nkwFxUSckVF2nYlibA/640?wx_fmt=gif&random=0.4447566002908574&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
  
**加入安全交流群**  
  
  
[](http://mp.weixin.qq.com/s?__biz=MzkxNDAyNTY2NA==&mid=2247513602&idx=1&sn=98045772ff9aebe8792552e32523bf83&chksm=c1764badf601c2bbcc199da519611ac8c36c17e5a0554fe32ab9d9769403a495187058f19f3d&scene=21#wechat_redirect)  
  
 			                  
  
  
**信 安 考 证**  
  
  
  
需要考以下各类安全证书的可以联系我，下方扫码回复  
**考证**  
进交流群，价格优惠、组团更便宜，还送【  
渗透安全团队  
】知识星球**1**  
年！  
<table><tbody><tr style="outline: 0px;"><td data-colwidth="557" width="557" valign="top" style="outline: 0px;word-break: break-all;hyphens: auto;"><p style="outline: 0px;"><span style="outline: 0px;font-size: 14px;letter-spacing: 0.51px;"><span leaf="">CISP、PTE、PTS、DSG、IRE、IRS、</span></span><span style="outline: 0px;font-size: 14px;letter-spacing: 0.51px;"><span leaf="">NISP、</span></span><span style="outline: 0px;font-size: 14px;letter-spacing: 0.51px;"><span leaf="">PMP、CCSK、CISSP、ISO27001...</span></span></p></td></tr></tbody></table>  
  
![](https://mmbiz.qpic.cn/mmbiz_png/pPVXCo8Wd8AOzYX7kxefGbGGZg3g1ltkN30q9hceg23PiczgUqMT0EE9w0fLK9uw1eKWwQX9TljXQe1OQeHRZ2Q/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
**教程如下图**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/pPVXCo8Wd8C3Gu1libJC0muV1WmOFa3XM3fTyOiaOJYPgCiaHV6gkJJBia6Fjeds9w9pxxyyPNJhbcfK3I1tcGueTg/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ndicuTO22p6ibN1yF91ZicoggaJJZX3vQ77Vhx81O5GRyfuQoBRjpaUyLOErsSo8PwNYlT1XzZ6fbwQuXBRKf4j3Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1&tp=wxpic "")  
  
  
  
**推荐阅读**  
  
  
  
**干货｜史上最全一句话木马**  
  
  
**干货 | CS绕过vultr特征检测修改算法**  
  
  
**实战 | 用中国人写的红队服务器搞一次内网穿透练习**  
  
  
**实战 | 渗透某培训平台经历**  
  
  
**实战 | 一次曲折的钓鱼溯源反制**  
  
  
**免责声明**  
  
由于传播、利用本公众号渗透安全团队所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，公众号渗透安全团队及作者不为**此**  
承担任何责任，一旦造成后果请自行承担！如有侵权烦请告知，我们会立即删除并致歉。谢谢！  
  
好文分享收藏赞一下最美点在看哦  
  
  
