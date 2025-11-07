# 工单 API
## /api/v1/tickets/apply-asset-tickets/open/

### POST
- **描述：**
创建工单


- **请求头（Headers）：**  

| 键              | 值                                      | 备注                                                                 |
|-----------------|-----------------------------------------|----------------------------------------------------------------------|
| Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4 为管理员的token 信息。       |
| X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002 为组织 ID，此 id号为默认组织：Default，留空则默认为 Default 组织。 |
| Content-Type    | application/json                        | 输出为json格式              

- **请求体参数（Body）：**  

| 参数名           | 类型                | 描述                                                                 | 是否必选 | 默认值                                                                 |
|------------------|---------------------|----------------------------------------------------------------------|----------|------------------------------------------------------------------------|
| title            | String              | 工单标题                                                             | 是       | -                                                                      |
| org_id           | String              | 组织                                                                 | 是       | -                                                                      |
| apply_node       | String              | 申请节点id                                                           | 否       | 支持模糊搜索，最多显示10项                                             |
| apply_assets     | string[]            | 申请资产id                                                           | 否       | 支持模糊搜索，最多显示10项                                             |
| apply_accounts   | String[]            | 申请账号id                                                           | 否       | "@ALL"：所有账号；"@SPEC"：指定账号；"@INPUT"：手动账号；"@USER"：同名账号 |
| apply_actions    | Integer             | 动作                                                                 | 是       | 默认：all；可选值：[all, connect, upload_file, download_file, updownload, clipboard_copy, clipboard_paste, clipboard_copy_paste] |
| is_active        | Boolean             | 激活中                                                               | 否       | true                                                                   |
| apply_date_start | String(datetime)    | 开始日期                                                             | 是       | -                                                                      |
| apply_date_expired| String(datetime)   | 失效日期（原文“失效日志”应为笔误）                                   | 是       | -                                                                      |
| comment          | String              | 备注                                                                 | 否       | -                                                                      |

**请求示例：**

**CURL**
```sh
curl -X POST 'https://localhost/api/v1/tickets/apply-asset-tickets/open/' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
    -d '{
        "title":"test_tickets_1",
        "apply_accounts":["@ALL"],
        "apply_actions":["all"],
        "org_id":"00000000-0000-0000-0000-000000000002",
        "apply_assets":["b4f205af-4353-49ef-befa-ff9095d52a27"],
        "apply_date_start":"2023-03-28T02:10:23.245Z",
        "apply_date_expired":"2023-04-04T02:10:23.245Z"
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

def apply_asset_tickets():
    url = f"{API_URL}/api/v1/tickets/apply-asset-tickets/open/"
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
        "title":"test_tickets_1",
        "apply_accounts":["@ALL"],
        "apply_actions":["all"],
        "org_id": ORG_ID,
        "apply_date_start":"2025-01-01T00:00:00.245Z",
        "apply_date_expired":"2095-01-01T00:00:00.245Z"
    }

    try:
        response = requests.post(
            url, auth = auth, headers = headers,
            data = json.dumps(data)
        )
        response.raise_for_status()
        print("工单创建成功:")
        print(json.dumps(response.json(), indent = 2))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    apply_asset_tickets()
```

- **返回参数:**

| 字段名称               | 数据类型          | 字段描述               | 备注 |
|------------------------|-------------------|------------------------|------|
| id                     | String            | id                     |      |
| title                  | String            | 标题                   |      |
| org_id                 | String            | 组织                   |      |
| comment                | String            | 备注                   |      |
| type                   | String            | 类型                   |      |
| apply_nodes            | String[]          | 申请节点               |      |
| apply_assets           | String[]          | 申请资产               |      |
| apply_accounts         | String[]          | 申请账号               |      |
| apply_actions          | String[]          | 申请动作               |      |
| serial_num             | String            | 序列号                 |      |
| approval_step          | String            | 流程步骤               |      |
| state                  | String            | 工单动作               |      |
| status                 | String            | 工单状态               |      |
| applicant              | String            | 申请人                 |      |
| org_name               | String            | 组织名称               |      |
| apply_permission_name  | String            | 工单授权名称           |      |
| apply_date_start       | String(date-time) | 申请开始时间           |      |
| apply_date_expired     | String(date-time) | 申请结束时间           |      |


### GET
- **描述：**
获取工单

- **请求头（Headers）：**  

| 键              | 值                                      | 备注                                                                 |
|-----------------|-----------------------------------------|----------------------------------------------------------------------|
| Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4 为管理员的token 信息。       |
| X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002 为组织 ID，此 id号为默认组织：Default，留空则默认为 Default 组织。 |
| Content-Type    | application/json                        | 输出为json格式                                                       |

- **请求体参数（Body）：**  

