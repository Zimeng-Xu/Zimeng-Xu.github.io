---
title: JavaWeb学习笔记
date: 2026-09-21 21:48:09
tags: JavaSE
categories: 学习笔记
---

# Maven

## Maven坐标

Maven中的坐标是**资源的唯一标识，通过该坐标可以唯一定位资源位置**。
- Maven坐标的组成：
  - `groupId`：定义当前Maven项目隶属组织名称。
  - `artifactId`：定义当前Maven项目名称。
  - `version`：定义当前项目版本号。

## 依赖

依赖：指当前项目运行所需要的 `jar` 包，一个项目中可以引入多个依赖。
- 配置：
  - 在 `pom.xml` 中编写 `<dependencies>` 标签。
  - 在 `<dependencies>` 标签中使用 `<dependency>` 引入坐标。
  - 定义坐标的 `groupId`、`artifactId`、`version`。
  - 点击刷新按钮，引入最新加入的坐标。

> 查找依赖坐标信息：https://mvnrepository.com/

- 排除：
  - 在需要排除依赖的 `<dependency>` 中添加 `<exclusions>` 标签。
  - 在 `<exclusions>` 标签中使用 `<exclusion>` 指定需要排除的依赖。
  - 定义需要排除依赖的 `groupId` 和 `artifactId`。

- 范围：
  在默认情况下，依赖的jar包可以在任何地方使用。可以通过`<scope>...</scope>`设置其作用范围。
  |scope值|主程序|测试程序|打包（运行）|
  |:-:|:-:|:-:|:-:|
  |compile（默认）|Y|Y|Y|
  |test|-|Y|-|
  |provided|Y|Y|-|
  |runtime|-|Y|Y|
  
# Web入门

## HTTP协议

超文本传输协议，规定了浏览器和服务器之间数据传输的规则。

### 特点

1.基于`TCP协议`：面向连接，安全。
2.基于`请求-响应模型`：一次请求对应一次响应。
3.`HTTP协议`是无状态的协议，对事务处理没有记忆能力，每次请求-响应都是独立的。
  - 缺点：多次请求间不能共享数据。
  - 优点：速度快。

### 请求数据的格式

*示例：*
```http
GET /brand/findAll?name=OPPO&status=1 HTTP/1.1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/...
```

> **请求行**：请求数据第一行，包含请求方式、资源路径、协议及版本号。
> **请求头**：第二行开始，格式：`key:value`。
> **请求体**：只有`POST`请求才有的部分，用于存放请求参数。与请求行和请求头之间使用空行隔开。

- 常见的请求头：
  | 请求头 | 说明 |
  |---|---|
  | `Host` | 请求的主机名。 |
  | `User-Agent` | 浏览器版本，例如 Chrome 浏览器的标识类似 `Mozilla/5.0 ... Chrome/79`，IE 浏览器的标识类似 `Mozilla/5.0 (Windows NT ...) like Gecko`。 |
  | `Accept` | 表示浏览器能接收的资源类型，如 `text/*`、`image/*` 或 `*/*`（表示所有类型）。 |
  | `Accept-Language` | 表示浏览器偏好的语言，服务器可以据此返回不同语言的网页。 |
  | `Accept-Encoding` | 表示浏览器支持的压缩类型，例如 `gzip`、`deflate` 等。 |
  | `Content-Type` | 请求主体的数据类型。 |
  | `Content-Length` | 请求主体的大小，单位为字节。 |

- 请求方式：
  - `GET`：请求参数在请求行中，会使请求路径后面携带参数；**没有请求体**；`GET`请求大小**有限制**。
  - `POST`：请求参数在请求体中；`POST`请求大小**无限制**。

### 响应格式

*示例：*
```http
HTTP/1.1 200 OK
Content-Type: application/json
Transfer-Encoding: chunked
Date: Tue, 10 May 2022 07:51:07 GMT
Keep-Alive: timeout=60
Connection: keep-alive

[{"id": 1, "brandName": "阿里巴巴", "companyName": "腾讯计算机系统有限公司", "description": "玩玩玩"}]
```

> **响应行**：响应数据第一行，包含协议、状态码、描述。
> **响应头**：从第二行开始，格式：`key:value`。
> **响应体**：最后一部分，与响应行和响应头之间用空行隔开，存放响应数据。

