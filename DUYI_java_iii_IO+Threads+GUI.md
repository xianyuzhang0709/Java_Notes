# I/O - java.io

输入/输出  流(数据流动)

数据流动的方向：读input  写output

流：文件流，字符流，对象流，网络流。。。

1. 什么是文件
   * 一种电脑的储存形式
   * 文件格式：.txt  .doc  .ppt  .mp4  .jpg  .rar  ...
   * 文件夹？目录路径。**File类(第一章)**-->与电脑上的文件夹或文件产生一一对应的映射关系
2. 什么是流？
   * 按方向区分：in(读取)  out(写入)
   * 按目标区分：文件流，数组流，字符串流，数据流，对象流，网络流。。。
   * 学习**文件流(第二章)**：顾名思义，读取文件中的信息in，将信息写入文件中out。
     * 文件流按照读取和写入的单位(字节数)大小来区分：
       1. 字节型文件流（1字节）- byte
          * **FileInputStream** / **FileOutputStream**
       2. 字符型文件流（2字节——1字符）- char
          * **FileReader** / **FileWriter**



### 一、File类

**An abstract representation of file and directory pathnames;** 

File与真实硬盘中的文件夹不是同一个东西。它是内存中的一个对象，与硬盘上的文件或文件夹是映射关系。

构造函数：

* **File(String pathname)**

* **File(String parent, String child)** 

  File(File parent, String child)

* File(URI uri)

```java
File file = new File("D://test//test.txt");  //file与test.txt形成了映射关系
```

* Test.txt文件，写成test.txt也能生出映射。--> 硬盘上文件的名字是不区分大小写的。

方法：

1. 关于文件的操作：

   * .canRead()  --> boolean

   * .canWrite()

   * .canExecute()

   * .isHidden() --> boolean

   * .isFile() 是否是一个文件（如果是directory则false）

   * .isDirectory()

   * .length() 文件字节长度（即文件大小）--> long

     ```java
     Array.length  List.size()  String.length()  File.length()
     ```

   * .lastModified() 最后修改时间 --> long毫秒值

     ```java
     long time = file.lastModified();
     Date d = new Date(time);  //格林威治时间
     SimpleDateFormat s = new SimpleDateFormat("yyyy-MM-dd KK:mm:ss")
     System.out.println(s.format(date));//我们所熟知的时间表示方法
     ```

   * .setLastModified(time) 设置最后修改时间

   * .getAbsolutePath() 获取绝对路径 --> String

     * 绝对路径有盘符（硬盘位置，eg：C:）。
     * 相对路径没有盘符，从当前项目工程所在位置开始寻找。

   * .getName() 文件名 --> String

   * .createNewFile() 创建新文件 ---> boolean

     ```java
     File file = new File("D://test//abc.text");//此文件在硬盘中并没有
     try{
       file.creatNewFile();//会产生编译时异常，所以要try-catch处理
       System.out.println(file.getAbsolutePath());
     }catch(IOException e){
       e.printStackTrace();
     }
     ```

   * .mkdir() 创建路径 --> boolean (前提是父文件夹，必须真实存在)

     ```java
     File f = new File("D://test//aaa"); //硬盘中没有此文件
     f.mkdir();  //这个不会产生异常，因为文件夹本就不是一个真实存在的文件
     ```

   * .mkdirs() 创建多个路径 --> boolean (即使外层文件夹也不存在，此方法会一并全部创建)
     * 注意：当我们创建文件或文件夹时，不论你的路径名有没有文件类型后缀（比如aaa.txt)，还是文件名没有后缀（比如abc），系统只看你调用了哪个方法。调用了创建路径的方法，路径名就会是aaa.txt，但它是一个路径。文件名也一样。

   * .getParent() 获取当前file的父亲file名字 --> String pname
   * .**getParentFile()** 获取当前file的父亲file对象 --> File (一般是个路径)

   * .list() 所有子元素的名字 --> String[] （没有子元素就是null）
   * .**listFiles**() 所有子元素(对象) --> File[]
   * .**delete**() 删除文件或空文件夹（连回收站都没有了）--> boolean
     * 有文件的文件夹删不了