| 参数名   | 类型   | 描述                                                                 | 是否必选 | 默认值 |
|----------|--------|----------------------------------------------------------------------|----------|--------|
| state    | String | 动作                                                                 | 否       | -      |
|          |        | 可选值：pending（待处理）、approved（已同意）、rejected（已拒绝）    |          |        |
| status   | String | 状态                                                                 | 否       | -      |
|          |        | 可选值：closed（已关闭、拒绝）、open（打开）                          |          |        |
| type     | String | 类型                                                                 | 否       | -      |
|          |        | 可选值：apply_asset（申请资产）、login_confirm（用户登录复核）、command_confirm（命令复核）、login_asset_confirm（资产登录复核） |          |        |

**请求示例**

**CURL**
```sh
curl -X GET 'https://localhost/api/v1/tickets/tickets/?state=pending&status=open&type=apply_asset' \
    -H 'Content-Type: application/json' \
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

def search_tickets():
    url = f"{API_URL}/api/v1/tickets/tickets/"
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
        "state": "pending",
        "status": "open",
        "type": "apply_asset"
    }

    try:
        response = requests.get(
            url, auth = auth, headers = headers,
            params = params
        )
        response.raise_for_status()
        nodes_data = response.json()
        if not nodes_data:
            print(f"未找到工单")
        else:
            print(f"查询到 {len(nodes_data)} 个匹配的工单：")
            print(json.dumps(nodes_data, indent = 2, ensure_ascii = False))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    search_tickets()
```

- **返回参数：**

| 字段名称       | 数据类型          | 字段描述       | 备注 |
|----------------|-------------------|----------------|------|
| id             | String            | id             |      |
| title          | String            | 标题           |      |
| org_id         | String            | 组织           |      |
| serial_num     | String            | 编号           |      |
| approval_step  | String            | 工单审批步骤   |      |
| type           | String            | 类型           |      |
| state          | String            | 动作           |      |
| applicant      | String            | 申请人         |      |
| status         | String            | 状态           |      |
| org_name       | String            | 组织名称       |      |
| date_created   | String(date-time) | 创建时间       |      |
| date_update    | String(date-time) | 更新时间       |      |


## /api/v1/tickets/apply-asset-tickets/{id}/approve/

### PATCH
- **描述：**
审批工单

- **请求头（Headers）：**  

| 键              | 值                                      | 备注                                                                 |
|-----------------|-----------------------------------------|----------------------------------------------------------------------|
| Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4 为管理员的token 信息。       |
| X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002 为组织 ID，此 id号为默认组织：Default，留空则默认为 Default 组织。 |
| Content-Type    | application/json                        | 输出为json格式                                                       |

- **请求体参数（Body）：**  

| 参数名           | 类型                | 描述                                                                 | 是否必选 | 默认值                                                                 |
|------------------|---------------------|----------------------------------------------------------------------|----------|------------------------------------------------------------------------|
| org_id           | String              | 组织id                                                               | 是       | -                                                                      |
| apply_node       | String              | 申请节点id                                                           | 是       | -                                                                      |
| apply_assets     | string[]            | 申请资产id                                                           | 是       | -                                                                      |
| apply_accounts   | String[]            | 申请账号id                                                           | 是       | "@ALL"：所有账号；"@SPEC"：指定账号；"@INPUT"：手动账号；"@USER"：同名账号 |
| apply_actions    | Integer             | 动作                                                                 | 是       | 默认：all；可选值：[all, connect, upload_file, download_file, updownload, clipboard_copy, clipboard_paste, clipboard_copy_paste] |
| apply_date_start | String(datetime)    | 开始日期（原文格式“String(date time)”修正为标准datetime格式表述）     | 是       | -                                                                      |
| apply_date_expired| String(datetime)   | 失效日期（原文“失效日志”应为笔误，格式“String(date time)”修正为标准datetime格式表述） | 是       | -                                                                      |
| comment          | String              | 备注                                                                 | 否       | -                                                                      |

**请求示例**

**CURL**
```sh
curl -X PATCH 'https://localhost/api/v1/tickets/apply-asset-tickets/41b36621-dd4d-492e-a72c-be20b2daeea8/approve/' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
    -d '{
        "org_id": "00000000-0000-0000-0000-000000000002",
        "apply_assets": ["b4f205af-4353-49ef-befa-ff9095d52a27"],
        "apply_accounts": ["@ALL"],
        "apply_actions": ["all"],
        "apply_date_start": "2025-03-28 00:00:00",
        "apply_date_expired": "2025-04-04 00:00:00"
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
TICKET_ID   = "your ticket id"
ASSET_ID    = "your asset id"

def approve_tickets():
    url = f"{API_URL}/api/v1/tickets/apply-asset-tickets/{TICKET_ID}/approve/"
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
            "org_id": ORG_ID,
            "apply_assets": [ASSET_ID],
            "apply_accounts": ["@ALL"],
            "apply_actions": ["all"],
            "apply_date_start": "2025-03-28 00:00:00",
            "apply_date_expired": "2025-04-04 00:00:00"
    }

    try:
        response = requests.patch(
            url, auth = auth, headers = headers,
            data = json.dumps(data)
        )
        response.raise_for_status()
        print("工单审批成功:")
        print(json.dumps(response.json(), indent = 2))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    approve_tickets()
```

