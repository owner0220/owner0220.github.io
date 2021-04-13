---
title: 직렬화(Serialization) with Java Example
date: 2021-04-13 13:23:00
categories:
 - JAVA
tag:
 - Serialization
---

### Serialization

### 직렬화 (Serialization) 

직렬화는 객체(Object)의 상태를 바이트 스트림으로 변환하는 매커니즘입니다. 역직렬화는 바이트 스트림을 사용해 메모리에서 실제 Java 개체를 다시 만드는 역방향 프로세스 입니다.

![serialization](/images/serialization.PNG)

<!-- more -->

> https://www.geeksforgeeks.org/serialization-in-java/

생성된 바이트 스트림은 플랫폼에 독립적입니다. 따라서 한 플랫폼에서 직렬화 된 객체를 다른 플랫폼에서 역직렬화 할 수 있습니다.

JAVA에서 직렬화 하기 위해서는 java.io.Serializable 인터페이스를 구현하면 됩니다.

직렬화 하기 위한 메서드 public final void **writeObject(Object obj)**

역직렬화 하기 위한 메서드 public final **Object readObject()**

직렬화를 하는 이유

1. 객체의 상태를 저장하고 유지
2. 네트워크를 통해 전송하기 위함

![Advantages of Serialization](/images/Advantages of Serialization.PNG)

※ 기억해야 할 사항

- 부모 클래스가 Serializable 인터페이스를 구현했다면, 자식 클래스는 이를 구현할 필요가 없이잠 그 반대의 경우도 마찬가지입니다.
- Non-static data 멤버만 직렬화 될 수 있다.
- static data와 transient data 멤버는 직렬화 프로세스에 포함되지 않습니다.
- 역직렬화 과정에는 객체의 생성자가 호출되지 않습니다.
- 관련된 객체는 직렬화 구현이 되어 있어야 합니다.

```java
class A implements Serializable {
    // B also implements Serializable
    // interface
    B ob = new B();
}
```



#### SerialVersionUID

직렬화 런타임은 SerialVersionUID라는 각 Serializable 클래스와 버전 번호를 연결합니다. **SerialVersionUID**는 직렬화 된 개체의 보낸 사람과 받는 사람이 `직렬화와 관련하여 호환되는 해당 개체에 대한 클래스를 로드했는지 확인`하기 위해 Deserialization 중에 사용됩니다. SerialVersionUID가 다른 객체 클래스를 로드한 경우 InvalidClassException을 발생시킵니다.

SerialVersionUID는 private modifier 사용을 권장합니다.

##### serialver

serialver는 JDK와 함께 제공되는 도구입니다. Java 클래스의 serialVersionUID 번호를 가져오는 데 사용합니다.

사용법 )  serialver [-classpath classpath] [-show] [classname…]

```bash
# MySerial은 클래스이름
$ serialver -classpath /home/test/ MySerial
```

#### 예제1) 직렬화, 역직렬화 자바 코드 예제

```java
import java.io.*;

class Demo implements java.io.Serializable
{
    public int a;
    public String b;
    
    public Demo(int a, String b)
    {
   		this.a = a;
        this.b = b;
    }
}
class Test
{
    public static void main(String[] args)
    {
        Demo object = new Demo(1, "geeksforgeeks");
        String filename = "file.ser";
        // Serialization
        
        try
        {
            // 파일 생성할 연결
         	FileOutputStream file = new FileOutputStream(filename);
			// 파일 내용을 채워 넣는 객체 내용
            ObjectOutputStream out = new ObjectOutputStream(file);
            
            out.writeObject(object);
            
            out.close();
            file.close();
            
            System.out.println("Object has been serialized");
        }
        
        catch(IOException ex)
        {
            System.out.println("IOException is caught");
        }
        
        Demo object1 = null;
        
        //Deserialization
        try
        {
            //파일로 부터 객체 읽어 들이기
            FileInputStream file = new FileInputStream(filename);
            ObjectInputStream in = new ObjectInputStream(file);
            
            //역직렬화 메서드
            object1 = (Demo)in.readObject();
            
            in.close();
            file.close();
            
            System.out.println("Object has been deserialized");
            System.out.println("a= " + object1.a);
            System.out.println("b= " + object1.b);
            
        }
        catch(IOExceptino ex)
        {
            System.out.println("IOException is caught");
        }
        
        catch(ClassNotFoundException ex)
        {
            System.out.println("ClassNotFoundException is caught");
        }
    }
}
```

##### Output

```bash
Object has been serialized
Object has been deserialized 
a = 1
b = geeksforgeeks
```



#### 예제2) Static 값 반영 안됨 확인

```java
import java.io.*;

class Emp implements Serializable {
    private static final long serialversionUID = 129348938L;
    transient int a;
    static int b;
    String name;
    int age;
    
    public Emp(String name, int age, int a, int b)
    {
        this.name = name;
        this.age = age;
        this.a = a;
        this.b = b;
    }
}

public class SerialExample {
    public static void printdata(Emp object1)
    {
        System.out.println("name = " + object1.name);
        System.out.println("age = " + object1.age);
        System.out.println("a = " + object1.a);
        System.out.println("b = " + object1.b);
    }
    public static void main(String[] args)
    {
        Emp object = new Emp("ab", 20, 2, 1000);
        String filename = "shubham.txt";
        
        //직렬화
        try{
            FileOutputStream file = new FileOutputStream(filename);
            ObjectOutputStream out = new ObjectOutputStream(file);
            
            out.writeObject(object);
            
            out.close();
            file.close();
            
            System.out.println("Object has been serialized\n" + "Data before Deserialization");
            printdata(object);
            //static 값 변경
            object.b = 2000;
        }
        catch (IOException ex){
            System.out.println("IOException is caught");
        }
        
        object = null;
        
        try {
            FileInputStream file = new FileInputStream(filename);
            ObjectInputStream in = new ObjectInputStream(file);
            
            object = (Emp)in.readObject();
            
            in.close();
            file.close();
            System.out.println("Object has been deserialized\n" + "Data after Deserialization");
            
            printdata(object);
        }
        
        catch (IOException ex){
            System.out.println("IOException is caught");
        }
        
        catch (ClassNotFoundException ex){
            System.out.println("ClassNotFoundException" + " is caught")
        }
    }
}
```

##### Output

```bash
Object has been serialized
Data before Deserialization.
name = ab
age = 20
a = 2
b = 1000
Object has been deserialized
Data after Deserialization.
name = ab
age = 20
a = 0
b = 2000
```



**※ 명심할 것**

- transient 값 : 직렬화 시 포함되지 않기 때문에 역직렬화 시 objects 는 null 값을, int는 0을 갖는다.
- static 값 : 직렬화 시 포함되지 않았지만 역직렬화 시 클래스에 정의된 현재 값으로 로드 된다.

직렬화 할때 직렬화 되지 않는 오브젝트들이 있는데 그런 것들을 transient로 정의해서 구현한다.

ex) transient Scanner sc = new Scanner(System.in)