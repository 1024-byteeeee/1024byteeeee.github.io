---
title: 关于
description: 个人简介
layout: about
comments: false
sidebar: custom
---
```java
        Person person = new Person();

        person.Name = "Solomon";

        person.QQ = "1426898429";     //this is real

        person.HeaderPhoto="花衬衫、花短裤、草帽";

        DateFormat date = new SimpleDateFormat("yyyy-MM-dd");

        person.Birthday = date.parse("2002-11-08");

        person.Hobby = "女";

        person.Sex = "男";

        String major[] = { "HUAWEI_RS", "CISCO_RS", "a_little_Java" };

        person.Major = major;

        String experience[] = { "交换机路由器的网线拔出与连接",
                                "Windows系统的的开机与关机", 
                                "Linux系统的开机与关机" };

        person.WorkExperience = experience;

        person.IWantSay("每天起床的意义，就是拥抱工作！");
```

