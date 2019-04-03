# Servlet3.0 異步處理

## 介紹
在服務器沒有結束響應之前，瀏覽器是看不到響應內容的，只有響應結束時，瀏覽器才能顯示結果。現在異步李的作用: 在服務器開始響應後，瀏覽器就可以看到響應內容，不用等待服務器響應結束

## 步驟
- 得到 AsyncContext, 他一步上下文對象
```java
AsysncContext ac = request.startAsync(request, response);
```

- 給上下文一個 Runnable 對象，啟動他。(給上下文一個任務，讓他完成)
```java
ac.start(new Runnable() {
    public void run() {
        ...    
    } 
});
```

- @WebServlet(urlPatterns="/AServlet", asyncSupported=true)

## Example
```java
package pers.web.servlet;

import java.io.IOException;

import javax.servlet.AsyncContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 添加WebServlet注解
 * @author jack870131
 *
 */
@WebServlet(urlPatterns="/AServlet", asyncSupported=true)
public class AServlet extends HttpServlet {
//	public static void main(String[] args) {
//		System.out.println("hello");
//		new Thread() {
//			public void run() {
//				
//			}
//		}.start();
//		
//		System.out.println("不知道上面的線程是否結束！");
//	}
	public void doGet(final HttpServletRequest req, final HttpServletResponse resp)
			throws ServletException, IOException {
		resp.setContentType("text/html;charset=utf-8");
		
	// 支持IE！如果輸出不足512B，沒有異步效果！
		for(int i = 0; i <= 512; i++) {
			resp.getWriter().print("a");
		}
		resp.getWriter().flush();
		
		/*
		 * 1. 得到異步上下文對象
		 */
		final AsyncContext ac = req.startAsync(req, resp);
		
		/*
		 * 2. 給上下文對象一個Runnable對象，讓它執行這個任務
		 */
		ac.start(new Runnable() {
			public void run() {
				println("現在開始<br/>", resp);
				sleep(2000);
				for(char c = 'A'; c <= 'Z'; c++) {
					println(c+"", resp);
					sleep(250);
				}
				
				// 通知Tomcat我們已經執行結束了！
				ac.complete();
			}
		});
	}
	
	public void println(String text, HttpServletResponse resp) {
		try {
			resp.getWriter().print(text);
			resp.getWriter().flush();
		} catch (IOException e) {
		}
	}
	
	public void sleep(long ms) {
		try {
			Thread.sleep(ms);
		} catch (InterruptedException e) {
		}
	}
}
package pers.web.servlet;

import java.io.IOException;

import javax.servlet.AsyncContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 添加WebServlet注解
 * @author jack870131
 *
 */
@WebServlet(urlPatterns="/AServlet", asyncSupported=true)
public class AServlet extends HttpServlet {
//	public static void main(String[] args) {
//		System.out.println("hello");
//		new Thread() {
//			public void run() {
//				
//			}
//		}.start();
//		
//		System.out.println("不知道上面的線程是否結束！");
//	}
	public void doGet(final HttpServletRequest req, final HttpServletResponse resp)
			throws ServletException, IOException {
		resp.setContentType("text/html;charset=utf-8");
		
	// 支持IE！如果輸出不足512B，沒有異步效果！
		for(int i = 0; i <= 512; i++) {
			resp.getWriter().print("a");
		}
		resp.getWriter().flush();
		
		/*
		 * 1. 得到異步上下文對象
		 */
		final AsyncContext ac = req.startAsync(req, resp);
		
		/*
		 * 2. 給上下文對象一個Runnable對象，讓它執行這個任務
		 */
		ac.start(new Runnable() {
			public void run() {
				println("現在開始<br/>", resp);
				sleep(2000);
				for(char c = 'A'; c <= 'Z'; c++) {
					println(c+"", resp);
					sleep(250);
				}
				
				// 通知Tomcat我們已經執行結束了！
				ac.complete();
			}
		});
	}
	
	public void println(String text, HttpServletResponse resp) {
		try {
			resp.getWriter().print(text);
			resp.getWriter().flush();
		} catch (IOException e) {
		}
	}
	
	public void sleep(long ms) {
		try {
			Thread.sleep(ms);
		} catch (InterruptedException e) {
		}
	}
}
```
