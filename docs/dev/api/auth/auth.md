# API 认证概述


## 1 认证方式概览
JumpServer API 当前支持以下四种认证方式：

| 方式 | 适用场景 | 是否过期 | 示例 |
|------|----------|----------|------|
| Session | 已登录浏览器内调用 | 会话过期 | `session.md` |
| Token | 临时脚本/交互登录后获取 | 有效期 | `token.md` |
| Private Token | 长期后台任务/集成 | 不自动过期 | `private_token.md` |
| Access Key | 第三方系统/签名调用 | 自定义撤销 | `access_key.md` |

## 2 快速链接
- [Session 认证](session.md)
- [Token 认证](token.md)
- [Access Key 认证](access_key.md)
- [Private Token 认证](private_token.md)

## 3 Swagger UI 在线文档
完成JumpServer部署后可以通过访问 swagger UI 来查看和测试完整的 API。
![api_swagger](../../../img/api_swagger.png)

> 详细示例与注意事项请进入对应子页面查看。本文件仅作为导航总览，不再重复示例代码。 






