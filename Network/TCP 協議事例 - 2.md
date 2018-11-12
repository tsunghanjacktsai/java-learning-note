# TCP 協議事例 - 2

## 上傳文本文件 - 演示
```
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class UploadServer {

	public static void main(String[] args) throws IOException {
		
		System.out.println("Server Uploaded");

		ServerSocket ss = new ServerSocket(10006);
		
		Socket s = ss.accept();
		System.out.println(ss.getInetAddress().getHostAddress() + " connected");
		
		BufferedReader bufrIn = new BufferedReader(new InputStreamReader(s.getInputStream()));
		
		BufferedWriter bufw = new BufferedWriter(new FileWriter("C:\\Users\\asus\\Documents\\Java\\JAVA SE Practice\\Server.txt"));
		
		String line = null;
		while((line = bufrIn.readLine()) != null) {
			
//			if("over".equals(line))
//				break;
			
			bufw.write(line);
			bufw.newLine();
			bufw.flush();
		}
		
		PrintWriter out = new PrintWriter(s.getOutputStream(), true);
		out.println("Upload Complete");
		
		bufw.close();
		
		s.close();
		ss.close();
	}

}

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

public class UploadClient {

	public static void main(String[] args) throws IOException {
		
		System.out.println("Client Uploaded");

		Socket s = new Socket("192.168.0.103", 10006);
		
		BufferedReader bufr = new BufferedReader(new FileReader("C:\\Users\\asus\\Documents\\Java\\JAVA SE Practice\\Client.txt"));
		
		PrintWriter out = new PrintWriter(s.getOutputStream(), true);
		
		String line = null;
		while((line = bufr.readLine()) != null) {
			
			out.println(line);
		}

		//告訴服務端，客戶端寫完了
//		out.println("over");
		s.shutdownOutput();
		
		BufferedReader bufrIn = new BufferedReader(new InputStreamReader(s.getInputStream()));
		
		String str = bufrIn.readLine();
		System.out.println(str);
		
		bufr.close();
		s.close();
	}

}
```
