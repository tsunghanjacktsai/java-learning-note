# XStream

## 介紹
- 可以把 JavaBean 轉換成(序列化為) xml

## XStream 的 jar 包
- 核心 jar 包: xstream-1.4.7.jar;
- 必須依賴包: xpp3_min-1.1.4c(XML Pull Parser, 一款速度很快的 XML 解析器)

## 使用步驟
- XStream xstream = new XStream();
- String xmlStr = xstream.toXML(javabean);

## 使用細節
- 別名: 把類型對應的元素名修改了
    - xstream.alias("china", List.class); //讓 List 類型生成的元素名為 china
    - xstream.alias("province", Province.class); //讓 Province 類型生成的元素名為 province
- 使用屬性: 默認類的成員，生成的是元素的子元素。我們希望讓累的成員生成元素的屬性
    - xstream.useAttributeFor(Province.class, "name"); //把 Province 類的名為 name 成員，生成 <province> 元素的 name 屬性
- 去除 Collection 類型的成員: 我們只需要 Collection 的內容，而不希望 Collection 本身也生成一個元素
    - xstream.addImplicitCollection(Province.class, "cities"); 讓 Province 類的名為 cities(他是 List 類型的，他的內容還會生成元素)的成員不生成元素
- 去除類的指定成員，讓其不生成 xml 元素
    - xstream.omitField(City.class, "description"); 在生成的 xml 中不會出現 City 類的，名為 description 的對應元素

## Example
```java
//Province.java
package pers.demo1;

import java.util.ArrayList;
import java.util.List;

public class Province {
	private String name; //省名
	private List<City> cities = new ArrayList<City>();
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public List<City> getCities() {
		return cities;
	}
	public void setCities(List<City> cities) {
		this.cities = cities;
	}
	public void addCity(City city) {
		cities.add(city);
	}
}

//City.java
package pers.demo1;

public class City {
	private String name; //市名
	private String descroption; //描述
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getDescroption() {
		return descroption;
	}
	public void setDescroption(String descroption) {
		this.descroption = descroption;
	}
	@Override
	public String toString() {
		return "City [name=" + name + ", descroption=" + descroption + "]";
	}
	public City(String name, String descroption) {
		super();
		this.name = name;
		this.descroption = descroption;
	}
	public City() {
		super();
		// TODO Auto-generated constructor stub
	}
}

//Demo1.java
package pers.demo1;

import java.util.ArrayList;
import java.util.List;

import org.junit.Test;

import com.thoughtworks.xstream.XStream;

/**
 * 演示 XStream
 * @author jack870131
 *
 */
public class Demo1 {
	//返回 javabean 集合
	public List<Province> getProvinceList() {
		Province p1 = new Province();
		p1.setName("台灣");
		p1.addCity(new City("台北市", "Taipei"));
		p1.addCity(new City("台中市", "Taichung"));
		
		Province p2 = new Province();
		p2.setName("北京");
		p2.addCity(new City("東城區", "Taipei"));
		p2.addCity(new City("昌平區", "Taichung"));
		
		List<Province> provinceList = new ArrayList<Province>();
		provinceList.add(p1);
		provinceList.add(p2);
		
		return provinceList;
	}
	
	@Test
	public void fun1() {
		List<Province> proList = getProvinceList();
		/*
		 * 創建 XStream 對象
		 * 調用 toXML 把集合轉換成 xml 字符串
		 */
		XStream xstream = new XStream();
		String s = xstream.toXML(proList);
		System.out.println(s);
	}
	
	/*
	 * 別名(alias)
	 * 希望:
	 *  - 默認 List 類型對應的 <list> 元素，希望讓 List 類型對應 <china> 元素
	 *  - 默認 Province 類型對應 <pers.demo1.Province>, 希望讓他對應 <province>
	 *  - 默認 City 類型對應 <pers.demo1.City>, 希望他對應 <city> 元素
	 */
	@Test
	public void fun2() {
		List<Province> proList = getProvinceList();
		XStream xstream = new XStream();
		/*
		 * 給指定的類型指定別名
		 */
		xstream.alias("china", List.class); //給 List 類型指定別名為 china
		xstream.alias("province", Province.class); //給 Province 類型指定別名為 province
		xstream.alias("city", City.class); //給 City 類型指定別名為 city
		
		String s = xstream.toXML(proList);
		System.out.println(s);
	}
	
	/*
	 * 默認 javabean 的屬性都會生成子元素，而現在希望生成元素的屬性
	 */
	@Test
	public void fun3() {
		List<Province> proList = getProvinceList();
		XStream xstream = new XStream();
		xstream.alias("china", List.class); //給 List 類型指定別名為 china
		xstream.alias("province", Province.class); //給 Province 類型指定別名為 province
		xstream.alias("city", City.class); //給 City 類型指定別名為 city
		
		/*
		 * 把 Province 類型的 name 屬性，生成 <province> 元素的屬性
		 */
		xstream.useAttributeFor(Province.class, "name");
		
		String s = xstream.toXML(proList);
		System.out.println(s);		
	}
	
	/*
	 * 去除 List 類型的屬性，只把 List 中的元素生成 xml 元素
	 */
	@Test
	public void fun4() {
		List<Province> proList = getProvinceList();
		XStream xstream = new XStream();
		xstream.alias("china", List.class); //給 List 類型指定別名為 china
		xstream.alias("province", Province.class); //給 Province 類型指定別名為 province
		xstream.alias("city", City.class); //給 City 類型指定別名為 city
		xstream.useAttributeFor(Province.class, "name"); //把 Province 類型的 name 屬性，生成 <province> 元素的屬性
		
		/*
		 * 去除 <cities> 這樣的 Collection 類型的屬性
		 * 去除 Province 類的名為 cities 的 List 類型的屬性
		 */
		xstream.addImplicitCollection(Province.class, "cities");
		
		String s = xstream.toXML(proList);
		System.out.println(s);	
	}
	
	/*
	 * 去除不想要的 javabean 屬性	
	 * 就是讓某些 javabean 屬性不生成對應的 xml 元素
	 */
	@Test
	public void fun5() {
		List<Province> proList = getProvinceList();
		XStream xstream = new XStream();
		xstream.alias("china", List.class); //給 List 類型指定別名為 china
		xstream.alias("province", Province.class); //給 Province 類型指定別名為 province
		xstream.alias("city", City.class); //給 City 類型指定別名為 city
		xstream.useAttributeFor(Province.class, "name"); //把 Province 類型的 name 屬性，生成 <province> 元素的屬性
		//去除 <cities> 這樣的 Collection 類型的屬性
	    //去除 Province 類的名為 cities 的 List 類型的屬性
		xstream.addImplicitCollection(Province.class, "cities");
		
		
		/*
		 * 讓 City 類的，名為 description 屬性不生成對應的 xml 元素
		 */
		xstream.omitField(City.class, "description");										
		
		String s = xstream.toXML(proList);
		System.out.println(s);
	}
}
```
