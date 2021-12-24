# Chap 15 입출력

# 자바의 입출력

---

## 입출력이란

- 입출력은 컴퓨터 내부 또는 외부의 장치와 프로그램간의 데이터를 주고받는 것

## 스트림

- 자바에서 입출력을 수행하려면, 두 대상을 연결하고 데이터를 전송할 수 있는 무언가가 필요한데 이것을 스트림이라고 정의했다.
- 입출력의 스트림과 스트림 api는 다른 개념
- 스트림이란 `데이터를 운반하는데 사용되는 연결통로` 이다.
    - 연속적인 데이터의 흐름을 물에 비유해서 붙여진 이름
    - 단방향 통신만 가능
- 따라서 입력과 출력을 동시에 수행하려면 입력을 위한 `입력스트림`과, 출력을 위한 `출력스트림` 2개의 스트림이 필요하다.
- 스트림은 먼저 보낸 데이터를 먼저 받게 되어 있으며 중간에 건너뜀 없이 연속적으로 데이터를 주고받는다
- 큐와 같은 FIFO 구조로 되어 있다고 생각하면 이해가 쉽다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dd63aa95-1e52-4db6-b569-a710af49af11/Untitled.png)

## 바이트 기반 스트림 - InputStream, OutputStream

- 스트림은 바이트 단위로 데이터를 전송하며 입출력 대상에 따라 다음과 같은 입출력스트림이 있다.
- [java.io](http://java.io) 패키지에 입출력 관련 클래스 제공

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bea94862-ad8d-4515-a76a-06fff3a6daab/Untitled.png)

- 이들은 모두 InputStream, OutputStream의 서브 클래스들

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/68888d5d-89fc-4dfd-abbd-690adcb41104/Untitled.png)

    - 추상 메서드 read와 write를 알맞게 구현해야 한다.


## 보조 스트림

- 스트림의 기능을 향상시키거나 새로운 기능을 추가하기 위해 사용
- 독립적으로 입출력을 처리할 수 없다.
- 모든 보조스트림 역시 InputStream과 OutputStream의 서브클래스들

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/63b09544-8472-4acc-80d2-0f86b7eac3b1/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa9e267d-910d-477d-9706-1678c9eea904/Untitled.png)

## 문자기반 스트림 - Reader, Writer

- 입출력 단위가 문자(char, 2byte)인 스트림
- 문자기반 스트림의 최고 슈퍼클래스이다.
- 바이트기반 스트림과 문자기반 스트림 비교

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5a841e6a-55c8-476c-9eb2-71ac20fa7038/Untitled.png)

- 이름만 InputStream을 Reader, OutputStream을 Writer로 바꾸면 된돠
    - byte배열 대신 char 배열 사용

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ffb78d50-ece3-4f46-a3b2-4a51f7b1db4d/Untitled.png)

- 바이트기반 보조스트림과 문자기반 보조스트림

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c4a4cb1b-a477-4a3f-aacc-0f889c7dc696/Untitled.png)


# 바이트기반 스트림

---

## InputStream과 OutputStream

- InputStream의 메서드들
    - 스트림의 종류에 따라 mark()와 reset()을 사용하여 이미 읽은 데이터를 되돌려서 다시 읽을 수 있다.
    - 이 기능이 가능한지는 markSupported()로 확인 가능

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/df9de9f6-c0af-44f9-9eb4-ad4872ba4750/Untitled.png)

- OutputStream의 메서드들
    - flush는 버퍼가 있는 출력스트림의 경우에만 의미가 있다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/febf230d-59b7-4a2c-93f6-7bb36b21465f/Untitled.png)


## ByteArrayInputStream과 ByteArrayOutputStream

