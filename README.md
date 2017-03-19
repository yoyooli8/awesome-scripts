awesome-scripts [![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)
----
[![GitHub stars](https://img.shields.io/github/stars/superhj1987/useful-scripts.svg?style=social&label=Star&)](https://github.com/superhj1987/useful-scripts/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/superhj1987/useful-scripts.svg?style=social&label=Fork&)](https://github.com/superhj1987/useful-scripts/fork)

本项目fork自<https://github.com/oldratlee/useful-scripts/>，现已经用于公司运维环境中，基于原项目所做更新如下:

- 自安装脚本的修改：[self-installer.sh](self-installer.sh)
- Linux守护进程模板: [tpl/run-cmd-tpl](tpl/run-cmd-tpl)
- 增加脚本统一入口：[opscripts](opscripts)
- 集成开源java运维工具脚本：housemd、greys、sjk、jtop等
- 部分脚本的编写

------

## 运行/安装环境

- Linux
- git1.7+

## 安装

三种方法

- `curl -s "https://raw.githubusercontent.com/superhj1987/awesome-scripts/master/self-installer.sh" | bash -s`
- git/svn the source and `make install`
- git/svn the source and set `bin` to the System Path to use the common usage shells

此外，可以下载和运行单个文件，以[`show-busy-java-threads`](https://raw.githubusercontent.com/superhj1987/awesome-scripts/master/java/bin/show-busy-java-threads)为例。

- 直接运行：

```
curl -sLk 'https://raw.githubusercontent.com/superhj1987/awesome-scripts/master/java/bin/show-busy-java-threads' | bash
```

- 下载单个文件

```
wget --no-check-certificate https://raw.githubusercontent.com/superhj1987/awesome-scripts/master/java/bin/show-busy-java-threads
chmod +x show-busy-java-threads.sh
./show-busy-java-threads.sh
```

## 卸载

opscripts uninstall

## 使用

* `opscripts`
> show system command

* `opscripts list`
> show command list

* `opscripts update`
> update opscripts

## 命令列表及文档

### :coffee: [`Java`相关脚本](docs/java.md)

1. [show-busy-java-threads](docs/java.md#beer-show-busy-java-threadssh)  
    
    > 用于快速排查`Java`的`CPU`性能问题(`top us`值过高)，自动查出运行的`Java`进程中消耗`CPU`多的线程，并打印出其线程栈，从而确定导致性能问题的方法调用。
1. [show-duplicate-java-classes](docs/java.md#beer-show-duplicate-java-classes)  
    
    > 找出`jar`文件和`class`目录中的重复类。用于排查`Java`类冲突问题。
1. [find-in-jars](docs/java.md#beer-find-in-jarssh)  
    
    > 在目录下所有`jar`文件里，查找类或资源文件。
    
1. [housemd](java/bin/housemd)

	`housemd pid [java_home]`
	> 使用housemd对java程序进行运行时跟踪，支持的操作有：
	>
	> - 查看加载类
	> - 跟踪方法
	> - 查看环境变量
	> - 查看对象属性值
	> - 详细信息请参考: https://github.com/CSUG/HouseMD/wiki/UserGuideCN
	
1. [jargrep](java/bin/jargrep)

	`jargrep "text" <path or jarfile>`
	> 在jar包中查找文本，可查找常量字符串、类引用。
	
1. [findcycle](java/bin/findcycle)

	`findcycle [path]`
	> 查找当前工程中是否存在循环引用（目前仅支持maven工程，默认为当前路径）

1. [jvm](java/bin/jvm)	

	`jvm pid`

	> 执行jvm debug工具，包含对java栈、堆、线程、gc等状态的查看，支持的功能有： 
	><pre>
	>========线程相关=======
	>1 : 查看占用cpu最高的线程情况
	>2 : 打印所有线程
	>3 : 打印线程数
	>4 : 按线程状态统计线程数
	>========GC相关=======
	>5 : 垃圾收集统计（包含原因）可以指定间隔时间及执行次数，默认10秒
	>6 : 显示堆中各代的空间1000 5
	>7 : 垃圾收集统计。可以指定间隔时间及执行次数，默认10秒
	>8 : 打印perm区内存情况*会使程序暂停响应*
	>9 : 查看directbuffer情况
	>========堆对象相关=======
	>10 : dump heap到文件*会使程序暂停响应*默认保存到`pwd`/dump.bin,可指定其它路径
	>11 : 触发full gc。*会使程序暂停响应*
	>12 : 打印jvm heap统计*会使程序暂停响应*
	>13 : 打印jvm heap中top20的对象。*会使程序暂停响应*参数：1:按实例数量排序,2:按内存占用排序，默认为1
	>14 : 触发full gc后打印jvm heap中top20的对象。*会使程序暂停响应*参数：1:按实例数量排序,2:按内存占用排序，默认为1
	>15 : 输出所有类装载器在perm里产生的对象。可以指定间隔时间及执行次数
	>========其它=======
	>16 : 打印finalzer队列情况
	>17 : 显示classloader统计
	>18 : 显示jit编译统计
	>19 : 死锁检测
	>20 : 等待X秒，默认为1
	>q : exit
	></pre>
	>
	>  进入jvm工具后可以输入序号执行对应命令  
	>  可以一次执行多个命令，用分号";"分隔，如：1;3;4;5;6  
	>  每个命令可以带参数，用冒号":"分隔，同一命令的参数之间用逗号分隔，如：  
	>  Enter command queue:1;5:1000,100;10:/data1/output.bin 
    
1. [greys](java/bin/greys)

    `greys <PID>[@IP:PORT]`
    > 使用greys对java程序进行运行时跟踪(不传参数，需要先`greys -C pid`,再greys)。支持的操作有：
    >
    > - 查看加载类，方法信息
    > - 查看JVM当前基础信息
    > - 方法执行监控（调用量，失败率，响应时间等）
    > - 方法执行数据观测、记录与回放（参数，返回结果，异常信息等）
    > - 方法调用追踪渲染
    > - 详细信息请参考: https://github.com/oldmanpushcart/greys-anatomy/wiki
    
1. [sjk](java/bin/sjk)

    `sjk <cmd> <arguments>`
    `sjk --commands`
    `sjk --help <cmd>`
    > 使用sjk对Java诊断、性能排查、优化工具
    >
    > - ttop:监控指定jvm进程的各个线程的cpu使用情况
    > - jps: 强化版
    > - hh: jmap -histo强化版
    > - gc: 实时报告垃圾回收信息
    > - 更多信息请参考: https://github.com/aragozin/jvm-tools

### :shell: [`Shell`相关脚本](docs/shell.md)

1. [c](docs/shell.md#beer-c)  
    
    > 原样命令行输出，并拷贝标准输出到系统剪贴板，省去`CTRL+C`，`CTRL+V`操作。
1. [colines](docs/shell.md#beer-colines)  
    
    > 彩色`cat`出文件行，方便人眼区分不同的行。
1. [a2l](docs/shell.md#beer-a2l)  
    
    > 按行彩色输出参数，方便人眼查看。
1. [ap & rp](docs/shell.md#beer-ap-and-rp)  
    
    > 批量转换文件路径为绝对路径/相对路径，会自动跟踪链接并规范化路径。
1. [echo-args](docs/shell.md#beer-echo-args)    
    
    > 输出脚本收到的参数，在控制台运行时，把参数值括起的括号显示成 **红色**，方便人眼查看。用于调试脚本参数输入。
1. [console-text-color-themes](docs/shell.md#beer-console-text-color-themes)  
    
    > 显示`Terminator`的全部文字彩色组合的效果及其打印方式。
1. [tcp-connection-state-counter](docs/shell.md#beer-tcp-connection-state-counter)   
    
    > 统计各个`TCP`连接状态的个数。用于方便排查系统连接负荷问题。
1. [parseOpts](docs/shell.md#beer-parseopts)   
    
    > 提供命令行选项解析函数`parseOpts`，支持选项的值有多个值（即数组）。
1. [xpl and xpf](docs/shell.md#beer-xpl-and-xpf)    
    
    > 在文件浏览器中 打开/选中 指定的文件或文件夹。
1. [show-cpu-and-memory](docs/shell.md#beer-show-cpu-and-memory)    
    
    > 显示当前cpu和内存使用状况，包括全局和各个进程的。
1. [monitor-host](docs/shell.md#beer-monitor-host)    
    
    > 监控当前的内存、cpu、io以及网络状况，写入相应的log文件,建议使用crontab，定时调用此脚本。

1. [check-vm](docs/shell.md#beer-check-vmpy)    
    
    > 检查当前linux是否是在虚拟机上，包括openvz/xen、pv/uml、VmWare。
1. [get-pip](docs/shell.md#beer-get-pippy)    
    
    > 安装pip, 将pip程序封装在了文件中，可以避免网络安装pip过慢。
1. [redis](bin/redis)
    
    `redis ip1[:port1][,ip2[:port2]] [port] "command"``
    >批量执行redis命令(需要redis-cli)
1. [send](bin/send)
	
	`send filename [port]`
	> 使用nc发送文件（默认8888端口），接收方可以通过浏览器下载
1. [hex](bin/hex)
	
	`hex [0x]number[b]`
	> 计算数字的10进制、16进制及2进制文本，输入参数默认为10进制，可选16进制（0x）及二进制（b）。

1. [swap](bin/swap)
	
	`sudo swap [-s [-r]] [-g GREP_ARG]`
	> 查询当前服务器各个进程占用swap的情况。
	> -s 表示对swap占用量进行排序（升序） -r 表示对swap占用量进行排序（降序），使用-r的前提是-s参数开启。 -g grep命令的封装，用于查找特定类型的进程。比如我想查找带有java的进程，可以使用sudo wtool swap -g java
注意，使用该功能需要sudo权限

1. [tpl/run-cmd-tpl.sh](docs/shell.md#beer-tplrun-cmd-tplsh)    
    
    > linux下后台执行守护程序的模板shell脚本,修改文件中几个选项的值为需要执行的程序即可使用。

### :watch: [`VCS`相关脚本](docs/vcs.md)

1. [swtrunk](docs/vcs.md#beer-swtrunk)  
    
    > 自动`svn`工作目录从分支（`branches`）切换到主干（`trunk`）。
1. [svn-merge-stop-on-copy](docs/vcs.md#beer-svn-merge-stop-on-copy)  
    
    > 把指定的远程分支从刚新建分支以来的修改合并到本地`svn`目录或是另一个远程分支。
1. [cp-svn-url](docs/vcs.md#beer-cp-svn-url)  
    
    > 拷贝当前`svn`目录对应的远程分支到系统的粘贴板，省去`CTRL+C`操作。
1. [change_git_origin_remote](bin/change_git_origin_remote)  
    
    > 改变当前git项目的origin remote的url。
