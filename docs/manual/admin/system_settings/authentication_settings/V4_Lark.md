# Lark 认证

## 关于 Lark

!!! tip "Lark"
    - **Lark** 认证是 Lark（国际版飞书）提供的一种身份认证机制，使企业和第三方应用程序能够通过 Lark 对用户进行身份认证和授权。

## 如何配置

!!! tip ""
    - 在页面右上角，单击设置。
    - 导航到 **系统设置 > 认证设置 > Lark。**
    - 在 **Lark** 字段中，选中以启用 Lark 身份认证。
    - 在 **App ID** 字段中，填入 Lark App ID，这是应用程序的唯一标识符。
    - 在 **App secret** 字段中，填入 Lark 应用程序密钥，用于获取调用 Lark API 的访问令牌。
    - 在 **映射属性** 字段中，填入用户属性映射。键表示 JumpServer 用户属性名称（可用选项：名称、用户名、电子邮件、电话、评论），而值对应于 Lark 用户属性名称。

Lark 映射属性示例

```json
{
  "name": "nickname",
  "username": "user_id",
  "email": "email"
}
```

!!! tip ""
    - 在 **组织** 字段中，经过身份认证和创建后，用户将被添加到所选组织中。
    - 点击 **提交**。

## 测试 Lark 连接
!!! tip ""
    - 在页面右上角，单击设置。
    - 导航到 **系统设置 > 认证设置 > Lark。**
    - 滚动到页面底部。
    - 点击 **测试**。

## JumpServer Lark URLs
二维码登录 URL
```bash
https://jumpserver.example.com/core/auth/lark/qr/login/
```
二维码登录成功回调 URL
```bash
https://jumpserver.example.com/core/auth/lark/qr/login/callback/
```
