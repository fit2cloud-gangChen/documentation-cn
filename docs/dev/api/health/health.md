## /api/v1/health/

### GET
- **描述：** 系统健康检查接口，用于探测服务是否存活及基础依赖是否正常。

- **请求头（Headers）：**

| 键 | 值 | 备注 |
|----|----|------|
| Accept | application/json | 可选 |

- **返回参数：**

| 字段 | 类型 | 描述 | 备注 |
|------|------|------|------|
| status | Boolean | 服务总体可用状态 | true=可用 |
| db_status | Boolean | 数据库可用状态 | true=正常 |
| db_time | Float | 数据库探测耗时 (秒) | 精度为秒的小数 |
| redis_status | Boolean | Redis 可用状态 | true=正常 |
| redis_time | Float | Redis 探测耗时 (秒) | 精度为秒的小数 |
| time | Int | 服务器当前时间 (Unix 时间戳, 秒) | 可用于时钟漂移检测 |


- **响应示例**
```json
{
	"status": true,
	"db_status": true,
	"db_time": 0.0032639503479003906,
	"redis_status": true,
	"redis_time": 0.0004906654357910156,
	"time": 1762484673
}
```

- **请求示例**

**CURL**
```sh
curl -X GET 'https://demo.jumpserver.org/api/v1/health/'
```

**Python**
```python
import requests

API_URL = 'https://demo.jumpserver.org'

def check_health():
	url = f"{API_URL}/api/v1/health/"
	r = requests.get(url, timeout=5)
	if r.status_code == 204 or not r.content:
		print('健康: 无内容返回 (204/空)')
		return
	r.raise_for_status()
	try:
		data = r.json()
		print('健康信息:')
		print(data)
	except ValueError:
		print('健康: 非 JSON 内容, 原始:')
		print(r.text)

if __name__ == '__main__':
	check_health()
```


