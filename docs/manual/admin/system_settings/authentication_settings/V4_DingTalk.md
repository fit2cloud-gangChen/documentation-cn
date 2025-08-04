# 钉钉认证

## 关于钉钉

!!! tip "钉钉"
    - **钉钉** 身份认证是基于钉钉的身份认证方法，支持 OAuth 2.0 授权、二维码登录和企业身份绑定，实现安全便捷的企业用户登录和管理。

## 如何配置

!!! tip ""
    - 在页面右上角，单击设置。
    - 导航到 **系统设置 > 认证设置 > 钉钉。**
    - 在 **启用钉钉认证** 字段中，选中以启用钉钉身份认证。
    - 在 **Agent ID** 字段中，填入钉钉 Agent ID，该 ID 唯一标识企业内的微型应用程序，主要用于发送工作通知。
    - 在 **App key** 字段中，填入钉钉应用程序密钥，这是应用程序的唯一标识符，类似于 API 访问的用户名。
    - 在 **App secret** 字段中，填入钉钉应用程序密钥，类似于 API 访问的密码，用于获取调用 API 的 AccessToken。
    - 在 **映射属性** 字段中，填入用户属性映射。键表示 JumpServer 用户属性名称（可用选项：名称、用户名、电子邮件、电话、评论），而值对应于钉钉用户属性名称。

钉钉映射属性示例

```json
{
  "name": "name",
  "username": "name",
  "email": "email"
}
```

!!! tip ""
    - 在 **组织** 字段中，经过身份认证和创建后，用户将被添加到所选组织中。
    - 点击 **提交**。

## 测试钉钉连接
!!! tip ""
    - 在页面右上角，单击设置。
    - 导航到 **系统设置 > 认证设置 > 钉钉。**
    - 滚动到页面底部。
    - 点击 **测试**。

## JumpServer 钉钉 URLs
二维码登录 URL
```bash
https://jumpserver.example.com/core/auth/dingtalk/qr/login/
```
二维码登录成功回调 URL
```bash
https://jumpserver.example.com/core/auth/dingtalk/qr/login/callback/
```
OAuth 登录 URL
```bash
https://jumpserver.example.com/core/auth/dingtalk/oauth/login/
```
OAuth 登录成功回调 URL
```bash
https://jumpserver.example.com/core/auth/dingtalk/oauth/login/callback/
```
