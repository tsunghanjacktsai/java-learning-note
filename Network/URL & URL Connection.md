# URL & URL Connection

```java
import java.io.IOException;
import java.io.InputStream;
import java.net.MalformedURLException;
import java.net.URL;
import java.net.URLConnection;

public class URLDemo {

	public static void main(String[] args) throws IOException {

		String str_url = "http://192.168.0.103:8080/MyWeb/MyFirstWeb.html?name = Jack";// ? 後為參數信息
		
		URL url = new URL(str_url);
		
		/*
		System.out.println("Protocol: " + url.getProtocol());//協議
		System.out.println("Host: " + url.getHost());//主機
		System.out.println("Port: " + url.getPort());//端口
		System.out.println("File: " + url.getFile());//文件(帶參數信息)
		System.out.println("Path: " + url.getPath());//路徑(不帶參數信息)
		System.out.println("Query: " + url.getQuery());//參數信息
		*/
		
		InputStream in = url.openStream();//簡寫
		
		//獲取 url 對象的 url 連接器對象，將連接封裝成對象 -> Java 中內置可以解析具體協議的對象 + Socket
//		URLConnection con = url.openConnection();//sun.net.www.protocol.http.HttpURLConnection:http://192.168.0.103:8080/MyWeb/MyFirstWeb.html?name = Jack
		
//		InputStream in = con.getInputStream();
		
		byte[] buf = new byte[1024];
		
		int len = in.read(buf);
		
		String text = new String(buf, 0, len);
		
		System.out.println(text);
		
		//url 不用關
		in.close();
	}
}
```
