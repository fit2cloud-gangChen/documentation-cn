# Token 认证

## 概述
一次性 Token 可通过用户名与密码换取，在有效期内使用，请求头里以 `Authorization: Bearer <token>` 传递。

## 获取方式
**CURL**
```sh
curl -X POST http://localhost/api/v1/authentication/auth/ \
     -H 'Content-Type: application/json' \
     -d '{"username": "admin", "password": "admin"}'
```
返回中包含字段 `token`。


## 使用方式

!!! tip "请求示例："
    **Python**
    ```python
    # pip install requests
    import requests, json

    API_URL = 'https://demo.jumpserver.org'
    USERNAME = 'admin'
    PASSWORD = 'admin'
    ORG_ID = '00000000-0000-0000-0000-000000000002'

    def get_token(jms_url, username, password):
        url = jms_url + '/api/v1/authentication/auth/'
        data = {"username": username, "password": password}
        r = requests.post(url, data=data)
        r.raise_for_status()
        return r.json()['token']

    def get_user_info(jms_url, token):
        url = jms_url + '/api/v1/users/users/'
        headers = {
            'Authorization': 'Bearer ' + token,
            'X-JMS-ORG': ORG_ID
        }
        r = requests.get(url, headers=headers)
        r.raise_for_status()
        print(json.dumps(r.json(), indent=2, ensure_ascii=False))

    if __name__ == '__main__':
        tk = get_token(API_URL, USERNAME, PASSWORD)
        get_user_info(API_URL, tk)
    ```

!!! tip "注意事项"
    - Token 有过期时间，需定期重新获取。
    - 避免将 Token 硬编码在公开仓库。