## /api/v1/perms/asset-permissions/

### POST
- **描述：**
创建资产授权

- **请求头（Headers）：**  

| 键 (Header)      | 示例值                                       | 说明                            |
| ---------------- | -------------------------------------------- | ------------------------------- |
| Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
| X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
| Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

- **请求体参数（Body）：**  

| 参数名 | 描述 | 默认值 |
| --- | --- | --- |
| name* | 类型：String，名称 | - |
| users | 类型：String[]，用户id | - |
| user_groups | 类型：String[]，用户组id | - |
| assets | 类型：String[]，资产id（原文“资id”修正为“资产id”，确保语义完整） | - |
| nodes | 类型：String[]，节点id | - |
| accounts | 类型：String[]，账号id，可选特殊值：<br>- "@ALL"：所有账号<br>- "@SPEC"：指定账号<br>- "@INPUT"：手动账号<br>- "@USER"：同名账号 | - |
| actions* | 类型：String[]，动作，可选值：[all, connect, upload_file, download_file, updownload, clipboard_copy, clipboard_paste, clipboard_copy_paste] | all |
| is_active | 类型：Boolean，激活中 | true |
| date_start | 类型：String(datetime)，开始时间 | - |
| date_expired | 类型：String(datetime)，失效时间 | - |
| comment | 类型：String，备注 | - |

> 注：带 * 的参数为必填项。
- **请求示例**

**CURL**
```sh
curl -X POST 'https://localhost/api/v1/perms/asset-permissions/' \
-H 'Content-Type:application/json' \
-H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
-H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
-d '{
        "name":"create_asset_permission",
        "user_groups":[{"pk":"745980b1-54be-4c5e-b6ab-89826c2e2054"}],
        "nodes":["3728f004-99a2-4fca-9577-84d5ffcf9eff"],
        "accounts":["@ALL"],
        "actions":["connect","upload","download","copy","paste"],
        "is_active":true,
        "date_start":"2025-01-01T00:00:00.879Z",
        "date_expired":"2095-01-01T00:00:00.879Z"
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
GROUP_ID    = "your group id"
NODE_ID     = "your node id"

def create_asset_permissions():
    url = f"{API_URL}/api/v1/perms/asset-permissions/"
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
        "name":"create_asset_permission",
        "user_groups":[{"pk": GROUP_ID}],
        "nodes":[NODE_ID],
        "accounts":["@ALL"],
        "actions":["connect","upload","download","copy","paste"],
        "is_active": True,
        "date_start":"2025-01-01T00:00:00.879Z",
        "date_expired":"2095-01-01T00:00:00.879Z"
    }

    try:
        response = requests.post(
            url, auth = auth, headers = headers,
            data = json.dumps(data)
        )
        response.raise_for_status()
        print("资产授权创建成功:")
        print(json.dumps(response.json(), indent = 2))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    delete_assets_hosts()
```

- **返回参数：**  

| 字段名称         | 数据类型          | 字段描述               | 备注 |
|------------------|-------------------|------------------------|------|
| id               | String            | id                     |      |
| name             | String            | 名称                   |      |
| users            | String[]          | 用户                   |      |
| users_groups     | String[]          | 用户组                 |      |
| assets           | String[]          | 资产                   |      |
| nodes            | String[]          | 节点                   |      |
| accounts         | String[]          | 授权账号               |      |
| actions          | String[]          | 动作                   |      |
| is_active        | Boolean           | 激活中                 |      |
| created_by       | String            | 创建者                 |      |
| comment          | String            | 备注                   |      |
| date_created     | String(date-time) | 授权规则创建时间       |      |
| date_start       | String(date-time) | 授权规则开始时间       |      |
| date_expired     | String(date-time) | 授权规则失效时间       |      |


## /api/v1/perms/asset-permissions/{id}/

### DELETE
- **描述：**
删除资产授权

- **请求头（Headers）：**  

| 键 (Header)      | 示例值                                       | 说明                            |
| ---------------- | -------------------------------------------- | ------------------------------- |
| Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
| X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
| Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

- **路径参数（Path Params）：**  

| 名称 | 说明 | 必填 |
| --- | --- | --- |
| id | 类型：String，授权ID | 是 |

- **请求示例**

