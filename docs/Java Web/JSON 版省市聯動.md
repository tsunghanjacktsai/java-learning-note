# JSON 版省市聯動

## DAO
提供兩個方法

- 一個是查詢所有省
- 通過省名稱查詢指定的城市

## Servlet
兩個方法

- 一個把所有省轉換成 json, 發送給客戶端
- 通過獲取省名這個參數，然後查詢該省下的所有城市，轉換成 json, 發送給客戶端

## ajax1.jsp
- 頁面加載完成後:訪問 servlet, 得到所有省，然後顯示在 &lt;select id="province"&gt;
- 頁面加載完成後: 給 &lt;select id="province"&gt; 添加 onchange 事件監聽，獲取選擇的省名稱，訪問 servlet, 得到所有市，顯示在 &lt;select id="city"&gt; 中

## Example
```java
//web.xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>Java_Web</display-name>
  <servlet>
    <servlet-name>ProvinceServlet</servlet-name>
    <servlet-class>pers.web.servlet.ProvinceServlet</servlet-class>
  </servlet>
  <servlet>
    <servlet-name>CityServlet</servlet-name>
    <servlet-class>pers.web.servlet.CityServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>ProvinceServlet</servlet-name>
    <url-pattern>/ProvinceServlet</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
    <servlet-name>CityServlet</servlet-name>
    <url-pattern>/CityServlet</url-pattern>
  </servlet-mapping>
  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>
</web-app>

//Dao.java
package pers.dao;

import java.sql.SQLException;
import java.util.List;

import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanListHandler;

import cn.itcast.jdbc.TxQueryRunner;
import pers.domain.City;
import pers.domain.Province;

public class Dao {
	private QueryRunner qr = new TxQueryRunner();
	/**
	 * 查詢所有的省
	 * @return
	 */
	public List<Province> findAllProvince() {
		try {
			String sql = "select * from t_province";
			return qr.query(sql, new BeanListHandler<Province>(Province.class));
		} catch(SQLException e) {
			throw new RuntimeException(e);
		}
	}
	
	/**
	 * 查詢指定省下的所有城市
	 * @param proName
	 * @return
	 */
	public List<City> findByProvince(int pid) {
		try {
			String sql = "select * from t_city where pid=?";
			return qr.query(sql, new BeanListHandler<City>(City.class), pid);
		} catch(SQLException e) {
			throw new RuntimeException(e);
		}
	}
}

//Province.java
package pers.domain;

import java.util.List;

public class Province {
	private int pid;
	private String name;
	private List<City> cityList; //一方關聯多方
	
	public int getPid() {
		return pid;
	}
	public void setPid(int pid) {
		this.pid = pid;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public List<City> getCityList() {
		return cityList;
	}
	public void setCityList(List<City> cityList) {
		this.cityList = cityList;
	}
}

//City.java
package pers.domain;

public class City {
	private int cid;
	private String name;
	private Province province; //多方關聯一方
	
	public int getCid() {
		return cid;
	}
	public void setCid(int cid) {
		this.cid = cid;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Province getProvince() {
		return province;
	}
	public void setProvince(Province province) {
		this.province = province;
	}
}

//ProvinceServlet.java
package pers.web.servlet;

import java.io.IOException;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import net.sf.json.JSONArray;
import pers.dao.Dao;
import pers.domain.Province;

public class ProvinceServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		
		/*
		 * 1. 通過 dao 得到所有的省
		 * 2. 把 List<Province> 轉換成 json
		 * 3. 發送給客戶端
		 */
		Dao dao = new Dao();
		List<Province> provinceList = dao.findAllProvince();
		String json = JSONArray.fromObject(provinceList).toString();
		
		response.getWriter().print(json);
	}
}

//CityServlet.java
package pers.web.servlet;

import java.io.IOException;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import net.sf.json.JSONArray;
import pers.dao.Dao;
import pers.domain.City;

public class CityServlet extends HttpServlet {
	public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		
		/*
		 * 1. 獲取名為 pid 的參數
		 * 2. 使用這個省id查詢數據庫，得到 List<City>
		 * 3. 轉發成 json, 轉發給客戶端
		 */
		int pid = Integer.parseInt(request.getParameter("pid"));
		Dao dao = new Dao();
		List<City> cityList	= dao.findByProvince(pid);
		
		String json = JSONArray.fromObject(cityList).toString();
		response.getWriter().print(json);
	}
}

//ajax1.jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%
	String path = request.getContextPath();
	String basePath = request.getScheme() + "://" + request.getServerName() + ":" + request.getServerPort()
			+ path + "/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
<base href="<%=basePath%>">

<title>My JSP 'ajax1.jsp' starting page</title>

<meta http-equiv="pragma" content="no-cache">
<meta http-equiv="cache-control" content="no-cache">
<meta http-equiv="expires" content="0">
<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
<meta http-equiv="description" content="This is my page">
<!--
	<link rel="stylesheet" type="text/css" href="styles.css">
	-->

<script type="text/javascript">
	/*
	1. 在文檔加載完成後
		- 獲取所有省，添加到 <select id="province"> 中
		- 給 <select id="province"> 這個元素添加 onchange 事件
	事件內容:
		1. 獲取當前的省 id
		2. 使用省 id 訪問 servlet, 得到該省下所有市
		3. 把每個市添加到<select	id="city"> 中
	*/
	function createXMLHttpRequest() {
		try {
			return new XMLHttpRequest();	
		} catch(e) {
			try {
				return new ActiveXObject("Msxml2.XMLHTTP");
			} catch(e) {
				return new ActiveXObject("Microsoft.XMLHTTP");
			}
		}
	}

	window.onload = function() {
		/*
		發送異步請求，得到所有省，然後使用每個省生成一個<option>元素添加到<select id="province">中
		*/
		//得到核心對象
		var xmlHttp	= createXMLHttpRequest();
		//打開連接
		xmlHttp.open("GET", "<c:url value='/ProvinceServlet'/>", true);
		//發送
		xmlHttp.send(null);
		//添加監聽
		xmlHttp.onreadystatechange = function() {
			if(xmlHttp.readyState == 4) {
				if(xmlHttp.status == 200) {
					//執行服務器發送過來的json字符串，得到 js 對象
					var proArray = eval("(" + xmlHttp.responseText + ")");
					for(var i = 0; i < proArray.length; i++) {
						var pro = proArray[i]; //得到每個 pro 對象
						//創建<option>元素
						var optionEle = document.createElement("option");
						//給<option>元素的 value 屬性賦值
						optionEle.value = pro.pid; //給 <option> 的實際值賦為pid, 而不是 name
						var textNode = document.createTextNode(pro.name); //使用省名稱來創建 textNode
						optionEle.appendChild(textNode); //把 textNode 添加到 option 元素中
						
						//把 option 元素添加到 select 元素中
						document.getElementById("province").appendChild(optionEle);
					}
				}	
			}
		};
		
		/*
		2. 給 <select id="province">添加到 onchange 監聽
		*/
		document.getElementById("province").onchange = function() {
			var xmlHttp = createXMLHttpRequest();
			xmlHttp.open("POST", "<c:url value='/CityServlet'/>", true);
			xmlHttp.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
			xmlHttp.send("pid=" + this.value); //用戶選擇的省
			xmlHttp.onreadystatechange = function() {
				if(xmlHttp.readyState == 4) {
					if(xmlHttp.status == 200) {
						/*
						0. 先要清空原來的<option>元素
						1. 得到服務器發送過來的所有市
						2. 使用每個市生成<option>元素添加到<select id="city">中
						*/
						/*
						清空<select id="city">中選項
						*/
						var citySelect = document.getElementById("city");
						//獲取 select 中的所有元素
						var cityOptionArray = citySelect.getElementsByTagName("option");
						while(cityOptionArray.length > 1) { //這裡只控制循環的次數
							citySelect.removeChild(cityOptionArray[1]); //只刪除 1下標，不會刪除0
						}
						
						/*
						得到服務器發送過來的市
						*/
						var cityArray = eval("(" + xmlHttp.responseText	+ ")");
						//循環遍歷每個 city 對象，用來生成<option>元素添加到<select id="city">中
						for(var i = 0; i < cityArray.length; i++) {
							var city = cityArray[i]; //得到每個 city 對象
							//創建<option>元素
							var optionEle = document.createElement("option");
							//給<option>元素的 value 屬性賦值
							optionEle.value = city.cid; //給 <option> 的實際值賦為cid, 而不是 name
							var textNode = document.createTextNode(city.name); //使用省名稱來創建 textNode
							optionEle.appendChild(textNode); //把 textNode 添加到 option 元素中
							
							//把 option 元素添加到 select 元素中
							citySelect.appendChild(optionEle);
						}
					}
				}
			};
		};
	};
</script>
</head>

<body>
	<h1>省市聯動</h1>
	省:
	<select name="province" id="province">
		<option value="">===請選擇省分===</option>
	</select> 市:
	<select name="city" id="city">
		<option value="">===請選擇城市===</option>
	</select>
</body>
</html>
```
