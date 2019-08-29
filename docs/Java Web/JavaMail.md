# JavaMail

## 介紹
是 java 提供的一組 API, 用來發送和接收郵件。
![](https://github.com/jack870131/Markdown-Pic/blob/master/Picture/JavaMail%201.png?raw=true)

## 郵件協議
與 HTTP 相同，收發郵件也是需要有傳輸協議的
- SMTP (Simple Mail Transfer Protocol, 簡單郵件傳輸協議): 發郵件協議。
- POP3 (Post Office Protocol Version 3, 郵局協議第3版): 收郵件協議。
- IMAP (Internet Message Access Protocol, 因特網消息訪問協議): 收發郵件協議。

## 郵件收發過程
![](https://github.com/jack870131/Markdown-Pic/blob/master/Picture/JavaMail%202.png?raw=true)

- 導包
    - mail.jar
    - activation.jar
- 核心類
    - Session
        - 如果得到 Session, 表示已經與服務器連接上了，與 Connection 的作用相似。
        - 得到 Session，需要使用 Session.getInstance(Properties, Authenticator);

        ```java
        Properties props = new Properties();
        props.setProperty("mail.host", "smtp.outlook.com");
        props.setProperty("mail.smtp.auth", "true");
        Authenticator auth = new Authenticator() {
         protected PasswordAuthentication getPasswordAuthentication() {
            return new PasswordAuthentication("jack870131", "xxx");
         }
        };
        Session session = Session.getInstance(props, auth);
        ```
    - MimeMessage
        - 表示一個郵件對象，可以調用他的 setFrom(), 設置發件人、設置收件人、設置主題、設置正文
    - TransPort
        - 只有一個功能，發郵件。
        
        
## Example
```java
package pers.javamail;

import java.io.File;
import java.util.Properties;

import javax.mail.Authenticator;
import javax.mail.Message.RecipientType;
import javax.mail.PasswordAuthentication;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeBodyPart;
import javax.mail.internet.MimeMessage;
import javax.mail.internet.MimeMultipart;

import org.junit.Test;

import com.sun.xml.internal.messaging.saaj.packaging.mime.internet.MimeUtility;


public class Demo1 {
	@Test
	public void fun1() throws Exception {
		/*
		 * 1. 得到 Session
		 */
		Properties props = new Properties();
		props.setProperty("mail.host", "smtp.outlook.com");
		props.setProperty("mail.smtp.auth", "true");
		
		Authenticator auth = new Authenticator() {
			@Override
			protected PasswordAuthentication getPasswordAuthentication() {
				return new PasswordAuthentication("jack870131@outlook.com", "tyler0131");
			}
		};
		
		Session session = Session.getInstance(props, auth);
		
		/*
		 * 2. 創建 MimeMessage
		 */
		MimeMessage msg = new MimeMessage(session);
		msg.setFrom(new InternetAddress("jack870131@outlook.com")); //設置發件人
		msg.setRecipients(RecipientType.TO, "jack1998870131@gmail.com"); //設置收件人
		msg.setRecipients(RecipientType.CC, "jack1998870131@126.com"); //設置抄送
		msg.setRecipients(RecipientType.BCC, "jack1998870131@163.com"); //設置暗送
		
		msg.setSubject("Hello World");
		msg.setContent("這是一封測試郵件", "text/html;charset=utf-8");
		
		/*
		 * 3. 發送
		 */
		Transport.send(msg);
	}
	
	@Test
	public void fun2() throws Exception {
		/*
		 * 1. 得到 Session
		 */
		Properties props = new Properties();
		props.setProperty("mail.host", "smtp.outlook.com");
		props.setProperty("mail.smtp.auth", "true");
		
		Authenticator auth = new Authenticator() {
			@Override
			protected PasswordAuthentication getPasswordAuthentication() {
				return new PasswordAuthentication("jack870131@outlook.com", "xxxxx");
			}
		};
		
		Session session = Session.getInstance(props, auth);
		
		/*
		 * 2. 創建 MimeMessage
		 */
		MimeMessage msg = new MimeMessage(session);
		msg.setFrom(new InternetAddress("jack870131@outlook.com")); //設置發件人
		msg.setRecipients(RecipientType.TO, "jack1998870131@gmail.com"); //設置收件人
		
		msg.setSubject("Hello World");
		msg.setContent("這是一封測試郵件", "text/html;charset=utf-8");
		
		/*
		 * 當發送包含附件的郵件時，郵件體就為多部件形式
		 * 1. 創建一個多部件的部件內容
		 * 	MimeMultipart 就是一個集合，用來裝載多個主體部件
		 * 2. 需要創建兩個主體部件，一個是文本內容，另一個是附件的
		 * 	主體部件較 MimeBodyPart
		 * 3. 把 MimeMultipart 設置給 MimeMessage 的內容	
		 */
		MimeMultipart list = new MimeMultipart(); //創建多部分內容
		//創建 MimeBodyPart
		MimeBodyPart part1 = new MimeBodyPart();
		//設置主體部件的內容
		part1.setContent("這是一封測試郵件", "text/html;charset=utf-8");
		//把主體部件添加到集合中
		list.addBodyPart(part1);
		
		//創建 MimeBodyPart
		MimeBodyPart part2 = new MimeBodyPart();
		part2.attachFile(new File("C:/Users/asus/Pictures/Camera Roll/Other/1.jpg"));
		part2.setFileName(MimeUtility.encodeText("你好.jpg")); //設置顯示的文件名稱，其中 encodeText 用來處理中文亂碼問題
		list.addBodyPart(part2);
		
		msg.setContent(list); //把他設置給郵件作為郵件的內容
		
		
		/*
		 * 3. 發送
		 */
		Transport.send(msg);
	}
}
```
