## /api/v1/accounts/account-backup-plans/

=== "GET"
    - **描述：**
    查询账号备份策略

    - **请求头（Headers）：**  

    | 键              | 值                                      | 备注                                                                 |
    |-----------------|-----------------------------------------|----------------------------------------------------------------------|
    | Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4 为管理员的token 信息。       |
    | X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002 为组织 ID，此 id号为默认组织：Default，留空则默认为 Default 组织。 |
    | Content-Type    | application/json                        | 输出为json格式                                                       |

    - **请求体参数（Body）：**  
  
    | 参数名 | 类型   | 描述           | 是否必选 | 默认值 |
    |--------|--------|----------------|----------|--------|
    | name   | String | 名称           | 否       | -      |
    | limit  | int    | 每一页显示条数 | 是       | -      |
    | offset | int    | 分页偏移量     | 是       | -      |

    !!! tip "请求示例"  
        ```sh  
            curl -X POST 'https://localhost/api/v1/accounts/account-backup-plans/?offset=0&limit=15' \  
            -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \  
            -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002'  
        ```
        
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


=== "POST"
    - **描述：**
    创建账号备份策略

    - **请求头（Headers）：**  

    | 键              | 值                                      | 备注                                                                 |
    |-----------------|-----------------------------------------|----------------------------------------------------------------------|
    | Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4 为管理员的token 信息。       |
    | X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002 为组织 ID，此 id号为默认组织：Default，留空则默认为 Default 组织。 |
    | Content-Type    | application/json                        | 输出为json格式                                                       |

    - **请求体参数（Body）：**  

    | 参数名 | 类型   | 描述           | 是否必选 | 默认值 |
    |--------|--------|----------------|----------|--------|
    | name   | String | 名称           | 否       | -      |
    | limit  | int    | 每一页显示条数 | 是       | -      |
    | offset | int    | 分页偏移量     | 是       | -      |

    !!! tip "请求示例"
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


## /api/v1/accounts/account-backup-plans/{id}
=== "DELETE"
- **描述：**
删除账号备份策略

- **请求头（Headers）：**  

| 键              | 值                                      | 备注                                                                 |
|-----------------|-----------------------------------------|----------------------------------------------------------------------|
| Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4 为管理员的token 信息。       |
| X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002 为组织 ID，此 id号为默认组织：Default，留空则默认为 Default 组织。 |
| Content-Type    | application/json                        | 输出为json格式                                                       |

- **请求体参数（Body）：**  

| 参数名 | 类型   | 描述       | 是否必选 | 默认值 |
|--------|--------|------------|----------|--------|
| id     | String | 备份策略ID | 是       | -      |

!!! tip "请求示例"
    ```sh  
    curl -X DELETE 'https://localhost/api/v1/accounts/account-backup-plans/37e4c30a-71ea-4b58-91f3-a692715dcf99/' \  
    -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \  
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002'  
    ```

## /api/v1/accounts/account-backup-plan-executions/

=== "POST"
- **描述：**
执行账号备份策略


- **请求头（Headers）：**  

| 键              | 值                                      | 备注                                                                 |
|-----------------|-----------------------------------------|----------------------------------------------------------------------|
| Authorization   | Bearer b96810faac725563304dada8c323c4fa061863d4 | b96810faac725563304dada8c323c4fa061863d4 为管理员的token 信息。       |
| X-JMS-ORG       | 00000000-0000-0000-0000-000000000002    | 00000000-0000-0000-0000-000000000002 为组织 ID，此 id号为默认组织：Default，留空则默认为 Default 组织。 |
| Content-Type    | application/json                        | 输出为json格式               

- **请求体参数（Body）：**  

| 参数名 | 类型   | 描述       | 是否必选 | 默认值 |
|--------|--------|------------|----------|--------|
| plan   | String | 备份策略ID | 是       | -      |

!!! tip "请求示例"
    ```sh  
    curl -X POST 'https://localhost/api/v1/accounts/account-backup-plan-executions/' \  
    -H 'Content-Type: application/json' \  
    -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \  
    -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \  
    -d '{  
        "plan": "37e4c30a-71ea-4b58-91f3-a692715dcf99"  
    }'  
    ```
- **返回参数：**

| 字段名称 | 数据类型 | 字段描述 | 备注 |
|----------|----------|----------|------|
| task     | String   | 任务id   |      |