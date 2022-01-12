### Scanner

```java
// 常用方法

public boolean hasNext()
public String next()
public boolean hasNextXxx()
public xxx nextXxx()
public boolean hasNext(String pattern)
public String next(String pattern)
    
skip(String chart)   // 要跳过的字符串 （开头）
Delimiter()       // c
UseDelimiter(String chart) / UseDelimiter(String pattern)    // 规定分隔符（字符 / 正则表达式）
redix() // 查看当前进制
UseRedix(int redix)  // 使用 某种 进制
```

```java
//常用构造器

public Scanner(InputStream source)  // 指定输入流

public Scanner(File source) throws FileNotFoundException  // 指定文件的来源，需要 File 类作为参数

public Scanner(File source，String charsetName) throws FileNotFoundException // 同上，并指定字符集
```

```java
package classuse.Scanner_Study;

import java.io.*;
import java.util.Scanner;

/**
 * @author dong
 * @create 2021-09-08 8:07
 */
public class Construct_study {
    public static void main(String[] args) throws IOException {
        // 1:     Scanner(InputStream source)  --->  指定输出流
        Scanner scan = new Scanner(System.in);
		var n = scan.nextInt();

        
        // 2:     Scanner(File source)  --->  指定读入文件内容
        Scanner scan_file = null;
        try { // 处理异常
            scan_file = new Scanner(new File("f:"+File.separator+"hello.txt"));
            // 使用迭代器对其进行遍历
            while(scan_file.hasNext()){
                System.out.println(scan_file.next());
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        scan_file.useDelimiter(" ");// 指定 空格 为分隔符



     //3:Scanner(File source,String charsetName)  --->  指定读入文件内容,使用charsetName 编码格式
        Scanner scan_file2 = null;
        try {  //  异常处理
            scan_file2 = new Scanner(new File("f:"+File.separator+"hello_1.txt"),"utf-8");
            while(scan_file2.hasNext()){
                System.out.println(scan_file2.next());
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        scan_file2.useDelimiter("\n");


        scan_file2.close();
        scan_file.close();
        scan.close();

    }
    
}
```



