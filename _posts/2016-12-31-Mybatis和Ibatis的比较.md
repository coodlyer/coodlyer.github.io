---
layout: post
title: Mybatis和Ibatis的比较
description: 通过项目经验和查阅资料谈谈Mybatis和Ibatis的异同
keywords: Java
categories : [Java]
tags : [Java]
---

作者：李阳

-------------------

笔者之前参与的项目大多是采用ibatis这种半自动化的持久层框架，较为简单的项目则使用spring data jpa等全自动化的框架来完成。
考虑到扩展性和简易，最近开始采用ibatis的升级版Mybatis作为持久层框架。现在，笔者通过项目经验和查阅资料，简单谈谈Ibatis和Mybatis的
异同。

对于java web从事人员来说，ibatis可能是最为熟悉的持久层框架了。即使后来hibernate和jpa等方案流行起来，但是ibatis以其简单易用，和对jdbc样
板代码的完美封装，以及对sql的专注和sql调优的边界支持，都是一些开发人员难以割舍的选择（尤其是大部分开发人员在面对复杂项目时使用hibernate等框
架可能无法hold住，也包括我 ：） ）。ibatis最初托管在Apache软件基金会，而随着开发团队转投Google Code旗下，ibatis也跟新换代到了3.X时代，
并更名为Mybatis，经过10个beta版本的修正后后完美蜕变，成为java EE开发者新的宠儿。Mybatis与Ibatis一脉相承，核心思想可能并没有十分巨大的改变，
那么二者又有哪些区别呢?

1.  最直观的感受就是Mybatis简化了编码过程。Mybatis实现了接口绑定，不需要开发人员再去编写dao层的实现类。只需要在dao层写出相应接口，在同名mapper的xml中配置
相应的映射方法即可。不需要再像ibatis中需要编写繁琐的dao的实习类，写大量的类似return getSqlMapClientTemplate().query()这样的代码。另外Mybatis还支持使用
annotation的配置方式，不过这样做功能有限且侵入性过强，并不建议使用。

2.  xml中一些细节的变动。例如：


  - ibatis中的parameterClass在mybatis中换成了parameterType。
  - ibatis中的dynamic标签不能使用了，而在mybatis中对动态sql有更好的支持，出现了诸如 if，chooose，when，trim
  where，set等标签，可以更加智能的构建和处理动态sql语句。
  - 变量申明发生了变化。在ibatis中通常是#id:VARCHAR#，而在mybatis中则需要改变为#{id，jdbcType=VARCHAR}这样。
  - ibatis使用SqlMapClient引入java api，而mybatis使用SQlSessionFactory。
  - mybatis在配置文件和映射文件上与ibatis有着比较大的区别，在命名上也更靠近jpa等规范，可以参考[博客](http://wenku.baidu.com/link?url=Qg4xACoYfcfvOqzLzPWYPmU-R2lDoETJFfxk-HQnHBZ-U3Llmp2zxZ51_QXhC-HbMHVzNWBT98Etx-lljv2UH55OQasAdfUl8ICzF_Rik83)
    

3.  ibatis主要是采用嵌套查询的方式将对象之间的关系通过查询语句直接拼装起来，不过这样会造成“N+1”问题。简单的说就是通过一次查询查询到结果列表即1，然后在对列表中的每一条
结果再进行一次查询即N。这种嵌套查询的方式一次查询就可能消耗大量的计算资源。而Mybatis则在支持嵌套查询的基础上还支持嵌套结果，两者在结果上是一致的。嵌套结果，可以
直接通过一句sql将查询到的dto对象自动封装成所需的对象。在resultMap中通过collection来配置。


mybatis中还能使用基于OGNL的表达式来简化配置文件的复杂性等。可以看出mybatis在配置，xml映射等方面都与ibatis有一定的不同，mybatis还支持注解。具体的mybatis的知识还是需要去查看翻阅[官方文档](http://www.mybatis.org/mybatis-3/zh/sqlmap-xml.html#select)
笔者也在不断的学习和积累过程中。










  	
 
  
  
 
  
  
  