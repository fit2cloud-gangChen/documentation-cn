## /api/v1/users/users/

### GET

- **描述：**
获取用户列表

- **请求头（Headers）：**

| 键 (Header) | 示例值 | 说明 |
| ----------- | ------ | ---- |
| Authorization | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
| X-JMS-ORG | `00000000-0000-0000-0000-000000000002` | 组织 ID，不传则默认归属 `Default` 组织 |
| Content-Type | `application/json` | 请求/响应体为 JSON 格式 |

- **返回参数：**

| 字段名称 | 描述 | 备注 |
| --- | --- | --- |
| count | 类型：int，总数 | 分页总记录数 |
| next | 类型：string，下一页链接 | 无更多页为 null |
| previous | 类型：string，上一页链接 | 无上一页为 null |
| results | 类型：list，用户数据列表 | 列表元素为用户对象(见下) |
| id | 类型：string，用户ID | UUID |
| name | 类型：string，显示名称 |  |
| username | 类型：string，用户名 | 登录名 |
| email | 类型：string，邮箱 |  |
| mfa_level | 类型：object，MFA 等级 | {"value":0,"label":"禁用"} 等 |
| source | 类型：object，用户来源 | local/ldap/openid/radius/cas/saml2/oauth2/custom |
| wecom_id | 类型：string，企业微信ID | 绑定时存在 |
| dingtalk_id | 类型：string，钉钉ID | 绑定时存在 |
| feishu_id | 类型：string，飞书ID | 绑定时存在 |
| created_by | 类型：string，创建者 |  |
| updated_by | 类型：string，更新者 |  |
| comment | 类型：string，备注 |  |
| groups | 类型：list，用户组 | 对象或ID列表 |
| system_roles | 类型：list，系统角色 | 默认含“用户”角色 |
| org_roles | 类型：list，组织角色 | 默认含“组织用户” |
| password_strategy | 类型：list，密码策略 | email / custom 等 |
| is_service_account | 类型：boolean，是否组件账号 | true表示系统内部账号 |
| is_valid | 类型：boolean，是否有效 |  |
| is_expired | 类型：boolean，是否到期 |  |
| is_active | 类型：boolean，是否启用 |  |
| is_otp_secret_key_bound | 类型：boolean，是否绑定OTP密钥 |  |
| can_public_key_auth | 类型：boolean，是否允许SSH公钥 |  |
| mfa_enabled | 类型：boolean，是否开启MFA |  |
| need_update_password | 类型：boolean，下次登录需改密 |  |
| mfa_force_enabled | 类型：boolean，是否强制MFA |  |
| is_first_login | 类型：boolean，是否第一次登录 |  |
| login_blocked | 类型：boolean，登录阻止 |  |
| date_expired | 类型：string(date-time)，过期时间 |  |
| date_joined | 类型：string(date-time)，加入时间 |  |
| last_login | 类型：string(date-time)，最后登录时间 |  |
| date_updated | 类型：string(date-time)，更新时间 |  |
| date_password_last_updated | 类型：string(date-time)，密码更新时间 |  |

- **请求示例**

**CURL**

``` sh
curl -X GET 'https://localhost/api/v1/users/users/?offset=0&limit=15' \
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

def get_user_info():
    url = f"{API_URL}/api/v1/users/users/"
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
        return response.json()
    except requests.RequestException as e:
        print(f"API 请求失败:{e}")
        return None

if __name__ == "__main__":
    result = get_user_info()
    print(json.dumps(result, indent = 2, ensure_ascii = False))
```

### POST

- **描述：**
创建用户

- **请求头（Headers）：**

| 键 (Header) | 示例值 | 说明 |
| ----------- | ------ | ---- |
| Authorization | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
| X-JMS-ORG | `00000000-0000-0000-0000-000000000002` | 组织 ID，留空则默认为 `Default` 组织 |
| Content-Type | `application/json` | 请求/响应体为 JSON 格式 |

- **请求体参数（Body）：**

