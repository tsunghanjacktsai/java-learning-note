# TCP 協議事例 - 1

## 需求
客戶端輸入字母數據，發送給服務端，
服務端收到後顯示在控制台，並將該數據轉成大寫返回給客戶端，
直到客戶端輸入 over，轉換結束。
創建一個英文大寫轉換器。

## 分析
有客戶端和服務端，使用 TCP 傳輸。

## 思路
**文本轉換客戶端**
1. 需要先有 Socket 端點。
2. 客戶端的數據源: 鍵盤。
3. 客戶端的目的: Socket.
4. 接收服務端的數據。源: Socket.
5. 將數據打印出來。目的: 控制台。
6. 在這些流中操作的數據，都是**文本數據**。
步驟
1. 創建 Socket 客戶端對象。
2. 獲取鍵盤錄入。
3. 將錄入的信息發送給 Socket 輸出流。

**文本轉換服務端**
1. ServerSocket 服務
2. 獲取 Socket 對象
3. 服務端的數據源: Socket，讀取客戶端發過來需要轉換的數據。
4. 服務端的目的: 顯示在控制台上。
5. 將數據轉成大寫發給客戶端。
步驟
1. 創建 ServerSocket 服務端對象。
2. 獲取 Socket 對象。
3. 獲取 Socket 讀取流，並裝飾。
4. 獲取 Socket 輸出流，並裝飾。

## 演示
```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

public class TransClient {
	
	public static void main(String[] args) throws IOException {

		//創建 Socket 客戶端對象
		Socket s = new Socket("192.168.0.103", 10005);
		
		//獲取鍵盤錄入
		BufferedReader bufr = new BufferedReader(new InputStreamReader(System.in));
		
		//Socket 輸出流
//		new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
		PrintWriter out = new PrintWriter(s.getOutputStream(), true);
		
		
		//Socket 輸入流讀取服務端返回的大寫數據
		BufferedReader bufrIn = new BufferedReader(new InputStreamReader(s.getInputStream()));
		
		String line = null;
		
		while((line = bufr.readLine()) != null) {
			
			if("over".equals(line))
				break;
			
//			out.print(line + "\r\n");
//			out.flush();
			out.println(line);//自動檢測換行符，實現自動刷新
			
			//讀取服務端發回的一行大寫數據
			String upperStr = bufrIn.readLine();
			
			System.out.println(upperStr);
		}
		
		s.close();
	}

}

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class TransServer {

	public static void main(String[] args) throws IOException {
		
		//創建 ServerSocket 服務端對象。
		ServerSocket ss = new ServerSocket(10005);
				
		//獲取 Socket 對象
		Socket s = ss.accept();
		
		//獲取 IP
		String ip = s.getInetAddress().getHostAddress();
		System.out.println(ip + " connected");
		
		//獲取 Socket 讀取流，並裝飾
		BufferedReader bufIn = new BufferedReader(new InputStreamReader(s.getInputStream()));
		
		//獲取 Socket 輸出流，並裝飾。
		PrintWriter out = new PrintWriter(s.getOutputStream(), true);
		
		String line = null;
		while((line = bufIn.readLine()) != null) {
			
			System.out.println(line);
//			out.print(line.toUpperCase() + "\r\n");
//			out.flush();
			out.println(line.toUpperCase());//自動檢測換行符，實現自動刷信
		} 
		
		s.close();
		ss.close();	
	}
}
```
