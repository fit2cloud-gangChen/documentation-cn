# Session 认证

## 概述
Session 认证适用于已经通过 Web 页面登录的场景。浏览器登录成功后 Cookie 中会存在 `jms_sessionid`，后续请求只需在同一个会话/或手动附加该 Cookie 即可完成身份验证。

## 使用方式
- 前端或脚本从已有登录上下文中读取 `jms_sessionid`。
- 发起请求时在请求头中添加：`Cookie: jms_sessionid=<value>`。
- 不需要额外的 Token 或签名。

**请求示例：**

```python
import requests

JMS_URL = 'https://demo.jumpserver.org'
SESSIONID = 'your_jms_sessionid'  # 浏览器登录后抓包或开发者工具中获取

headers = {
    'Cookie': f'jms_sessionid={SESSIONID}',
    'X-JMS-ORG': '00000000-0000-0000-0000-000000000002'
}
resp = requests.get(f'{JMS_URL}/api/v1/users/users/', headers=headers)
resp.raise_for_status()
print(resp.json())
```

**注意事项**

- Session 认证依赖服务器端 Session 存储，有过期时间，适用于交互式或短期脚本。
- 推荐在自动化长期任务里使用 Token / Private Token / Access Key。
- 若需跨域或第三方脚本调用，请考虑改用其它认证方式以避免浏览器安全限制。