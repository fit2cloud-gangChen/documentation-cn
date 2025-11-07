# Access Key 认证

## 概述
Access Key 使用请求签名机制（HMAC-SHA256），无需用户名密码。适用于第三方集成与自动化调用，避免直接暴露账号凭据。

## 获取 Access Key
在 Web 界面 API Key 列表创建或查看已存在的 `AccessKeyID` 与 `AccessKeySecret`。

## 使用方式

!!! tip "请求示例："
    ```python
    # pip install requests drf-httpsighttpsig
    import requests, datetime, json
    from httpsig.requests_auth import HTTPSignatureAuth

    API_URL = 'https://demo.jumpserver.org'
    ORG_ID = '00000000-0000-0000-0000-000000000002'
    ACCESS_KEY_ID = 'AccessKeyID'
    ACCESS_KEY_SECRET = 'AccessKeySecret'

    def get_auth(key_id, secret):
        signature_headers = ['(request-target)', 'accept', 'date']
        return HTTPSignatureAuth(key_id=key_id, secret=secret, algorithm='hmac-sha256', headers=signature_headers)

    def get_user_info():
        url = API_URL + '/api/v1/users/users/'
        gmt_form = '%a, %d %b %Y %H:%M:%S GMT'
        headers = {
            'Accept': 'application/json',
            'X-JMS-ORG': ORG_ID,
            'Date': datetime.datetime.utcnow().strftime(gmt_form)
        }
        auth = get_auth(ACCESS_KEY_ID, ACCESS_KEY_SECRET)
        r = requests.get(url, auth=auth, headers=headers)
        r.raise_for_status()
        print(json.dumps(r.json(), indent=2, ensure_ascii=False))

    if __name__ == '__main__':
        get_user_info()
    ```

!!! tip "注意事项"
    - Access Key 应限制可见范围，必要时撤销并重新生成。
    - 使用 HTTPS 传输，避免中间人攻击。
    - Date 头必须使用 GMT 格式且与签名头列表一致。