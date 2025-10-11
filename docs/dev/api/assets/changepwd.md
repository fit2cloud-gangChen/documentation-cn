## /api/v1/accounts/change-secret-automations/

=== "GET"
    - **描述：**
    查询改密计划

    - **请求头（Headers）：** 
   
    | 键              | 值                                      | 备注                                                                 |
    |-----------------|-----------------------------------------|----------------------------------------------------------------------|
    | Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4为管理员的token信息。        |
    | X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002为组织ID，此id号为默认组织：Default，留空则默认为 Default 组织。 |
    | Content-Type    | application/json                        | 输出为json格式                                                       |


    !!! tip "请求示例"

        === "CURL"
            ```sh  
            curl -X GET 'https://localhost/api/v1/accounts/change-secret-automations/?offset=0&limit=15' \
                -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
                -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002'
            ```

        === "Python"
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

            def search_change_secret_automations(keyword):
                url = f"{API_URL}/api/v1/accounts/change-secret-automations/"
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
                        print(f"未找到改密计划")
                    else:
                        print(f"查询到 {len(nodes_data)} 个匹配的改密计划：")
                        print(json.dumps(nodes_data, indent = 2, ensure_ascii = False))
                except Exception as e:
                    print(f"错误:{e}")

            if __name__ == "__main__":
                search_change_secret_automations(SEARCH_WORD)
            ```

    - **返回参数:**
  
    | 字段名称         | 数据类型          | 字段描述                 | 备注                |
    |------------------|-------------------|--------------------------|---------------------|
    | count            | Int               | 总数                     |                     |
    | next             | String            | 下一页链接               |                     |
    | previous         | String            | 上一页链接               |   |
    | results          | List              | 数据                     |                     |
    | id               | String            | id                       |                     |
    | name             | String            | 名称                     |                     |
    | accounts         | String[]          | 资产账户名               |                     |
    | assets           | List[Object]      | 资产ID                   |                     |
    | nodes            | List[Object]      | 资产节点                 |                     |
    | is_active        | Boolean           | 是否激活                 |                     |
    | is_periodic      | Boolean           | 是否定期执行             |                     |
    | crontab          | String            | 定期执行crontab表达式     |                     |
    | interval         | String            | 周期执行                 |                     |
    | secret_strategy  | String            | 密文生成策略             |                     |
    | secret_type      | Object            | 密文类型                 |                     |
    | secret           | String            | 密码                     |                     |
    | password_rules   | Object            | 随机密码长度             |                     |
    | recipients       | List[Object]      | 收件人                   |                     |
    | org_id           | String            | 组织ID                   |                     |
    | org_name         | String            | 组织名称                 |                     |
    | comment          | String            | 备注                     |                     |
    | date_created     | String[date]      | 创建时间                 |                     |
    | date_updated     | String[date]      | 更新时间                 |                     |
    | created_by       | String            | 创建人                   |                     |


=== "POST"
    - **描述：**
    创建改密计划


    - **请求头（Headers）：** 

    | 键              | 值                                      | 备注                                                                 |
    |-----------------|-----------------------------------------|----------------------------------------------------------------------|
    | Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4为管理员的token信息。        |
    | X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002为组织ID，此id号为默认组织：Default，留空则默认为 Default 组织。 |
    | Content-Type    | application/json                        | 输出为json格式                                                       |


    !!! tip "请求示例"

        === "CURL"
            ```sh  
            curl -X POST 'https://localhost/api/v1/accounts/change-secret-automations/' \
                -H 'Content-Type: application/json' \
                -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
                -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
                -d '{
                    "accounts": ["root"],
                    "secret_strategy": "random",
                    "secret_type": "password",
                    "password_rules": {
                        "length": "16"
                    },
                    "is_periodic": true,
                    "interval": 24,
                    "is_active": true,
                    "name": "test",
                    "assets": ["9266b1f8-f74d-482c-805a-6eed0e099a42"],
                    "comment": "test"
                }'
            ```

        === "Python"
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

            def create_change_secret_automations():
                url = f"{API_URL}/api/v1/accounts/change-secret-automations/"
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
                    "accounts": ["root"],
                    "secret_strategy": "random",
                    "secret_type": "password",
                    "password_rules": {
                        "length": "16"
                    },
                    "is_periodic": True,
                    "interval": 24,
                    "is_active": True,
                    "name": "test",
                    "assets": [ASSET_ID],
                    "comment": "test"
                }

                try:
                    response = requests.post(
                        url, auth = auth, headers = headers,
                        data = json.dumps(data)
                    )
                    response.raise_for_status()
                    print("改密计划创建成功:")
                    print(json.dumps(response.json(), indent = 2))
                except Exception as e:
                    print(f"错误:{e}")

            if __name__ == "__main__":
                create_change_secret_automations()
            ```

    - **返回参数:**

    | 字段名称         | 数据类型          | 字段描述                 | 备注 |
    |------------------|-------------------|--------------------------|------|
    | id               | String            | id                       |      |
    | name             | String            | 名称                     |      |
    | accounts         | String[]          | 资产账户名               |      |
    | assets           | List[Object]      | 资产ID                   |      |
    | nodes            | List[Object]      | 资产节点                 |      |
    | is_active        | Boolean           | 是否激活                 |      |
    | is_periodic      | Boolean           | 是否定期执行             |      |
    | crontab          | String            | 定期执行crontab表达式     |      |
    | interval         | String            | 周期执行                 |      |
    | secret_strategy  | String            | 密文生成策略             |      |
    | secret_type      | Object            | 密文类型                 |      |
    | secret           | String            | 密码                     |      |
    | password_rules   | Object            | 随机密码长度             |      |
    | recipients       | List[Object]      | 收件人                   |      |
    | org_id           | String            | 组织ID                   |      |
    | org_name         | String            | 组织名称                 |      |
    | comment          | String            | 备注                     |      |
    | date_created     | String[date]      | 创建时间                 |      |
    | date_updated     | String[date]      | 更新时间                 |      |
    | created_by       | String            | 创建人                   |      |


