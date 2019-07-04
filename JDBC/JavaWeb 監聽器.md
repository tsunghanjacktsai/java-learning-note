# JavaWeb 監聽器

## 介紹
- JavaWeb 三大組件: Servlet, Listener, Filter
- 他是一個接口，內容由我們來實現
- 他需要註冊，例如註冊在按紐上
- 監聽器中的方法，會在特殊事件發生時被調用

在 JavaWeb 被監聽的事件源為: ServletContext, HttpSession, ServletRequest, 即三大域對象。

- ServletContext
    - 生命週期監聽: ServletContextListener, 他有兩個方法，一個在出生時調用，一個在死亡時調用。
        - void contextInitialized(ServletContextEvent sce): 創建 ServletContext 時。
        - void contextDestroyed(ServletContextEvent sce): 銷毀 ServletContext 時。
    - 屬性監聽: ServletContextAttributeListener, 他有三個方法，一個在添加屬性時調用，一個在替換屬性時調用，最後一個是在移除屬性時調用。
        - void attributeAdded(ServletContextAttributeEvent event): 添加屬性時。
        - void attributeReplaced(ServletContextAttributeEvent): 替換屬性時。
        - void attributeRemoved(ServletContextAttributeEvent event): 移除屬性時。
- HttpSession
    - 生命週期監聽: HttpSessionListener, 他有兩個方法，一個在出生時調用，一個在死亡時調用
        - void sessionCreated(HttpSessionEvent se): 創建 session 時。
        - void sessionDestroyed(HttpSessionEvent se): 銷毀 session 時
    - 屬性監聽: HttpSessionAttributeListener, 他有三個方法，一個在添加屬性時調用，一個在替換屬性時調用，最後一個是在移除屬性時調用。
        - void attributeAdded(HttpSessionBindingEvent event): 添加屬性時。
        - void attributeReplaced(HttpSessionBindingEvent): 替換屬性時。
        - void attributeRemoved(HttpSessionBindingEvent event): 移除屬性時。
- ServletRequest
    - 生命週期監聽: ServletRequestListener, 他有兩個方法，一個在出生時調用，一個在死亡時調用
        - void requestInitialized(ServletRequestEvent sre): 創建 request 時。
        - void requestDestroyed(ServletRequestEvent sre): 銷毀 request 時。
    - 屬性監聽: ServletRequestAttributeListener, 他有三個方法，一個在添加屬性時調用，一個在替換屬性時調用，最後一個是在移除屬性時調用。
        - void attributeAdded(ServletRequestAttributeEvent event): 添加屬性時。
        - void attributeReplaced(ServletRequestAttributeEvent): 替換屬性時。
        - void attributeRemoved(ServletRequestAttributeEvent event): 移除屬性時。
    

- JavaWeb 中完成編寫監聽器
    - 寫一個監聽器類: 要求必須實現某個監聽器接口。
    - 註冊，是在 web.xml 中配置來完成註冊。
- 事件對象:
    - ServletContextEvent: ServletContext getServletContext()
    - HttpSessionEvent: HttpSession getSession()
    - ServletRequest:   
        - ServletContext getServletContext()
        - ServletRequest getServletRequest()
    - ServletContextAttributeEvent:
        - ServletContext getServletContext()
        - String getName(): 獲取屬姓名
        - String getValue(): 獲取屬性值
    - HttpSessionBindingEvent
    - ServletRequestAttributeEvent
- 感知監聽(都與 HttpSession 相關)
    - 他用來添加到 JavaBean 上，而不是添加到三大域上
    - 這兩個監聽器都不需要在 web.xml 中註冊。
