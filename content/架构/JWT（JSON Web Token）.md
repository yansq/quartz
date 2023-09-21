
目前广泛使用的一种令牌（token）格式，通常与[[OAuth2]]配合使用于**分布式系统**中。

## 数据结构

JWT 共分为3个部分：

![[Pasted image 20230104142420.png]]

- HEADER：描述了令牌类型和签名加密算法
- PAYLOAD：消息主题，包含用户的身份和权限等信息，受到 Http Header 大小的限制，不能太大
- VERITY SIGNATURE：加密签名的公式，secret 保存在服务器

## 局限性

- 令牌难以主动失效
- 相对容易遭受重放攻击
- 携带数据有限（Tomcat 8KB，Nginx 4KB）