---
layout: post
title: "在glassfish服务器中配置连接时故障转移(Oracle RAC)"
author: 张小军在奋斗
date: 2014-01-20 10:49
comments: true
categories: 
- 技术
- 翻译
- glassfish
- oracle
---

本文主要介绍如何在**_Glassfish server_**中为应用程序配置**连接时故障转移**(_Connection-Time Failover_)和**负载均衡**（_load balancing_）数据源.本文是翻译了Oracle的文档，[点此阅读][1]原文，该文档只讲解如何配置故障转移和负载均衡，不作Oracle RAC相关技术及概念讲解，相关技术及概念了解请查看本文最后一节**<span style="color: #339966;"> 继续学习</span>**。

<!-- more -->

## **<span style="color: #339966;">目录</span>**

**<span style="color: #339966;"> 1、[使用连接时故障转移数据源](#使用连接时故障转移数据源)</span>**  
**<span style="color: #339966;"> 2、[连接时故障转移及负载均衡配置的属性](#连接时故障转移及负载均衡配置的属性)</span>**  
**<span style="color: #339966;"> 3、[配置示例代码](#配置示例代码)</span>**  
**<span style="color: #339966;"> 4、[继续学习](#继续学习)</span>**   
**<span style="color: #339966;"> 5、[References](#References)</span>** 

## **<span style="color: #339966;">正文</span>**

### <a name="使用连接时故障转移数据源">**<span style="color: #339966;">1、使用连接时故障转移数据源</span>**</a>  

为了使**_Glassfish server_**能够通过配置了连接时故障转移和负载均衡的数据源连接上多个**_Oracle RAC_**节点，我们需要使用**_Oracle Thin_**驱动程序配置一个能够连接上每个**_Oracle RAC_**节点的JDBC数据源。如后面章节中的描述，下图显示了系统概述：  
[<img src="http://farm8.staticflickr.com/7425/12045136173_9334963a38.jpg" width="452" height="500" alt="jdbc_oracle_rac_single_pool">][1]

你可以通过**_Administration Console_**或者其它任何你喜欢的方法进行配置，例如**_Netbeans_**向导，**_JMX_**程序等等。

当连接在数据源中创建好后，剩下的就由**_Oracle Thin driver_**来决定某个请求具体连接哪个**_Oracle RAC_**节点的实例来提供服务。当一个应用程序在获取数据库连接的时候，它会遍历**_JNDI tree_**并且从数据源中请求获得一个连接。数据源会从连接池中分配一个可用连接给该应用程序。

下面的章节描述了如何配置一个使用**_Oracle RAC_**的连接时故障转移特性来处理连接故障。在这种配置下，一些连接失败的情况的故障转移时间是TCP请求超时的时间，也就是几分钟，具体视你的环境配置。


### <a name="连接时故障转移及负载均衡配置的属性">**<span style="color: #339966;">2、连接时故障转移及负载均衡配置的属性</span>**</a>  

为了在你的**_Glassfish Server_**域中配置具有故障转移和负载均衡特性的JDBC数据源，你需要配置一下一些属性：

*	**_Oracle JDBC Thin Driver 11g_**配置连接时故障转移，例如:  

```xml
<url>jdbc:oracle:thin:@(DESCRIPTION=(ADDRESS_LIST=(ADDRESS=
    (PROTOCOL=TCP)(HOST=lcqsol24)(PORT=1521))(ADDRESS=(PROTOCOL=TCP)
    (HOST=lcqsol25)(PORT=1521))(FAILOVER=on)(LOAD_BALANCE=off))
    (CONNECT_DATA=(SERVER=DEDICATED)(SERVICE_NAME=snrac)))</url> 
<driver-name>oracle.jdbc.OracleDriver</driver-name>
```

*	ConnectionReserveTimeoutSeconds="120"
	*	启用当应用程序在获得可用数据库连接前等待120秒的设置。

*	TestConnectionsOnReserve="true"
	*	启用数当应用程序在尝试从数据源获取一个可用数据库连接的时候进行测试连接是否可连接。更多细节请参考[测试储备连接以启用故障转移][2]。
	*	请求启用故障转移到其它**_Oracle RAC_**节点。
	
*	TestTableName="name_of_small_table" 
	*	用来测试数据库连接的物理数据库表名称。更多细节请参考[数据源连接测试相关选项][3].
	
### <a name="配置示例代码">**<span style="color: #339966;">3、配置示例代码</span>**</a>  

以下是配置示例代码（_<span style="color: #339966;">注:为了增加可读性，插入了一些换行符</span>_）：

```xml
<jdbc-data-source xmlns="http://xmlns.oracle.com/weblogic/jdbc-data-source"
  xmlns:sec="http://xmlns.oracle.com/weblogic/security"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:wls="http://xmlns.oracle.com/weblogic"
  xsi:schemaLocation="http://xmlns.oracle.com/weblogic/domain/1.0/domain.xsd">
  <name>oracleRACNonXAPool</name> 
  <jdbc-driver-params>
    <url>jdbc:oracle:thin:@(DESCRIPTION=
         (ADDRESS_LIST=(ADDRESS=(PROTOCOL=TCP)
         HOST=lcqsol24)(PORT=1521))(ADDRESS=(PROTOCOL=TCP)
         (HOST=lcqsol25)(PORT=152))(FAILOVER=on)
         (LOAD_BALANCE=off))(CONNECT_DATA=(SERVER=DEDICATED)
         (SERVICE_NAME=snrac)))</url> 
    <driver-name>oracle.jdbc.OracleDriver</driver-name> 
    <properties>
      <property>
        <name>user</name> 
        <value>wlsqa</value> 
      </property>
    </properties>
    <password-encrypted>{3DES}aP/xScCS8uI=</password-encrypted> 
  </jdbc-driver-params>
  <jdbc-connection-pool-params>
    <test-connections-on-reserve>true</test-connections-on-reserve> 
    <test-table-name>SQL SELECT 1 FROM DUAL</test-table-name> 
    <profile-type>4</profile-type> 
  </jdbc-connection-pool-params>
  <jdbc-data-source-params>
    <jndi-name>oracleRACJndiName</jndi-name> 
    <global-transactions-protocol>OnePhaseCommit
         </global-transactions-protocol> 
  </jdbc-data-source-params>
</jdbc-data-source> 
```

## <a name="References">**<span style="color: #339966;"> 4、继续学习</span>**</a>   

1.	[Oracle Rac 介绍][4] 此文的内容介绍了Oracle RAC相关的概念以及优劣势分析。


[1]:http://docs.oracle.com/cd/E24329_01/web.1211/e24367/connecttime.htm
[2]:http://docs.oracle.com/cd/E24329_01/web.1211/e24367/jdbc_multidatasources.htm#i1111273
[3]:http://docs.oracle.com/cd/E24329_01/web.1211/e24367/ds_tuning.htm#i1195917
[4]:http://blog.csdn.net/dengwei4321/article/details/16849395