| 参数名 | 描述 | 可选值 / 备注 |
| --- | --- | --- |
| name* | 类型：string，名称 / 显示名称 | - |
| username* | 类型：string，用户名（登录名） | - |
| email* | 类型：string，邮箱 | - |
| wechat | 类型：string，微信 | - |
| phone | 类型：string，手机 | - |
| groups | 类型：string[]，用户组 ID 列表 | - |
| password* | 类型：string，密码（当 password_strategy=custom 时必填） | - |
| need_update_password | 类型：boolean，是否下次登录需修改密码 | 默认 false；[true,false] |
| public_key | 类型：string，SSH 公钥 | - |
| system_roles* | 类型：object[]/string[]，系统角色 | 元素含 pk ID |
| org_roles* | 类型：object[]/string[]，组织角色 | 元素含 pk ID |
| password_strategy | 类型：string，密码策略 | email / custom；email=邮件设置密码 |
| source | 类型：string，用户来源 | 默认 default；local/ldap/openid/radius/cas/saml2/oauth2/custom |
| mfa_level | 类型：integer，MFA 等级 | 0=禁用 1=启用 2=强制 |
| date_expired | 类型：string(date-time)，用户失效时间 | 例如：2023-02-04T00:54:39.000Z |

> 注：带 * 的参数为必填项。
- **返回参数：** （创建成功返回完整用户对象，与 GET 列表中单个元素结构一致）

| 字段名称 | 描述 | 备注 |
| --- | --- | --- |
| id | 类型：string，用户ID | UUID |
| name | 类型：string，堡垒机用户名称 |  |
| username | 类型：string，堡垒机用户名 | 登录名 |
| email | 类型：string，邮箱 |  |
| wechat | 类型：string，微信 |  |
| phone | 类型：string，电话号码 |  |
| mfa_level | 类型：object，MFA 等级 | {"value":0,"label":"禁用"} 等 |
| source | 类型：object，用户来源 | local/ldap/.../custom |
| wecom_id | 类型：string，企业微信ID | 绑定时存在 |
| dingtalk_id | 类型：string，钉钉ID | 绑定时存在 |
| feishu_id | 类型：string，飞书ID | 绑定时存在 |
| created_by | 类型：string，创建者 |  |
| updated_by | 类型：string，更新者 |  |
| comment | 类型：string，备注 |  |
| groups | 类型：list，用户组 | 对象或ID列表 |
| system_roles | 类型：list，系统角色 | 默认含“用户”角色 |
| org_roles | 类型：list，组织角色 | 默认含“组织用户” |
| password_strategy | 类型：string，密码策略 | email / custom |
| is_service_account | 类型：boolean，是否组件账号 | true 表示系统内部账号 |
| is_valid | 类型：boolean，是否有效 |  |
| is_expired | 类型：boolean，是否到期 |  |
| is_active | 类型：boolean，是否启用 |  |
| is_otp_secret_key_bound | 类型：boolean，是否绑定OTP密钥 |  |
| can_public_key_auth | 类型：boolean，是否允许SSH公钥 |  |
| mfa_enabled | 类型：boolean，是否开启MFA |  |
| need_update_password | 类型：boolean，下次登录需改密 |  |
| mfa_force_enabled | 类型：boolean，是否强制MFA |  |
| is_first_login | 类型：boolean，是否第一次登录 |  |
| login_blocked | 类型：boolean，登录阻止 |  |
| date_expired | 类型：string(date-time)，过期时间 |  |
| date_joined | 类型：string(date-time)，加入时间 |  |
| last_login | 类型：string(date-time)，最后登录时间 |  |
| date_updated | 类型：string(date-time)，更新时间 |  |
| date_password_last_updated | 类型：string(date-time)，密码更新时间 |  |

- **请求示例**

**CURL**

