# TCP 協議事例 - 3

## 上傳圖片客戶端
```java
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;
import java.net.UnknownHostException;

public class UploadPicClient {

	public static void main(String[] args) throws IOException, UnknownHostException {

		//1. 創建客戶端
		Socket s = new Socket("192.168.0.103", 10008);
		
		//2. 讀取客戶端要上傳的圖片文件
		FileInputStream fis = new FileInputStream("C:\\Users\\asus\\Documents\\Java\\JAVA SE Practice\\0.bmp");
		
		//3. 獲取 Socket 輸出流，將讀到的圖片數據發送給服務端
		OutputStream out = s.getOutputStream();
		
		byte[] buf = new byte[1024];
		
		int len = 0;
		
		while((len = fis.read(buf)) != -1) {
			
			out.write(buf);
		}
		
		//告訴服務端這邊的數據已讀取完畢，讓服務端停止讀取
		s.shutdownInput();
		
		//讀取服務端發回的內容
		InputStream in = s.getInputStream();
		
		byte[] bufIn = new byte[1024];
		
		int lenIn = in.read(bufIn);
		String text = new String(bufIn, 0, lenIn);
		System.out.println(text);
		
		fis.close();
		s.close();
	}

}

import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;

public class UploadPicServer {

	public static void main(String[] args) throws IOException {

		//1. 創建 TCP 的 Socket 服務端
		ServerSocket ss = new ServerSocket(10008);
		
		while(true) {
			//2. 獲取客戶端
			Socket s = ss.accept();
			
			new Thread(new UploadTask(s)).start();
			
		}
//		ss.close();
	}

}

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;

public class UploadTask implements Runnable {

	private Socket s;
	
	public UploadTask(Socket s) {
		
		 this.s = s;
	}
	
	@Override
	public void run() {
		
		int count = 0;
		String ip = s.getInetAddress().getHostAddress();
		System.out.println(ip + " connected");

		try {
			//3. 讀取客戶端發來的數據
			InputStream in = s.getInputStream();
					
			//4. 將讀取到的數據存儲到一個文件中
			File dir = new File("C:\\Users\\asus\\Documents\\Java\\JAVA SE Practice\\Pic");
			if(!(dir.exists())) {
				dir.mkdir();
			}
					
			File file = new File(dir, ip + ".bmp");
			//如果文件已存在於服務器端
			while(file.exists()) {
				
				file = new File(dir, ip + " (" + (++count) + ")" + ".bmp");
			}
			
			FileOutputStream fos = new FileOutputStream(file);
					
			byte[] buf = new byte[1024];
					
			int len = 0;
					
			while((len = in.read(buf)) != -1) {
				fos.write(buf, 0, len);
			}
					
			//5. 獲取 Socket 輸出流，將上傳成功字樣發給客戶端
			OutputStream out = s.getOutputStream();
			out.write("Upload Complete".getBytes());
					
			fos.close();
			s.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

}
```