## /api/v1/tickets/flows/

### GET
- **描述：**
查询流程

- **请求头（Headers）：**  

| 键              | 值                                      | 备注                                                                 |
|-----------------|-----------------------------------------|----------------------------------------------------------------------|
| Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4 为管理员的token 信息。       |
| X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002 为组织 ID，此 id号为默认组织：Default，留空则默认为 Default 组织。 |
| Content-Type    | application/json                        | 输出为json格式      

- **请求体参数（Body）：**  

| 参数名 | 类型   | 描述           | 是否必选 | 默认值 |
|--------|--------|----------------|----------|--------|
| limit  | int    | 每一页显示条数 | 是       | -      |
| offset | int    | 分页偏移量     | 是       | -      |

**请求示例**

**CURL**
```sh
curl -X GET 'https://localhost/api/v1/tickets/flows/?offset=0&limit=15' \
    -H 'Content-Type: application/json' \
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

def search_flows():
    url = f"{API_URL}/api/v1/tickets/flows/"
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
        nodes_data = response.json()
        if not nodes_data:
            print(f"未找到流程")
        else:
            print(f"查询到 {len(nodes_data)} 个匹配的流程：")
            print(json.dumps(nodes_data, indent = 2, ensure_ascii = False))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    search_flows()
```

- **返回参数：**

| 字段名称         | 数据类型          | 字段描述       | 备注 |
|------------------|-------------------|----------------|------|
| id               | String            | id             |      |
| type             | Object            | 流程类型       |      |
| org_id           | String            | 组织           |      |
| org_name         | String            | 组织名称       |      |
| approval_level   | int               | 审批级别       |      |
| rules            | Array             | 审批流程       |      |
| level            | int               | 流程级别       |      |
| strategy         | Object            | 审批角色       |      |
| assignees_display| Array             | 审批人名称     |      |
| created_by       | String            | 创建人         |      |
| date_created     | String(date-time) | 创建时间       |      |
| date_update      | String(date-time) | 更新时间       |      |


## /api/v1/tickets/flows/{flowId}/
### PATCH
- **描述：**
更新流程

- **请求头（Headers）：**  

| 键              | 值                                      | 备注                                                                 |
|-----------------|-----------------------------------------|----------------------------------------------------------------------|
| Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4 为管理员的token 信息。       |
| X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002 为组织 ID，此 id号为默认组织：Default，留空则默认为 Default 组织。 |
| Content-Type    | application/json                        | 输出为json格式                           

- **请求体参数（Body）：**  

| 参数名           | 类型   | 描述       | 是否必选 | 默认值 |
|------------------|--------|------------|----------|--------|
| type             | string | 类型       | 是       | -      |
| approval_level   | int    | 审批级别   | 是       | -      |
| rules            | Array  | 审批流程   | 是       | -      |
| level            | int    | 流程级别   | 是       | -      |
| strategy         | object | 审批角色   | 是       | -      |
| assignees_display | Array  | 审批人名称 | 否       | -      |

**请求示例**

**CURL**
```sh
curl -X PATCH 'https://localhost/api/v1/tickets/flows/' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
    -d '{
        "type": "apply_asset",
        "approval_level": 1,
        "rules": [{
            "level": 1,
            "strategy": {
                "value": "super_admin",
                "label": "超级管理员"
            },
            "assignees_display": ["Administrator(admin)"],
            "assignees": []
        }]
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
TICKET_ID   = "your ticket id"

def update_tickets_flows():
    url = f"{API_URL}/api/v1/tickets/flows/{TICKET_ID}/"
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
        "type": "apply_asset",
        "approval_level": 1,
        "rules": [{
            "level": 1,
            "strategy": {
                "value": "super_admin",
                "label": "超级管理员"
            },
            "assignees_display": ["Administrator(admin)"],
            "assignees": []
        }]
    }

    try:
        response = requests.patch(
            url, auth = auth, headers = headers,
            data = json.dumps(data)
        )
        response.raise_for_status()
        print("修改流程成功:")
        print(json.dumps(response.json(), indent = 2))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    update_tickets_flows()
```

- **返回参数:**

| 字段名称         | 数据类型          | 字段描述       | 备注 |
|------------------|-------------------|----------------|------|
| id               | String            | id             |      |
| type             | Object            | 流程类型       |      |
| org_id           | String            | 组织           |      |
| org_name         | String            | 组织名称       |      |
| approval_level   | int               | 审批级别       |      |
| rules            | Array             | 审批流程       |      |
| level            | int               | 流程级别       |      |
| strategy         | object            | 审批角色       |      |
| assignees_display| Array             | 审批人名称     |      |
| created_by       | String            | 创建人         |      |
| date_created     | String(date-time) | 创建时间       |      |
| date_update      | String(date-time) | 更新时间       |      |