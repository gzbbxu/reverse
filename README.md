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
