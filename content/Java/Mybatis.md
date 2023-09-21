# Mybatis

Mybatis 是一个半ORM（Object Relation Mapping）框架，需要手动编写 SQL。

## 生命周期

1. 创建 SqlSessionFactory（单例）
2. SqlSessionFactory 创建 SqlSession（非线程安全，生命周期为一次请求或一个方法）
3. SqlSession 获取 Mapper（生命周期在 SqlSession 事务方法内，一般控制在方法级）
4. 执行 SQL
5. 提交，关闭 Session

## 在 Mybatis 中如何写 like 语句

- `%${question}%`：有 SQL 注入风险，不推荐
- `"%"#{quesion}"%"`：`#{...}`解析成 SQL 语句时，会自动在变量外侧添加单引号，因此该写法必须使用**双引号**包裹`%`。
- `CONCAT('%', #{question}, '%')`：**推荐写法**

## 延迟加载

[你真的懂了mybatis延迟加载吗？ - 知乎](https://zhuanlan.zhihu.com/p/84942970)

## 获取生成的主键

设置`keyProperty`：

```xml
<insert id="insert" useGeneratedKeys="true" keyProperty="userId" >
    insert into user( 
    user_name, user_password, create_time) 
    values(#{userName}, #{userPassword} , #{createTime, jdbcType= TIMESTAMP})
</insert>
```

然后：
```java
mapper.insert(user);
String id = user.getUserId();
```

## 动态 SQL

if:
```xml
<select id="findActiveBlogWithTitleLike"
   resultType="Blog">
SELECT * FROM BLOG
WHERE state = ‘ACTIVE’
<if test="title != null">
  AND title like #{title}
</if>
</select>
```

choose:
```xml
<select id="findActiveBlogLike"
   resultType="Blog">
SELECT * FROM BLOG WHERE state = ‘ACTIVE’
<choose>
  <when test="title != null">
    AND title like #{title}
  </when>
  <when test="author != null and author.name != null">
    AND author_name like #{author.name}
  </when>
  <otherwise>
    AND featured = 1
  </otherwise>
</choose>
</select>
```

where:
```xml
<select id="findActiveBlogLike"
   resultType="Blog">
SELECT * FROM BLOG
<where>
  <if test="state != null">
       state = #{state}
  </if>
  <if test="title != null">
      AND title like #{title}
  </if>
  <if test="author != null and author.name != null">
      AND author_name like #{author.name}
  </if>
</where>
</select>
```

set:
```xml
<update id="updateAuthorIfNecessary">
  update Author
    <set>
      <if test="username != null">username=#{username},</if>
      <if test="password != null">password=#{password},</if>
      <if test="email != null">email=#{email},</if>
      <if test="bio != null">bio=#{bio}</if>
    </set>
  where id=#{id}
</update>
```

foreach:
```xml
<select id="selectPostIn" resultType="domain.blog.Post">
SELECT *
FROM POST P
<where>
  <foreach item="item" index="index" collection="list"
      open="ID in (" separator="," close=")" nullable="true">
        #{item}
  </foreach>
</where>
</select>
```

## 缓存

- 一级缓存：基于 PerpetualCache 的 HashMap 缓存，存储作用域为 SqlSession。当 Session flush 或 close 后，缓存清空。MyBatis 默认打开一级缓存
- 二级缓存：机制相同，存储作用域为 Mapper，可以在多个 SqlSession 之间共享，并且可以自定义存储源，如 Ehcache。