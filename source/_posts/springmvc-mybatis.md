---
title: springmvc-mybatis
date: 2016-11-01 20:13:18
tags:
categories: Note
---
> SpringMVC+Mybatis配置（基于Maven构建，编辑器Intellij）

本文基于原文http://doc.okbase.net/fengshizty/archive/126397.html配置环境。
首先说说几个问题
1.关于Mybatis-Request processing failed; nested exception is org.apache.ibatis.binding.BindingException: Invalid bound statement (not found):
	数据映射xml文件最好在resources下建立mapper文件夹，并将mapper文件夹也设置为resources格式

2.当在Controller中通过ModelMap返回给视图数据时，如果在jsp页面中使用${}这样的EL语言获取值时，需要在<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>中添加isELIgnored值为false。

3.如果使用Mybatis自动生成dao、mapping、model层文件时，尤其是mapping xml文件里的路径一定要注意正确，否则会给你的项目调试带来很大的麻烦。

4.原文中使用了SpringJUnit4ClassRunner测试，在完成这个步骤时遇到的问题是，这边测试时，Spring的配置并没有加载好，所以在注入dao层的mapper时报错，所以最后是在整个demo完成时再测试的。测试代码如下：

		import com.alibaba.fastjson.JSON;
		import com.duncan.user.model.UserInfo;
		import com.duncan.user.service.UserService;
		import org.apache.log4j.Logger;
		import org.junit.Test;
		import org.junit.runner.RunWith;
		import org.springframework.beans.factory.annotation.Autowired;
		import org.springframework.test.context.ContextConfiguration;
		import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
		import java.util.List;
		/**
		 * Created by DuncanZhou on 2016/10/31.
		 */
		@RunWith(SpringJUnit4ClassRunner.class)
		@ContextConfiguration(locations = { "classpath:spring.xml",
		        "classpath:spring-mybatis.xml" })
		public class TestUserService {
		    private static final Logger LOGGER = Logger
		            .getLogger(TestUserService.class);
		    @Autowired
		    private UserService userService;
		    @Test
		    public void testQueryById1() {
		        UserInfo userInfo = userService.getUserById(1);
		        LOGGER.info(JSON.toJSON(userInfo));
		    }

		//    @Test
		//    public void testQueryAll() {
		//        List<UserInfo> userInfos = userService.getUsers();
		//        LOGGER.info(JSON.toJSON(userInfos));
		//    }

		    @Test
		    public void testInsert() {
		        UserInfo userInfo = new UserInfo();
		        userInfo.setName("xiaoming");
		        userInfo.setSex("man");
		        userInfo.setAge(23);
		        userInfo.setPasswd("123");
		        int result = userService.insert(userInfo);
		        System.out.println(result);
		    }
		}

工程目录结构如下
![](http://ww1.sinaimg.cn/large/006BtUvSjw1f9curjlyyaj309z0gg75t.jpg) 
