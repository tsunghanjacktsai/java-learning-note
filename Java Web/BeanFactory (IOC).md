# BeanFactory (IOC)

## Example
```java
//Student.java
package pers.domain;

public class Student {
	private String number;
	private String name;
	private int age;
	private String sex;
	private Teacher teacher;
	
	public Teacher getTeacher() {
		return teacher;
	}
	public void setTeacher(Teacher teacher) {
		this.teacher = teacher;
	}
	public String getNumber() {
		return number;
	}
	public void setNumber(String number) {
		this.number = number;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}
	@Override
	public String toString() {
		return "Student [number=" + number + ", name=" + name + ", age=" + age + ", sex=" + sex + "]";
	}
	public Student() {
		super();
	}
	public Student(String number, String name, int age, String sex) {
		super();
		this.number = number;
		this.name = name;
		this.age = age;
		this.sex = sex;
	}
}

//Teacher.java
package pers.domain;

public class Teacher {
	private String tid;
	private String name;
	private double salary;
	public String getTid() {
		return tid;
	}
	public void setTid(String tid) {
		this.tid = tid;
	}
	public double getSalary() {
		return salary;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	@Override
	public String toString() {
		return "Teacher [tid=" + tid + ", salary=" + salary + "]";
	}
	public Teacher(String tid, double salary) {
		super();
		this.tid = tid;
		this.salary = salary;
	}
	public Teacher() {
		super();
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
}

//StudentDao.java
package pers.dao;

import pers.domain.Student;

public interface StudentDao {
	void add(Student stu);
	void update(Student stu);
}

//StudentDaoImpl
package pers.dao.impl;

import pers.dao.StudentDao;
import pers.domain.Student;

public class StudentDaoImpl implements StudentDao {
	@Override
	public void add(Student stu) {
		System.out.println("StudentImpl.add()");
	}

	@Override
	public void update(Student stu) {
		System.out.println("StudentImpl.update()");
	}
}

//StudentService.java
package pers.service;

public interface StudentService {
	void login();
}

//StudentServiceImpl.java
package pers.service.impl;

import pers.dao.StudentDao;
import pers.service.StudentService;

public class StudentServiceImpl implements StudentService {
	private StudentDao studentDao = null;
	
	//誰調用 service 方法，誰就需要先調用本方法，提供 dao
	public void setStudentDao(StudentDao studentDao) {
		this.studentDao = studentDao;
	}
	
	@Override
	public void login() {
		studentDao.add(null);
		studentDao.update(null);
	}
}

//beans.xml
<?xml version="1.0" encoding="UTF-8"?>
<beans>
	<bean id="stu1" className="pers.domain.Student">
		<property name="number" value="00001"/>
		<property name="name" value="Jack"/>
		<property name="age" value="30"/>
		<property name="sex" value="Male"/>
		<property name="teacher" ref="tea"/> <!-- ref 的值必須是另一個 id -->
	</bean>
	<bean id="stu2" className="pers.domain.Student">
		<property name="number" value="00002"/>
		<property name="name" value="Jervis"/>
		<property name="age" value="40"/>
		<property name="sex" value="Male"/>
		<property name="teacher" ref="tea"/> <!-- ref 的值必須是另一個 id -->
	</bean>
	<bean id="tea" className="pers.domain.Teacher">
		<property name="tid" value="00002"/>
		<property name="name" value="Jason"/>
		<property name="salary" value="19203"/>
	</bean>
	
	<bean id="stuDao" className="pers.dao.impl.StudentDaoImpl">
	</bean>
	
	<bean id="stuService" className="pers.service.impl.StudentServiceImpl">
		<property name="studentDao" ref="stuDao"/>
	</bean>
</beans>

//Demo1.java
package test;

import org.junit.Test;

import cn.itcast.beanfactory.BeanFactory;
import pers.dao.impl.StudentDaoImpl;
import pers.domain.Student;
import pers.service.StudentService;

/*
 * 面向接口編程
 * dao
 * -> daoImpl
 * service
 * -> serviceImpl
 */
public class Demo1 {
	@Test
	public void fun1() {
		/*
		 * 1. 創建工廠
		 * 2. 從工廠中獲取 bean 對象
		 */
		BeanFactory bf = new BeanFactory("beans.xml");
		Student s1 = (Student) bf.getBean("stu1");
		Student s2 = (Student) bf.getBean("stu2");
		System.out.println(s1 == s2);
	}
	
	@Test
	public void fun2() {
		/*
		 * 1. 創建工廠
		 * 2. 從工廠中獲取 bean 對象
		 */
		BeanFactory bf = new BeanFactory("beans.xml");
		Student s1 = (Student) bf.getBean("stu1");
		Student s2 = (Student) bf.getBean("stu2");
		System.out.println(s1.getTeacher() == s2.getTeacher());
	}
	
	@Test
	public void fun3() {
		BeanFactory bf = new BeanFactory("beans.xml");
		StudentDaoImpl stuDao = (StudentDaoImpl) bf.getBean("stuDao");
		stuDao.add(null);
		stuDao.update(null);
	}
	
	@Test
	public void fun4() {
		BeanFactory bf = new BeanFactory("beans.xml");
		StudentService service = (StudentService) bf.getBean("stuService");
		service.login();
	}
}
```