- 响应状态码：
  | 状态码 | 含义 | 说明 |
  |---|---|---|
  | `1xx` | 响应中 | 临时状态码，表示请求已接收，告知客户端应继续请求，或者如果请求已完成则忽略它。 |
  | `2xx` | 成功 | 表示请求已被成功接收并处理完成。 |
  | `3xx` | 重定向 | 重定向到其他地方，让客户端再次发起请求以完成整个处理。 |
  | `4xx` | 客户端错误 | 处理发生错误，责任在客户端。例如：请求了不存在的资源、客户端未被授权、禁止访问等。 |
  | `5xx` | 服务器错误 | 处理发生错误，责任在服务器端。例如：程序抛出异常等。 |
  
  > 查找响应状态码：https://cloud.tencent.com/developer/chapter/13553
  
- 常见的响应头：
  | 响应头 | 含义 |
  |---|---|
  | `Content-Type` | 表示响应内容的类型，例如 `text/html`、`application/json`。 |
  | `Content-Length` | 表示响应内容的长度，单位为字节。 |
  | `Content-Encoding` | 表示响应内容使用的压缩算法，例如 `gzip`。 |
  | `Cache-Control` | 指示客户端如何缓存响应，例如 `max-age=300` 表示响应在新鲜度有效期内最多可缓存 300 秒。 |
  | `Set-Cookie` | 用于向浏览器发送 Cookie。 |

### 注意事项

`HTTP协议`默认端口号为`80`。

# Web后端开发

## 请求响应

- 请求`HttpServletRequest`：获取请求数据。
- 响应`HttpServletResponse`：设置响应数据。
- BS架构：`Browser/Server`，浏览器/服务器架构模式。客户端只需要浏览器，应用程序的逻辑和数据都存储在服务器。
- CS架构：`Client/Server`，客户端/服务器架构模式。

### 请求

#### 简单参数

- 原始方式：在原始的Web程序中，获取请求参数，需要通过`HttpServletRequest`对象手动获取。
- SpringBoot方式：参数名与形参变量名相同，定义形参即可接收参数。如果方法形参名称与请求参数名称不匹配，可以使用`@RequestParam`完成映射。
  > `@RequestParam`中的`require`属性默认为`true`，代表该请求参数必须传递，如果不传递将报错。如果该参数是可选的，可以将`require`属性设置为`false`。

#### 实体参数

- 简单实体对象：请求参数名与形参对象属性名相同，定义`POJO`接收即可。
- 复杂实体对象：请求参数名与形参对象属性名相同，按照对象层次结构关系即可接受嵌套`POJO`属性参数。

#### 数组集合参数

- 数组参数：请求参数名与形参数组名称相同且请求参数为多个，定义数组类型形参即可接收参数。
- 集合参数：请求参数名与形参集合名称相同且请求参数为多个，使用`@RequestParam`绑定参数关系。

#### 日期参数

使用`@DateTimeFormat`注解完成日期参数格式转换。

#### Json参数

`JSON`数据键名与形参对象属性名相同，定义`POJO`类型形参即可接收参数，需要使用`@RequestBody`标识。

#### 路径参数

通过请求URL直接传递参数，使用`{...}`来标识该路径参数，需要使用`@PathVariable`获取路径参数。

### 响应

- `@ResponseBody`
  - 类型：方法注解、类注解。
  - 位置：`Controller`方法/类上
  - 作用：将方法返回值直接响应。如果返回值类型是实体对象/集合，将会转换为`JSON`格式响应。
  - 说明：`@RestController` = `@Controller` + `ResponseBody`。

- 统一响应结果：
  ```java
  public class Result {
    private Integer code;
    private String message;
    private Object data;
  
    public Result() {
    }
  
    public Result(Integer code, String message, Object data) {
        this.code = code;
        this.message = message;
        this.data = data;
    }
  
    public Integer getCode() {
        return code;
    }
  
    public void setCode(Integer code) {
        this.code = code;
    }
  
    public String getMessage() {
        return message;
    }
  
    public void setMessage(String message) {
        this.message = message;
    }
  
    public Object getData() {
        return data;
    }
  
    public void setData(Object data) {
        this.data = data;
    }
  
    // 成功响应
    public static Result success(Object data) {
        return new Result(200, "success", data);
    }
  
    // 失败响应
    public static Result error(String message) {
        return new Result(500, message, null);
    }
  }
  ```

