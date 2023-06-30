Java解析json数据（fastjson2）

--------

# Json数据

JSON（JavaScript Object Notation）是一种轻量级的数据交换格式。它以易于阅读和编写的方式来表示结构化数据，常用于在不同系统之间进行数据交互和传输。

JSON使用键值对的方式来组织数据，具有以下几个特点：

- 具有简洁的语法：JSON使用了人类可读的文本格式，易于理解和编写。
- 支持多种数据类型：JSON支持字符串、数值、布尔值、**数组、对象**等多种数据类型。
- 基于键值对的数据结构：JSON中的数据由键值对组成，键是字符串，值可以是字符串、数值、布尔值、数组或嵌套的JSON对象。
- 平台无关性：JSON是独立于编程语言和操作系统的，可以在不同的平台和环境中使用和解析。

```json
{
        "name":"John",
        "age":30,
        "city":"New York",
        "is_student":false,
        "hobbies":["reading","traveling","gardening"],
        "area":
        {
            "province" : "XXX",
            "city" : "XXX",
            "district" : "XXX"
        }
}
```

参考上面的例子，`"name":"John"`这样的就是普通的字符串数据类型，而`hobbies`就是对应的**数组**格式了，`area`对应的是**对象**格式。**数组里面是可以嵌套对象和其他数据格式的**，也就是说json非常灵活，我们在获取其数据的时候，不同的数据类型要用不同的获取方式。

# 代码（遍历）

```java
package com.ruoyi.qcc;

import com.alibaba.fastjson2.JSON;
import com.alibaba.fastjson2.JSONArray;
import com.alibaba.fastjson2.JSONObject;

/**
 * @BelongsProject: ruoyi
 * @BelongsPackage: com.ruoyi.qcc
 * @Author: chuanwei.yang 42624
 * @CreateTime: 2023-06-27  21:03
 * @Description: TODO
 * @Version: 1.0
 */
public class JsonTraversalExample {
    public static void main(String[] args) {
        String jsonStr = "{\"name\":\"John\", \"age\":30, \"city\":\"New York\", \"pets\":[\"dog\",\"cat\"]}";
        // 获取一个JSONObject对象
        JSONObject json = JSON.parseObject(jsonStr);
        // 调用遍历方法
        traverseJson(json);
    }

    public static void traverseJson(JSONObject json) {
        // 获取所有的 key 值
        for (String key : json.keySet()) {
            // get 方法获取 value
            Object value = json.get(key);

            System.out.println("Key: " + key + ", Value: " + value.toString());
            // 如果value值是一个数组，则使用JSONArray接收，然后遍历。
            if (value instanceof JSONArray) {
                // 强转 JSONArray
                JSONArray array = (JSONArray) value;
                for (int i = 0; i < array.size(); i++) {
                    System.out.println("Array Element: " + array.getString(i));
                }
            }
        }
    }
}
```

上面的代码提供了一种遍历的方法。更多的时候，我们需要按需取里面的数据，所以下面的方法可能会更实用一点。

# 代码（取值）

```java
			// 解析 JSON
			// jsonString 对应需要被解析的json数据变量
            JSONObject jsonObject = JSON.parseObject(jsonString);
			// 获取其中 key 值为 Result 的 value 值
            JSONObject parentJson = jsonObject.getJSONObject("Result");
			// 如果 Industry 的 value 是一个数组 要使用 JSONArray 来获取
            JSONArray originalNameJsonArray = parentJson.getJSONArray("Industry");
			// jsonArray 可以直接转换为 java 的 List 类型
            List<JSONObject> originalNameJsonList = originalNameJsonArray.toList(JSONObject.class);
			// 之后就可以遍历了
            for (JSONObject jsonItem : originalNameJsonList) {
                System.out.println(jsonItem);
            }
			// 获取某个值
//            String EnglishName = resultJson.getString("EnglishName");
```

