---
title: JavaEE进阶知识学习----Quartz-3-Scheduler函数
tags:
  - Quartz
originContent: ''
categories:
  - Quartz
toc: true
abbrlink: 13512
date: 2019-03-22 19:43:54
---
####Scheduler的主要函数
1. Date scheduleJob = scheduler.scheduleJob(jobDetail, trigger);
<!-- more -->
绑定jobDetail和trigger，将它注册进Scheduler中，将trigger中的时间来触发jobDetail中的业务逻辑，返回的时间是最近一次的执行时间。

2. void start();启动Scheduler
3. void standdby();暂停

核心代码如下

	SchedulerFactory sFactory = new StdSchedulerFactory();

	Scheduler scheduler = sFactory.getScheduler();

	scheduler.start();

	scheduler.scheduleJob(jobDetail, trigger);

	//scheduler执行两秒后挂起
	Thread.sleep(2000L);

	scheduler.standby();
	//scheduler挂起三秒后重新执行

	Thread.sleep(3000L);
	scheduler.start();
执行结果为：

	当前时间为：2018-01-24 15:25:20
	Hello world
	当前时间为：2018-01-24 15:25:21
	Hello world
	当前时间为：2018-01-24 15:25:22
	Hello world
	当前时间为：2018-01-24 15:25:25
	Hello world
	当前时间为：2018-01-24 15:25:25
	Hello world
	当前时间为：2018-01-24 15:25:25
	Hello world
	当前时间为：2018-01-24 15:25:26
	Hello world
4. void shutdown();关闭

方法中可以传入一个布尔类型的参数，true表示等待所有正在执行的job执行完毕后，再关闭scheduler，false表示直接关闭scheduler。
####quartz.properties
项目启动会加载项目根目录下的quartz.properties文件，没有就会加载jar下的quartz.properties文件，文件中的属性如下
1. 调度器属性：org.quartz.scheduler.instanceName属性用来区分特定的调度器实例，可以按照功能用途来给调度器起名。
org.quartz.scheduler.instanceId属性和前者一样，也允许任何字符串，但是这个值必须在所有调度器实例中是唯一的，尤其在一个集群中，作为集群的唯一key。
2. 线程池属性：重要的属性是threadCount，threadPriority优先值
3. 作业存储设置：描述了调度器实例的生命周期，Job和Trigger信息是如何被存储的。
4. 插件配置：满足特定需求用到的Quartz插件的配置。

