#记录
##分析
准备换工作之前需要学习很多东西，现在的目标是以学习视觉处理技术为主要目标。将写的乱七八糟的文档和整理的资料都放在Git仓库里面。方便查询。

- Halcon
- OpenCV
- Python
- C#和WPF技术
- 机器学习
- 英语口语
##实战目标
- 在设备登录设置人脸系统
- Halcon实现物体分类

##资料和时间计划
###Halcon
学习以官方文档和超人视频培训为主，同时多做一些例程和项目实战
###OpenCV
分为C++版本和Python版本
以Python版本为先，然后再建立C++工程，以写算法，并生成dll为主。
###Python
万门的大学的课程已经涵盖了基本的知识点和操作，学习完万门大学的课程后就可是直接做一些项目实践了。然后以算法实现为主。
###C#和WPF技术
iMOOC上田老师的课程学习完以后，就开始项目实战和温习功课。
- 事件和委托
- 多线程刷新I/O
- 画图
- WPF的基本操作和数据binding
###机器学习
还是以TenserFlow等框架学习为主。
###英语学习
口语和跟读，精读为主

##现状
现在在进行的学习有：
> 1. iMooc上的C#课程
> 2. 万门大学的Python课程
> 3. Hlacon的官方手册学习
> 4. 超人视觉培训  

#2018-05-15
##知识点记录
###Python
1. zip，zip(*)，数组组合，数组解耦合
2. type()类型
3. 序列化，
4. 读取文档等操作都是类似的
5. 显示所有变量，加上这一行前缀

		from IPython.core.interactiveshell import InteractiveShell
		InteractiveShell.ast_node_interactivity = "all"

6. 






#2018-05-17
##知识点记录
###Python
1. generator
2. Decorators
Somtimes, you want to modify an existing function without changing its source code. Some common use cases are logging and debugging.
	
	例程：
		
		def should_log(func):
	    def func_with_log(*args, **kwargs):
	        print("Calling:", func.__name__)
	        return func(*args, **kwargs)
	    return func_with_log
	
		add_with_log = should_log(add)
		add_with_log(1, 2)
		Calling: func_with_log
		Calling: add
		3
		
		@should_log
		def add(op1, op2):
		    return op1 + op2
		​
		add(1, 2)
		Calling: add
	
3. OOP super函数，使用父类的函数

#2018-05-17
### Magic methods for comparison:
		
		
		__eq__:==
		__ne__: !=
		__lt__: <
		__gt__: >
		__le__: <=
		__ge__: >=
		Magic methods for numbers:
		
		__add__: +
		__sub__: -
		__mul__: *
		__floordiv__: //
		__truediv__: /
		__mod__: %
		__pow__: **
		