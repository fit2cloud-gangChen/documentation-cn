## /api/v1/accounts/integration-applications/

### GET
- **描述：**
获取集成应用列表，支持分页、排序和关键词搜索。

- **请求头（Headers）：**

| 键 | 值 | 备注 |
| --- | --- | --- |
| Authorization | Bearer b96810faac725563304dada8c323c4fa061863d4 | 管理员的 token 信息。 |
| X-JMS-ORG | 00000000-0000-0000-0000-000000000002 | 组织 ID，留空则默认为 Default 组织 |
| Content-Type | application/json | 请求/响应体为 JSON 格式 |

- **查询参数（Query Params）：**

| 参数名 | 描述 | 默认值 |
| --- | --- | --- |
| limit | 类型：Int，每页返回数量 | - |
| offset | 类型：Int，分页偏移量 | - |
| order | 类型：String，排序字段 | - |
| search | 类型：String，搜索关键词 | - |

- **返回参数：**

| 字段名称 | 字段描述 | 备注 |
| --- | --- | --- |
| count | 类型：Int，总记录数 |  |
| next | 类型：String，下页链接 | 无更多数据时为 null |
| previous | 类型：String，上页链接 | 无上一页时为 null |
| results | 类型：List[Object]，集成应用列表 | 见下表 |

**results[] 字段说明**

| 字段名称 | 字段描述 | 备注 |
| --- | --- | --- |
| id | 类型：String(UUID)，集成应用 ID |  |
| name | 类型：String，集成应用名称 |  |
| logo | 类型：String(URL)，应用 Logo |  |
| accounts | 类型：Object，授权账号信息 | 返回结构与后端配置一致 |
| ip_group | 类型：String[]，允许访问的 IP 范围 | 默认值为 ["*"] |
| accounts_amount | 类型：Int，授权账号数量 | 只读字段 |
| org_id | 类型：String，组织 ID | 只读字段 |
| org_name | 类型：String，组织名称 | 只读字段 |
| is_active | 类型：Boolean，是否启用 |  |
| date_last_used | 类型：String(date-time)，最近使用时间 | 可能为 null |
| date_created | 类型：String(date-time)，创建时间 | 只读字段 |
| date_updated | 类型：String(date-time)，更新时间 | 只读字段 |
| comment | 类型：String，备注 |  |

- **请求示例**

**CURL**
```sh
curl -X GET 'https://localhost/api/v1/accounts/integration-applications/?offset=0&limit=15' \
	-H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
	-H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002'
```

### POST
- **描述：**
创建集成应用，需指定名称、授权账号等信息。

- **请求头（Headers）：**

| 键 | 值 | 备注 |
| --- | --- | --- |
| Authorization | Bearer b96810faac725563304dada8c323c4fa061863d4 | 管理员的 token 信息。 |
| X-JMS-ORG | 00000000-0000-0000-0000-000000000002 | 组织 ID，留空则默认为 Default 组织 |
| Content-Type | application/json | 请求/响应体为 JSON 格式 |

- **请求体参数（Body）：**

| 参数名 | 描述 | 默认值 |
| --- | --- | --- |
| name* | 类型：String，集成应用名称 | - |
| logo | 类型：String(URL)，应用 Logo 地址 | - |
| accounts* | 类型：Object，授权账号配置 | 请参阅后端定义 |
| ip_group | 类型：String[]，允许访问的 IP 范围 | ["*"] |
| is_active | 类型：Boolean，是否启用 | true |
| comment | 类型：String，备注说明 | - |

> 注：带 * 的参数为必填项。

- **返回参数：**

返回字段同 GET 接口中 `results[]` 字段说明。

- **请求示例**

**CURL**
```sh
curl -X POST 'https://localhost/api/v1/accounts/integration-applications/' \
	-H 'Authorization: Bearer b96810faac725563304dada8c323c4fa061863d4' \
	-H 'X-JMS-ORG: 00000000-0000-0000-0000-000000000002' \
	-H 'Content-Type: application/json' \
	-d '{
		"name": "auto-sync",
		"accounts": {"ids": ["account-id-1"]},
		"ip_group": ["*"]
	}'
```

## /api/v1/accounts/integration-applications/{id}/

### GET
- **描述：**
根据集成应用 ID 获取详情。