### 其他

Springboot项目的静态资源（HTML/CSS/JS等前端资源）默认存放目录为：classpath:/static、classpath:/public、classpath:/resources。

## 分层解耦

### 三层架构

- 控制层`Controller`：接收前端发送的请求，对请求进行处理，并响应数据。
- 业务逻辑层`Service`：处理具体的业务逻辑。
- 数据访问层（持久层）`Dao`：负责数据访问操作，包括数据的增、删、改、查。

### 分层解耦

- 控制反转`IOC`：对象的创建控制权由程序自身转移到外部（容器）。
- 依赖注入`DI`：容器为应用程序提供运行时，所依赖的资源。
- Bean对象：IOC容器中创建、管理的对象。

### IOC&DI

1.将Service层及Dao层的实现类交给IOC容器管理。使用`@Component`注解。
2.为Controller及Service注入运行时依赖的对象。使用`@Autowired`注解。
3.运行测试。

#### Bean的声明

| 注解 | 说明 | 使用位置 |
|---|---|---|
| `@Component` | 声明 Bean 的基础注解 | 不属于以下三类时使用 |
| `@Controller` | `@Component` 的衍生注解 | 控制器类上 |
| `@Service` | `@Component` 的衍生注解 | 业务类上 |
| `@Repository` | `@Component` 的衍生注解 | 数据访问类上（由于与 MyBatis 整合，使用较少） |

> 声明 bean 的时候，可以通过 `value` 属性指定 Bean 的名字，如果没有指定，默认为类名首字母小写。
> 使用以上四个注解都可以声明 Bean，但是在 SpringBoot 集成 Web 开发中，声明控制器 Bean 只能用 `@Controller`。

#### Bean的组件扫描

- 声明 Bean 的注解需要被组件扫描注解 `@ComponentScan` 扫描后，才会生效。
- `@ComponentScan` 注解虽然没有显式配置，但是实际上已经包含在了启动类声明注解 `@SpringBootApplication` 中，**默认扫描的范围是启动类所在包及其子包**。

#### Bean注入

- `@Autowired`注解默认是按照**类型**进行的，如果存在多个相同类型的Bean将报错。
- 解决方法：
  - `@Primary`
  - `@Autowired`+@Qualifier("Bean的名称")`
  - `@Resource(name="Bean的名称")`

> `@Autowired` 是 Spring 框架提供的注解，而 `@Resource` 是 JDK 提供的注解。
> `@Autowired` 默认是按照类型注入，而 `@Resource` 默认是按照名称注入。

# MySQL

## SQL通用语法

- SQL 语句可以单行或多行书写，以分号结尾。
- SQL 语句可以使用空格/缩进来增强语句的可读性。
- MySQL 数据库的 SQL 语句不区分大小写。
- 注释：
  - 单行注释：`-- 注释内容` 或 `# 注释内容`（MySQL 特有）
  - 多行注释：`/* 注释内容 */`

## DDL

DDL，数据定义语言，用来定义数据库对象（数据库、表）。

### 数据库

- 查询数据库：
  - 查询所有数据库：`SHOW DATABASES`;
  - 查询当前使用的数据库：`SELECT DATABASE()`;

- 使用数据库：`USE 数据库名;`
- 创建数据库：`CREATE DATABASE [IF NOT EXISTS] 数据库名;`
  > `IF NOT EXISTS`：如果数据库不存在，则创建；如果已存在，则不创建。

- 删除数据库：`DROP DATABASE [IF EXISTS] 数据库名;`
  > `IF EXISTS`：如果数据库存在，则删除；如果不存在，则不执行删除操作。

> 上述语法中的`DATABESE`也可以替换成`SCHEMA`。

### 表

#### 创建表

```sql
CREATE TABLE 表名 (
    字段1 字段类型 [约束] [COMMENT '字段注释'],
    字段2 字段类型 [约束] [COMMENT '字段注释']
) [COMMENT '表注释'];
```

