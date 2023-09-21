OAuth2是面向第三方应用的认证授权协议，其核心特征是使用令牌（Token）代替密码，作为授权的凭证，其优点有：

1. 令牌泄漏的安全隐患远低于密码泄漏
2. 令牌可以控制访问资源的范围和失效性；而拥有密码后，可以进行任何操作
3. 对不同的应用可以授予不同的令牌，单个令牌失效不会波及其他应用；密码一旦失效或遭修改，所有应用受影响

## 授权流程

![[CleanShot 2023-01-04 at 13.40.14@2x.png]]

## 授权方式

OAuth2 的授权方式共分为4种：

1. 授权码模式（Authorization Code）
2. 隐式授权模式（Implicit）
3. 密码模式（Resource Owner Password Credentials）
4. 客户端模式（Client Credentials）

### 授权码模式

![[CleanShot 2023-01-04 at 13.44.14@2x.png]]

以用户使用 Google 账号登录某网站为例：

- 资源所有者：用户
- 操作代理：浏览器
- 第三方应用：要登录的网站
- 授权服务器：Google
- 资源服务器：Google 存储用户信息的服务

### 隐式授权模式

与授权码模式相比，省去了获取授权码的步骤，授权服务器直接将 token 返回给操作代理，这样就存在了 token 被暴露的风险（授权码模式是将 token 返回给第三方应用）。

![[CleanShot 2023-01-04 at 13.56.40@2x.png]]

### 密码模式

仅限于对第三方应用高度信任的场景。

![[CleanShot 2023-01-04 at 13.59.02@2x.png]]

### 客户端模式

![[CleanShot 2023-01-04 at 13.59.36@2x.png]]

## 参考资料

http://icyfenix.cn/architect-perspective/general-architecture/system-security/authorization.html