``` sh
curl -X POST 'https://localhost/api/v1/users/users/' \
    -H 'Content-Type:application/json' \
    -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
    -d '{ 
            "name": "api_test",
            "username": "api_test",
            "password": "apitest",
            "password_strategy":"custom", 
            "email":"api_test@fit2cloud.com", 
            "mfa_level":0, 
            "source":"local", 
            "system_roles":[ 
                {"pk":"00000000-0000-0000-0000-000000000003"} 
            ], 
            "org_roles":[ 
                {"pk":"00000000-0000-0000-0000-000000000007"} 
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

def create_user():
    url = f"{API_URL}/api/v1/users/users/"
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
        "name": "api_test",
        "username": "api_test",
        "password": "apitest",
        "password_strategy": "custom",
        "email": "api_test@fit2cloud.com",
        "mfa_level": 0,
        "source": "local",
        "system_roles": [{"pk": "00000000-0000-0000-0000-000000000003"}],
        "org_roles": [{"pk": "00000000-0000-0000-0000-000000000007"}]
    }

    try:
        response = requests.post(
            url, auth = auth, headers = headers,
            data = json.dumps(data)
        )
        response.raise_for_status()
        print("用户创建成功:")
        print(json.dumps(response.json(), indent = 2))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    create_user()
```

## /api/v1/users/users/{id}/

### GET

- **描述：**
获取用户详情

- **请求头（Headers）：**

| 键 (Header) | 示例值 | 说明 |
| ----------- | ------ | ---- |
| Authorization | `Bearer <token>` | 认证 Token，管理员或具备查看权限的用户 |
| X-JMS-ORG | `00000000-0000-0000-0000-000000000002` | 组织 ID，缺省为 Default |
| Content-Type | `application/json` | 响应体为 JSON |

- **路径参数（Path Params）：**

| 名称 | 说明 | 必填 |
| --- | --- | --- |
| id | 类型：string(UUID)，用户 ID | 是 |

- **返回参数：**（结构与创建/列表单个元素一致）

| 字段名称 | 描述 | 备注 |
| --- | --- | --- |
| id | 类型：string，用户ID | UUID |
| name | 类型：string，堡垒机用户名称 |  |
| username | 类型：string，堡垒机用户名 | 登录名 |
| email | 类型：string，邮箱 |  |
| wechat | 类型：string，微信 |  |
| phone | 类型：string，电话号码 |  |
| mfa_level | 类型：object，MFA 等级 | {"value":0,"label":"禁用"} 等 |
| source | 类型：object，用户来源 | local/ldap/.../custom |
| wecom_id | 类型：string，企业微信ID | 绑定时存在 |
| dingtalk_id | 类型：string，钉钉ID | 绑定时存在 |
| feishu_id | 类型：string，飞书ID | 绑定时存在 |
| created_by | 类型：string，创建者 |  |
| updated_by | 类型：string，更新者 |  |
| comment | 类型：string，备注 |  |
| groups | 类型：list，用户组 | 对象或ID列表 |
| system_roles | 类型：list，系统角色 | 默认含“用户”角色 |
| org_roles | 类型：list，组织角色 | 默认含“组织用户” |
| password_strategy | 类型：string，密码策略 | email / custom |
| is_service_account | 类型：boolean，是否组件账号 |  |
| is_valid | 类型：boolean，是否有效 |  |
| is_expired | 类型：boolean，是否到期 |  |
| is_active | 类型：boolean，是否启用 |  |
| is_otp_secret_key_bound | 类型：boolean，是否绑定OTP密钥 |  |
| can_public_key_auth | 类型：boolean，是否允许SSH公钥 |  |
| mfa_enabled | 类型：boolean，是否开启MFA |  |
| need_update_password | 类型：boolean，下次登录需改密 |  |
| mfa_force_enabled | 类型：boolean，是否强制MFA |  |
| is_first_login | 类型：boolean，是否第一次登录 |  |
| login_blocked | 类型：boolean，登录阻止 |  |
| date_expired | 类型：string(date-time)，过期时间 |  |
| date_joined | 类型：string(date-time)，加入时间 |  |
| last_login | 类型：string(date-time)，最后登录时间 |  |
| date_updated | 类型：string(date-time)，更新时间 |  |
| date_password_last_updated | 类型：string(date-time)，密码更新时间 |  |

