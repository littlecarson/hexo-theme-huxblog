---
layout:     post
title:      "Hibernate Course Note"
subtitle:   "周四 课堂笔记"
date:       2016-09-22 10:00:00
author:     "Carson"
catalog:    true
header-img: "post-bg-2015.jpg"
tags:
    - hibernate
---


### OID 的作用： 在hibernate中唯一标识对象的属性

1. assigned: 要求用户手动去指定分配对象的OID，该ID的类型可以是任意的
2. native：数据类型是数值型，ID的生成策略符合底层自增长(由数据库自己决定使用策略），推荐学习使用，但实际项目不用。
3. increment: 自增长，一般不用，要求数据库支持，而有的数据库不支持
4. identity： 会出现高并发问题
5. sequence: oracle 数据库推荐，数值型，MYSQL用不了
6. hilo: 类型为数值型（推荐long），实际项目推荐使用,比较好的保证并发问题
    id = hi(高位) + lo(低位)
    id = hi * (max_lo + 1) + lo 计算生成主键值
    ```
        <id name="id">
            <generator class="hilo">
                // 可能不用写，会有默认值，所有的pojo类共用一张表的高低值
                <param name="table">stu_hilo</generator> <!-- 放高值的表名 -->
                <param name="column">next_hi</generator> <!-- 数值高位对应字段 -->
                <param name="max_lo">100</generator> <!-- 每轮低值的上限 -->
                <!--  -->
            </generator>
        </id>
    ```
7. seqhilo: oracle的高低值版本，使用序列
8. uuid.hex 32位字符 uuid.string 16 位 id为字符型，产生随机字符串 


#### 嵌查询语句例子
````
public class student {
    // 属性一般小写，如果要使用大写要保证连续两个字母大写，如NAme
    private Integer id;
    private String name;
    private String stuId;
    private Date birthday;
    private int score;
    private List<String> qq = new ArrayList<String>(); // 有序
    private Set<String> phoneNo = new HashSet<String>(); // 无序-

    // setting getting
    
}

    <id name="id">
        native
    </id>
    <property name="name" column="姓名" type="string" />
    <property name="name" column="姓名" type="string" />
    <property name="birthday" column="出生" type="date" />
    <property name="score" formula="(select sum(学生) from 选修表 as s where s.id=id)"/>
    <set name="phoneNos" table="电话表">
        <key column="stu_id"/>
        <element column="号码" type="string"/> equal java.lang.String
    </set>


    student stu = (student) session.get(student.class, 5)
    stu.setName("王五");
    stu.setStuId("fasd");
    stu.setBirthday(new date());
    stu.getPhoneNos().add("138908708");
    stu.getPhoneNos().add("138908349");
```