2. 文件夹的遍历(递归Recursion)

   ![image-20210105120024821](DUYI_java_iii_IO+Threads+GUI.assets/image-20210105120024821.png)

   循环能做的事，递归都能做，但是递归能做的，循环不一定能做。

   递归的逻辑：先等着别人先干完事，我再执行。

   * 所以for循环是1-5层盖宝塔。递归是5-1等待，1执行完，逐步完成最后的执行。
   * 递归是调用从上到下，执行从下到上。
   * ![image-20210105120835092](DUYI_java_iii_IO+Threads+GUI.assets/image-20210105120835092.png)

   ```java
   public void buildTower(int floor){
     if(floor>1){  //停止递归的条件
       this.buildTower(floor-1);  //递归
     }
     System.out.println("盖到了第"+floor+"层");
   }
   ```

   ![image-20210105121711369](DUYI_java_iii_IO+Threads+GUI.assets/image-20210105121711369.png)

   * 递归更消耗内存

3. 遍历文件夹

   ```java
   public class TestFIle{
     public void showFIle(File file){
       //判断file如果是一个文件夹，且有元素，则找一个人去里面找
       File[] files = file.listFiles();
       if(files!=null && files.length!=0){//说明此元素是文件夹，且有子元素
         for(File f:files){
           showFile(f);//每个子元素完成类似操作
         }
       }
       System.out.println(file.getAbsolutePath());//如果file只是个文件或空文件夹，那么打印绝对路径
     }
   }
   ```

   ![image-20210105150514158](DUYI_java_iii_IO+Threads+GUI.assets/image-20210105150514158.png)

   * 绿色为如果把输出本文件的绝对路径放在第一行执行。（红色是放在最后一行执行）

4. 删除一个文件夹

   * 因为.detete()方法只能删文件或空文件夹，不能删有子元素的文件夹。所以要设计一个递归方法来删除一个文件夹。

   * ```java
     public void deleteFiles(File file){
       FIle files = file.listFiles();
       if(files!=null && files.length!=0){//注意files是一个数组
         for(File f:files){
           deleteFiles(f); //对每一个子元素来一遍这个操作
         }
       }
       file.delete();//删除一个文件或空文件夹
     }
     ```

5. 文件夹的路径（找父路径）

### 二、字节型文件流 - Stream

容器：

1. 变量：只能存一份
2. 数组：能存多个，数据类型统一
3. 集合：能存多个，数据类型统一，存储数量可变，泛型
   * 以上都存储在由JVM开辟的内存空间中，数据存储是临时性的。
4. 文件：能存多个
   * 文件存在硬盘上，永久性保存。
   * 数据虽然安全了，但需要通过IO操作文件。不能像前三个一样，直接操作。
   * 这就是数据库。

#### 2.1 FileInputStream类 - 继承InputStream(字节型输入流的父类)

构造函数：

1. **FileInputStream(File file)**

   ```java
   FileInputStream fis = new FileInputStream(new FIle("D://test//Test.text"));
   ```

2. **FileInputStream(String name)**

   ```java
   try{
     FileInputStream fis = new FileInputStream("D://test//Test.txt"); //很有可能会抛出文件不存在的异常，所以必须处理
   }catch(FileNotFoundException e){ //编译时异常
     e.printStackTrace
   }
   ```

方法：

1. .read() 每次读取一个字节(有个指针，每次调用读取下一个字节)

   *  --> int(该字节对应的Unicode码)，读不到则-1
   * `\n换行-结束本行=10  \r回车-光标会到最前=13`  可以把int转为char或任何你希望看到的输出形式
   * 会有编译时异常，所以也要try-catch