- **请求示例**

**CURL**
```sh
curl -X GET 'https://localhost/api/v1/users/users/USER_ID/' \
    -H 'Authorization: Bearer <token>' \
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
USER_ID     = "your user id"

def get_user_info():
    url = f"{API_URL}/api/v1/users/users/{USER_ID}/"
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
        return response.json()
    except requests.RequestException as e:
        print(f"API 请求失败:{e}")
        return None

if __name__ == "__main__":
    result = get_user_info()
    print(json.dumps(result, indent = 2, ensure_ascii = False))
```

### PUT

- **描述：**
更新用户

- **请求头（Headers）：**

| 键 (Header) | 示例值 | 说明 |
| ----------- | ------ | ---- |
| Authorization | `Bearer <token>` | 认证 Token，需具备修改权限 |
| X-JMS-ORG | `00000000-0000-0000-0000-000000000002` | 组织 ID，缺省 Default |
| Content-Type | `application/json` | 请求/响应体为 JSON |

- **路径参数（Path Params）：**

| 名称 | 说明 | 必填 |
| --- | --- | --- |
| id | 类型：string(UUID)，用户 ID | 是 |

- **请求体参数（Body）：**

| 参数名 | 描述 | 可选值 / 备注 |
| --- | --- | --- |
| name* | 类型：string，名称 / 显示名称 | - |
| username* | 类型：string，用户名（登录名） | - |
| email* | 类型：string，邮箱 | - |
| wechat | 类型：string，微信 | - |
| phone | 类型：string，手机 | - |
| groups | 类型：string[]，用户组 ID 列表 | - |
| password | 类型：string，密码 | password_strategy=custom 时必填 |
| need_update_password | 类型：boolean，下次登录需改密 | 默认 false |
| public_key | 类型：string，SSH 公钥 | - |
| system_roles* | 类型：object[]/string[]，系统角色 | 元素含 pk |
| org_roles* | 类型：object[]/string[]，组织角色 | 元素含 pk |
| password_strategy | 类型：string，密码策略 | email / custom |
| source | 类型：string，用户来源 | default/local/ldap/... |
| mfa_level | 类型：integer，MFA 等级 | 0=禁用 1=启用 2=强制 |
| date_expired | 类型：string(date-time)，用户失效时间 | 2023-02-04T00:54:39.000Z |

> 注：带 * 的参数为必填项。
- **返回参数：**

| 字段名称 | 描述 | 备注 |
| --- | --- | --- |
| id | 类型：string，用户ID | UUID |
| name | 类型：string，堡垒机用户名称 |  |
| username | 类型：string，堡垒机用户名 | 登录名 |
| email | 类型：string，邮箱 |  |
| wechat | 类型：string，微信 |  |
| phone | 类型：string，电话号码 |  |
| mfa_level | 类型：object，MFA 等级 | {"value":0,"label":"禁用"} 等 |
| source | 类型：object，用户来源 | local/ldap/.../custom |
| wecom_id | 类型：string，企业微信ID | 绑定时存在 |
| dingtalk_id | 类型：string，钉钉ID | 绑定时存在 |
| feishu_id | 类型：string，飞书ID | 绑定时存在 |
| created_by | 类型：string，创建者 |  |
| updated_by | 类型：string，更新者 |  |
| comment | 类型：string，备注 |  |
| groups | 类型：list，用户组 | 对象或ID列表 |
| system_roles | 类型：list，系统角色 | 默认含“用户”角色 |
| org_roles | 类型：list，组织角色 | 默认含“组织用户” |
| password_strategy | 类型：string，密码策略 | email / custom |
| is_service_account | 类型：boolean，是否组件账号 |  |
| is_valid | 类型：boolean，是否有效 |  |
| is_expired | 类型：boolean，是否到期 |  |
| is_active | 类型：boolean，是否启用 |  |
| is_otp_secret_key_bound | 类型：boolean，是否绑定OTP密钥 |  |
| can_public_key_auth | 类型：boolean，是否允许SSH公钥 |  |
| mfa_enabled | 类型：boolean，是否开启MFA |  |
| need_update_password | 类型：boolean，下次登录需改密 |  |
| mfa_force_enabled | 类型：boolean，是否强制MFA |  |
| is_first_login | 类型：boolean，是否第一次登录 |  |
| login_blocked | 类型：boolean，登录阻止 |  |
| date_expired | 类型：string(date-time)，过期时间 |  |
| date_joined | 类型：string(date-time)，加入时间 |  |
| last_login | 类型：string(date-time)，最后登录时间 |  |
| date_updated | 类型：string(date-time)，更新时间 |  |
| date_password_last_updated | 类型：string(date-time)，密码更新时间 |  |