- 约束：作用于表中字段上的规则，用于限制存储在表中的数据。用于保证数据库中数据的正确性、有效性和完整性。
  | 约束名称 | 作用 | 关键字 |
  |---|---|---|
  | 非空约束 | 限制字段值不能为 `NULL` | `NOT NULL` |
  | 唯一约束 | 保证字段值唯一、不重复 | `UNIQUE` |
  | 主键约束 | 唯一标识一条记录，要求非空且唯一 | `PRIMARY KEY`（`AUTO_INCREMENT`自增） |
  | 默认约束 | 未指定字段值时，使用默认值 | `DEFAULT` |
  | 外键约束 | 建立两张表之间的关联，保证数据的一致性和完整性 | `FOREIGN KEY` |

- 数据类型：
  - 数值类型：
    | 类型 | 大小(byte) | 描述 | 备注 |
    |---|---|---|---|
    | tinyint | 1 | 小整数值 | |
    | smallint | 2 | 大整数值 | |
    | mediumint | 3 | 大整数值 | |
    | int | 4 | 大整数值 | |
    | bigint | 8 | 极大整数值 | |
    | float | 4 | 单精度浮点数值 | float(5,2)：5表示整个数字长度，2表示小数位个数 |
    | double | 8 | 双精度浮点数值 | double(5,2)：5表示整个数字长度，2表示小数位个数 |
    | decimal | | 小数值（精度更高） | decimal(5,2)：5表示整个数字长度，2表示小数位个数 |

  - 字符串类型：
    | 类型 | 大小 | 描述 |
    |---|---|---|
    | char | 0-255 bytes | 定长字符串 |
    | varchar | 0-65535 bytes | 变长字符串 |
    | tinyblob | 0-255 bytes | 不超过255个字符的二进制数据 |
    | tinytext | 0-255 bytes | 短文本字符串 |
    | blob | 0-65 535 bytes | 二进制形式的长文本数据 |
    | text | 0-65 535 bytes | 长文本数据 |
    | mediumblob | 0-16 777 215 bytes | 二进制形式的中等长度文本数据 |
    | mediumtext | 0-16 777 215 bytes | 中等长度文本数据 |
    | longblob | 0-4 294 967 295 bytes | 二进制形式的极大文本数据 |
    | longtext | 0-4 294 967 295 bytes | 极大文本数据 |

  - 日期时间类型：
    | 类型 | 大小（byte） | 格式 | 描述 |
    |---|---|---|---|
    | date | 3 | YYYY-MM-DD | 日期值 |
    | time | 3 | HH:MM:SS | 时间值或持续时间 |
    | year | 1 | YYYY | 年份值 |
    | datetime | 8 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值 |
    | timestamp | 4 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值，时间戳 |
    
#### 查询

- 查询当前数据库所有表：`SHOW TABLES;`
- 查询表结构：`DESC 表名;`
- 查询建表语句：`SHOW CREATE TABLE 表名;`

#### 修改

- 添加字段：`ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束];`
- 修改字段类型：`ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度);`
- 修改字段名和字段类型：`ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束];`
- 删除字段：`ALTER TABLE 表名 DROP COLUMN 字段名;`
- 修改表名：`RENAME TABLE 表名 TO 新表名;`

#### 删除

- 删除表：`DROP TABLE [IF EXISTS] 表名;`

## DML

DML，数据操作语言，用来对数据库中表的数据记录进行增、删、改操作。

- `INSERT`添加数据：
  - 指定字段添加数据：`INSERT INTO 表名 (字段名1, 字段名2) VALUES (值1, 值2);`
  - 全部字段添加数据：`INSERT INTO 表名 VALUES (值1, 值2, ...);`
  - 批量添加数据（指定字段）：`INSERT INTO 表名 (字段名1, 字段名2) VALUES (值1, 值2), (值1, 值2);`
  - 批量添加数据（全部字段）：`INSERT INTO 表名 VALUES (值1, 值2, ...), (值1, 值2, ...);`

- `UPDATE`修改数据：`UPDATE 表名 SET 字段名1 = 值1, 字段名2 = 值2, ... [WHERE 条件];`
- `DELETE`删除数据：`DELETE FROM 表名 [WHERE 条件];`
  > `DELETE`语句不能删除某一个字段的值。如要操作，可以使用`UPDATE`将该字段的值设置为`NULL`。

## DQL