- 바이트 배열에 데이터를 입출력하는 바이트기반 스트림
- 주로 다른 곳에 입출력하기 전에 데이터를 임시로 바이트배열에 담아서 변환 등의 작업을 하는데 사용
- ex1 - inSrc의 데이터를 outSrc로 복사하는 예제
    - read(), wrrite()를 사용하는 기본 방법

    ```java
    import java.io.*;
    import java.util.Arrays;
    
    class IOEx1 {
    	public static void main(String[] args) {
    		byte[] inSrc = {0,1,2,3,4,5,6,7,8,9};
    		byte[] outSrc = null;
    
    		ByteArrayInputStream  input  = null;
    		ByteArrayOutputStream output = null;
    
    		input  = new ByteArrayInputStream(inSrc);
    		output = new ByteArrayOutputStream();
    
    		int data = 0;
    
    		while((data = input.read())!=-1) {
    			output.write(data);	// void write(int b)
    		}
    
    		outSrc = output.toByteArray(); // 스트림의 내용을 byte배열로 반환한다.
    
    		System.out.println("Input Source  :" + Arrays.toString(inSrc));
    		System.out.println("Output Source :" + Arrays.toString(outSrc));
    	}
    }
    ```

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eccceff8-8765-4b0c-b74e-6cf999ac97bd/Untitled.png)

- ex2 - 1byte씩  읽어오던 부분 수정

    ```java
    import java.io.*;
    import java.util.Arrays;	
    
    class IOEx4 {
    	public static void main(String[] args) {
    		byte[] inSrc = {0,1,2,3,4,5,6,7,8,9};
    		byte[] outSrc = null;
    
    		byte[] temp = new byte[4];
    
    		ByteArrayInputStream  input  = null;
    		ByteArrayOutputStream output = null;
    
    		input  = new ByteArrayInputStream(inSrc);
    		output = new ByteArrayOutputStream();
    
    		try {
    			while(input.available() > 0) {
    				int len = input.read(temp); // 읽어 온 데이터의 개수를 반환한다.
    				output.write(temp, 0, len); // 읽어 온 만큼만 write한다.
    			}
    		} catch(IOException e) {}
    
    		outSrc = output.toByteArray();
    
    		System.out.println("Input Source  :" + Arrays.toString(inSrc));
    		System.out.println("temp          :" + Arrays.toString(temp));
    		System.out.println("Output Source :" + Arrays.toString(outSrc));
    ```

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/30951c19-76d9-4a8c-82b7-fc1814bd921d/Untitled.png)


## FileInputStream과 FileOutputStream

- file에 데이터를 입출력하는 바이트기반 스트림
- 많이 사용되는 스트림 중의 하나
- 아래 사진의 나와있는 생성자 뿐만 아니라 FileDescriptor를 인자로 받는 생성자도 존재

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/070f07b5-691d-41d7-9f6b-028945b2f3f4/Untitled.png)

    - append 값이 true면 출력시 기존 파일내용의 마지막에 덧붙인다.


# 바이트 기반의 보조스트림

---

## FilterInputStream과 FilterOutputStream

- 모든 바이트기반 보조스트림의 최고 슈퍼클래스
- 보조스트림은 자체적으로 입출력을 수행할 수 없기때문에 `기반스트림`을 필요로 한다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee6729ce-613b-43e0-ab56-b320c085061a/Untitled.png)

- FilterInputStream과 FilterOutputStream의 모든 메서드는 단순히 `기반스트림의 메서드`를 그대로 `호출`할 뿐이다.
- FilterInputStream과 FilterOutputStream은 상속을 통해 read()와 write()를 원하는 기능대로 오버라이딩해야 한다.
- 상속받아서 기반스트림에 보조기능을 추가한 보조스트림 클래스들

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/02be98fd-e782-4af8-bf67-bb8fb91c9b3a/Untitled.png)


## BufferedInputStream과 BufferedOutputStream

- 입출력 효율을 높이기 위해 버퍼(byte[])를 사용하는 보조스트림
- 버퍼크기는 입력소스로부터 한 번에 가져올 수 있는 데이터의 크기로 지정하면 좋다.
- 입력소스가 파일인 경우 8192정도의 크기로 하는 것이 보통
- read 메서드를 호출하면 BufferedInputStream은 입력소스로부터 버퍼 크기만큼의 데이터를 읽어다 자신의 내부 버퍼에 저장한다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/da31bb6e-ff22-434c-ad0d-8a5083a15beb/Untitled.png)

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/288b66e3-2430-4a7c-8d44-488cf8fe4100/Untitled.png)

