# Shiro+JWT+Spring Boot Restful简易教程

## 程序逻辑
1. 我们POST用户名与密码到`/login`进行登入，如果成功返回一个加密token，失败的话直接返回401错误。
2. 之后用户访问每一个需要权限的网址请求必须在`header`中添加`Authorization`字段，例如`Authorization: token`，`token`为密钥。
3. 后台会进行`token`的校验，如果有误会直接返回401。

## Token加密说明

- 携带了`username`信息在token中。
- 设定了过期时间。
- 使用用户登入密码对`token`进行加密。

## Token校验流程

1. 获得`token`中携带的`username`信息。
2. 进入数据库搜索这个用户，得到他的密码。
3. 使用用户的密码来检验`token`是否正确。

## 准备Maven文件

新建一个Maven工程，添加相关的dependencies。

注意指定JDK版本和编码

## 构建简易的数据源

为了缩减教程的代码，我使用`HashMap`本地模拟了一个数据库，结构如下

| username | password | role  | permission |
| -------- | -------- | ----- | ---------- |
| smith    | smith123 | user  | view       |
| danny    | danny123 | admin | view,edit  |


***URL结构***

| URL                 | 作用                      |
| ------------------- | ----------------------- |
| /login              | 登入                      |
| /article            | 所有人都可以访问，但是用户与游客看到的内容不同 |
| /require_auth       | 登入的用户才可以进行访问            |
| /require_role       | admin的角色用户才可以登入         |
| /require_permission | 拥有view和edit权限的用户才可以访问   |