DQL，数据查询语言，用来查询数据库表中的记录。
- 语法：
  ```sql
  SELECT 
  		字段列表
  FROM 
  		表名
  WHERE 
  		条件
  GROUP BY 
  		分组字段
  HAVING 
  		分组后条件
  ORDER BY 
  		排序字段
  LIMIT 
  		分页参数;
  ```

### 基本查询

- 查询多个字段：`SELECT 字段1, 字段2, 字段3 FROM 表名;`
- 查询所有字段（通配符`*` ）：`SELECT * FROM 表名;`
- 设置别名（AS 可省略）：`SELECT 字段1 [AS 别名1], 字段2 [AS 别名2] FROM 表名;`
- 去除重复记录：`SELECT DISTINCT 字段列表 FROM 表名;`

### 条件查询

- 语法：`SELECT 字段列表 FROM 表名 WHERE 条件列表;`、
- 运算符：
  - 比较运算符
    | 比较运算符 | 功能 |
    |---|---|
    | `>` | 大于 |
    | `>=` | 大于等于 |
    | `<` | 小于 |
    | `<=` | 小于等于 |
    | `=` | 等于 |
    | `<>` 或 `!=` | 不等于 |
    | `BETWEEN ... AND ...` | 在某个范围之内（含最小值、最大值） |
    | `IN (...)` | 在 `IN` 之后的列表中的值，多选一 |
    | `LIKE` | 模糊匹配（`_` 匹配单个字符，`%` 匹配任意个字符） |
    | `IS NULL` | 判断值是否为 `NULL` |
    
  - 逻辑运算符
    | 逻辑运算符 | 功能 |
    |---|---|
    | `AND` 或 `&&` | 并且（多个条件同时成立） |
    | `OR` 或 `\|\|` | 或者（多个条件任意一个成立） |
    | `NOT` 或 `!` | 非、不是 |
    
### 分组查询

- 聚合函数：将一列数据作为一个整体，进行纵向计算。
  - 语法：`select 聚合函数(字段列表) from 表名;`
  - 函数：
    | 函数      | 功能   |
    | ------- | ---- |
    | `count` | 统计数量 |
    | `max`   | 最大值  |
    | `min`   | 最小值  |
    | `avg`   | 平均值  |
    | `sum`   | 求和   |
  
  - 注意事项：
    1.ull` 值不参与所有聚合函数运算。
    2.统计数量可以使用：`count(*)`、`count(字段)`、`count(常量)`，推荐使用 `count(*)`。

- 分组查询语法：`select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后过滤条件];`
  > `where` 与` having`区别：
  > 1. 执行时机不同：`where`是分组之前进行过滤，不满足 `where`条件，不参与分组；而 `having`是分组之后对结果进行过滤。
  > 2. 判断条件不同：`where`不能对聚合函数进行判断，而 `having`可以。

### 排序查询

- 语法：`select 字段列表 from 表名 [where 条件列表] [group by 分组字段] order by 字段1 排序方式1, 字段2 排序方式2, ...;`
- 排序方式：
  - `ASC`：升序（默认值）
  - `DESC`：降序

### 分页查询

- 语法：`select 字段列表 from 表名 limit 起始索引, 查询记录数;`
- 注意事项：
  1. 起始索引从 0 开始，起始索引 =（查询页码 - 1）\* 每页显示记录数。
  2. 分页查询是数据库的方言，不同的数据库有不同的实现，MySQL 中是 `LIMIT`。
  3. 如果查询的是第一页数据，起始索引可以省略，直接简写为 `limit 查询记录数`。

## 条件判断函数与表达式

### IF 函数

```sql
IF(条件表达式, true取值, false取值)
```

### CASE 表达式

```sql
CASE 表达式
    WHEN 值1 THEN 结果1
    WHEN 值2 THEN 结果2
    ...
    ELSE ...
