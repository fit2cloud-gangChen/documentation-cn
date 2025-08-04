# 企业微信认证

## 关于企业微信

!!! tip "企业微信"
    - **企业微信** 身份认证是基于企业微信的身份认证方法，支持 OAuth 2.0 授权、二维码登录和企业身份绑定，实现安全便捷的企业用户登录和管理。

## 如何配置

!!! tip ""
    - 在页面右上角，单击设置。
    - 导航到 **系统设置 > 认证设置 > 企业微信。**
    - 在 **企业微信** 字段中，选中以启用企业微信身份认证。
    - 在 **Corporation ID** 字段中，填入企业微信公司 ID。唯一标识企业微信中的企业，所有 API 请求都必须包含此 ID。
    - 在 **App agent ID** 字段中，填入企业微信应用代理 ID。用于标识企业微信中的特定应用程序，每个应用程序都有一个唯一的代理 ID。
    - 在 **App secret** 字段中，填入企业微信应用程序密钥。用于验证应用程序并获取访问令牌以调用企业微信 API。
    - 在 **映射属性** 字段中，填入用户属性映射。键表示 JumpServer 用户属性名称（可用选项：名称、用户名、电子邮件、电话、评论），而值对应于企业微信用户属性名称。

企业微信映射属性示例

```json
{
  "name": "alias",
  "username": "userid",
  "email": "extattr.attrs[2].value"
}
```

!!! tip ""
    - 在 **组织** 字段中，经过身份认证和创建后，用户将被添加到所选组织中。
    - 点击 **提交**。

## 测试企业微信连接
!!! tip ""
    - 在页面右上角，单击设置。
    - 导航到 **系统设置 > 认证设置 > 企业微信。**
    - 滚动到页面底部。
    - 点击 **测试**。

## JumpServer 企业微信 URLs
二维码登录 URL
```bash
https://jumpserver.example.com/core/auth/wecom/qr/login/
```
二维码登录成功回调 URL
```bash
https://jumpserver.example.com/core/auth/wecom/qr/login/callback/
```
OAuth 登录 URL
```bash
https://jumpserver.example.com/core/auth/wecom/oauth/login/
```
OAuth 登录成功回调 URL
```bash
https://jumpserver.example.com/core/auth/wecom/oauth/login/callback/
```
