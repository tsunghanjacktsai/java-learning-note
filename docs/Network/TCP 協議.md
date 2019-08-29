# TCP 協議

## 使用
- Socket & ServerSocket
- 建立客戶端和服務器端
- 建立連接後，通過 Socket 中的 IO 流進行數據的傳輸。
- 關閉 Socket 
- 客戶端與服務器端是兩個獨立的應用程序。

## 建立 TCP 服務端
1. 創建 TCP 客戶端 Socket 服務，使用的是 **Socket 對象**。
   建議該對象一創建就明確**目的地**。(要連接的主機)
2. 如果連接建立成功，說明**數據傳輸通道**已建立，該通道就是 **Socket 流**，是底層建立好的。既然是流，說明**有輸入也有輸出**。
   想要**輸入或者輸出流**對象，可以找 Socket 來獲取。
   可以通過 **getOutputStream()**，和 **getInputStream()** 來獲取兩個字節流。
3. 使用**輸出流**，將數據寫出。
4. 關閉資源。

##建立 TCP 客戶端
1. 創建服務端 Socket 服務，通過 ServerSocket 對象。
2. 服務端必須對外提供**端口**，否則客戶端無法連接。
3. 獲取連接過來的**客戶端對象**。
4. 通過客戶端對象獲取 Socket 流讀取客戶端發來的數據，並打印在控制台上。
5. 關閉資源。關客戶端和服務端。

## Example
```
import java.io.IOException;
import java.io.OutputStream;
import java.net.Socket;

public class TCPClientDemo {

	public static void main(String[] args) throws IOException {
		//創建客戶端服務
		Socket socket = new Socket("192.168.0.103", 10002);
		
		//獲取 Socket 流的輸出流
		OutputStream out = socket.getOutputStream();
		
		//使用輸出流將指定的數據寫出去
		out.write("TCP Coming".getBytes());
		
		//關閉資源，將連接斷開
		socket.close();
	}

}

import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPServerDemo {

	public static void main(String[] args) throws IOException {

		//1. 創建服務端對象
		ServerSocket ss = new ServerSocket(10002);
		
		//2. 獲取連接過來的客戶端對象
		Socket s = ss.accept();//阻塞式
		
		String ip = s.getInetAddress().getHostAddress();
		
		//3. 通過 Socket 對象獲取輸入流，要讀取客戶端發來的數據
		InputStream in = s.getInputStream();
		
		byte[] buf = new byte[1024];
		
		int len = in.read(buf);
		String text = new String(buf, 0, len);
		
		System.out.println(ip + " @ " + text);
		
		s.close();
		ss.close();//一般不關
	}

}
```
打印結果
```
192.168.0.103 @ TCP Coming
```
---
```
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;

public class TCPClientDemo2 {

	public static void main(String[] args) throws IOException {
		//創建客戶端服務
		Socket socket = new Socket("192.168.0.103", 10002);
		
		//獲取 Socket 流的輸出流
		OutputStream out = socket.getOutputStream();
		
		//使用輸出流將指定的數據寫出去
		out.write("TCP Coming".getBytes());
		
		//讀取服務器返回的數據，使用 Socket 讀取流
		InputStream in = socket.getInputStream();
		byte[] buf = new byte[1024];
		
		int len = in.read(buf);
		
		String text = new String(buf, 0, len);
		
		System.out.println(text);
		
		//關閉資源，將連接斷開
		socket.close();
	}

}
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPServerDemo2 {

	public static void main(String[] args) throws IOException {

		//1. 創建服務端對象
		ServerSocket ss = new ServerSocket(10002);
		
		//2. 獲取連接過來的客戶端對象
		Socket s = ss.accept();//阻塞式
		
		String ip = s.getInetAddress().getHostAddress();
		
		//3. 通過 Socket 對象獲取輸入流，要讀取客戶端發來的數據
		InputStream in = s.getInputStream();
		
		byte[] buf = new byte[1024];
		
		int len = in.read(buf);
		String text = new String(buf, 0, len);
		
		System.out.println(ip + " @ " + text);
		
		//使用客戶端的 Socket 對象的輸出流給客戶端返回數據
		OutputStream out = s.getOutputStream();
		out.write("Got it!".getBytes());
		
		s.close();
		ss.close();//一般不關
	}

}

```
打印結果
```
Got it!
```
