## **/api/v1/assets/nodes/**

=== "GET"
    - **描述：**
    查询节点

    - **请求头（Headers）：**

    | 键 (Header)      | 示例值                                       | 说明                            |
    | ---------------- | -------------------------------------------- | ------------------------------- |
    | Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
    | X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
    | Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

    - **请求体参数（Body）：**

    | 参数名 | 类型   | 描述               | 是否必选 | 可选值 |
    |--------|--------|--------------------|----------|--------|
    | search | String | 搜索词             | 否       | -      |
    | limit  | int    | 每一页显示条数，支持节点名搜索 | 是       | -      |
    | offset | int    | 分页偏移量         | 是       | -      |

    - **返回参数：**

    | 字段名称   | 数据类型   | 字段描述       | 备注 |
    |------------|------------|----------------|------|
    | id         | String     | 节点id         |      |
    | key        | String     | 键             |      |
    | value      | String     | 值\节点名称    |      |
    | org_id     | String     | 组织           |      |
    | name       | String[]   | 用户           |      |
    | full_value | String     | 全称           |      |
    | org_name   | String     | 组织名称       |      |

    !!! tip "请求示例"
        ```sh
        curl -X GET 'https://localhost/api/v1/assets/nodes/?search=/Default/xx公司' \ 
        -H 'Content-Type:application/json' \ 
        -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \ 
        -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002'
        ```
##  /api/v1/assets/nodes/{id}/children/
=== "POST"
    - **描述：**
    创建节点

    - **请求头（Headers）：**

    | 键 (Header)      | 示例值                                       | 说明                            |
    | ---------------- | -------------------------------------------- | ------------------------------- |
    | Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
    | X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
    | Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

    - **路径参数（Path Params）：**

    | 名称 | 类型   | 说明     | 必填 |
    | ---- | ------ | -------- | ---- |
    | id   | string | 节点ID   | 是   |

    - **请求体参数（Body）：**

    | 参数名 | 类型   | 描述     | 是否必选 | 可选值 |
    |--------|--------|----------|----------|--------|
    | value  | String | 节点名称 | 是       | -      |

    - **返回参数：**

    | 字段名称   | 数据类型   | 字段描述       | 备注 |
    |------------|------------|----------------|------|
    | id         | String     | 节点id         |      |
    | key        | String     | 键             |      |
    | value      | String     | 值\节点名称    |      |
    | org_id     | String     | 组织           |      |
    | name       | String[]   | 用户           |      |
    | full_value | String     | 全称           |      |
    | org_name   | String     | 组织名称       |      |

    !!! tip "请求示例"
        ```sh
        curl -X POST 'https://localhost/api/v1/assets/nodes/3728f004-99a2-4fca-9577-84d5ffcf9eff/children/' \ 
        -H 'Content-Type:application/json' \ 
        -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \ 
        -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \ 
        -d '{"value":"test_create_node"}'
        ```
##  /api/v1/users/groups/{id}/ 
=== "DELETE"
    - **描述：**
    删除节点

    - **请求头（Headers）：**

    | 键 (Header)      | 示例值                                       | 说明                            |
    | ---------------- | -------------------------------------------- | ------------------------------- |
    | Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
    | X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
    | Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

    - **路径参数（Path Params）：**

    | 名称 | 类型   | 说明     | 必填 |
    | ---- | ------ | -------- | ---- |
    | id   | string | 节点ID   | 是   |


    !!! tip "请求示例"
        ```sh
        curl -X DELETE 'https://localhost/api/v1/assets/nodes/89c68ef6-7790-4f20-8f8d-fdd76d229b3d/' \ 
        -H 'Content-Type:application/json' \ 
        -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \ 
        -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \ 
        -d '{"value":"test_create_node"}'
        ```
