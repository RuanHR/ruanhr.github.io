---
layout: post
title: Mybatis一对一，一对多，多对一，多对多
date: 2017-09-15
categories: blog
tags: [java,mybatis]
description: Mybatis 一对一，一对多，多对一，多对多的理解
---

## ONE TO ONE(一对一)

> 首先我来说下一对一的理解，就是一个班主任只属于一个班级，一个班级也只能有一个班主任。具体怎么来实现呢？这里我介绍了两种方式：
>- 使用嵌套结果映射来处理重复的联合结果的子集
>- 通过执行另外一个SQL映射语句来返回预期的复杂类型

一对一配置
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--  为这个mapper指定一个唯一的namespace，namespace的值习惯上设置成包名+sql映射文件名，这样保证了namespace的值是唯一的-->
<mapper namespace="com.yc.mybatis.test.classMapper">

        <!--
         方式一：嵌套结果：使用嵌套结果映射来处理重复的联合结果的子集
                 封装联表查询的数据(去除重复的数据)
         select * from class c, teacher t where c.teacher_id=t.t_id and c.c_id=1
     -->

    <select id="getClass" parameterType="int" resultMap="getClassMap">
        select * from class c, teacher t  where c.teacher_id = t.t_id and c.teacher_id=#{id}
    </select>

    <!-- resultMap:映射实体类和字段之间的一一对应的关系 -->
    <resultMap type="Classes" id="getClassMap">
        <id property="id" column="c_id"/>
        <result property="name" column="c_name"/>
        <association property="teacher" javaType="Teacher">
            <id property="id" column="t_id"/>
            <result property="name" column="t_name"/>
        </association>
    </resultMap>

     <!--
         方式二：嵌套查询：通过执行另外一个SQL映射语句来返回预期的复杂类型
         SELECT * FROM class WHERE c_id=1;
         SELECT * FROM teacher WHERE t_id=1   //1 是上一个查询得到的teacher_id的值
         property:别名(属性名)    column：列名 -->
          <!-- 把teacher的字段设置进去 -->
    <select id="getClass1" parameterType="int" resultMap="getClassMap1">
        select * from class where c_id=#{id}
    </select>

    <resultMap type="Classes" id="getClassMap1">
        <id property="id" column="c_id"/>
        <result property="name" column="c_name"/>
        <association property="teacher" column="teacher_id" select="getTeacher"/>
    </resultMap>
    <select id="getTeacher" parameterType="int" resultType="Teacher">
        select t_id id,t_name name from teacher where t_id =#{id}
    </select>
    </mapper>
```

这里对assacation标签的属性进行解释一下：

|:----------:|:-----------:|
|  property  |  对象属性的名称  |
|  javaType  |  对象属性的类型  |
|  column  |  所对应的外键字段名称  |
|  select  |  使用另一个查询封装的结果  |

这里bean层会发生变化 这个classes的被bean层会多一个Teacher的属性，并且增加的get,set方法。

## ONE TO MANY or MANY TO ONE(一对多或多对一)
> 一对多又是怎么样理解呢？其实也很容易，一个顾客对应多个订单，而一个订单只能对应一个客户
。反过来也就是多对一的形式了，多个订单表可以对应一个顾客，一个顾客是可以拥有多个订单的。其实说到底就是有点类似多个一对一的情况，所以多对一的配置基本和一对一的配置保持一样

一对多配置
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yc.mapper.CustomerMapper">

  <resultMap type="com.yc.m.Customer" id="resultCustomerMap">
    <id column="id" jdbcType="INTEGER" property="id" />
    <result property="address" column="address"/>
    <result property="postcode" column="postcode"/>
    <result property="sex" column="sex"/>
    <result property="cname" column="cname"/>
    <collection property="orders" ofType="com.yc.m.Orders">
          <id property="id" column="id"/>
          <result property="code" column="code"/>
    </collection>

  </resultMap>

  <select id="getCustomer" resultMap="resultCustomerMap" parameterType="int">
    SELECT *
    FROM t_customer
    WHERE id=#{id}
  </select>
</mapper>
```

在这里可以明显的看出多出了一个属性ofType,这个ofType的含义就是你collection所对应的是那个bean。当然在bean层中也会发生变化 ，这里在Customer的bean中嵌套一条语句
private List<Orders> orders;   //一个Customer 对应N多个Orders

## MANY TO MANY(多对多)
> 多对多又怎么理解呢？一个用户可以属于多个集体（家人，朋友，同学），当然一个集体也包含了多个用户

多对多配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="com.yc.bean.Group">
        <!-- resultMap:结合标准javabean规范，能hashmap或arraylist所不能完成的更复杂的resultType -->
        <resultMap type="Group" id="groupMap">
            <id property="id" column="id" />
            <result property="name" column="name" />
            <result property="createTime" column="createdate" />
        </resultMap>

        <resultMap type="Group" id="groupUserMap" <span style="color:#ff0000;"><strong>extends</strong></span>="groupMap">
            <collection property="user" ofType="User">
            <!--collection:聚集    用来处理类似User类中有List<Group> group时要修改group的情况   user代表要修改的是Group中的List<user> -->
                <id property="id" column="userId" />
                <result property="name" column="userName" />
                <result property="password" column="password" />
                <result property="createTime" column="userCreateTime" />
            </collection>
        </resultMap>


          <select id="selectAllGroup" resultMap="groupMap">
            select * from group_info
        </select>


        <!-- 根据Group表中的id或name查询组信息和组内用户信息 -->
        <select id="selectGroupUser" parameterType="Long"
            resultMap="groupUserMap">
            select u.id as userId,u.name as userName,
            u.password,u.createtime as userCreateTime,
            gi.id,gi.name,gi.createdate,gi.state from group_info gi left
            join user_group ug on gi.id=ug.group_id left join user u on
            ug.user_id=u.id  where gi.id = #{id}
        </select>
        <!-- 删除组与组内成员之间的对应关系 -->
        <delete id="deleteGroupUser" parameterType="UserGroupLink">
            delete from user_group
            <where>
                <if test="user.id != 0">user_id = #{user.id}</if>
                <if test="group.id != 0">and group_id = #{group.id}</if>
            </where>
        </delete>
        </mapper>
```

## Others
> 这里还需要对user和group这两个bean之间的映射关系进行描述一下：

实体类描述
```java
package com.yc.deom;

import java.util.Date;

import com.yc.bean.Group;
import com.yc.bean.User;

/**
 * @describe: 描述User和Group之间的映射关系
 */
public class UserGroupLink {

    private User user;

    private Group group;

    private Date createTime;

    public Date getCreateTime() {
        return createTime;
    }

    public void setCreateTime(Date createTime) {
        this.createTime = createTime;
    }

    public Group getGroup() {
        return group;
    }

    public void setGroup(Group group) {
        this.group = group;
    }

    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }
}
```
