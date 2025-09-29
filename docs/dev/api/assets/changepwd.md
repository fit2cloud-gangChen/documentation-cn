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
        ```sh  
        curl -X GET 'https://localhost/api/v1/accounts/change-secret-automations/?offset=0&limit=15' \  
            -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \  
            -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002'  
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
                "ssh_key_change_strategy": "add",  
                "is_periodic": true,  
                "interval": 24,  
                "is_active": true,  
                "name": "test",  
                "assets": ["9266b1f8-f74d-482c-805a-6eed0e099a42"],  
                "nodes": [{  
                    "pk": "90cdb498-9167-4308-8087-cd27b953f5fb"  
                }],  
                "crontab": "0 0 * */1 *",  
                "recipients": [{  
                    "pk": "d1a02c44-e40b-41ac-884a-c44f3c664209"  
                }],  
                "comment": "test"  
            }'  
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
        ```sh  
        curl -X PUT 'https://localhost/api/v1/accounts/change-secret-automations/0a6a2e40-f92b-4aca-94ef-5f6ae5b0966c' \  
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
            "ssh_key_change_strategy": "add",  
            "is_periodic": true,  
            "interval": 24,  
            "is_active": true,  
            "name": "test",  
            "assets": ["9266b1f8-f74d-482c-805a-6eed0e099a42"],  
            "nodes": [{  
                "pk": "90cdb498-9167-4308-8087-cd27b953f5fb"  
            }],  
            "crontab": "0 0 * */1 *",  
            "recipients": [{  
                "pk": "d1a02c44-e40b-41ac-884a-c44f3c664209"  
            }],  
            "comment": "test"  
        }'  
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
        ```sh  
        curl -X DELETE 'https://localhost/api/v1/accounts/change-secret-automations/0a6a2e40-f92b-4aca-94ef-5f6ae5b0966c' \  
            -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \  
            -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002'  
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
    ```sh  
    curl -X POST 'https://localhost/api/v1/accounts/change-secret-executions/' \  
        -H 'Content-Type: application/json' \  
        -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \  
        -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \  
        -d '{  
            "automation": "bc778562-630e-4c89-971a-3ec629d4fd3f"  
        }'  
    ```

- **返回参数:**

| 字段名称 | 数据类型 | 字段描述 | 备注 |
|----------|----------|----------|------|
| task     | String   | 任务id   |      |
