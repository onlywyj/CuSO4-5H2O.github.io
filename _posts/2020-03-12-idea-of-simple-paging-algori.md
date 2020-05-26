---
layout: post
title: '简单分页算法的实现思路'
date: 2020-03-12
author: Only
tags: 分页算法 MySQL 
---

> 分页，是在Web开发中比较常见的一种方法

###  补充

发现一个插件：[MyBatis分页插件--PageHelper](https://pagehelper.github.io/)

### 分析

分页算法的核心，我认为是围绕SQL语句`SELECT * FROM table LIMIT [offset] , rows | rows OFFSET offset`进行的展开和补充。`offset`为指定第一个返回记录行的偏移量，也就是在表的什么位置开始返回数据，`rows | rows OFFSET offset`为指定返回记录行的最大数目。其中`offset`的默认初始值为**0**。例子：

```bash
SELECT * FROM table LIMIT 5,10; // 检索记录行 6-15
SELECT * FROM table LIMIT 95,-1; // 检索记录行 98-last
```

当数据量越来越大时，分页的数目也会越来越多，越往后`offset`所代表的偏移量也会越大，类似于：

```bash
SELECT * FROM table LIMIT 10000,20; //检索记录行 10000-10020
```

这会造成查询速度变慢，此时我们需要多种方式来提高查询效率，例如使用索引：

```bash
SELECT * FROM table WHERE user_id = 123 ORDER BY id LIMIT 50, 10; 
```

下面主要介绍数据量较少的情况。

### 思路

通过SQL语句`SELECT COUNT (*) FROM table`来获取该表数据的总条数，例子：

```bash
SELECT COUNT (*) FROM table; //如果表中数据有3条，就返回3
```

计算需要分多少页

```bash
	// 指定或者页面参数
	private int pageSize; // 每页显示多少条
	
	// 查询数据库
	private int recordCount;// 数据库一共有多少条
	
	// 计算
	private int pageCount; // 计算后得到的数值，总页数
	
	// 计算总页码
	pageCount = (recordCount + pageSize - 1) / pageSize;
```

如果页数过多，底部页码标签显示太多，可以尝试下面的方法：

```bash
	// 指定或者页面参数
	private int currentPage;// 当前页
	
	// 计算
	private int beginPageIndex;// 页面列表的开始索引（包括）
	private int endPageIndex; // 页面列表的结束索引（包括）
	
	// 计算beginPageIndex和endPageIndex
	// 总页数不大于10，则全部显示
	if (pageCount <= 10) {
			beginPageIndex = 1;	
			endPageIndex = pageCount;		
		} else {
			// 总页数大于10，则显示当前页附近的10页		
			beginPageIndex = currentPage - 4;	
			endPageIndex = currentPage + 5;	
			// 当前面的页码数量不足4个时，则显示前10个页码		
			if (beginPageIndex < 1) {			
				beginPageIndex = 1;			
				endPageIndex = 10;
			}	
			// 当后面的页码数量不足5个时，则显示后10个页码
			if (endPageIndex > pageCount) {	
				endPageIndex = pageCount;			
				beginPageIndex = pageCount - 10 + 1;
			}
		}
```

拿到数据，建议使用mapper接口的方法，使用MyBatis逆向工程自动生成相应的接口文件，根据需求增减功能，提高开发效率

dao层

```bash
@Override
	public ArrayList<User> getAllUsersByPage(int currentPage, int pageSize) {
		ArrayList<User> arrayUsersByPage = new ArrayList<User>();
		String sql = "select * from tuserlogin limit " + (currentPage - 1) * pageSize + "," + pageSize;
		String[] parameters = null;
		arrayUsersByPage = SQLHelper.executeQueryUser(sql, parameters);
		return arrayUsersByPage;
	}
```

mapper接口

```bash
<select id="getAllUserByPage" parameterType="Integer" resultType="user"> 
		select * from employee limit #{0},#{1}
</select>
```

```bash
int beginIndex = (currentPage-1)*pageSize;
ArrayList<Employee> allUser = userService.getAllUserByPage(beginIndex, pageSize); 
```

JSP页面分页标签显示

```bash
<!-- 数据分页 -->
<div style="text-align: center">
	<nav aria-label="Pagenavigation">
	<ul class="pagination">
		<li><a href="#" aria-label="Previous"><span aria-hidden="true">&laquo;</span></a></li>
		<c:forEach begin="1" end="${pageBean.pageCount}" var="i">
			<li><a href="PageServlet?currentPage=${i}">${i}</a></li>
		</c:forEach>
		<li><a href="#" aria-label="Next"><span aria-hidden="true">&raquo;</span></a></li>
	</ul>
	</nav>
</div>
<!-- 分页结束-->
```
