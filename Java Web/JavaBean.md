# JavaBean

## 規範
- 必須要有一個默認構造器。
- 提供 get/set 方法，如果只有 get 方法，那麼這個屬性是只讀屬性。
- 屬性: 有 get/set 方法的成員，還可以沒有成員，只有 get/set 方法，屬性名稱由 get/set 方法來決定，而不是成員名稱。
- 方法名稱滿足一定的規範，那麼他就是屬性。 boolean 類型的屬性，他的讀方法可以是 is 開頭，也可以是 get 開頭。

## 內省
內省類 -> Bean 信息 -> 屬性描述符 -> 屬性的 get/set 對應的 Method -> 反射(依賴反射)

- commons-beanutils: 依賴內省完成。
    - 導包:
        - commons-beanutils.jar
        - commons-logging.jar

```java
//Person.java
public class Person {
	private String name;
	private int age;
	private String gender;
	private boolean bool;
	
	public boolean isBool() {
		return bool;
	}

	public void setBool(boolean bool) {
		this.bool = bool;
	}

	public String getId() {
		return "fdsafdafdas";
	}

	public String getName() {
		return name;
	}

	public void setName(String username) {
		this.name = username;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public String getGender() {
		return gender;
	}

	public void setGender(String gender) {
		this.gender = gender;
	}

	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + ", gender=" + gender
				+ "]";
	}

	public Person() {
		super();
		// TODO Auto-generated constructor stub
	}

	public Person(String name, int age, String gender) {
		super();
		this.name = name;
		this.age = age;
		this.gender = gender;
	}
}

//User.java
public class User {
	private String username;
	private String password;
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	@Override
	public String toString() {
		return "User [username=" + username + ", password=" + password + "]";
	}
	public User() {
		super();
		// TODO Auto-generated constructor stub
	}
	public User(String username, String password) {
		super();
		this.username = username;
		this.password = password;
	}
}

//CommonUtils.java
import java.util.Map;
import java.util.UUID;

import org.apache.commons.beanutils.BeanUtils;

public class CommonUtils {
	/**
	 * 生成不重复的32位长的大写字符串
	 * @return
	 */
	public static String uuid() {
		return UUID.randomUUID().toString().replace("-", "").toUpperCase();
	}
	
	/**
	 * 把map转换成指定类型的javaBean对象
	 * @param map
	 * @param clazz
	 * @return
	 */
	public static <T> T toBean(Map map, Class<T> clazz) {
		try {
			/*
			 * 1. 创建指定类型的javabean对象
			 */
			T bean = clazz.newInstance();
			/*
			 * 2. 把数据封装到javabean中
			 */
			BeanUtils.populate(bean, map);
			/*
			 * 返回javabean对象
			 */
			return bean;
		} catch(Exception e) {
			throw new RuntimeException(e);
		}
	}
}

//Demo1.java
import java.util.HashMap;
import java.util.Map;

import org.apache.commons.beanutils.BeanUtils;
import org.junit.Test;

import utils.CommonUtils;

public class Demo1 {
	@Test
	public void fun1() throws Exception {
		String className = "domain.Person";
		Class clazz = Class.forName(className);
		Object bean = clazz.newInstance();
		
		BeanUtils.setProperty(bean, "name", "Jack");
		BeanUtils.setProperty(bean, "age", "23");
		BeanUtils.setProperty(bean, "gender", "Male");
		BeanUtils.setProperty(bean, "xxx", "XXX");
		
		String age = BeanUtils.getProperty(bean, "age");
		System.out.println(age);
		System.out.println(bean);
	}
	
	/*
	 * 把map中的属性直接封装到一个bean中　
	 * 
	 * Map: {"username":"zhangSan", "password","123"}
	 * 我们要把map的数据封装到一个javabean中！要求map的key与bean的属性名相同！
	 */
	@Test
	public void fun2() throws Exception {
		Map<String,String> map = new HashMap<String,String>();
		map.put("username", "zhangSan");
		map.put("password", "123");
		
		User user = new User();
		BeanUtils.populate(user, map);
		
		System.out.println(user);
	}
	
	@Test
	public void fun3() {
		Map<String,String> map = new HashMap<String,String>();
		map.put("username", "zhangSan");
		map.put("password", "123");
		
		/*
		 * request.getParameterMap();
		 */
		
		User user = CommonUtils.toBean(map, User.class);
		System.out.println(user);
	}
}
```

## JSP 中與 JavaBean 相關的標籤
- jsp:useBean -> 創建或查詢 bean
    - &lt;jsp:useBean id="user" class"domain.user" scope="session"&gt; -> 在 session 域中查找名為 user1 的 bean, 如果不存在，創建之。
- jsp:setProperty
    - &lt;jsp:setProperty property="username" name="user1" value"admin&gt; -> 設置名為 user1 的 javabean 的 javaname 的 username 屬性值為 admin
- jsp:getProperty
    - &lt;getProperty property="username" name="user1"&gt; -> 獲取名為 user1 的 javabean 的名為 username 的屬性值
    
