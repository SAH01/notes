python解析json数据

# demo

```python
data = {
    "name": "John",
    "age": 30,
    "hobbies": ["reading", "running", "swimming"],
    "scores": [85, 90, 95]
}

for key, value in data.items():
    if isinstance(value, list):
        # 当值为列表类型时
        print(f"{key} is a list:")
        for item in value:
            print(item)
    else:
        # 当值不是列表类型时
        print(f"{key}: {value}")

```

# 读取json文件

```python
# 从JSON文件中读取数据
    with open('example_json.json', 'r', encoding='utf-8') as file:
        data = json.load(file)
    res_list = []
    res = data["xxx"]
    for key, value in res.items():
        print(key)
    
```

