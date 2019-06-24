# BaseServlet

## 介紹
- 我們希望在一個 Servlet 中可以有多個請求處理方法。
- 客戶端發送請求時，必須多給出一個參數，用來說明要調用的方法。
  請求處理方法的簽名必須與 **service 相同**，即返回值和參數，以及聲明的異常都相同。
- 客戶端必須傳遞名為 methoed 的參數

## Example
```java
//BaseServlet.java
package servlet;

import java.io.IOException;
import java.lang.reflect.Method;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public abstract class BaseServlet extends HttpServlet {
	@Override
	public void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		/*
		 * 1. 獲取參數，用來識別用戶請求方法
		 * 2. 判斷是哪一個方法，是哪一個我們就調用哪一個
		 */
		String methodName = req.getParameter("method");
		
		if(methodName == null || methodName.trim().isEmpty()) {
			throw new RuntimeException("您沒有傳遞 method 參數，無法確定您想調用的方法");
		}
		
		/*
		 * 得到方法名稱，是否可以通過反射來調用方法
		 * 1. 得到方法名，通過方法名再得到 Method 類的對象
		 * 		- 需要得到 Class, 然後調用他的方法進行查詢，得到 Method
		 * 		- 我們要查詢的是當前類的方法，所以我們需要得到當前類的 Class
		 */
		Class c = this.getClass(); //得到當前類的 Class 對象
		Method method = null;
		try {
			method = c.getMethod(methodName, 
					HttpServletRequest.class, HttpServletResponse.class);
		} catch (Exception e) {
			throw new RuntimeException("您要調用的方法: " + methodName + "不存在");
		}
		
		/*
		 * 調用 method 表示的方法
		 */
		try {
			String result = (String) method.invoke(this, req, resp);
			/*
			 * 獲取請求處理方法執行後返回的字符串，他表示轉發或重定向的路徑
			 * 幫他完成轉發或重定向
			 */
			/*
			 * 如果用戶返回的字符串為 null, 或"", 那麼我們甚麼也不做
			 */
			if(result == null || result.trim().isEmpty()) {
				return;
			}
			/*
			 * 查看返回的字符串中是否包含冒號，如果沒有，表示轉發
			 * 如果有，使用冒號分割字符串，得到前綴和後綴
			 * 其中前綴如果是f, 表示轉發，如果是r表示重定向, 後綴就是要轉發或重定向的路徑
			 */
			if(result.contains(":")) {
				//使用冒號分隔字符串，得到前綴和後綴
				int index = result.indexOf(":"); //獲取冒號的位置
				String s = result.substring(0, index); //擷取出前綴，表示操作
				String path = result.substring(index + 1); //擷取出後綴，表示路徑
				if(s.equalsIgnoreCase("r")) { //如果前綴是 r，表示重定向
					resp.sendRedirect(req.getContextPath() + path);
				} else if(s.equalsIgnoreCase("f")) {
					req.getRequestDispatcher(path).forward(req, resp);
				} else {
					throw new RuntimeException("你指定的操作:" + s + ", 當前版本還不支持");
				}
			} else { //沒有冒號默認為轉發
				req.getRequestDispatcher(result).forward(req, resp);
			}
		} catch (Exception e) {
			System.out.println("您調用的方法: " + methodName + "(HttpServletRequest, HttpServletResposne), 內部拋出異常");
			throw new RuntimeException(e);
		}
	}
}

//AServlet.java
package servlet;

import java.io.IOException;
import java.io.PrintWriter;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 給出多個請求處理方法
 * 請求處理方法: 除了名稱以外，都與 service 相同
 * @author jack870131
 *
 */
public class AServlet extends BaseServlet {
	public void addUser(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("addUser()");
		throw new IOException("Test");
	}
	
	public void editUser(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("editUser()");
	}

	public void deleteUser(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("deleteUser()");
	}
	
	public void loadUser(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("loadUser()");
	}
}

//BServlet.java
package servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class BServlet extends BaseServlet {
	public String fun1(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("fun1()");
		return "index.jsp";
	}

	public String fun2(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("fun2()");
		return "r:/index.jsp";
	}
	
	public String fun3(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("fun3()");
		return null;
	}
}
```
