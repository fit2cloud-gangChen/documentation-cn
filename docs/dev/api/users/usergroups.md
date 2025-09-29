# 用户组 API

## **/api/v1/users/groups/{id}/**

=== "POST"

    - **描述：**
    创建用户组

    - **请求头（Headers）：**

    | 键 (Header)      | 示例值                                       | 说明                            |
    | ---------------- | -------------------------------------------- | ------------------------------- |
    | Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
    | X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
    | Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

    - **请求体参数（Body）：**

    | 参数名   | 类型     | 描述     | 是否必选 | 可选值 |
    | -------- | -------- | -------- | -------- | ------ |
    | name     | string   | 名称     | 是       | -      |
    | users    | string[] | 用户     | 否       | -      |

    - **返回参数：**


    | 字段名称       | 数据类型   | 字段描述   | 备注 |
    |----------------|------------|------------|------|
    | id             | String     | 用户组id   |      |
    | name           | String     | 用户组名称 |      |
    | comment        | String     | 备注       |      |
    | created_by     | String     | 创建者     |      |
    | users          | String[]   | 用户       |      |
    | users_amount   | String     | 用户数量   |      |
    | org_id         | String     | 组织       |      |
    | org_name       | String     | 组织名称   |      |
    | date_created   | String     | 创建时间   |      |


    !!! tip "请求示例"
        ```sh
        curl -X POST 'https://localhost/api/v1/users/groups/cdd61c0e-2012-416a-bde3-f2c642ded82a/' \
            -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \ 
            -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002'
        ```

=== "DELETE"

    - **描述：**
    删除用户组

    - **请求头（Headers）：**

    | 键 (Header)      | 示例值                                       | 说明                            |
    | ---------------- | -------------------------------------------- | ------------------------------- |
    | Authorization     | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
    | X-JMS-ORG         | `00000000-0000-0000-0000-000000000002`         | 组织 ID，不传则默认归属 `Default` 组织 |
    | Content-Type      | `application/json`                            | 请求/响应体为 JSON 格式           |

    - **请求体参数（Body）：**
     
    | 字段名称 | 类型 | 描述 | 备注 |
    | -------- | ---- | ---- | ---- |
    | None     |      |      |      |


    !!! tip "请求示例"
        ```sh
        curl -X DELETE 'https://localhost/api/v1/users/groups/7413d36d-cf37-45f4-b42a-cd5eeff1f4ff/' \
            -H 'Content-Type:application/json' \
            -H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
            -H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002'
        ```

