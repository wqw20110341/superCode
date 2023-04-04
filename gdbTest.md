nasserver调试技巧
1、查看nasserver进程好

断点命令: break，缩写b
break <source_file_name:line_num> 文件名:行号,
break <function_name> 面数名
删除断点命令: delete <break_point>, 缩写d,
delete 1 删除1号断点。 
查看堆栈命令: backtrace, 缩写bt  (打印最内n层: bt n，最外n层，  bt -n)
查看所有线程堆伐命令: thread apply all bt
查看变量命令:p
p<para_name> 变量名 〈可使用p直接修改变量值: p <para_name> =xx)
查看所有的局部变量: info local
转到线程令: thread <thread_number>
转到顿命令: frame <frame_number>
执行下一步命令: next，缩写n
继续运行直到达到下一个断点命令: continue,缩写c
进入子函数命令: step, 缩写s
结束子函数返回到调用函数命令: finish, 缩写f
结束循环命令: unti1，缩写u

查看内存地址: examine, 缩写x,
x/<n/f/u><addr>,
输出格式
x按十六进制格式显示变量
d按十进制格式显示变量
u按十六进制格式显示无符号整型
o 按八进制格式显示变量
t按二进制格式显示变量
a按十六进制格式显示变量
c按字符格式显示变量
f按浮点数格式显示变量
s按字符串格式显示变量
举例: x/10wx addr 查看10个4字节按16进制输出
x/10i addr 查看该地址的地条指令
查看数据: print，缩写，
print /f exp exp是表达式，/f指定打印时的格式
打印格式和x输出格式一致


信号 signals,
x/<n/f/u> <addr>,
info signals
打印信号种类表及GDB对他们相应的处理
handle signal keyword
改变GDB对signal信号的处理
stop/nostop
print/noprint
pass/nopass
常用:  handle SIG35 nostop noprint (产品一般会自定义该信号的处理，因此调试过程先过滤掉)
watch 内存,
   watch expr: GDB在expr被程序写及其值改变时停止;
   rwatch expr: GDB在expr被程序读时停止;
    awatch expr:   GDB在expr被程序读和写时停止;
   使用较多的是watch一个地址内存是否有变化，watch *(int*)addr
info register 查看寄存器，
ir (缩写) :查看当前枝的所有寄存器信息, 定位问题的时候经常用到;


gdb 不阻塞进程运行,
般业务进程都会有心跳机制，我们使用gdb调试的时候一般需要先关闭心跳功能，避免长时间断住进程而
导致复位
以下两种方式也可以做到gdb执行命令后快速退出:
1. gdb -batch -ex "command" -p pid (如 gdb -batch -ex "call test_show() "-p 23467) ;
2. 使用GDB的command命令，自定义操作，该使用在问题复现场景较多;
gdb attach pid         ---gdb进程
handle SIG35 SIG36 SIGUSR2 nostop noprint  --屏蔽信号，gdb不处理
set height 0
def oops         ----自定义操作, 断点之后的操作
bt          ---打印调用栈
calltest_show() ---调用西数, 打印信息
p/x g_pstModInfo    ---查看全局变量
fin 
i   r     ---查看寄存器
Detach    ---退出
q
end
b test_del  --打断点
command
-command 关键字
Oops自定义操作
end命令结束
c      --b test_del 之后的继续运行

进程: 系统进行资源分配和调度的基本单位;
线程: 操作系统能够进行运算调度的最小单位;
简单理解:
1.进程申请的资源, 进程内线程可以访问;
2.进程退出了, 进程内所有线程也就消亡了;
3.一般所说的并发, 大部分指进程内线程之间访问同一个进程资源;
   
   GDB调试就是attach到进程上, 对线程进行调试
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
