# JAVA I/O

## 1.InputStream、OutputStream

处理字节流的抽象类

InputStream 是字节输入流的所有类的超类,一般我们使用它的子类,如FileInputStream等.

OutputStream是字节输出流的所有类的超类,一般我们使用它的子类,如FileOutputStream等.

## 2.InputStreamReader OutputStreamWriter

处理字符流的抽象类

InputStreamReader 是字节流通向字符流的桥梁,它将字节流转换为字符流.

OutputStreamWriter是字符流通向字节流的桥梁，它将字符流转换为字节流.

## 3.BufferedReader BufferedWriter

BufferedReader 由Reader类扩展而来，提供通用的缓冲方式文本读取，readLine读取一个文本行，

从字符输入流中读取文本，缓冲各个字符，从而提供字符、数组和行的高效读取。

BufferedWriter 由Writer 类扩展而来，提供通用的缓冲方式文本写入， newLine使用平台自己的行分隔符，

将文本写入字符输出流，缓冲各个字符，从而提供单个字符、数组和字符串的高效写入。

## InputStream

能从來源处读取一個一個byte,

所以它是最低级的，

例：

```java
import java.io.*;     
public class Main {     
    public static void main(String[] args) {     
             
        try {     
            FileInputStream fis=new FileInputStream("d://desktop//test.txt");     
            int i;     
                 
            try {     
                while((i=fis.read()) != -1){     
                    System.out.println(i);     
                }     
                /*假設test.txt檔裡頭只有一個文字 "陳" ,且其編碼為 utf8   
                 * 輸出   
                 *  233   
                    153   
                    179   
                 */    
            } catch (IOException e) {     
                // TODO Auto-generated catch block     
                e.printStackTrace();     
            }     
                 
                 
        } catch (FileNotFoundException e) {     
            // TODO Auto-generated catch block     
            e.printStackTrace();     
        }     
                     
    }     
}   
```

## InputStreamReader 

InputStreamReader封裝了InputStream在里头, 

它以较高级的方式,一次读取一个一个字符, 

下例是假设有一个编码为utf8的文档, 其內容只有一个中文字 "陳"

```java
import java.io.*;     
public class Main {     
    public static void main(String[] args) throws FileNotFoundException, UnsupportedEncodingException {     
                     
            FileInputStream fis=new FileInputStream("d://desktop//test.txt");     
            try {     
                InputStreamReader isr=new InputStreamReader(fis,"utf8");     
                int i;     
                while((i=isr.read()) != -1){     
                    System.out.println((char)i);  //輸出  陳     
                }     
            } catch (Exception e) {     
                // TODO Auto-generated catch block     
                e.printStackTrace();     
            }                                            
                     
    }     
}    
```

## BufferedReader 

BufferedReader则是比InputStreamReader更高级, 

它封裝了StreamReader类, 一次读取取一行的字符

```java
import java.io.*;     
public class Main {     
    public static void main(String[] args) throws FileNotFoundException, UnsupportedEncodingException {     
                     
            FileInputStream fis=new FileInputStream("d://desktop//test.txt");     
                 
            try {     
                InputStreamReader isr=new InputStreamReader(fis,"utf8");                     
                BufferedReader br=new BufferedReader(isr);     
                     
                String line;     
                while((line=br.readLine()) != null){     
                    System.out.println(line);     
                }     
            } catch (Exception e) {     
                // TODO Auto-generated catch block     
                e.printStackTrace();     
            }                                            
                     
    }     
}    
```