## /api/v1/accounts/change-secret-automations/{id}/
=== "PUT"
    - **描述：**
    更新改密计划

    - **请求头（Headers）：** 

    | 键              | 值                                      | 备注                                                                 |
    |-----------------|-----------------------------------------|----------------------------------------------------------------------|
    | Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4为管理员的token信息。        |
    | X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002为组织ID，此id号为默认组织：Default，留空则默认为 Default 组织。 |
    | Content-Type    | application/json                        | 输出为json格式                                                       |

    - **请求体参数（Body）：** 
    
    | 参数名           | 类型       | 描述                                  | 是否必选 | 默认值                                  |
    |------------------|------------|---------------------------------------|----------|-----------------------------------------|
    | id               | String     | 改密计划ID                            | 是       | -                                       |
    | name             | String     | 名称                                  | 是       | -                                       |
    | accounts         | String[]   | 资产账户名                            | 是       | -                                       |
    | assets           | String[]   | 资产ID                                | 否       | -                                       |
    | nodes            | string[]   | 资产节点ID                            | 否       | -                                       |
    | is_active        | Boolean    | 是否激活                              | 是       | -                                       |
    | is_periodic      | Boolean    | 是否定期执行                          | 是       | -                                       |
    | crontab          | String     | 定期执行crontab表达式                 | 否       | is_periodic=true时填写                  |
    | interval         | String     | 周期执行                              | 否       | is_periodic=true时填写，默认24          |
    | secret_strategy  | String     | 密文生成策略                          | 是       | 默认: 指定specific；随机：random        |
    | secret_type      | String     | 密文类型                              | 是       | 默认：password；可选值：ssh_key         |
    | password_rules   | int        | 随机密码长度                          | 否       | 默认30；secret_strategy=random且secret_type=password时必填 |
    | secret           | String     | 密码                                  | 否       | secret_strateg=specific且secret_type=password时必填 |
    | comment          | String     | 备注                                  | 否       | -                                       |

    !!! tip "请求示例"

        === "CURL"
            ```sh  
            curl -X PUT 'https://localhost/api/v1/accounts/change-secret-automations/0a6a2e40-f92b-4aca-94ef-5f6ae5b0966c/' \
            -H 'Content-Type: application/json' \
            -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
            -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
            -d '{
                "accounts": ["root"],
                "secret_strategy": "random",
                "secret_type": "password",
                "password_rules": {
                    "length": "16"
                },
                "is_periodic": true,
                "interval": 24,
                "is_active": true,
                "name": "test_update",
                "assets": ["9266b1f8-f74d-482c-805a-6eed0e099a42"],
                "comment": "test"
            }'
            ```

        === "Python"
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
            MIS_ID      = "your mission id"

            def update_change_secret_automations():
                url = f"{API_URL}/api/v1/accounts/change-secret-automations/{MIS_ID}/"
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
                    "accounts": ["root"],
                    "secret_strategy": "random",
                    "secret_type": "password",
                    "password_rules": {
                        "length": "16"
                    },
                    "is_periodic": True,
                    "interval": 24,
                    "is_active": True,
                    "name": "test",
                    "assets": [ASSET_ID],
                    "comment": "test"
                }

                try:
                    response = requests.put(
                        url, auth = auth, headers = headers,
                        data = json.dumps(data)
                    )
                    response.raise_for_status()
                    print("改密计划更新成功:")
                    print(json.dumps(response.json(), indent = 2))
                except Exception as e:
                    print(f"错误:{e}")

            if __name__ == "__main__":
                update_change_secret_automations()
            ```

    - **返回参数:**

    | 字段名称         | 数据类型          | 字段描述                 | 备注 |
    |------------------|-------------------|--------------------------|------|
    | id               | String            | id                       |      |
    | name             | String            | 名称                     |      |
    | accounts         | String[]          | 资产账户名               |      |
    | assets           | List[Object]      | 资产ID                   |      |
    | nodes            | List[Object]      | 资产节点                 |      |
    | is_active        | Boolean           | 是否激活                 |      |
    | is_periodic      | Boolean           | 是否定期执行             |      |
    | crontab          | String            | 定期执行crontab表达式     |      |
    | interval         | String            | 周期执行                 |      |
    | secret_strategy  | String            | 密文生成策略             |      |
    | secret_type      | Object            | 密文类型                 |      |
    | secret           | String            | 密码                     |      |
    | password_rules   | Object            | 随机密码长度             |      |
    | recipients       | List[Object]      | 收件人                   |      |
    | org_id           | String            | 组织ID                   |      |
    | org_name         | String            | 组织名称                 |      |
    | comment          | String            | 备注                     |      |
    | date_created     | String[date]      | 创建时间                 |      |
    | date_updated     | String[date]      | 更新时间                 |      |
    | created_by       | String            | 创建人                   |      |

=== "DELETE"
    - **描述：**
    删除改密计划

    - **请求头（Headers）：**  

    | 键 (Header)      | 示例值                                       | 说明                            |
    | ---------------- | -------------------------------------------- | ------------------------------- |
    | Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
    | X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
    | Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

    - **请求体参数（Body）：**  

    | 参数名 | 类型   | 描述       | 是否必选 | 默认值 |
    |--------|--------|------------|----------|--------|
    | id     | String | 改密计划ID | 是       | -      |

    !!! tip "请求示例"

        === "CURL"
            ```sh  
            curl -X DELETE 'https://localhost/api/v1/accounts/change-secret-automations/0a6a2e40-f92b-4aca-94ef-5f6ae5b0966c/' \
                -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
                -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002'
            ```

        === "Python"
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
            MIS_ID      = "your mission id"

            def delete_change_secret_automations():
                url = f"{API_URL}/api/v1/accounts/change-secret-automations/{MIS_ID}/"
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
                    print("改密计划删除成功")
                except Exception as e:
                    print(f"错误:{e}")

            if __name__ == "__main__":
                delete_change_secret_automations()
            ```