2. .read(byte[] b) 每一次读一个字节，存到数组里

   * --> int 读取到的有效字节个数，

   ![image-20210105155257609](DUYI_java_iii_IO+Threads+GUI.assets/image-20210105155257609.png)

   * 得到的输出结果：

     ```java
     abcdefg
     hijklmn
     opq
     op  --> 这不在原文件中，为什么重复出现了？因为数组是不断被覆盖的。当没有足够多的元素覆盖到数组时，旧的元素还在里面。  
     ```

     程序修改：

     ```java
     String value = new String(b, 0, count); //读了多少个字符，就把多少字符存入String。（就数组依然没有覆盖完毕）
     ```

3. .read(byte[], int off, int len) --> int
4. .avaliable() 流管道中有多少缓存的字节（并不是文件中的字节，而是流管道中读取了文件的多少字节）
   * 可能在读取网络数据的时候，流管道中的字节数可能会有问题。不一定是同一批一起来的。
5. .skip(long n) 跳过几个字节 --> long(一般这个返回值不接收)
   * 多线程：利用几个线程同时读取文件
   * 即100000个字节，5个线程同时读取：1-2000，2001-4000，。。。（迅雷下载）

6. .close() 关闭流管道。(流管道必须要关，否则进程一直不结束)

   * 管道一定要关。一般放在finally里面。

   * 由于fis.close()必须放在finally里，而fis是定义在try里面的，所以办法是：把变量fis放在try外，赋值放在try里。另外finally里也要做判断和try-catch。

     ```java
     FileInputStream fis;
     try{
       fis = new FileInputStream(new File("D://test//Test.txt"));
       fis.skip(5);
       int code = fis.read();
     	System.out.print(code);
     }catch(IOException e){
       e.printStackTrace();
     }finally{
       try{//保险起见，因为fis如果没有成功找到file，可能有空指针的问题。
         if(fis!=null){
           fis.close();
         }catch(IOException e){
           e.printStackTrace();
         }
       }
     }
     ```

![image-20210105121551130.png](DUYI_java_iii_IO+Threads+GUI.assets/image-20210105121551130.png)

* 通过File对象来间接操作文件。

#### 2.2 FileOutputStream类 - 继承OutputStream(字节型输出流的父类)

构造函数：

0. 文件输出流不会造成FileNotFoundException，因为它总是会帮你创建一个新的指定文件。

1. **FileOutputStream(String path)**
   * 此方法的底层是：无论它是否能找到指定文件，它都会帮你创建一个新的此文件，并且覆盖掉旧的。
2. **FileOutputStream(String path, boolean append)**
   * 如果append==true，那么调用.write()会在文件(如果能找到这个文件)文字内容后面追加字节。如果文件找不到，则新建，并且文件支持追加。
3. **FileOutputStream(File f, [boolean append])**

方法：

1. .write(int code) 把写入流管道，一次写一个。.flush() 把流管道中的文字写入文件 - 刷新（新版本java把这个方法隐藏了）

   * int可以写char，'c'会自动转化为int。

2. .wirte(byte[] b)

   ```java
   String str = "1+1=2";
   Byte[] b = str.getByte();
   fos.write(b);  //前提是fos的append==true
   ```

3. .close()

![image-20210105172311263](DUYI_java_iii_IO+Threads+GUI.assets/image-20210105172311263.png)

#### 2.n 练习

1. 文件的复制

   C://test//aaa.txt ---复制到---> D://某个位置

   步骤：找到C盘中的源文件，读内容。去D盘创建新文件，写入内容。

2. 文件的加密

   加密和解密

3. 文件夹的复制（递归）
4. 文件的剪切：先复制，然后删除原文件

### 三、字符型文件流 - Reader

字节型输入流**读取中文**：会按照字节范围读，中文字符占两个字节。拆能拆着读，关键是返回时组合不能有问题。

* 可以用byte[] b读取文件

  ```java
  try{
    FileInputStream fis = new FileInputStream("D://test//Test.txt");
  	byte[] b = new byte[5];
  	int count = fis.read(b);//一次读5个字节，有效读取次数存入count
    try{
      while(count!=-1){
        System.out.print(new String(b,0,count,"GBK")); //组合时
        count = fis.read(b);
      }
    }
  }catch(IOException e){
    e.printStackTrace();
  }finally{
  	fis.close();
  }
  ```

