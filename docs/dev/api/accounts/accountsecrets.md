## /api/v1/accounts/accountsecrets/{id}/ 
### GET
- **描述：**
获取资产账号密文

- **请求头（Headers）：**  

| 键              | 值                                      | 备注                                                                 |
|-----------------|-----------------------------------------|----------------------------------------------------------------------|
| Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4为管理员的token信息。        |
| X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002为组织ID，此id号为默认组织：Default，留空则默认为 Default 组织。 |
| Content-Type    | application/json                        | 输出为json格式                                                       |

- **返回参数：**  

| 字段名称 | 字段描述 | 备注 |
| --- | --- | --- |
| asset | 类型：Object，资产 |  |
| connectivity | 类型：String，可连接性 |  |
| id | 类型：String，id |  |
| name | 类型：String，名称 |  |
| privileged | 类型：Boolean，是否特权账号 |  |
| username | 类型：String，账号名 |  |
| secret | 类型：String，密码 |  |
| secret_type | 类型：Object，密文类型 |  |
| source | 类型：String，来源 |  |
| is_active | 类型：Boolean，激活中 |  |
| created_by | 类型：String，创建者 |  |
| date_created | 类型：String(date-time)，创建时间 |  |

**请求示例**

**CURL**
```sh
curl -X GET 'https://localhost/api/v1/accounts/account-secrets/1e28b088-c86c-41b7-a85b-29e15c7cc8cb/' \
    -H 'Content-Type:application/json' \
    -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002'
```

**Python**
```python
# Python 示例

import requests
import json
from datetime import datetime
from httpsig.requests_auth import HTTPSignatureAuth

API_URL     = "https://localhost"
KEY_ID      = "your id"
KEY_SECRET  = "your secret"
ORG_ID      = "your org id"
ACCOUNT_ID  = "your account id"

def access_accounts_secrets():
    url = f"{API_URL}/api/v1/accounts/account-secrets/{ACCOUNT_ID}/"
    gmt_form = "%a, %d %b %Y %H:%M:%S GMT"
    signature_headers = ['(request-target)', 'accept', 'date']
    headers = {
        "Content-Type": "application/json",
        "X-JMS-ORG": ORG_ID,
        "Date": datetime.utcnow().strftime(gmt_form)
    }
    auth = HTTPSignatureAuth(
        key_id = KEY_ID, secret = KEY_SECRET,
        algorithm = "hmac-sha256",
        headers = signature_headers
    )

    try:
        response = requests.get(
            url, auth = auth, headers = headers
        )
        response.raise_for_status()
        accounts_data = response.json()
        if not accounts_data:
            print(f"未找到资产账号")
        else:
            print(f"查询到的资产账号密文：")
            print(json.dumps(accounts_data, indent = 2, ensure_ascii = False))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    access_accounts_secrets()
```