**CURL**
```sh
curl -X DELETE 'https://localhost/api/v1/perms/asset-permissions/b2c45ce6-6b9c-4270-a073-567ff0e0ade8/' \
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
PER_ID      = "your permission id"

def delete_assets_permissions():
    url = f"{API_URL}/api/v1/perms/asset-permissions/{PER_ID}/"
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
            url, auth = auth, headers = headers
        )
        response.raise_for_status()
        print("资产授权删除成功")
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    delete_assets_permissions()
```

### PUT
- **描述：**
更新资产授权

- **请求头（Headers）：**  

| 键 (Header)      | 示例值                                       | 说明                            |
| ---------------- | -------------------------------------------- | ------------------------------- |
| Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
| X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
| Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

- **请求体参数（Body）：**  

| 参数名 | 描述 | 默认值 |
| --- | --- | --- |
| name* | 类型：String，名称 | - |
| users | 类型：String[]，用户id | - |
| user_groups | 类型：String[]，用户组id | - |
| assets | 类型：String[]，资产id | - |
| nodes | 类型：String[]，节点id | - |
| accounts | 类型：String[]，账号，可选特殊值：<br>- "@ALL"：所有账号<br>- "@SPEC"：指定账号<br>- "@INPUT"：手动账号<br>- "@USER"：同名账号 | - |
| actions* | 类型：String[]，动作，可选值：[all, connect, upload_file, download_file, updownload, clipboard_copy, clipboard_paste, clipboard_copy_paste] | all |
| is_active | 类型：Boolean，激活中 | true |
| date_start | 类型：String(datetime)，开始时间 | - |
| date_expired | 类型：String(datetime)，失效时间 | - |
| comment | 类型：String，备注 | - |

> 注：带 * 的参数为必填项。
- **请求示例**

**CURL**
```sh
curl -X PUT 'https://localhost/api/v1/perms/asset-permissions/ca90421b-bc45-48c0-bd49-4a213ff61b7c/' \
    -H 'Content-Type:application/json' \
    -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
    -d '{
            "name":"create_asset_permission_update",
            "user_groups":[{"pk":"745980b1-54be-4c5e-b6ab-89826c2e2054"}],
            "nodes":["3728f004-99a2-4fca-9577-84d5ffcf9eff"],
            "accounts":["@ALL"],
            "actions":["connect","upload","download","copy","paste"],
            "is_active":true,
            "date_start":"2025-01-01T00:00:00.879Z",
            "date_expired":"2095-01-01T00:00:00.879Z"
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
GROUP_ID    = "your group id"
NODE_ID     = "your node id"
PER_ID      = "your permission id"

def update_asset_permissions():
    url = f"{API_URL}/api/v1/perms/asset-permissions/{PER_ID}/"
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
        "name":"create_asset_permission_update",
        "user_groups":[{"pk": GROUP_ID}],
        "nodes":[NODE_ID],
        "accounts":["@ALL"],
        "actions":["connect","upload","download","copy","paste"],
        "is_active": True,
        "date_start":"2025-01-01T00:00:00.879Z",
        "date_expired":"2095-01-01T00:00:00.879Z"
    }

    try:
        response = requests.put(
            url, auth = auth, headers = headers,
            data = json.dumps(data)
        )
        response.raise_for_status()
        print("资产授权更新成功:")
        print(json.dumps(response.json(), indent = 2))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    update_asset_permissions()
```

- **返回参数：**

| 字段名称         | 数据类型          | 字段描述               | 备注 |
|------------------|-------------------|------------------------|------|
| id               | String            | id                     |      |
| name             | String            | 名称                   |      |
| users            | String[]          | 用户                   |      |
| users_groups     | String[]          | 用户组                 |      |
| assets           | String[]          | 资产                   |      |
| nodes            | String[]          | 节点                   |      |
| accounts         | String[]          | 授权账号               |      |
| actions          | String[]          | 动作                   |      |
| is_active        | Boolean           | 激活中                 |      |
| created_by       | String            | 创建者                 |      |
| comment          | String            | 备注                   |      |
| date_created     | String(date-time) | 授权规则创建时间       |      |
| date_start       | String(date-time) | 授权规则开始时间       |      |
| date_expired     | String(date-time) | 授权规则失效时间       |      |
