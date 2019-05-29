# Linux Learn
本文档记录对于linux核心命令进行记录

:100:鸣谢
- 感谢学习本笔记的每一个观众
- 感谢修改本笔记的每一个贡献者

## 命令列表
- [top](#top)
- [tptim](#uptime)
- [vmstat](#vmstat)
- [pidstat](#pidstat)
- [jps](#jps)
- [jinfo](#jinfo)
- [jmap](#jmap)
- [jstack](#jstack)
- [jconsole](#jconsole)
- [jvisualvm](#jvisualvm)

## jvisualvm
![Alt text](https://github.com/independenter/source-learning/blob/master/linux%E7%BB%8F%E5%85%B8%E5%91%BD%E4%BB%A4/pic/jvisualvm.png)

## jconsole
- 图形化监控工具
- 可以查看Java应用程序的运行概况，监控堆信息、永久区使用情况、类加载情况等
![Alt text](https://github.com/independenter/source-learning/blob/master/linux%E7%BB%8F%E5%85%B8%E5%91%BD%E4%BB%A4/pic/jconsole.png)

## jstack
打印线程dump
- -l 打印锁信息
- -m 打印java和native的帧信息
- -F 强制dump，当jstack没有响应时使用
![Alt text](https://github.com/independenter/source-learning/blob/master/linux%E7%BB%8F%E5%85%B8%E5%91%BD%E4%BB%A4/pic/jstack.png)

## jmap
生成Java应用程序的堆快照和对象的统计信息
- jmap -histo 5848 >1.txt
- jmap -dump:format=b,file=d:\heap.hprof 5848
- jhat d:\heap.hprof
![Alt text](https://github.com/independenter/source-learning/blob/master/linux%E7%BB%8F%E5%85%B8%E5%91%BD%E4%BB%A4/pic/jmap.png)

## jinfo
可以用来查看正在运行的Java应用程序的扩展参数，甚至支持在运行时，修改部分参数
- -flag <name>：打印指定JVM的参数值
- -flag [+|-]<name>：设置指定JVM参数的布尔值
- -flag <name>=<value>：设置指定JVM参数的值
![Alt text](https://github.com/independenter/source-learning/blob/master/linux%E7%BB%8F%E5%85%B8%E5%91%BD%E4%BB%A4/pic/jinfo.png)

## jps
列出java进程，类似于ps命令
- 参数-q可以指定jps只输出进程ID ，不输出类的短名称
- 参数-m可以用于输出传递给Java进程（主函数）的参数
- 参数-l可以用于输出主函数的完整路径
- 参数-v可以显示传递给JVM的参数
![Alt text](https://github.com/independenter/source-learning/blob/master/linux%E7%BB%8F%E5%85%B8%E5%91%BD%E4%BB%A4/pic/jps.png)

## top
![Alt text](https://github.com/independenter/source-learning/blob/master/linux%E7%BB%8F%E5%85%B8%E5%91%BD%E4%BB%A4/pic/top.png)

## uptime
![Alt text](https://github.com/independenter/source-learning/blob/master/linux%E7%BB%8F%E5%85%B8%E5%91%BD%E4%BB%A4/pic/uptime.png)

## vmstat
![Alt text](https://github.com/independenter/source-learning/blob/master/linux%E7%BB%8F%E5%85%B8%E5%91%BD%E4%BB%A4/pic/vmstat.png)

## pidstat
- pidstat -p 2676 1 3 -u -t
![Alt text](https://github.com/independenter/source-learning/blob/master/linux%E7%BB%8F%E5%85%B8%E5%91%BD%E4%BB%A4/pic/pidstat.png)

