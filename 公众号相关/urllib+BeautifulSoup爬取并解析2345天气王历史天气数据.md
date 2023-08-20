urllib+BeautifulSoup爬取并解析2345天气王历史天气数据

---

网址：[东城历史天气查询_历史天气预报查询_2345天气预报](https://tianqi.2345.com/wea_history/71445.htm)

![image-20230702161423470](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230702161423470.png)

# 1、代码

```python
import json
import logging
import urllib.parse
from datetime import date, datetime
from random import randint
from time import sleep

import pymysql
from bs4 import BeautifulSoup
# 定义目标URL
import requests

def weather_req():
    month_list = [1,2,3,4,5,6]  # 月份
    code_list = get_code()  # 获取所有的 天气code 和 地区code
    # 需要 2018 1月 到 2023 6月
    url = "https://tianqi.2345.com/Pc/GetHistory"   # 原始URL
    full_url = ""   # 最终拼好的url
    # 定义请求头
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36 Edg/114.0.1823.58',
    }
    # 定义GET参数
    params = {
        'areaInfo[areaId]': 70809,
        'areaInfo[areaType]': 2,
        'date[year]': 2023,
        'date[month]': 6
    }
    # 遍历 天气code 和 地区code 的列表
    for code_item in code_list:
        weather_code = code_item[0] # 拿到天气code
        area_code = code_item[1]    # 拿到地区code
        # 修改 url 参数 天气code的值
        params['areaInfo[areaId]'] = weather_code
        # 开始遍历月份列表
        for month_item in month_list:
            print(f"正在爬取天气ID为【{weather_code}】，地区ID为【{area_code}】的第【{month_item}】月的数据！")
            # 修改 month 的值为新值
            params['date[month]'] = month_item
            # 编码 GET参数
            encoded_params = urllib.parse.urlencode(params)
            # 拼接完整的URL
            full_url = url + '?' + encoded_params
            print(full_url)
            try:
                sleep(randint(1, 3))    # 睡眠(随机1-3秒)
                # 发起请求
                res = requests.get(full_url, headers=headers)
                res_data = json.loads(res.text)
                weather_data = res_data['data']
                # print(weather_data)
                # 解析数据
                soup = BeautifulSoup(weather_data, 'html.parser')
                # 拿到需要的table
                table_data = soup.find('table', attrs={'class': 'history-table'})
                # print(type(table_data),'\n',table_data)
                all_tr = table_data.find_all('tr')  # 拿到所有的tr
                # print(all_tr[0])
                weather_list = []   # 这是要存储数据的list
                # 开始遍历tr列表 一个列表存储了某地区的某年份的某月完整的数据
                for i in range(1, len(all_tr)):
                    temp_list = []  # 暂时存储一天的数据 每次循环都刷新
                    tr_item = all_tr[i] # 拿到一个tr数据
                    all_td = tr_item.find_all("td") # 拿到一个tr里的所有td，td里面的text就是需要的值
                    rdate = str(all_td[0].text)  # 日期 2023-01-01 周日
                    # 日期需要转化格式，去掉星期
                    rdate_new = rdate.split(" ")[0] # 拿到日期字符串
                    # 解析字符串为日期对象
                    date_object = datetime.strptime(rdate_new, "%Y-%m-%d")
                    # 将日期对象格式化为 MySQL 可存储的日期字符串
                    mysql_date = date_object.strftime("%Y-%m-%d")   # 最终被存储的日期
                    wind_and_power = all_td[4].text # 风向和风力是在一起的 需要解析
                    wind = str(wind_and_power).split("风")[0]    # 风向
                    winp = str(wind_and_power).split("风")[1]   # 风力
                    temp_max = str(all_td[1].text)  # 最高温
                    temp_min = str(all_td[2].text)  # 最低温
                    weather = str(all_td[3].text)   # 天气情况
                    # 把上面的变量存储到 temp_list 然后再一起存到 weather_list
                    temp_list.append(mysql_date)    # 日期
                    temp_list.append(weather_code)  # 天气编码
                    temp_list.append(area_code) # 地区编码
                    temp_list.append(wind)  # 风向
                    temp_list.append(winp) # 风力
                    temp_list.append(temp_max)  # 最高温度
                    temp_list.append(temp_min)  # 最低温度
                    temp_list.append(weather)   # 天气情况
                    weather_list.append(temp_list)
                print(weather_list)
                # 开始插入数据 【某个地区的，某一年的，某一个月份的数据】
                conn_his,cursor_his = get_conn()    # 建立数据库连接
                # 遍历数据
                for save_item in weather_list:
                    INSERT_SQL = "insert into w_weather_day_history (rdate,weather_code,area_code,wind,winp,temp_max,temp_min,weather) " \
                                 "values(%s,%s,%s,%s,%s,%s,%s,%s)" \
                                 "              "%("\""+save_item[0]+"\"",
                                                  "\""+save_item[1]+"\"",
                                                  "\""+save_item[2]+"\"",
                                                  "\""+save_item[3]+"\""
                                                 ,"\""+save_item[4]+"\""
                                                 ,"\""+save_item[5]+"\""
                                                 ,"\""+save_item[6]+"\""
                                                 ,"\""+save_item[7]+"\"")

                    print(INSERT_SQL)
                    cursor_his.execute(INSERT_SQL)  # 执行sql语句
                    conn_his.commit()  # 提交事务
                    print("--------------------------------------------------")
            except urllib.error.URLError as e:
                print("发生错误：", e)

def get_code():
    conn,cursor = get_conn()
    SQL = "select fwc.weather_code,fwc.area_code from f_weather_area_code fwc;"
    cursor.execute(SQL)
    res = cursor.fetchall()
    print(res)
    return res

def get_conn():
    """
    :return: 连接，游标
    """
    # 创建连接
    conn = pymysql.connect(host="127.0.0.1",
                    user="root",
                    password="reliable",
                    db="weather",
                    charset="utf8")
    # 创建游标
    cursor = conn.cursor()  # 执行完毕返回的结果集默认以元组显示
    return conn, cursor

def close_conn(conn, cursor):
    if cursor:
        cursor.close()
    if conn:
        conn.close()

if __name__ == '__main__':
    # get_code()
    weather_req()
```

![image-20230702161542526](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230702161542526.png)

# 2、分析

url构成如下：

基础url：https://tianqi.2345.com/Pc/GetHistory

参数：

```python
params = {
        'areaInfo[areaId]': 70809,
        'areaInfo[areaType]': 2,
        'date[year]': 2023,
        'date[month]': 6
    }
```

areaInfo[areaId] 表示的是 某地区的天气编码，这个需要去自己获取。

areaInfo[areaType] 不用管

后面两个参数就是年份和月份了

# 3、发起请求demo

```python
url = "https://tianqi.2345.com/Pc/GetHistory"   # 原始URL
    full_url = ""   # 最终拼好的url
    # 定义请求头
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36 Edg/114.0.1823.58',
    }
    # 定义GET参数
    params = {
        'areaInfo[areaId]': 70809,
        'areaInfo[areaType]': 2,
        'date[year]': 2023,
        'date[month]': 6
    }
    
    # 解析参数
    encoded_params = urllib.parse.urlencode(params)
    # 拼接完整的URL
    full_url = url + '?' + encoded_params
    sleep(randint(1, 3))    # 睡眠(随机1-3秒)
    # 发起请求
    res = requests.get(full_url, headers=headers)
    res_data = json.loads(res.text)
    weather_data = res_data['data']
```

# 4、解析数据demo

```python
# 解析数据
soup = BeautifulSoup(weather_data, 'html.parser')
# 拿到需要的table
table_data = soup.find('table', attrs={'class': 'history-table'})
# print(type(table_data),'\n',table_data)
all_tr = table_data.find_all('tr')  # 拿到所有的tr
```