END
```

## 多表设计

### 一对多

在数据库表中多的一方添加字段来关联“一”的一方的主键。
- 外键约束：
  ```sql
  -- 创建表时指定
  create table 表名(
      字段名 数据类型,
      ...
      [constraint] [外键名称] foreign key (外键字段名) references 主表 (字段名)
  );
  
  -- 建完表后，添加外键
  alter table 表名 add constraint 外键名称 foreign key (外键字段名) references 主表(字段名);
  ```

### 一对一

在任意一方加入外键，关联另外一方的主键，并且设置外键为唯一的(UNIQUE)。

### 多对多

建立第三张中间表，中间表至少包含两个外键，分别关联两方主键。、

## 多表查询

### 分类

- 连接查询
    - 内连接：相当于查询A、B交集部分数据。
    - 外连接
        - 左外连接：查询左表所有数据（包括两张表交集部分数据）。
        - 右外连接：查询右表所有数据（包括两张表交集部分数据）。
- 子查询

### 内连接

- 隐式内连接：`SELECT 字段列表 FROM 表1, 表2 WHERE 条件 ...;`
- 显式内连接：`SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 连接条件 ...;`

### 外连接

- 左外连接：`SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 连接条件 ...;`
- 右外连接：`SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 连接条件 ...;`

### 子查询

- 介绍：SQL语句中嵌套`SELECT`语句，称为嵌套查询，又称子查询。
- 形式：`SELECT * FROM t1 WHERE column1 = (SELECT column1 FROM t2 ...);`
- 子查询外部的语句可以是INSERT / UPDATE / DELETE / SELECT的任何一个，最常见的是SELECT。
- 分类：
  - 标量子查询：子查询返回的结果为单个值。
  - 列子查询：子查询返回的结果为一列。
  - 行子查询：子查询返回的结果为一行。
  - 表子查询：子查询返回的结果为多行多列。

#### 标量子查询

- 子查询返回的结果是单个值（数字、字符串、日期等）。
- 常用的操作符：`=` `<>` `>` `>=` `<` `<=`。

#### 列子查询

- 子查询返回的结果是一列（可以是多行）。
- 常用的操作符：`IN`、`NOT IN`等。

#### 行子查询

- 子查询返回的结果是一行（可以是多列）。
- 常用的操作符：`=`、`<>`、`IN`、`NOT IN`。

#### 表子查询

- 子查询返回的结果是多行多列，常作为临时表。
- 常用的操作符：`IN`。

## 事务

### 介绍

事务是一组操作的集合，它是一个不可分割的工作单位。事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即**这些操作要么同时成功，要么同时失败**。
> 默认MySQL的事务是自动提交的，也就是说，当执行一条DML语句，MySQL会立即隐式的提交事务。

### 操作

- 开启事务：`START TRANSACTION;` / `BEGIN;`
- 提交事务：`COMMIT;`
- 回滚事务：`ROLLBACK;`

### 四大特性

- **原子性（Atomicity）**：事务是不可分割的最小单元，要么全部成功，要么全部失败。
- **一致性（Consistency）**：事务完成时，必须使所有的数据都保持一致状态。
- **隔离性（Isolation）**：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。
- **持久性（Durability）**：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

## 索引

### 结构

- B+Tree（多路平衡搜索树）
    - 每一个节点，可以存储多个key（有n个key，就有n个指针）。
    - 所有的数据都存储在叶子节点，非叶子节点仅用于索引数据。
    - 叶子节点形成了一颗双向链表，便于数据的排序及区间范围查询。

### 语法

- 创建索引：`CREATE [UNIQUE] INDEX 索引名 ON 表名 (字段名,...);`
- 查看索引：`SHOW INDEX FROM 表名;`
- 删除索引：`DROP INDEX 索引名 ON 表名;`

> 主键字段，在建表时，会自动创建主键索引。
> 添加唯一约束时，数据库实际上会添加唯一索引。

# Mybatis

## 入门

### 开发流程

1. **准备工作**：创建Spring Boot工程，准备数据库表及对应的实体类。
2. **引入依赖，配置MyBatis**：配置数据库连接信息。
3. **编写SQL语句**：通过注解或XML编写SQL语句。
4. 单元测试。

### JDBC

使用Java语言操作关系型数据库的一套API。

### 数据库连接池

- 常见产品：
  - C3P0
  - DBCP
  - Druid：
    官方地址: https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter
  - Hikari（Spring Boot 默认）

### Lombok

Lombok 是一个实用的 Java 类库，能通过注解的形式自动生成构造器、getter/setter、equals、hashCode、toString 等方法，并可以自动化生成日志变量，简化 Java 开发、提高效率。

| 注解 | 作用 |
|---|---|
| `@Getter` / `@Setter` | 为所有的属性提供 get/set 方法 |
| `@ToString` | 会给类自动生成易阅读的 `toString` 方法 |
| `@EqualsAndHashCode` | 根据类所拥有的非静态字段自动重写 `equals` 方法和 `hashCode` 方法 |
| `@Data` | 提供了更综合的生成代码功能（`@Getter` + `@Setter` + `@ToString` + `@EqualsAndHashCode`） |
| `@NoArgsConstructor` | 为实体类生成无参的构造器方法 |
| `@AllArgsConstructor` | 为实体类生成除了 `static` 修饰的字段之外带有各参数的构造器方法 |

> Lombok会在编译时，自动生成对应的Java代码。使用Lombok时，还需要安装Lombok插件（IDEA自带）。

## 基础操作

- 准备：
  - 准备数据库表 `emp`。
  - 创建一个新的 Spring Boot 工程，选择引入对应的起步依赖（MyBatis、MySQL、Lombok）。
  - 在 `application.properties` 中引入数据库连接信息。
    *示例：*
    ```properties
    spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
    spring.datasource.url=jdbc:mysql://localhost:3306/mybatis
    spring.datasource.username=root
    spring.datasource.password=1234
    ```
    
  - 创建对应的实体类 `Emp`（实体类属性采用驼峰命名）。
  - 准备 Mapper 接口 `EmpMapper`。

- 删除：
  - 接口方法示例：
    ```java
    @Delete("delete from emp where id = #{id}")
    public void delete(Integer id);
    ```
    
    > 如果 Mapper 接口方法的形参只有一个普通类型的参数，`#{...}` 里面的属性名可以随便写，例如：`#{id}`、`#{value}`。

