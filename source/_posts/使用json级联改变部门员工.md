---
title: 使用json级联改变部门员工
date: 2017-10-30 09:55:07
tags: [Hibernate,json]
categories:
- json
- 级联
---


> 这里使用Hibernate + json + Ajax + 级联改变部门员工

## 基本的页面编写：
```html

<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
  </head>

  <body>
  	部门：<select id="dep"></select>
  	员工：<select id="emp"></select>
  </body>
</html>
```

------
## servlet的生成

> DepServlet
```Java

import java.io.IOException;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.dao.DepDAO;
import com.pojo.Dep;
@WebServlet("/dep.do")
public class DepServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;


	private DepDAO depDao = new DepDAO();

	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		response.setContentType("text/html;charset=utf-8");

		String method = request.getParameter("method");
		if ( "dep".equals(method) ) {
			doDep(request, response);
		}

	}

	/**
	 * @throws IOException
	 *
	 */
	private void doDep(HttpServletRequest request, HttpServletResponse response) throws IOException {
		List<Dep> list = depDao.findAll();


		/*将list转换为json格式的数据*/
		StringBuffer sb = new StringBuffer();
		sb.append("[");
		for (Dep dep : list) {
			sb.append("{ 'depId': '" + dep.getDepId() + "', 'depName': '" + dep.getDepName() + "' }," );
		}
		sb.deleteCharAt(sb.length() - 1);
		sb.append("]");
		response.getWriter().print(sb.toString());
	}

}

```

<!-- more -->

>EmpServlet
```Java
import java.io.IOException;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.dao.EmpDAO;
import com.pojo.Emp;

/**
 * Servlet implementation class EmpServlet
 */
@WebServlet("/emp.do")
public class EmpServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	private EmpDAO empDao = new EmpDAO();

	/* (non-Javadoc)
	 * @see javax.servlet.http.HttpServlet#service(javax.servlet.http.HttpServletRequest, javax.servlet.http.HttpServletResponse)
	 */
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		String method = request.getParameter("method");
		if("findAll".equals(method)) {
			doFind(request, response);
		}

	}

	/**
	 * @throws IOException
	 *
	 */
	private void doFind(HttpServletRequest request, HttpServletResponse response) throws IOException {

		String depId = request.getParameter("depId");
		List<Emp> list = empDao.findByProperty("dep.depId", Integer.parseInt(depId));

		System.out.println(list.isEmpty());
		StringBuffer sb = new StringBuffer();
		sb.append("[");
		for (Emp emp : list) {
			sb.append("{ 'empId': '" + emp.getEmpId() + "', 'empName': '" + emp.getEmpName() + "'},");
		}
		sb.deleteCharAt(sb.length()-1);
		sb.append("]");
		response.getWriter().print(sb.toString());

	}


}

```

### json的使用
> 当我们使用java的时候，经常要给页面返回东西，但是我们又不可以直接返回Java中的数据类型，所以只能通过中间格式进行转换，中间格式在这里也就是Java和JavaScript都可以解析的格式

#### 格式
> 使用json简单的保存一个数据

一个简单的json { '姓名': '李四', '年龄': '25'};

> 使用json保存数据组

[
  {
    '姓名': '王五',
    '年龄': '18'
  },
  {
    '姓名': '李四',
    '年龄': '19'
  },
  {
    '姓名': '赵六',
    '年龄': '58'
  }
]




-------

## Ajax的编写

```JavaScript
<script>
			var request = new XMLHttpRequest();
			request.open("GET", "dep.do?method=dep");
			request.send();

			/*首先操作部门*/
  			var dep = document.getElementById("dep");
			request.onreadystatechange = function () {
				if ( request.readyState == 4 && request.status == 200) {
					var list = request.responseText;
					var s = eval(list);
					for ( var i = 0; i < s.length; i++ ) {
						var option = new Option(s[i].depName, s[i].depId);
						dep.add(option);
					}

					emp();
				}
			}





			/*操作员工，第一次默认是哪个，然后改变事件员工也要随着改变*/
			function emp() {
				var emp = document.getElementById("emp");

				/*实现每次改变父节点的时候删除原有的子节点*/
				/*方法一：*/
				while(emp.hasChildNodes()) {
					emp.removeChild(emp.firstChild);
				}

				/*方法二：*/
				/*emp.innerHTML = "";*/



				var request2 = new XMLHttpRequest();
				request2.open("get", "emp.do?method=findAll&depId=" + dep.value);
				request2.send();
				request2.onreadystatechange = function() {
					if( request2.readyState == 4 && request2.status == 200 ) {
						var str = eval(request2.responseText);
						for ( var i = 0; i < str.length; i++ ) {
							var option = new Option(str[i].empName, str[i].empId);
							emp.add(option);
						}
					}
				}
			}


			dep.onchange = function() {
				emp();
			}



  </script>
```

----
