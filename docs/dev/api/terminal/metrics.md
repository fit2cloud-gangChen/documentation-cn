## /api/v1/terminal/components/metrics/

### GET
- **描述：**

终端组件 (Terminal Components) 指标状态，返回各组件类型的数量、在线情况与会话活跃数，以及按严重级别分类的主机名列表。

- **请求头（Headers）：**

| 键 (Header) | 示例值 | 说明 |
| ----------- | ------ | ---- |
| Authorization | `Bearer b96810faac725563304dada8c323c4fa061863d4` | 认证 Token，示例为管理员 token；格式固定为 `Bearer <token>` |
| Content-Type | `application/json` | 请求/响应体为 JSON 格式 |

- **返回参数：** 

| 字段 | 说明 |
| --- | --- |
| total | 类型：integer，该类型组件总数量 |
| type | 类型：string，组件类型标识，如 `core`, `lion`, `koko`, `celery`, `video_worker` 等 |
| session_active | 类型：integer，当前活跃会话数（与会话相关组件可能为 0） |
| high | 类型：array[string]，高风险 / 需关注状态的实例主机名列表（可能为空） |
| normal | 类型：array[string]，正常状态实例主机名列表 |
| offline | 类型：array[string]，离线实例主机名列表 |
| critical | 类型：array[string]，严重异常实例主机名列表 |

- **响应示例：** 

```json
[
	{
		"total": 1,
		"type": "celery",
		"session_active": 0,
		"high": [],
		"normal": ["[Celery]-mtls.example.internal"],
		"offline": [],
		"critical": []
	},
	{
		"total": 1,
		"type": "core",
		"session_active": 0,
		"high": [],
		"normal": ["[Core]-mtls.example.internal"],
		"offline": [],
		"critical": []
	}
]
```

- **请求示例**

**CURL**

```bash
curl -X GET \
	-H "Authorization: Bearer $TOKEN" \
	-H "X-JMS-ORG: $ORG" \
	https://example/api/v1/terminal/components/metrics/
```

**python**
```python
# 使用 AK/SK 访问示例 (HTTPSignatureAuth)
import requests
import json
from datetime import datetime
from httpsig.requests_auth import HTTPSignatureAuth

API_URL    = "https://example"          # 站点根地址
KEY_ID     = "your key id"              # Access Key ID
KEY_SECRET = "your key secret"          # Access Key Secret
ORG_ID     = "your org id"              # 组织 ID

def get_terminal_components_metrics():
	url = f"{API_URL}/api/v1/terminal/components/metrics/"
	gmt_form = "%a, %d %b %Y %H:%M:%S GMT"
	signature_headers = ['(request-target)', 'accept', 'date']
	headers = {
		"Content-Type": "application/json",
		"X-JMS-ORG": ORG_ID,
		"Date": datetime.utcnow().strftime(gmt_form)
	}
	auth = HTTPSignatureAuth(
		key_id=KEY_ID,
		secret=KEY_SECRET,
		algorithm="hmac-sha256",
		headers=signature_headers
	)
	try:
		resp = requests.get(url, auth=auth, headers=headers, timeout=10)
		resp.raise_for_status()
		data = resp.json()
		print(json.dumps(data, indent=2, ensure_ascii=False))
	except Exception as e:
		print(f"请求失败: {e}")

if __name__ == "__main__":
	get_terminal_components_metrics()
```


