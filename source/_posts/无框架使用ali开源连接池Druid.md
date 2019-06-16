---
title: 无框架使用alibaba开源代码连接池Druid
date: 2017-10-11 21:50:52
tags: [Java, Web]
categories:
- 开源软件
- Druid
---

> Druid是Java语言中最好的数据库连接池。Druid能够提供强大的监控和扩展功能。--温少(alibaba开源Druid负责人)

## 准备工作
1. <a href="https://github.com/sivanWu0222/Druid-Test/blob/master/druid-1.1.4.jar">下载核心jar包</a>
2. 下载log4j的<a href="https://github.com/sivanWu0222/Druid-Test/blob/master/log4j-1.2.9.jar">jar包</a>以及<a href="https://github.com/sivanWu0222/Druid-Test/blob/master/log4j.properties">配置文件</a>
3. 拷贝对应的数据库驱动jar包到工作空间中(由于所druid可以跨数据库进行开发，因此我们需要将我们自己使用的数据库驱动拷贝到工作空间中)

------

## 使用druid实现连接到数据库
1. WEB-INF下新建一个文件,命名为 db_server.properties ,配置如下
``` Html
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/master
username=root
password=root
filters=stat
initialSize=2
maxActive=300
maxWait=60000
timeBetweenEvictionRunsMillis=60000
minEvictableIdleTimeMillis=300000
validationQuery=SELECT 1
testWhileIdle=true
testOnBorrow=false
testOnReturn=false
poolPreparedStatements=false
maxPoolPreparedStatementPerConnectionSize=200
```
> 注意修改自己的数据库用户名，密码以及url和driverClassName



2. 创建一个连接池工具管理类，命名为DBPoolConnection.java


<!-- more -->
```Java

import java.io.File;
import java.io.FileInputStream;
import java.io.InputStream;
import java.sql.SQLException;
import java.util.Properties;
import org.apache.log4j.Logger;

import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.pool.DruidDataSourceFactory;
import com.alibaba.druid.pool.DruidPooledConnection;

/**
 * 要实现单例模式，保证全局只有一个数据库连接池
 */
public class DBPoolConnection {
	static Logger log = Logger.getLogger(DBPoolConnection.class);
	private static DBPoolConnection dbPoolConnection = null;
	private static DruidDataSource druidDataSource = null;

	static {
		Properties properties = loadPropertiesFile("db_server.properties");
		try {
			druidDataSource = (DruidDataSource) DruidDataSourceFactory.createDataSource(properties); /* DruidDataSrouce工厂模式*/
		} catch (Exception e) {
			log.error("获取配置失败");
		}
	}

	/**
	 * 数据库连接池单例
	 *
	 * @return
	 */
	public static synchronized DBPoolConnection getInstance() {
		if (null == dbPoolConnection) {
			dbPoolConnection = new DBPoolConnection();
		}
		return dbPoolConnection;
	}

	/**
	 * 返回druid数据库连接
	 *
	 * @return
	 * @throws SQLException
	 */
	public DruidPooledConnection getConnection() throws SQLException {
		return druidDataSource.getConnection();
	}

	/**
	 * @param string
	 *            配置文件名
	 * @return Properties对象
	 */
	private static Properties loadPropertiesFile(String fullFile) {
		String webRootPath = null;
		if (null == fullFile || fullFile.equals("")) {
			throw new IllegalArgumentException("Properties file path can not be null" + fullFile);
		}
		webRootPath = DBPoolConnection.class.getClassLoader().getResource("").getPath();
		webRootPath = new File(webRootPath).getParent();
		InputStream inputStream = null;
		Properties p = null;
		try {
			inputStream = new FileInputStream(new File(webRootPath + File.separator + fullFile));
			p = new Properties();
			p.load(inputStream);
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if (null != inputStream) {
					inputStream.close();
				}
			} catch (Exception e) {
				e.printStackTrace();
			}
		}

		return p;
	}

}

```
3. 将下载好的log4j的配置文件拷贝到src目录下，jar包拷贝到WEB-INF目录下的lib目录中

4. 编写测试代码
```Java
package com.test;

import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.pool.DruidPooledConnection;
import com.util.DBPoolConnection;

public class Test1 {

	public static void main(String[] args) {

		DBPoolConnection connection = DBPoolConnection.getInstance();
		DruidPooledConnection conn = null;
		try {
			conn = connection.getConnection();
		} catch (SQLException e1) {
			/* TODO Auto-generated catch block*/
			e1.printStackTrace();
		}
		System.out.println(conn);

	}

	}


```


------

## 使用druid实现监控功能

> druid不仅仅拥有美好的连接池功能，甚至提供了后台的监视功能

<img src="http://on3w7gc9m.bkt.clouddn.com/QQ%E5%9B%BE%E7%89%8720171011230452.png"/>

1. web.xml中配置filter:
```xml
<filter>
  	<filter-name>DruidWebStatFilter</filter-name>
  	<filter-class>com.alibaba.druid.support.http.WebStatFilter</filter-class>
  	<init-param>
  		<param-name>exclusions</param-name>
  		<param-value>*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*</param-value>
  	</init-param>
  </filter>
  <filter-mapping>
  	<filter-name>DruidWebStatFilter</filter-name>
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
```

2. web.xml中配置Servlet:
```xml
<servlet>
    <servlet-name>DruidStatView</servlet-name>
    <servlet-class>com.alibaba.druid.support.http.StatViewServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>DruidStatView</servlet-name>
    <url-pattern>/druid/*</url-pattern>
</servlet-mapping>
```

3. 开启服务器，url地址为 http://localhost:8080/项目名/druid/，就可以看到关于druid的监控状况

------

总结：之前Hibernate默认采用的c3p0数据源，由于8个小时不执行SQL操作，将会默认关闭所有连接，很不和谐，所以建议采用druid，同时可以进行监控后台SQL运行情况，甚至可以进行安全监视