## /api/v1/accounts/change-secret-executions/

=== "POST"
- **描述：**
执行改密计划

- **请求头（Headers）：**  

| 键 (Header)      | 示例值                                       | 说明                            |
| ---------------- | -------------------------------------------- | ------------------------------- |
| Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
| X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
| Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

- **请求体参数（Body）：**  

| 参数名     | 类型   | 描述       | 是否必选 | 默认值 |
|------------|--------|------------|----------|--------|
| automation | String | 改密计划ID | 是       | -      |


!!! tip "请求示例"

    === "CURL"
        ```sh
        curl -X POST 'https://localhost/api/v1/accounts/change-secret-executions/' \
            -H 'Content-Type: application/json' \
            -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
            -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
            -d '{
                "automation": "bc778562-630e-4c89-971a-3ec629d4fd3f"
            }'
        ```
    
    === "Python"
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
        MIS_ID      = "your mission id"

        def automations_executions():
            url = f"{API_URL}/api/v1/accounts/change-secret-executions/"
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
                "automation": MIS_ID
            }
            
            try:
                response = requests.post(
                    url, auth = auth, headers = headers,
                    data = json.dumps(data),
                    verify = False
                )
                response.raise_for_status()
                print(f"改密计划已执行")
            except Exception as e:
                print(f"错误:{e}")

        if __name__ == "__main__":
            automations_executions()
        ```

- **返回参数:**

| 字段名称 | 数据类型 | 字段描述 | 备注 |
|----------|----------|----------|------|
| task     | String   | 任务id   |      |