####quartz.properties文件实例

	# Default Properties file for use by StdSchedulerFactory
	# to create a Quartz Scheduler Instance, if a different
	# properties file is not explicitly specified.
	#
	# ===========================================================================
	# Configure Main Scheduler Properties 调度器属性
	# ===========================================================================
	org.quartz.scheduler.instanceName: DefaultQuartzScheduler
	org.quartz.scheduler.instanceid:AUTO
	org.quartz.scheduler.rmi.export: false
	org.quartz.scheduler.rmi.proxy: false
	org.quartz.scheduler.wrapJobExecutionInUserTransaction: false
	# ===========================================================================  
	# Configure ThreadPool 线程池属性  
	# ===========================================================================
	#线程池的实现类（一般使用SimpleThreadPool即可满足几乎所有用户的需求）
	org.quartz.threadPool.class: org.quartz.simpl.SimpleThreadPool
	#指定线程数，至少为1（无默认值）(一般设置为1-100直接的整数合适)
	org.quartz.threadPool.threadCount: 10
	#设置线程的优先级（最大为java.lang.Thread.MAX_PRIORITY 10，最小为Thread.MIN_PRIORITY 1，默认为5）
	org.quartz.threadPool.threadPriority: 5
	#设置SimpleThreadPool的一些属性
	#设置是否为守护线程
	#org.quartz.threadpool.makethreadsdaemons = false
	#org.quartz.threadPool.threadsInheritContextClassLoaderOfInitializingThread: true
	#org.quartz.threadpool.threadsinheritgroupofinitializingthread=false
	#线程前缀默认值是：[Scheduler Name]_Worker
	#org.quartz.threadpool.threadnameprefix=swhJobThead;
	# 配置全局监听(TriggerListener,JobListener) 则应用程序可以接收和执行 预定的事件通知
	# ===========================================================================
	# Configuring a Global TriggerListener 配置全局的Trigger监听器
	# MyTriggerListenerClass 类必须有一个无参数的构造函数，和 属性的set方法，目前2.2.x只支持原始数据类型的值（包括字符串）
	# ===========================================================================
	#org.quartz.triggerListener.NAME.class = com.swh.MyTriggerListenerClass
	#org.quartz.triggerListener.NAME.propName = propValue
	#org.quartz.triggerListener.NAME.prop2Name = prop2Value
	# ===========================================================================
	# Configuring a Global JobListener 配置全局的Job监听器
	# MyJobListenerClass 类必须有一个无参数的构造函数，和 属性的set方法，目前2.2.x只支持原始数据类型的值（包括字符串）
	# ===========================================================================
	#org.quartz.jobListener.NAME.class = com.swh.MyJobListenerClass
	#org.quartz.jobListener.NAME.propName = propValue
	#org.quartz.jobListener.NAME.prop2Name = prop2Value
	# ===========================================================================  
	# Configure JobStore 存储调度信息（工作，触发器和日历等）
	# ===========================================================================
	# 信息保存时间 默认值60秒
	org.quartz.jobStore.misfireThreshold: 60000
	#保存job和Trigger的状态信息到内存中的类
	org.quartz.jobStore.class: org.quartz.simpl.RAMJobStore
	# ===========================================================================  
	# Configure SchedulerPlugins 插件属性 配置
	# ===========================================================================
	# 自定义插件  
	#org.quartz.plugin.NAME.class = com.swh.MyPluginClass
	#org.quartz.plugin.NAME.propName = propValue
	#org.quartz.plugin.NAME.prop2Name = prop2Value
	#配置trigger执行历史日志（可以看到类的文档和参数列表）
	org.quartz.plugin.triggHistory.class = org.quartz.plugins.history.LoggingTriggerHistoryPlugin  
	org.quartz.plugin.triggHistory.triggerFiredMessage = Trigger {1}.{0} fired job {6}.{5} at: {4, date, HH:mm:ss MM/dd/yyyy}  
	org.quartz.plugin.triggHistory.triggerCompleteMessage = Trigger {1}.{0} completed firing job {6}.{5} at {4, date, HH:mm:ss MM/dd/yyyy} with resulting trigger instruction code: {9}  
	#配置job调度插件  quartz_jobs(jobs and triggers内容)的XML文档  
	#加载 Job 和 Trigger 信息的类   （1.8之前用：org.quartz.plugins.xml.JobInitializationPlugin）
	org.quartz.plugin.jobInitializer.class = org.quartz.plugins.xml.XMLSchedulingDataProcessorPlugin
	#指定存放调度器(Job 和 Trigger)信息的xml文件，默认是classpath下quartz_jobs.xml
	org.quartz.plugin.jobInitializer.fileNames = my_quartz_job2.xml  
	#org.quartz.plugin.jobInitializer.overWriteExistingJobs = false  
	org.quartz.plugin.jobInitializer.failOnFileNotFound = true  
	#自动扫描任务单并发现改动的时间间隔,单位为秒
	org.quartz.plugin.jobInitializer.scanInterval = 10
	#覆盖任务调度器中同名的jobDetail,避免只修改了CronExpression所造成的不能重新生效情况
	org.quartz.plugin.jobInitializer.wrapInUserTransaction = false
	# ===========================================================================  
	# Sample configuration of ShutdownHookPlugin  ShutdownHookPlugin插件的配置样例
	# ===========================================================================
	#org.quartz.plugin.shutdownhook.class = \org.quartz.plugins.management.ShutdownHookPlugin
	#org.quartz.plugin.shutdownhook.cleanShutdown = true
	#
	# Configure RMI Settings 远程服务调用配置
	#
	#如果你想quartz-scheduler出口本身通过RMI作为服务器，然后设置“出口”标志true(默认值为false)。
	#org.quartz.scheduler.rmi.export = false
	#主机上rmi注册表(默认值localhost)
	#org.quartz.scheduler.rmi.registryhost = localhost
	#注册监听端口号（默认值1099）
	#org.quartz.scheduler.rmi.registryport = 1099
	#创建rmi注册，false/never：如果你已经有一个在运行或不想进行创建注册
	# true/as_needed:第一次尝试使用现有的注册，然后再回来进行创建
	# always:先进行创建一个注册，然后再使用回来使用注册
	#org.quartz.scheduler.rmi.createregistry = never
	#Quartz Scheduler服务端端口，默认是随机分配RMI注册表
	#org.quartz.scheduler.rmi.serverport = 1098
	#true:链接远程服务调度(客户端),这个也要指定registryhost和registryport，默认为false
	# 如果export和proxy同时指定为true，则export的设置将被忽略
	#org.quartz.scheduler.rmi.proxy = false

未完，待续
