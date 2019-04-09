### 下载工具

1. android Killer 下载。找到APKTOOL管理器，从官网下载AptTool 之后，添加进去，否则有可能反编译失败。 

### Smali语法

1. ```
   iget-object v0, p0, Lmedia/zkk/com/myapplication/MainActivity$1;->this$0:Lmedia/zkk/com/myapplication/MainActivity;
    p0 是匿名内部类对象
    v0 =p0->this$0
    this$0 可以看作是 MainActivity.this  Java匿名内部类持有的外部类对象
    
    iget-object v3 ,v3,lf8/android03/MainActivity;->username:Landroid/widget/EditText;
    (EditText)v3 = v3 -> username;
    
    
    invoke-virtual{v3},Landroid/widget/EditText;->getText()Landroid/text/Editable;
    调用v3 的 getText 方法。
     v3 ->getText();
    
    move-result-object v3 移动上一次方法调用的对象引用返回值到v3。
     (Editable)v3 = v3->getText(); 上面的结果赋值到v3
     
     invoke-virtual {v3},Ljava/lang/Object;->toString()Ljava/lang/String;
     v3->toString();
     ove-result-object v2
     v2 = v3 ->toString();
     
     invoke-virtual {v3,v2},Lf8/android03/MainActivity;->getPasswordFromName(Ljava/lang/String)Ljava/lang/string;
     mov-result-object v0
     v0 = v3->getPasswordFromName(v2);
   
    
    .method pubic etPasswordFromName(Ljava/lang/String;)Ljava/lang/String;
    .param p1,"username"
    可以推断方法原型。
    
    public String getPasword(String username){
        const-strign v0,'www.baidu.com'
        String v0 = "www.baidu.com";
        invoke-virtual {p1},Ljava/lang/String;-getBytes()[B
        move-result-object v3
        byte [] v3 = p1->getBytes();
        
        array-lengh v2,v3
        int v2 = v3.length;
        
        new-array v4,v2,[B
        byte[] v4 = new byte[v2];
        
        const/4 v1,0x0
        int v1 = 0;
        
        if-get v1,v2,:cond_0
        如果v1>=v2 跳转到cond_0 这里写成while 循环应该是
        while(v1<v2){
            aget-byte v5,v3,v1
            v5= v3[1]
            invoke-virtual{v0},Ljava/lang/String->length();
            move-resut v6
            int v6 = v0->length();
            rem-int/2addr v5, v4   //计算vx % vy并将结果存入vx。
            int v5 = v5% v4;
            invoke-virtual {v0,v5},Ljava/lang/String;->chartAt(I)C
            move-result v5
            char v5 = v0->chartAt(v5);
            int-to-byte v5,v5   转换vy寄存器中的int型值为byte型值存入vx。
            byte v5 = v5->tobyte();
            
            aput-bye v5,v4,v1    //aput-byte vx, vy, vz  将vx的byte值作为元素存入byte数组，数								//组的引用位于vy，元素的索引位于vz。
            v4[v1] =v5;
            
            	add-int/lit8 vx, vy,0x1 
            	v1 = v1+1
        }
        new-instance v5 ,Ljava/lang/String
        String v5 = new Sting();
        
        invode-direct{v5,v4},Ljava/langString;-><init>([BV
         String v5 = new Sting(v4);
        
        return v5;
        
        
        
    }
    
      
    
   ```

2

### 调试技巧：

1，android studiao 下载插件 https://bitbucket.org/JesusFreke/smali/downloads/  smalidea

2，AS 导入反编译后的代码。android:debuggable="true" 清单文件怎加debug 模式。

3，入口增加断点。

4，EditConfigures 增加remote 设置调试端口

5，使用adb 以debug 方式启动apk

​	adb shell am start -D -n packageName/ActivityName

### smali 与java的转换

1，jd-jui  把smali 自动转换位java代码的工具，免费，并且反编译结果较位准确，方便平时查看用，对于一些较小型的apk 软件很好用。dex2jar 然后用jd-jui 打开

2，jeb 一个大型的smaili 代码反编译工具，需要购买，价格较贵。相对来说功能比较强大。反编译结果相对于上面来说更加准确。当然，对机子的要求也较高。真正的神器。

 


### 从0开始打造自己的破解代码
**一般是再java中写好代码，再反编译 再拷贝进去 **
~~~
.class public Lcom/demo/reversedemo/Hello;
.super Ljava/lang/Object;

.field public static final hStr:Ljava/lang/String; = "hello world static "
.field public hStr2:Ljava/lang/String;
.method public constructor<init>()V
.locals 1 
    invoke-direct {p0}, Ljava/lang/Object;-><init>()V
    const-string v0,"helloworld form init field"
    
    iput-object v0,p0,Lcom/demo/reversedemo/Hello;->hStr2:Ljava/lang/String;  
    return-void
.end method



.method public static retHello()Ljava/lang/String;
.locals 1
    const-string v0,"hello world from retHello"
    return-object v0

.end method

.method public static retHello2()Ljava/lang/String;    
.locals 1
    sget-object v0,Lcom/demo/reversedemo/Hello;->hStr:Ljava/lang/String;
    return-object v0
.end method

.method  public retHello3()Ljava/lang/String;
.locals 1
 const-string v0,"hello world from normal method"
 return-object v0
.end method

.method public retHello4()Ljava/lang/String;
.locals 3
iget-object v0, p0,Lcom/demo/reversedemo/Hello;->hStr2:Ljava/lang/String;
const-string v1, "l"
const-string v2, "t"
invoke-virtual {v0,v1,v2},Ljava/lang/String;->replace(Ljava/lang/CharSequence;Ljava/lang/CharSequence;)Ljava/lang/String;
move-result-object v0
return-object v0
.end method

~~~
### 堆栈跟踪 方法跟踪

invoke-static{},Ljava/lang/Thread;->dumpStack()V



方法跟踪需要加权限 
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>

invoke-static {},Landroid/os/Debug;->startMethodTracing()V

invoke-static {},Landroid/os/Debug;->stopMethodTracing()V

会在sdcard 中生成trace 文件。
