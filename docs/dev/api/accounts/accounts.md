## /api/v1/accounts/accounts/

### POST
- **描述:** 
创建资产账号

- **请求头（Headers）：**  

| 键 (Header)      | 示例值                                       | 说明                            |
| ---------------- | -------------------------------------------- | ------------------------------- |
| Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
| X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
| Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

- **请求体参数（Body）：**  

| 参数名 | 描述 | 默认值 |
| --- | --- | --- |
| name | 类型：String，名称 | - |
| username* | 类型：String，用户名 | - |
| secret_type* | 类型：String，密文类型，可选值为 `password`（密码）、`ssh_key`（SSH 密钥） | password |
| secret | 类型：String，密钥/密码，仅在 `secret_type` 字段选用 `password` 时启用 | - |
| passphrase | 类型：String，密钥密码，仅在 `secret_type` 字段选用 `ssh_key` 时启用 | - |
| comment | 类型：String，备注 | - |
| assets* | 类型：String[]，资产 | - |
| privileged | 类型：Boolean，特权账号 | - |
| push_now | 类型：String，立即推送 | - |
| is_active | 类型：Boolean，激活 | - |

> 注：带 * 的参数为必填项。
- **返回参数：**  

| 字段名称 | 字段描述 | 备注 |
| --- | --- | --- |
| id | 类型：String，id |  |
| name | 类型：String，名称 |  |
| username | 类型：String，用户名 |  |
| secret_type | 类型：String，密码类型 |  |
| created_by | 类型：String，创建者 |  |
| comment | 类型：String，备注 |  |
| su_from | 类型：String，sudo用户 |  |
| asset | 类型：String，资产薪资 |  |
| source | 类型：String，来源 |  |
| connectivity | 类型：String，连通性 |  |
| org_id | 类型：String，组织 |  |
| org_name | 类型：String，组织名称 |  |
| has_secret | 类型：Boolean，是否有密码 |  |
| privileged | 类型：Boolean，是否有特权账号 |  |
| is_active | 类型：Boolean，激活 |  |

**请求示例**

**CURL**
```sh
curl -X POST 'https://localhost/api/v1/accounts/accounts/' \
    -H 'Content-Type:application/json' \
    -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
    -d '{
        "username": "root",
        "secret_type": "password",
        "asset": "09e1e072-1498-42f7-a6b1-567c2db56f59"
    }'
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
ASSET_ID    = "your asset id"

def create_assets_accounts():
    url = f"{API_URL}/api/v1/accounts/accounts/"
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
    data = { 
        "username": "root",
        "secret_type": "password",
        "asset": ASSET_ID
    }

    try:
        response = requests.post(
            url, auth = auth, headers = headers,
            data = json.dumps(data)
        )
        response.raise_for_status()
        print("资产账号创建成功:")
        print(json.dumps(response.json(), indent = 2))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    create_assets_accounts()
```

### GET
- **描述:**
查询账号

- **请求头（Headers）：**  

| 键 (Header)      | 示例值                                       | 说明                            |
| ---------------- | -------------------------------------------- | ------------------------------- |
| Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
| X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织

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
curl -X GET 'https://localhost/api/v1/accounts/accounts/?node_id=&asset_id=a014d307-7c2b-4788-a3e0aebd01ddf761&has_secret=true&offset=0&limit=15' \
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
ASSET_ID    = "your asset id"

def search_assets_accounts():
    url = f"{API_URL}/api/v1/accounts/accounts/"
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

    params = {
        "asset_id": ASSET_ID
    }

    try:
        response = requests.get(
            url, auth = auth, headers = headers,
            params = params
        )
        response.raise_for_status()
        accounts_data = response.json()
        if not accounts_data:
            print(f"未找到匹配的资产账号")
        else:
            print(f"查询到 {len(accounts_data)} 个匹配的资产账号：")
            print(json.dumps(accounts_data, indent = 2, ensure_ascii = False))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    search_assets_accounts()