- **请求示例**

**CURL**
```sh
curl -X PUT 'https://localhost/api/v1/users/users/USER_ID/' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer <token>' \
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
    -d '{
        "name": "new_name",
        "username": "new_username",
        "password_strategy": "email",
        "email": "user@fit2cloud.com",
        "mfa_level": 1,
        "source": "local",
        "date_expired": "2093-02-05T08:28:41.726694Z",
        "system_roles": [{"pk": "00000000-0000-0000-0000-000000000003"}],
        "org_roles": [{"pk": "00000000-0000-0000-0000-000000000007"}]
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
USER_ID     = "your user id"

def update_user():
    url = f"{API_URL}/api/v1/users/users/{USER_ID}/"
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
    headers = {
        "Content-Type": "application/json",
        "Authorization": f"Bearer {TOKEN}",
        "X-JMS-ORG": ORG_ID
    }

    data = {
        "name": "api_test",
        "username": "api_test_update",
        "password": "apitest",
        "password_strategy": "custom",
        "email": "api_test@fit2cloud.com",
        "mfa_level": 0,
        "source": "local",
        "system_roles": [{"pk": "00000000-0000-0000-0000-000000000003"}],
        "org_roles": [{"pk": "00000000-0000-0000-0000-000000000007"}]
    }

    try:
        response = requests.put(
            url, auth = auth, headers = headers,
            data = json.dumps(data)
        )
        response.raise_for_status()
        print("用户更新成功:")
        print(json.dumps(response.json(), indent=2))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    update_user()
```

### PATCH

- **描述：**
部分更新用户

- **请求头（Headers）：**

| 键 (Header) | 示例值 | 说明 |
| ----------- | ------ | ---- |
| Authorization | `Bearer <token>` | 认证 Token，需具备修改权限 |
| X-JMS-ORG | `00000000-0000-0000-0000-000000000002` | 组织 ID，缺省 Default |
| Content-Type | `application/json` | 请求/响应体为 JSON |

- **路径参数（Path Params）：**

| 名称 | 说明 | 必填 |
| --- | --- | --- |
| id | 类型：string(UUID)，用户 ID | 是 |

- **请求体（Body 可选字段）：**

| 参数名 | 描述 | 备注 |
| --- | --- | --- |
| name | 类型：string，名称 | 选填 |
| email | 类型：string，邮箱 | 选填 |
| wechat | 类型：string，微信 | 选填 |
| phone | 类型：string，手机 | 选填 |
| groups | 类型：string[]，用户组 ID 列表 | 全量替换该列表 |
| password | 类型：string，新密码 | password_strategy=custom 时使用 |
| need_update_password | 类型：boolean，下次登录需改密 |  |
| public_key | 类型：string，SSH 公钥 |  |
| system_roles | 类型：object[]/string[]，系统角色 | 全量替换 |
| org_roles | 类型：object[]/string[]，组织角色 | 全量替换 |
| password_strategy | 类型：string，密码策略 | email/custom |
| source | 类型：string，用户来源 |  |
| mfa_level | 类型：integer，MFA 等级 | 0/1/2 |
| date_expired | 类型：string(date-time)，用户失效时间 |  |

- **返回参数：**

