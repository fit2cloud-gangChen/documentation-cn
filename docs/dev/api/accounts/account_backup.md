## /api/v1/accounts/account-backup-plans/

### GET
- **描述：**
查询账号备份策略

- **请求头（Headers）：**  

| 键              | 值                                      | 备注                                                                 |
|-----------------|-----------------------------------------|----------------------------------------------------------------------|
| Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4 为管理员的token 信息。       |
| X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002 为组织 ID，此 id号为默认组织：Default，留空则默认为 Default 组织。 |
| Content-Type    | application/json                        | 输出为json格式                                                       |

- **请求体参数（Body）：**  

| 参数名 | 描述 | 默认值 |
| --- | --- | --- |
| name | 类型：String，名称 | - |
| limit* | 类型：int，每一页显示条数 | - |
| offset* | 类型：int，分页偏移量 | - |

> 注：带 * 的参数为必填项。
- **返回参数:**

| 字段名称         | 数据类型          | 字段描述                                                                 | 备注 |
|------------------|-------------------|--------------------------------------------------------------------------|------|
| count            | Int               | 总数                                                                     |      |
| next             | String            | 下一页链接                                                               |      |
| previous         | String            | 上一页链接                                                               |      |
| results          | List              | 用户数据                                                                 |      |
| id               | String            | 备份策略ID                                                               |      |
| name             | String            | 名称                                                                     |      |
| org_id           | String            | 组织ID                                                                   |      |
| org_name         | String            | 组织名                                                                   |      |
| is_periodic      | Boolean           | 是否定时执行                                                             |      |
| interval         | Int               | 周期执行                                                                 |      |
| crontab          | String            | 定时执行Crontab表达式                                                    |      |
| recipients       | List[Object]      | 收件人                                                                   |      |
| types            | String[]          | 资产类型                                                                 | linux, windows, unix, other, general, switch, "router", "firewall", "mysql", "mariadb", "postgresql", "oracle", "sqlserver", "clickhouse", "mongodb", "redis", "public", "private", "k8s", "website" |
| date_created     | String[date]      | 创建时间                                                                 |      |
| date_updated     | String[date]      | 更新时间                                                                 |      |
| created_by       | String            | 创建人                                                                   |      |
| comment          | String            | 备注                                                                     |      |

- **请求示例**

**CURL**
```sh
curl -X GET 'https://localhost/api/v1/accounts/account-backup-plans/?offset=0&limit=15' \
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
SEARCH_WORD = "your search word"

def search_account_backup_plans(keyword):
    url = f"{API_URL}/api/v1/accounts/account-backup-plans/"
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
        "search": keyword
    }

    try:
        response = requests.get(
            url, auth = auth, headers = headers,
            params = params
        )
        response.raise_for_status()
        nodes_data = response.json()
        if not nodes_data:
            print(f"未找到账号备份策略")
        else:
            print(f"查询到 {len(nodes_data)} 个匹配的账号备份策略：")
            print(json.dumps(nodes_data, indent = 2, ensure_ascii = False))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    search_account_backup_plans(SEARCH_WORD)
```




### POST
- **描述：**
创建账号备份策略

- **请求头（Headers）：**  

| 键              | 值                                      | 备注                                                                 |
|-----------------|-----------------------------------------|----------------------------------------------------------------------|
| Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4 为管理员的token 信息。       |
| X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002 为组织 ID，此 id号为默认组织：Default，留空则默认为 Default 组织。 |
| Content-Type    | application/json                        | 输出为json格式                                                       |

- **请求体参数（Body）：**  

| 参数名 | 描述 | 默认值 |
| --- | --- | --- |
| name | 类型：String，名称 | - |
| limit* | 类型：int，每一页显示条数 | - |
| offset* | 类型：int，分页偏移量 | - |

> 注：带 * 的参数为必填项。
- **返回参数:**

| 字段名称         | 数据类型          | 字段描述                                                                 | 备注 |
|------------------|-------------------|--------------------------------------------------------------------------|------|
| id               | String            | 备份策略ID                                                               |      |
| name             | String            | 名称                                                                     |      |
| org_id           | String            | 组织ID                                                                   |      |
| org_name         | String            | 组织名                                                                   |      |
| is_periodic      | Boolean           | 是否定时执行                                                             |      |
| interval         | Int               | 周期执行                                                                 | 默认24 |
| crontab          | String            | 定时执行Crontab表达式                                                    |      |
| recipients       | List[Object]      | 收件人                                                                   |      |
| types            | String[]          | 资产类型                                                                 | linux, windows, unix, other, general, switch, "router", "firewall", "mysql", "mariadb", "postgresql", "oracle", "sqlserver", "clickhouse", "mongodb", "redis", "public", "private", "k8s", "website" |
| date_created     | String[date]      | 创建时间                                                                 |      |
| date_updated     | String[date]      | 更新时间                                                                 |      |
| created_by       | String            | 创建人                                                                   |      |
| comment          | String            | 备注                                                                     |      |


