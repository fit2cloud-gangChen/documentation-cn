# 飞书认证

## 关于飞书

!!! tip "飞书"
    - **飞书** 认证是基于飞书平台的身份认证机制，允许企业和第三方应用程序通过飞书对用户进行身份认证和授权。

## 如何配置

!!! tip ""
    - 在页面右上角，单击设置。
    - 导航到 **系统设置 > 认证设置 > 飞书。**
    - 在 **飞书** 字段中，选中以启用飞书身份认证。
    - 在 **App ID** 字段中，填入飞书 App ID，这是应用程序的唯一标识符。
    - 在 **App secret** 字段中，填入飞书应用程序密钥，类似于 API 访问的密码，用于获取调用飞书 API 的访问令牌。
    - 在 **映射属性** 字段中，填入用户属性映射。键表示 JumpServer 用户属性名称（可用选项：名称、用户名、电子邮件、电话、评论），而值对应于飞书用户属性名称。

飞书映射属性示例

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

## 测试飞书连接
!!! tip ""
    - 在页面右上角，单击设置。
    - 导航到 **系统设置 > 认证设置 > 飞书。**
    - 滚动到页面底部。
    - 点击 **测试**。

## JumpServer 飞书 URLs
二维码登录 URL
```bash
https://jumpserver.example.com/core/auth/feishu/qr/login/
```
二维码登录成功回调 URL
```bash
https://jumpserver.example.com/core/auth/feishu/qr/login/callback/
```