- `보조스트림을 닫으면 기반스트림도 닫힌다.`
    - 기반스트림의 close()를 호출하기 때문

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7468611f-4334-4b04-8ad6-b96f7ff065d9/Untitled.png)


## DataInputStream과 DataOutputStream

- byte가 아닌 기본 자료형의 단위로 읽고 쓰는 보조스트림
- 각 자료형의 크기가 다르므로 출력할 때와 입력할 때 순서에 주의

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e50f72dc-5b37-4fbc-ae13-de9b340ee807/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f7653de-ea62-405f-92cb-0c6577a0888b/Untitled.png)

## SequenceInputStream

- 여러 입력스트림을 연결해서 하나의 스트림처럼 다룰 수 있게 해준다.
- 큰 파일을 여러개의 작은 파일로 나누었다가 하나의 파일로 합치는 것과 같은 작업을 수행할 떄 사용하면 좋다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1e43758c-e767-4673-a078-b5255969813a/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aba21836-1f42-4e55-9bd2-542671c5b409/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/60171a74-6890-4d9f-bac5-fb73fc2fc696/Untitled.png)

## PrintStream

- 데이터를 다양한 형식의 문자로 출력하는 기능을 제공하는 보조스트림
- System.out과 System.err이 PrintStream이다.
- PrintStream보다 PrintWriter 사용을 권장한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4847b12d-cdea-4961-8010-838ee00079c8/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c75b0b50-d11c-46fb-83aa-81ed2378345d/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c4a96b48-44a3-4bdb-b135-fea469804230/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6f3f2c00-fb68-4189-bc4b-3d58f3631941/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b3f24e96-2089-4f74-9863-3172b65aad90/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a2cb79ce-aae6-4913-9e7b-1e7453b40f61/Untitled.png)

# 문자기반 스트림

---

## Reader와 Writer

- Reader : 문자기반 입력스트림의 최고 슈퍼 클래스

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7a4ffb5b-6f6f-4cbc-983f-56cfb6360ebd/Untitled.png)

- Writer : 문자기반 출력스트림의 최고 슈퍼 클래스

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/56f0984d-c972-4d1e-9969-d42096ed2b1f/Untitled.png)

- byte 배열 대신 char배열을 사용한다는 것 외에는 InputStream/OutputStream과 크게 다르지 않다.
- 문자기반 스트림과 그 서브클래스들은 어려 종류의 인코딩과 자바에서 사용하는 유니코드(uft-16)간의 변환을 자동적으로 처리해준다.

## FileReader와 FileWriter

- 문자기반의 파일 입출력
- 텍스트 파일의 입출력에 사용한다.
- ex 1
    - FileInputStream을 사용하면 한글이 깨지는 것을 볼 수 있다.

    ```java
    import java.io.*;
    
    class FileReaderEx1 {
    	public static void main(String args[]) {
    		try {
    			String fileName = "test.txt";
    			FileInputStream fis = new FileInputStream(fileName);
    			FileReader	    fr  = new FileReader(fileName);
    
    			int data =0;
    
    			// FileInputStream을 이용해서 파일내용을 읽어 화면에 출력한다.
    			while((data=fis.read())!=-1) {
    				System.out.print((char)data);
    			}
    			System.out.println();
    			fis.close();
    
    			// FileReader를 이용해서 파일내용을 읽어 화면에 출력한다.
    			while((data=fr.read())!=-1) {
    				System.out.print((char)data);
    			}
    			System.out.println();
    			fr.close();				
    
    		} catch (IOException e) {
    				e.printStackTrace();		
    		}
    	} // main
    }
    ```

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/126f8ba2-b4ef-41eb-841a-57ab2fb61c56/Untitled.png)


## PipedReader와 PipedWriter

- 프로세스(쓰레드)간의 통신(데이터를 주고받음)에 사용한다.
- 입력과 출력스트림을 하나의 스트림으로 연결해서 데이터를 주고받는 특징이 있다.
- 스트림을 생성한 다음에 어느 한쪽 쓰레드에서 connect()를 호출해서 입력 스트림과 출력스트림을 연결한다.
- 입출력을 마친 후에는 어느 한쪽 스트림만 닫아도 나머지 스트림은 자동으로 닫힌다.
- ex1