| 字段名称 | 描述 | 备注 |
| --- | --- | --- |
| id | 类型：string，用户ID | UUID |
| name | 类型：string，堡垒机用户名称 |  |
| username | 类型：string，堡垒机用户名 | 登录名 |
| email | 类型：string，邮箱 |  |
| wechat | 类型：string，微信 |  |
| phone | 类型：string，电话号码 |  |
| mfa_level | 类型：object，MFA 等级 | {"value":0,"label":"禁用"} 等 |
| source | 类型：object，用户来源 | local/ldap/.../custom |
| wecom_id | 类型：string，企业微信ID | 绑定时存在 |
| dingtalk_id | 类型：string，钉钉ID | 绑定时存在 |
| feishu_id | 类型：string，飞书ID | 绑定时存在 |
| created_by | 类型：string，创建者 |  |
| updated_by | 类型：string，更新者 |  |
| comment | 类型：string，备注 |  |
| groups | 类型：list，用户组 | 对象或ID列表 |
| system_roles | 类型：list，系统角色 | 默认含“用户”角色 |
| org_roles | 类型：list，组织角色 | 默认含“组织用户” |
| password_strategy | 类型：string，密码策略 | email / custom |
| is_service_account | 类型：boolean，是否组件账号 |  |
| is_valid | 类型：boolean，是否有效 |  |
| is_expired | 类型：boolean，是否到期 |  |
| is_active | 类型：boolean，是否启用 |  |
| is_otp_secret_key_bound | 类型：boolean，是否绑定OTP密钥 |  |
| can_public_key_auth | 类型：boolean，是否允许SSH公钥 |  |
| mfa_enabled | 类型：boolean，是否开启MFA |  |
| need_update_password | 类型：boolean，下次登录需改密 |  |
| mfa_force_enabled | 类型：boolean，是否强制MFA |  |
| is_first_login | 类型：boolean，是否第一次登录 |  |
| login_blocked | 类型：boolean，登录阻止 |  |
| date_expired | 类型：string(date-time)，过期时间 |  |
| date_joined | 类型：string(date-time)，加入时间 |  |
| last_login | 类型：string(date-time)，最后登录时间 |  |
| date_updated | 类型：string(date-time)，更新时间 |  |
| date_password_last_updated | 类型：string(date-time)，密码更新时间 |  |

- **请求示例**

**CURL**
```sh
curl -X PATCH 'https://localhost/api/v1/users/users/USER_ID/' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer <token>' \
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
    -d '{ "name": "partial_update_name" }'
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
USER_ID     = "your user id"

def partial_update_user():
    url = f"{API_URL}/api/v1/users/users/{USER_ID}/"
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
        "username": "api_test_update"
    }

    try:
        response = requests.patch(
            url, auth = auth, headers = headers,
            data = json.dumps(data)
        )
        response.raise_for_status()
        print("用户更新成功:")
        print(json.dumps(response.json(), indent = 2))
    except Exception as e:
        print(f"错误:{e}")

if __name__ == "__main__":
    partial_update_user()
```

### DELETE

- **描述：**
删除用户

- **请求头（Headers）：**

| 键 (Header) | 示例值 | 说明 |
| ----------- | ------ | ---- |
| Authorization | `Bearer <token>` | 认证 Token，需具备删除权限 |
| X-JMS-ORG | `00000000-0000-0000-0000-000000000002` | 组织 ID，缺省 Default |

- **路径参数（Path Params）：**

| 名称 | 说明 | 必填 |
| --- | --- | --- |
| id | 类型：string(UUID)，用户 ID | 是 |

- **返回：** 204 No Content（删除成功无响应体）

- **请求示例**

**CURL**
```sh
curl -X DELETE 'https://localhost/api/v1/users/users/USER_ID/' \
    -H 'Authorization: Bearer <token>' \
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
USER_ID     = "your user id"

def delete_user():
    url = f"{API_URL}/api/v1/users/users/{USER_ID}/"
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
        print("用户删除成功:")
        print(json.dumps(response.json(), indent=2))
    except Exception as e:
        print(f"API 请求失败:{e}")
        return None

if __name__ == "__main__":
    delete_user()
```
