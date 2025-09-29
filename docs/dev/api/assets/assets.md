## /api/v1/assets/hosts/

=== "POST"
- **描述：**
创建主机资产

- **请求头（Headers）：**  

| 键 (Header)      | 示例值                                       | 说明                            |
| ---------------- | -------------------------------------------- | ------------------------------- |
| Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
| X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
| Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

- **请求体参数（Body）：**  

| 参数名     | 类型       | 描述                                                                 | 是否必选 | 默认值  |
|------------|------------|----------------------------------------------------------------------|----------|---------|
| name       | String     | 名称                                                                 | 是       | -       |
| addrs      | String     | IP地址                                                               | 是       | -       |
| platform   | String     | 系统平台，示例值：1（代表Linux）、5（代表Windows）                    | 是       | -       |
| nodes      | String[]   | 节点                                                                 | 是       | -       |
| protocols  | String[]   | 协议/端口，参数格式示例：`{"name": "ssh", "port": 22}`                | 否       | -       |
| labels     | String[]   | 标签                                                                 | 否       | -       |
| is_active  | Boolean    | 激活状态                                                             | 否       | true    |
| accounts   | String[]   | 账号信息                                                             | 否       | -       |



!!! tip "请求示例"  
    ```sh  
    curl -X POST 'https://localhost/api/v1/assets/hosts/' \  
        -H 'Content-Type:application/json' \  
        -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \  
        -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \  
        -d '{  
            "name":"test_create_asset",  
            "address":"192.168.1.1",  
            "platform": {  
                "pk":1  
            },  
            "nodes": [  
                {  
                    "pk":"1ecb988f-ded3-4b57-bc8f-808467abbe2f"  
                }  
            ],  
            "protocols": [  
                {  
                    "name":"ssh",  
                    "port": 22  
                }  
            ],  
            "labels":[],  
            "is_active":true,  
            "accounts":[]  
        }'  
    ```

- **返回参数：**
| 字段名称       | 数据类型          | 字段描述       | 备注 |
|----------------|-------------------|----------------|------|
| id             | String            | id             |      |
| name           | String            | 名称           |      |
| addrs          | String            | ip             |      |
| comment        | String            | 备注           |      |
| platform       | String            | 系统平台       |      |
| nodes          | String[]          | 节点           |      |
| labels         | String[]          | 标签           |      |
| protocols      | String            | 协议/端口      |      |
| nodes_display  | String[]          | 资产节点名称   |      |
| category       | String            | 类别           |      |
| type           | String            | 类型           |      |
| is_active      | Boolean           | 激活状态       |      |
| date_created   | String(date-time) | 创建时间       |      |
| org_id         | String            | 组织           |      |
| org_name       | String            | 组织名称       |      |


## /api/v1/assets/hosts/{id}/
=== "DELETE"
- 描述：
删除主机资产接口

- **请求头（Headers）：**  

| 键 (Header)      | 示例值                                       | 说明                            |
| ---------------- | -------------------------------------------- | ------------------------------- |
| Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
| X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
| Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

- **路径参数（Path Params）：**  

| 名称 | 类型   | 说明     | 必填 |
| ---- | ------ | -------- | ---- |
| id   | string | 资产ID   | 是   |

!!! tip "请求示例"  
    ```sh  
    curl -X DELETE 'https://localhost/api/v1/assets/hosts/1b56ff93-18e9-478e-97c2-8b6a3720b95d/' \  
        -H 'Content-Type:application/json' \  
        -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \  
        -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002'  
    ```