```java
import java.io.*;

public class PipedReaderWriter {
	public static void main(String args[]) {
		InputThread   inThread = new InputThread("InputThread");
		OutputThread outThread = new OutputThread("OutputThread");

		inThread.connect(outThread.getOutput());	

		inThread.start();
		outThread.start();
	} // main
}

class InputThread extends Thread {
	PipedReader  input = new PipedReader();
	StringWriter sw    = new StringWriter();

	InputThread(String name) {
		super(name);		// Thread(String name);
	}

	public void run() {
		try {
			int data = 0;

			while((data=input.read()) != -1) {
				sw.write(data);
			}
			System.out.println(getName() + " received : " + sw.toString());
		} catch(IOException e) {}
	} // run

	public PipedReader getInput() {
		return input;
	}

	public void connect(PipedWriter output) {
		try {
			input.connect(output);
		} catch(IOException e) {}
	} // connect
}

class OutputThread extends Thread {
	PipedWriter output = new PipedWriter();

	OutputThread(String name) {
		super(name);		// Thread(String name);
	}

	public void run() {
		try {
			String msg = "Hello";
			System.out.println(getName() + " sent : " + msg);
			output.write(msg);
			output.close();
		} catch(IOException e) {}
	} // run

	public PipedWriter getOutput() {
		return output;
	}

	public void connect(PipedReader input) {
		try {
			output.connect(input);
		} catch(IOException e) {}
	} // connect
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d0f2411a-d822-4a74-86d0-bee8ba6477fd/Untitled.png)

## StringReader와 StringWriter

- CharArrayReader, CharArrayWriter처럼 메모리의 입출력에 사용한다.
- StringWriter에 출력되는 데이터는 내부의 StringBuffer에 저장된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/da0aa638-8c15-46a5-bd2c-595ccd37ab7d/Untitled.png)

# 문자기반 보조스트림

---

## BufferedReader와 BufferedWriter

- 입출력 효율을 높이기 위해 버퍼(char[])를 사용하는 보조스트림
- 라인 단위의 입출력이 편리하다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5d71445c-37ed-4162-8fab-02c4c33ae4df/Untitled.png)

## InputStreamReader와 OutputStreamWriter

- 바이트기반 스트림을 문자기반 스트림처럼 쓸 수 있게 해준다.
- 바이트기반 스트림의 데이터를 지정된 인코딩의 데이터로 변환하여 입출력할 수 있게 해준다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0770bacf-3b96-4fc0-bd67-0dabf5529f17/Untitled.png)

- 인코딩 변환하기

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bcb94cb2-66e5-4f9c-8918-78676e9819a5/Untitled.png)


# 표준입출력과 file

---

## 표준입출력 - System.in, System.out, System.err

- 콘솔을 통한 데이터의 입출력을 '표준 입출력'이라 한다.
- jvm이 시작되면서 자동적으로 생성되는 스트림이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/206969a7-fc5e-40ab-8b82-07c2533120c1/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d9a7e284-9d71-423f-884b-c7b87df42c80/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f33a001d-05a2-4e68-b2aa-f6241928fb4d/Untitled.png)

- in, out, err은 스태틱변수이다.
- 타입과는 다르게 실제로는 버퍼를 이용하는 BufferedInput, output 인스턴스를 사용한다.

## RandomAccessFile

- 하나의 스트림으로 파일에 입력과 출력을 모두 수행할 수 있는 스트림
- 다른 스트림들과 달리 Object의 자손이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/89aa2309-999f-4665-b172-0377694f305b/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/808f1e2b-3e8b-402a-b6d4-67bcfba1fca6/Untitled.png)

## File

- 파일과 디렉토리를 다루는데 사용되는 클래스

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/10f3ae88-4df0-4924-8919-bc68c6d94d71/Untitled.png)

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7b2b4b0e-0d95-47a8-9a35-c40180d0fa48/Untitled.png)

- 파일을 생성하기 위해서는 File 인스턴스를 생성한 다음, 출력 스트림을 생성하거나 createNewFile()을 호출해야 한다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/24291312-bff2-48b6-a729-953f0278215e/Untitled.png)


# 직렬화(Serialization)

---

## 직렬화란?

- 객체를 데이터 스트림으로 만드는 것을 뜻한다.
    - 즉, 객체에 저장된 데이터를 스트림에 쓰기위해  '연속적인(serial) 데이터'로 변환하는 것
    - 반대과정은 역직렬화라고 한다.
- 객체의 인스턴스변수들의 값을 일렬로 나열하는 것

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9d69fa3a-b6c2-45bd-b195-971e9c6ef72c/Untitled.png)

## ObjectInputStream, ObjectOutputStream

- 직렬화에는 ObjectInputStream, 역직렬화는 ObjectOutputStream
- 이 두 클래스들도 보조스트림
- 객체를 직렬화하여 입출력할 수 있게 해주는 보조스트림

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ead65307-2c5d-4231-99aa-c0d3ee39c320/Untitled.png)

- 객체를 파일에 저장하는 방법

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c2b1ca07-aff8-4887-b9c0-47ddf6128a40/Untitled.png)

- 파일에 저장된 객체를 다시 읽어오는 방법

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8fa9fc47-b883-4489-9130-c1b54144ff2b/Untitled.png)

- 다양한 메소드 제공

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3a204eea-4de8-4bf9-906d-c6c5b5e760ed/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/92ed019b-e272-4d91-8476-9dfe1fe3dd7f/Untitled.png)

## 직렬화가 가능한 클래스 만들기

- java.io.Serializable을 구현해야만 직렬화가 가능하다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/01d158c7-5671-4a32-a3f1-ef2b0f89bdda/Untitled.png)

- 제어자가 transient가 붙은 인스턴스변수는 직렬화 대상에서 제외된다.
    - 해당 타입의 기본값으로 직렬화된다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb275524-0efc-43ac-b96f-46ceda82236b/Untitled.png)

- Serializable을 구현하지 않은 클래스의 인스턴스 직렬화 대상에서 제외된다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2c2434b1-9475-4885-92df-76e2720f40da/Untitled.png)

- Serializable을 구현하지 않은 조상의 멤버들은 직렬화 대상에서 제외된다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8966739a-9bf3-4fec-8f63-2b893145ffdd/Untitled.png)

- readObject()와 writeObject()를 오버라이딩하면 커스텀 직렬화가 가능하다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c154cd8a-2a3b-4f8f-b15f-5f30af63a0d1/Untitled.png)

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d4cf50b8-5a52-4324-afe5-b4faeb77bace/Untitled.png)


## 직렬화가능한 클래스의 버전관리

- 직렬화했을 때와 역직렬화했을 때의 클래스가 같은지 확인할 필요가 있다.
    - 클래스의 이름이 같더라도 클래스의 내용이 변경된 경우 역직렬화는 실패하며 예외 발생

      ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b304e8d1-f8d8-456a-884e-e30d2b7069bd/Untitled.png)

- 직렬화할 때, 클래스의 버전(serialVersionUID)을 자동계산해서 저장한다.
    - 따라서 역직렬화할 때 클래스의 버전을 비교함으로써 직렬화할 때의 클래스 버전과 일치하는지 확인
    - 그러나 static 변수나 transient가 붙은 인스턴스 변수가 추가되는 경우에는 직렬화에 영향을 미치지 않아 버전을 다르게 인식할 필요가 없다.
- 네트워크로 객체를 직렬화하여 전송하는 경우, 클래스가 조금만 변경되어도 재배포 해야하므로 관리가 어렵다.
    - 이런경우 `클래스의 버전을 수동으로 관리`해야한다.
    - 클래스의 버전을 수동으로 관리하려면, 클래스 내에 정의해야 한다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e36f4be4-2674-4d44-ad10-e58604ef8bc4/Untitled.png)

- serialver.exe는 클래스의 serialVersionUID를 자동생성해준다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/955a6aa4-5450-471f-a6b5-f3d6cbdc536b/Untitled.png)
