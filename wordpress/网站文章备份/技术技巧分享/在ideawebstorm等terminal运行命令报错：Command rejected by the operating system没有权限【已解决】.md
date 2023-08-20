在idea/webstorm等编译器terminal窗口运行命令报错：Command rejected by the operating system没有权限【已解决】

----

# 1、修改terminal窗口

打开编译器，找到工具 -> Terminal 

修改shell path 为 cmd窗口，之后重启编译器即可。

![image-20230706105120776](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230706105120776.png)

## 2、或修改powershell窗口权限

```bash
#执行：
get-ExecutionPolicy，
#显示Restricted 表示状态是禁止的;
 
#执行命令修改策略：
set-ExecutionPolicy RemoteSigned
 
#再执行查询
get-ExecutionPolicy
#显示RemoteSigned 无限制
```

![image-20230706105304613](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230706105304613.png)

# 3、powershell和cmd的区别

Windows PowerShell和命令提示符（cmd）是Windows操作系统中两种常见的命令行工具，它们在以下几个方面有所区别：

1. 语言和功能：
   - 命令提示符使用的是CMD（Command Prompt）命令解释器，其命令语言基于DOS命令。它提供了一些基本的命令和功能，但对于复杂的脚本编写和自动化任务较为有限。
   - Windows [PowerShell则是一个构建在.NET](http://xn--powershell-zf2pyph63bcir8u1amdyy7e.net/) Framework上的脚本环境和命令行外壳。它使用PowerShell脚本语言，基于 .NET 平台并支持大量的.NET库和功能。PowerShell提供了强大的脚本编写能力、管道处理以及访问和管理Windows系统的广泛功能。
2. 对象导向：
   - 命令提示符主要基于文本流进行命令的传递和处理。输出通常是纯文本形式。
   - Windows PowerShell将对象和对象的属性作为数据进行处理。它可以通过Cmdlets（根据.NET实现的命令）返回结构化数据，并且这些数据可以被其他命令直接使用。这种对象导向的方法使得PowerShell非常适合处理和操作复杂的数据结构。
3. 功能扩展性：
   - PowerShell具有更强大的功能扩展性。除了使用内置的Cmdlets和函数，它还允许用户编写自定义的脚本和模块，以便更好地满足特定的需求。这使得PowerShell在自动化、系统管理、任务调度等方面非常有用。
   - 命令提示符的功能相对较为受限，用户不能像PowerShell一样轻松地扩展和定制其功能。

总的来说，命令提示符适用于简单的命令行任务和基本的批处理脚本，而Windows PowerShell则更适合处理复杂的脚本编写、自动化任务和系统管理。[尤其是对于需要与.NET](http://xn--jhqyll8ettqdgak58cqk2dytwa.net/) Framework和广泛的Windows系统功能进行交互的场景，Windows PowerShell提供了更多的功能和灵活性。