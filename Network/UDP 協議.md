# UDP 協議

## Socket
- Socket 就是為了網絡服務提供的一種機制。
- 通信的兩端都有 Socket。
- 網絡通信其實就是 Socket 間的通信。
- 數據在兩個 Socket 間通過 IO 傳輸。
- 如同現實生活中兩個地方的**港口**。

## UDP 傳輸
- DatagramSocket & DatagramPacket
- 建立發送端，接收端。
- 建立數據包。
- 調用 Socket 的發送接收方法。
- 關閉 Socket。
- 發送端 & 接收端是兩個獨立的運行程序。

## UDP 傳輸的發送端
- 思路:
1. 建立 UDP 的 Socket 服務。
2. 將要發送的數據封裝到數據包中。
3. 通過 UDP 的 Socket 服務將數據包發送出去。
4. 關閉 Socket 服務。

## UDP 傳輸的接收端
- 思路
1. 建立 UDP 的 Socket 服務。因為要接收數據，因此要明確**端口**。
2. 創建數據包，用於存儲接收到的數據。方便用數據包對象的方法解析這些數據。
3. 使用 Socket 服務的 receive 方法將接收的數據存儲到數據包中。
4. 通過數據包的方法解析數據包中的數據。
5. 關閉資源。

## Example
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

public class UDPSendDemo {

	public static void main(String[] args) throws IOException {

		System.out.println("Sending...");
		
		//1. UDP Socket 服務。使用 DatagramSocket 對象。
		DatagramSocket ds = new DatagramSocket();
		
		//2. 將要發送的數據封裝到數據包中
//		Scanner sc = new Scanner(System.in);
//		
//		String str = sc.nextLine();
		BufferedReader bufr = new BufferedReader(new InputStreamReader(System.in));
		
		String line = null;
		
		while((line = bufr.readLine()) != null) {
			
			if("Bye Bye".equals(line)) {
				break;
			}
			
			byte[] buf = line.getBytes();
			
			//使用 DatagramPacket 將數據封裝到該對象包中。
			DatagramPacket dp = new DatagramPacket(buf, buf.length, InetAddress.getByName("192.168.0.103"), 10000);
			
			//3. 通過 UDP 的 Socket 服務將數據包發送出去。使用 send 方法。
			ds.send(dp);
		}
		//4. 關閉資源。
		ds.close();
	}

}

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class UDPReceiveDemo {

	public static void main(String[] args) throws IOException {
		
		System.out.println("Receiving...");

		//1. 建立 UDP Socket 服務。
		DatagramSocket ds = new DatagramSocket(10000);
		
		while(true) {
		
		//2. 創建數據包。
		byte[] buf = new byte[1024];
		
		DatagramPacket dp = new DatagramPacket(buf, buf.length);
		
		//3. 使用接收方法將數據存儲到數據包中。
		ds.receive(dp);//阻塞式方法
		
		//4. 通過數據包對象的方法解析其中的數據。比如: 地址、端口、數據內容
		String ip = dp.getAddress().getHostAddress();
		
		int port = dp.getPort();
		
		String text = new String(dp.getData(), 0, dp.getLength());
		
		System.out.println(ip + " @ " + port + " @ " + text);
		
		}
		
		//5. 關閉資源。
//		ds.close();
	}

}
```
