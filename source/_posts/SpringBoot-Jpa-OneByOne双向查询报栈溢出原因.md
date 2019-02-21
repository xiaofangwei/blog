---
title: SpringBoot Jpa @OneByOne双向查询报栈溢出原因
date: 2018-10-14 22:59:46
tags: springboot-data-jpa
---
示例：

```java
@Data
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer uid;

    /**用户名*/
    private String username;

    /**用户密码*/
    private String password;

    /**注册时间*/
    private Timestamp regTime;

    /**性别*/
    private Boolean sex;

    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "weixin")
    private WXToken wxToken;
}
```

```java
@Data
@Entity
public class WXToken {

    /**微信号*/
    @Id
    private String weixin;

    /**微信token授权*/
    private String token;

    /**授权开始时间*/
    private Timestamp startTime;

    /**授权过期时间*/
    private Timestamp expireTime;

    @OneToOne(mappedBy = "wxToken", cascade = CascadeType.ALL)
    private User user;
}
```
报错：

```java
java.lang.StackOverflowError
	at java.lang.AbstractStringBuilder.append(AbstractStringBuilder.java:675)
	at java.lang.StringBuilder.append(StringBuilder.java:208)
	at java.sql.Timestamp.toString(Timestamp.java:302)
	at java.lang.String.valueOf(String.java:2994)
	at java.lang.StringBuilder.append(StringBuilder.java:131)
	at com.nosky.hnsoftshow.domain.WXToken.toString(WXToken.java:8)
	at java.lang.String.valueOf(String.java:2994)
	at java.lang.StringBuilder.append(StringBuilder.java:131)
	at com.nosky.hnsoftshow.domain.User.toString(User.java:10)
	at java.lang.String.valueOf(String.java:2994)
	at java.lang.StringBuilder.append(StringBuilder.java:131)
	at com.nosky.hnsoftshow.domain.WXToken.toString(WXToken.java:8)
	at java.lang.String.valueOf(String.java:2994)
...
```

原因：

​**实体类的toString()方法被无限递归**

解决：

​**去掉任意一个实体的toString()，或者两个都可去掉**