- 日志输出：可以在 `application.properties` 中，打开 MyBatis 的日志，并指定输出到控制台。
  ```properties
  #指定 MyBatis 输出日志的位置，输出到控制台
  mybatis.configuration.log-  impl=org.apache.ibatis.logging.stdout.StdOutImpl
  ```

- 预编译 SQL优势：
  - 性能更高。
  - 更安全（防止 SQL 注入）。
    > SQL 注入：通过操作输入的数据来修改事先定义好的 SQL 语句，以达到执行代码对服务器进行攻击的方法。

- 参数占位符
  - `#{...}`
    - 执行 SQL 时，会将 `#{...}` 替换为 `?`，生成预编译 SQL，会自动设置参数值。
    - 使用时机：参数传递，都使用 `#{...}`。
  - `${...}`
    - 拼接 SQL。直接将参数拼接在 SQL 语句中，存在 SQL 注入问题。
    - 使用时机：如果对表名、列表进行动态设置时使用。

- 新增：
  - 接口方法示例：
    ```java
    @Insert("insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time) " +
            "values(#{username}, #{name}, #{gender}, #{image}, #{job}, #{entrydate}, #{deptId}, #{createTime}, #{updateTime})")
    public void insert(Emp emp);
    ```

  - 主键返回示例：
    ```java
    // 自动获取生成的主键值，并赋值给 emp 对象的 id 属性
    @Options(keyProperty = "id", useGeneratedKeys = true)
    @Insert("insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time) " +
            "values(#{username}, #{name}, #{gender}, #{image}, #{job}, #{entrydate}, #{deptId}, #{createTime}, #{updateTime})")
    public void insert(Emp emp);
    ```
  
- 更新：
  - 接口方法示例：
    ```java
    @Update("update emp set username=#{username}, name=#{name}, gender=#{gender}, image=#{image}, job=#{job}, entrydate=#{entrydate}, dept_id=#{deptId}, update_time=#{updateTime} where id=#{id}")
    public void update(Emp emp);
    ```
  
- 查询：
  - 根据ID查询，接口方法示例：
    ```java
    @Select("select * from emp where id = #{id}")
    public Emp getById(Integer id);
    ```
    
  - 条件查询，接口方法示例：
    ```java
    @Select("select * from emp where name like concat('%', #{name}, '%') and gender = #{gender} and entrydate between #{begin} and #{end} order by update_time desc")
    public List<Emp> list(String name, Short gender, LocalDate begin, LocalDate end);
    ```
    
    > CONCAT()：拼接字符串，常用于模糊查询，将 '%' 与查询内容拼接。
  
