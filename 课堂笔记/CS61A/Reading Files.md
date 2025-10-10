```python
import json
for line in open('xxx.json'):
	r = json.loads(line) # 解析line得到一个Python对象
	do_something()
```