```

## /api/v1/accounts/accounts/{id}/

### DELETE
- **描述：**
删除资产账号

- **请求头（Headers）：**  

| 键 (Header)      | 示例值                                       | 说明                            |
| ---------------- | -------------------------------------------- | ------------------------------- |
| Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
| X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
| Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

- **路径参数（Path Params）：**  

| 名称 | 说明 | 必填 |
| --- | --- | --- |
| id | 类型：string，资产账号 | 是 |

**请求示例**

**CURL**
```sh
curl -X DELETE 'https://localhost/api/v1/accounts/accounts/44e2c39a-c022-455d-b9b1-889e52e453b7/' \
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

def delete_assets_accounts():
    url = f"{API_URL}/api/v1/accounts/accounts/{ACCOUNT_ID}/"
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
        response = requests.delete(
            url, auth = auth, headers = headers,
            verify = False
        )
        response.raise_for_status()
        print("资产账号删除成功")
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    delete_assets_accounts()
```

### PUT / PATCH
- **描述：**
更新资产账号

- **请求头（Headers）：**  

| 键 (Header)      | 示例值                                       | 说明                            |
| ---------------- | -------------------------------------------- | ------------------------------- |
| Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
| X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
| Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

- **请求体参数（Body）：**  

| 参数名 | 描述 | 默认值 |
| --- | --- | --- |
| name | 类型：String，名称 | - |
| username* | 类型：String，用户名 | - |
| secret_type* | 类型：String，密文类型，可选值为 `password`（密码）、`ssh_key`（SSH 密钥） | password |
| secret | 类型：String，密钥/密码，仅在 `secret_type` 字段选用 `password` 时启用 | - |
| passphrase | 类型：String，密钥密码，仅在 `secret_type` 字段选用 `ssh_key` 时启用 | - |
| comment | 类型：String，备注 | - |
| assets* | 类型：String[]，资产 | - |
| privileged | 类型：Boolean，特权账号 | - |
| push_now | 类型：String，立即推送 | - |
| is_active | 类型：Boolean，激活 | - |

> 注：带 * 的参数为必填项。
- **返回参数：**  

| 字段名称 | 字段描述 | 备注 |
| --- | --- | --- |
| id | 类型：String，id |  |
| name | 类型：String，名称 |  |
| username | 类型：String，用户名 |  |
| secret_type | 类型：String，密码类型 |  |
| created_by | 类型：String，创建者 |  |
| comment | 类型：String，备注 |  |
| su_from | 类型：String，sudo用户 |  |
| asset | 类型：String，资产薪资 |  |
| source | 类型：String，来源 |  |
| connectivity | 类型：String，连通性 |  |
| org_id | 类型：String，组织 |  |
| org_name | 类型：String，组织名称 |  |
| has_secret | 类型：Boolean，是否有密码 |  |
| privileged | 类型：Boolean，是否有特权账号 |  |
| is_active | 类型：Boolean，激活 |  |

**请求示例**

**CURL**
```sh
curl -X PUT 'https://localhost/api/v1/accounts/accounts/f3280232-113a-4135-a908-eddd1d9f27b6/' \
    -H 'Content-Type:application/json' \
    -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
    -d '{
        "username": "root_update",
        "secret_type": "password",
        "asset": "bf13858e-b43a-45d1-b952-5624869fdbce"
    }'
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
ASSET_ID    = "your asset id"
ACCOUNT_ID  = "your account id"

def update_assets_accounts():
    url = f"{API_URL}/api/v1/accounts/accounts/{ACCOUNT_ID}/"
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

    data = { 
        "username": "root",
        "secret_type": "password",
        "asset": ASSET_ID
    }

    try:
        response = requests.put(
            url, auth = auth, headers = headers,
            data=json.dumps(data)
        )
        response.raise_for_status()
        print("账号更新成功:")
        print(json.dumps(response.json(), indent=2))
    except Exception as e:
        print(f"错误:{e}")
        print(f"响应内容:{response.text if 'response' in locals() else '无响应'}")

if __name__ == "__main__":
    update_assets_accounts()
```