- 数据封装：
  - 实体类属性名和数据库表查询返回的字段名一致，MyBatis 会自动封装。
  - 如果实体类属性名和数据库表查询返回的字段名不一致，不能自动封装。
  - 解决方法：
    - **起别名**：在 SQL 语句中，对不一样的列名起别名，别名和实体类属性名一样。
    - **手动结果映射**： 通过 `@Results` 及 `@Result` 进行手动结果映射。
    - **开启驼峰命名**： 如果字段名与属性名符合驼峰命名规则，MyBatis 会自动通过驼峰命名规则映射。
      ```properties
      # 开启驼峰命名自动映射，即从数据库字段名 a_column 映射到 Java 属性名 aColumn。
      mybatis.configuration.map-underscore-to-camel-case=true
      ```
## XML映射文件

- 规范：
  - XML 映射文件的名称与 Mapper 接口名称一致，并且将 XML 映射文件和 Mapper 接口放置在相同包下（同包同名）。
  - XML 映射文件的 `namespace` 属性与 Mapper 接口全限定名一致。
  - XML 映射文件中 SQL 语句的 `id` 与 Mapper 接口中的方法名一致，并保持返回类型一致。

- 代码示例：
  - Mapper 接口
    ```java
    @Mapper
    public interface EmpMapper {
        public List<Emp> list(String name, Short gender, LocalDate begin, LocalDate end);
    }
    ```

  - XML 映射文件
    ```java
    <mapper namespace="com.itheima.mapper.EmpMapper">
        <select id="list" resultType="com.itheima.pojo.Emp">
            select * from emp where name like concat('%', #{name}, '%') and gender = #{gender}
            and entrydate between #{begin} and #{end} order by update_time desc
        </select>
    </mapper>
    ```
  
- 使用 MyBatis 的注解，主要是来完成一些简单的增删改查功能。如果需要实现复杂的 SQL 功能，建议使用 XML 来配置映射语句。
  > 官方说明：[https://mybatis.net.cn/getting-started.html](https://mybatis.net.cn/getting-started.html?utm_source=chatgpt.com)

## 动态SQL

### \<if\>

- `<if>`：用于判断条件是否成立。使用 `test` 属性进行条件判断，如果条件为 `true`，则拼接 SQL。
- `<where>`：`where` 元素只会在子元素有内容的情况下才插入 `where` 子句，而且会自动去除子句开头的 `AND` 或 `OR`。
- `<set>`：动态地在行首插入 `SET` 关键字，并会删除额外的逗号。（用于 `update` 语句中）

### \<foreach\>

- XML 映射文件示例：
  ```xml
  <delete id="deleteByIds">
      delete from emp where id in
      <foreach collection="ids" item="id" separator="," open="(" close=")">
          #{id}
      </foreach>
  </delete>
  ```

- 属性：
  - `collection`：集合名称。
  - `item`：集合遍历出来的元素/项。
  - `separator`：每一次遍历使用的分隔符。
  - `open`：遍历开始前拼接的片段。
  - `close`：遍历结束后拼接的片段。

### \<sql\> \<include\>

- `<sql>`：定义可重用的 SQL 片段。
- `<include>`：通过属性 `refid`，指定包含的 SQL 片段。

# 项目开发

## 环境搭建

- 准备项目所需的数据库及相关数据表。
- 创建 Spring Boot 工程，引入项目所需的起步依赖。
- 配置 `application.properties` 文件，包括数据库连接、持久层框架等相关配置信息。
- 创建实体类（Entity），用于封装数据。
- 搭建基础分层结构，包括 Mapper 层、Service 层（接口与实现类）和 Controller 层。

## 开发规范

### Restful

- **REST**，表述性状态转换是一种软件架构风格。通过 URL 定位资源，通过 HTTP 请求方法表示对资源的操作。
- **HTTP 请求方法**
  | 请求方法 | 作用 |
  |---|---|
  | GET | 查询资源 |
  | POST | 新增资源 |
  | PUT | 更新资源，通常表示整体替换 |
  | PATCH | 更新资源的部分内容 |
  | DELETE | 删除资源 |
  
> - REST 是风格，是约定方式，约定不是规定，可以打破。
> - 描述模块的功能通常使用复数，也就是加 s 的格式来描述，表示此类资源，而非单个资源。如：users。

### 统一响应结果

- 前后端交互统一响应结果 Result。

# 文件上传