- **请求示例**

**CURL**
```sh
curl -X POST 'https://localhost/api/v1/accounts/account-backup-plans/' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
    -d '{
        "types": ["linux", "windows", "unix", "other", "general", "switch", "router", "firewall", "mysql", "mariadb",
        "postgresql", "oracle", "sqlserver", "clickhouse", "mongodb", "redis", "public", "private", "k8s", "website"],
        "is_periodic": true,
        "interval": 24,
        "name": "test",
        "recipients": [{
            "pk": "d1a02c44-e40b-41ac-884a-c44f3c664209"
        }],
        "crontab": "0 0 * * *",
        "comment": "abcs"
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

def create_account_backup_plans():
    url = f"{API_URL}/api/v1/accounts/account-backup-plans/"
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
        "types": ["linux", "windows", "unix", "other", "general", "switch", "router", "firewall", "mysql", "mariadb",
        "postgresql", "oracle", "sqlserver", "clickhouse", "mongodb", "redis", "public", "private", "k8s", "website"],
        "is_periodic": True,
        "interval": 24,
        "name": "test",
        "crontab": "0 0 * * *",
        "comment": "abcs"
    }

    try:
        response = requests.post(
            url, auth = auth, headers = headers,
            data = json.dumps(data)
        )
        response.raise_for_status()
        print("账号备份计划创建成功:")
        print(json.dumps(response.json(), indent = 2))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    create_account_backup_plans()
```


## /api/v1/accounts/account-backup-plans/{id}
### DELETE
- **描述：**
删除账号备份策略

- **请求头（Headers）：**  

| 键              | 值                                      | 备注                                                                 |
|-----------------|-----------------------------------------|----------------------------------------------------------------------|
| Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4 为管理员的token 信息。       |
| X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002 为组织 ID，此 id号为默认组织：Default，留空则默认为 Default 组织。 |
| Content-Type    | application/json                        | 输出为json格式                                                       |

- **请求体参数（Body）：**  

| 参数名 | 描述 | 默认值 |
| --- | --- | --- |
| id* | 类型：String，备份策略ID | - |

> 注：带 * 的参数为必填项。
**请求示例**

**CURL**
```sh
curl -X DELETE 'https://localhost/api/v1/accounts/account-backup-plans/37e4c30a-71ea-4b58-91f3-a692715dcf99/' \
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
PLAN_ID     = "your plan id"

def delete_account_backup_plans():
    url = f"{API_URL}/api/v1/accounts/account-backup-plans/{PLAN_ID}/"
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
        print("账号备份计划删除成功")
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    delete_account_backup_plans()
```

## /api/v1/accounts/account-backup-plan-executions/

### POST
- **描述：**
执行账号备份策略


- **请求头（Headers）：**  

| 键              | 值                                      | 备注                                                                 |
|-----------------|-----------------------------------------|----------------------------------------------------------------------|
| Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4 为管理员的token 信息。       |
| X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002 为组织 ID，此 id号为默认组织：Default，留空则默认为 Default 组织。 |
| Content-Type    | application/json                        | 输出为json格式               

- **请求体参数（Body）：**  

| 参数名 | 描述 | 默认值 |
| --- | --- | --- |
| plan* | 类型：String，备份策略ID | - |

> 注：带 * 的参数为必填项。
- **返回参数：**

| 字段名称 | 数据类型 | 字段描述 | 备注 |
|----------|----------|----------|------|
| task     | String   | 任务id   |      |

**请求示例**

**CURL**
```sh
curl -X POST 'https://localhost/api/v1/accounts/account-backup-plan-executions/' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
    -d '{
        "plan": "37e4c30a-71ea-4b58-91f3-a692715dcf99"
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
PLAN_ID     = "your plan id"

def account_backup_plans_executions():
    url = f"{API_URL}/api/v1/accounts/account-backup-plan-executions/"
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
        "plan": PLAN_ID
    }

    try:
        response = requests.post(
            url, auth = auth, headers = headers,
            data = json.dumps(data),
            verify = False
        )
        response.raise_for_status()
        print(f"账号备份计划已执行")
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    account_backup_plans_executions()
```
