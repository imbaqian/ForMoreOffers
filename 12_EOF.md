#<center>EOF是什么？</center>

EOF: end of file,表示“文字流”(stream)的结尾。这里的“文字流”，可以是文件(file)，也可以是表中输入(stdin)。

比如下面这段代码：
<pre>
    int c;
    while((c=fget(fp)) != EOF){
        putchar(c);
    }
</pre>

很自然地，每个文件的结尾处，有一个叫做EOF的特殊字符，读取到这个字符，操作系统就认为文件结束了。

但是，EOF不是特殊字符，而是一个定义在头文件stdio.h的常量，一般等于-1。

\#define EOF (-1)

在Linux中，EOF根本不是一个字符，而是当系统读到文件结尾，所返回的一个信号值(也就是-1)。至于系统怎么知道文件的结尾，资料上是通过比较文件的长度。

所以处理文件可以写成这样：

<pre>
    int c;
    while((c = fget(fp)) != EOF){
        do something;
    }
</pre>

这样有个问题，fgetc()不仅是遇到文件结尾时就返回
EOF，而且当发生错误时，也返回EOF。因此，C语言又提供了feof()函数，用来保证是到了文件结尾。上面的代码feof()版本的写法就是:

<pre>
    int c;
    while(!feof(fp)){
        c = fgetc(fp);
        do something;
    }
</pre>

但是这样也有问题，fgetc()读取文件的最后一个字符以后，C语言的feof()函数依然返回0,表明没有到达文件结尾；只有当fgetc()向后再读取一个字符(越过最后一个字符),feof()才会返回一个非零值，表示到达文件结尾。

所以，按照上面的写法，如果一个文件含有n个字符，那么while荀晗的内部操作会运行n+1次。所以最保险的写法是：

<pre>
    int c;
    while(c != EOF){
        do something;
        c = fgetc(fp);
    }
    if(feof(fp)){
        printf("End of file reached.");
    }else{
        printf("something went wrong.");
    }
</pre>

除了表示文件结尾，EOF还可以表示标准输入的结尾。

<pre>
    int c;
    while((c = getchar() != EOF)){
        putchar(c);
    }
</pre>

但是，标准输入与文件不一样，无法事先知道输入的长度，必须手动输入一个字符，表示到达EOF。

Linux中，在新的一行的开头，按下Ctrl-D，就代表EOF(如果在一行的中间按下Ctrl-D，则表示输出“标准输入”的缓存区，所以这时必须按两次Ctrl-D)；Windows中，Ctrl-Z表示EOF。(Linux中，Ctrl-Z表示该进程中断，在后台挂起，用fg命令可以重新切回到前台；按下Ctrl-C表示终止该进程。)

那么，如果向输入Ctrl-D则么办？这时必须先按下Ctrl-V，然后输入Ctrl-D，系统就不会认为是EOF信号。Ctrl-V表示按“字面含义”解读下一个输入，要是想按"字面含义"输入Ctrl-V，连续输入两次就行了。