* 字节流的好处：什么文件都可以处理。不好处：可能会存在断码问题。
  * 能处理的文件是：用记事本打开能看懂的。docx就不是这类型。

#### 3.1 FileReader - InputStreamReader - Reader

构造函数：

1. FileReader(File f)
2. FileReader(String pathname)

* 也要处理异常

方法：

1. .read() 一次读一个字符 --> int
2. .read(char[] c) 一次读c.length个字符 --> int读取到的字符个数
   * .read(new char[1024])。把读取到的字符存入char数组。

#### 3.2 FileWriter - OutputStreamWriter - Writer

构造函数：

1. 带file参数，[boolean append]
2. 带String参数，[boolean append]

方法：

1. .write(int i) 一次写一个char字符
2. .write(char[] c)
3. .wirte(String s)

> #### Byte 和 char
>
> 1 bit = 0/1
>
> 1byte = 8bits
>
> 1kb = 1024 bytes
>
> |       |                 |          |
> | ----- | --------------- | -------- |
> | byte  | 1byte = 8 bits  | -128~127 |
> | short | 2bytes = 16bits | 3万2     |
> | int   | 4bytes = 32bits | 21亿     |
> | long  | 8bytes = 64bits |          |
>
> char是UTF-16(16-bit Unicode Transformation Format)编码的字符集。由1-2个16bits的代码单元组成。
>
> * is a [character encoding](https://en.wikipedia.org/wiki/Character_encoding) capable of encoding all 1,112,064 non-[surrogate](https://en.wikipedia.org/wiki/Universal_Character_Set_characters#Surrogate) [code points](https://en.wikipedia.org/wiki/Code_point) of Unicode。
>
> * UTF-16的编码方式为：
>
>   1. Characters with values less than 0x10000 are represented as a single 16-bit integer with a value equal to that of the character number. 
>
>      * 值<65536的char，直接用16-bit的整数来表示这个char。
>
>   2. Characters with values between 0x10000 and 0x10FFFF are represented by a 16-bit integer with a value between 0xD800 and 0xDBFF (within the so-called high-half zone or high surrogate area) followed by a 16-bit integer with a value between 0xDC00 and 0xDFFF (within the so-called low-half zone or low surrogate area).
>
>      ```
>      Note: Values between 0xD800 and 0xDFFF are specifically reserved for use with UTF-16, and don't have any characters assigned to them.
>      ```
>
>      * 65536<=char值<=1114111的char，用两个大小分别在55296~56319和56320~57343的16-bit整数来表示。
>      * 在这两个区间，没有对应的Unicode字符，这两个区间纯粹用于为UTF-16编码。
>
>   3. Characters with values greater than 0x10FFFF cannot be encoded in UTF-16.
>
> 在学String的时候，有codePoints和code unit的区别问题。
>
> * String的本质是一个char系列。但是char是UTF-16编码的。
> * 对于Unicode编码范围大于65535的char，要用两个code units来表示这个字符。
> * str.length() 得到的是这个字符串的code units数量。当你使用str.charAt(i)，得到的也是对应的code unit的结果，而不是真正的char。如果你想得到一个字符串真实的长度，要用str.codePointCount()。
> * 所以书中强烈不建议在程序中使用char类型。并且把字符串当做抽象数据类型处理。（没懂）

疑惑是：文件用byte读写和用char读写的区别是什么？

* 最终两种方式都可以把文件底层的010101完整地复制过来。但是如何转回呢？哦，因为我们不需要输出？？？
* 当我们要输出打印的时候，就要考虑，当初那个文字，在写入时，用的是什么字符集。数字就是那些数字，它包含着完整的信息。你如何解码，决定了这个字符它转为char之后，表现为什么——是英文，是数学符号，是德文，中文，阿拉伯文。所以说，解码由字符集决定。
* 你写了什么，就存进去了什么。中文也有储存的方式。但是怎么把复制过来的数字们，转成原文。需要字符集。