- **请求头（Headers）：**

| 键 | 值 | 备注 |
| --- | --- | --- |
| Authorization | Bearer b96810faac725563304dada8c323c4fa061863d4 | 管理员的 token 信息。 |
| X-JMS-ORG | 00000000-0000-0000-0000-000000000002 | 组织 ID，留空则默认为 Default 组织 |
| Content-Type | application/json | 请求/响应体为 JSON 格式 |

- **路径参数（Path Params）：**

| 名称 | 描述 | 备注 |
| --- | --- | --- |
| id* | 类型：String(UUID)，集成应用 ID | - |

> 注：带 * 的参数为必填项。

- **返回参数：**

返回字段同 GET 列表接口中 `results[]` 字段说明。

### PUT / PATCH
- **描述：**
更新集成应用信息，可使用 PUT 覆盖或 PATCH 局部更新。

- **请求头（Headers）：**

| 键 | 值 | 备注 |
| --- | --- | --- |
| Authorization | Bearer b96810faac725563304dada8c323c4fa061863d4 | 管理员的 token 信息。 |
| X-JMS-ORG | 00000000-0000-0000-0000-000000000002 | 组织 ID，留空则默认为 Default 组织 |
| Content-Type | application/json | 请求/响应体为 JSON 格式 |

- **路径参数（Path Params）：**

| 名称 | 描述 | 备注 |
| --- | --- | --- |
| id* | 类型：String(UUID)，集成应用 ID | - |

- **请求体参数（Body）：**

与创建接口相同，PUT 需提供全部必填字段，PATCH 仅需提供需变更字段。

> 注：带 * 的参数为必填项。

- **返回参数：**

返回字段同 GET 列表接口中 `results[]` 字段说明。

### DELETE
- **描述：**
删除指定集成应用。

- **请求头（Headers）：**

| 键 | 值 | 备注 |
| --- | --- | --- |
| Authorization | Bearer b96810faac725563304dada8c323c4fa061863d4 | 管理员的 token 信息。 |
| X-JMS-ORG | 00000000-0000-0000-0000-000000000002 | 组织 ID，留空则默认为 Default 组织 |

- **路径参数（Path Params）：**

| 名称 | 描述 | 备注 |
| --- | --- | --- |
| id* | 类型：String(UUID)，集成应用 ID | - |

> 注：带 * 的参数为必填项。

- **返回参数：**

删除成功返回 204，无返回体。

## /api/v1/accounts/integration-applications/{id}/secret/

### GET
- **描述：**
获取集成应用一次性密文，仅在首次调用时返回敏感内容。

- **请求头（Headers）：**

| 键 | 值 | 备注 |
| --- | --- | --- |
| Authorization | Bearer b96810faac725563304dada8c323c4fa061863d4 | 管理员的 token 信息。 |
| X-JMS-ORG | 00000000-0000-0000-0000-000000000002 | 组织 ID，留空则默认为 Default 组织 |
| Content-Type | application/json | 请求/响应体为 JSON 格式 |

- **路径参数（Path Params）：**

| 名称 | 描述 | 备注 |
| --- | --- | --- |
| id* | 类型：String(UUID)，集成应用 ID | - |

> 注：带 * 的参数为必填项。

- **返回参数：**

返回字段同 GET 列表接口中 `results[]` 字段说明，敏感信息仅在首次请求时返回。

## /api/v1/accounts/integration-applications/account-secret/

### GET
- **描述：**
获取集成应用关联账号的脱敏信息，用于审计或外部系统同步。

- **请求头（Headers）：**

| 键 | 值 | 备注 |
| --- | --- | --- |
| Authorization | Bearer b96810faac725563304dada8c323c4fa061863d4 | 管理员的 token 信息。 |
| X-JMS-ORG | 00000000-0000-0000-0000-000000000002 | 组织 ID，留空则默认为 Default 组织 |

- **返回参数：**

| 字段名称 | 字段描述 | 备注 |
| --- | --- | --- |
| asset | 类型：String，资产名称 |  |
| asset_id | 类型：String(UUID)，资产 ID | 可能为空 |
| account | 类型：String，账号名称 |  |
| account_id | 类型：String(UUID)，账号 ID | 可能为空 |

