## /api/v1/assets/hosts/

### POST
- **描述：**
创建主机资产

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
| addrs* | 类型：String，IP地址 | - |
| platform* | 类型：String，系统平台，示例值：1（代表Linux）、5（代表Windows） | - |
| nodes* | 类型：String[]，节点 | - |
| protocols | 类型：String[]，协议/端口，参数格式示例：`{"name": "ssh", "port": 22}` | - |
| labels | 类型：String[]，标签 | - |
| is_active | 类型：Boolean，激活状态 | true |
| accounts | 类型：String[]，账号信息 | - |


> 注：带 * 的参数为必填项。
- **返回参数：**

| 字段名称 | 字段描述 | 备注 |
| --- | --- | --- |
| id | 类型：String，id |  |
| name | 类型：String，名称 |  |
| addrs | 类型：String，ip |  |
| comment | 类型：String，备注 |  |
| platform | 类型：String，系统平台 |  |
| nodes | 类型：String[]，节点 |  |
| labels | 类型：String[]，标签 |  |
| protocols | 类型：String，协议/端口 |  |
| nodes_display | 类型：String[]，资产节点名称 |  |
| category | 类型：String，类别 |  |
| type | 类型：String，类型 |  |
| is_active | 类型：Boolean，激活状态 |  |
| date_created | 类型：String(date-time)，创建时间 |  |
| org_id | 类型：String，组织 |  |
| org_name | 类型：String，组织名称 |  |


- **请求示例**

**CURL**
```sh
curl -X POST 'https://localhost/api/v1/assets/hosts/' \
    -H 'Content-Type:application/json' \
    -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
    -d '{
        "name":"test_create_asset",
        "address":"192.168.1.1",
        "platform": {"pk":1},
        "nodes": [
            {"pk":"1ecb988f-ded3-4b57-bc8f-808467abbe2f"}
        ]
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
NODE_ID     = "your node id"

def create_assets():
    url = f"{API_URL}/api/v1/assets/hosts/"
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
        "name":"test_create_asset",
        "address":"192.168.1.1",
        "platform": {"pk":1},
        "nodes": [
            {"pk": NODE_ID}
        ]
    }

    try:
        response = requests.post(
            url, auth = auth, headers = headers,
            data = json.dumps(data)
        )
        response.raise_for_status()
        print("资产创建成功:")
        print(json.dumps(response.json(), indent = 2))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    create_assets()
```



## /api/v1/assets/hosts/{id}/
### DELETE
- 描述：
删除主机资产接口

- **请求头（Headers）：**  

| 键 (Header)      | 示例值                                       | 说明                            |
| ---------------- | -------------------------------------------- | ------------------------------- |
| Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
| X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
| Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

- **路径参数（Path Params）：**  

| 名称 | 说明 | 必填 |
| --- | --- | --- |
| id | 类型：string，资产ID | 是 |

- **请求示例**

**CURL**
```sh
curl -X DELETE 'https://localhost/api/v1/assets/hosts/1b56ff93-18e9-478e-97c2-8b6a3720b95d/' \
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

def delete_assets_hosts():
    url = f"{API_URL}/api/v1/assets/hosts/{ASSET_ID}/"
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
        print("资产删除成功")
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    delete_assets_hosts()
```
