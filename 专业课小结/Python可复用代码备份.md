## Python可复用代码备份

### 01 数据库操作

```python
import pymysql
# 获取连接
def get_conn():
    """
    :return: 连接，游标
    """
    # 创建连接
    conn = pymysql.connect(host="127.0.0.1",
                    user="root",
                    password="000429",
                    db="movierankings",
                    charset="utf8")
    # 创建游标
    cursor = conn.cursor()  # 执行完毕返回的结果集默认以元组显示
    return conn, cursor

# 连接关闭
def close_conn(conn, cursor):
    if cursor:
        cursor.close()
    if conn:
        conn.close()

# 查询函数
def query(sql,*args):
    """
    封装通用查询
    :param sql:
    :param args:
    :return: 返回查询结果以((),(),)形式
    """
    conn,cursor = get_conn();
    cursor.execute(sql)
    res=cursor.fetchall()
    close_conn(conn,cursor)
    return res
```

### 02 ajax

```python
$.ajax({
    url: "/query_tag",
    data: {
        type:"全部",date:"全部",area:"全部",
        first:"热门(正序)",num:"0"
    },
    success: function (data) {
        $(".ul_show").empty()
        if(data.data==""){
            alert("暂无数据！")
        }else{
            for (var i = 0; i < data.data.length; i++) {
                a="/movie_page?"+"title="+data.data[i][0]+"&scorenum="+data.data[i][7];
                appendUlBody ="<li> <p class='picture'>"
                    +"<img src="+"'"+data.data[i][8]+"'"+" height='200px' width='140px' referrer='no-referrer'/></p>"
                    +"<p class='instroction'>"
                    +'<a href="'+a+'" style="text-decoration:none;" target="_blank" >'
                    +data.data[i][0]+"</a><br><br>导演: "+leave_out(data.data[i][2])+"<br>主演: "+leave_out(data.data[i][1])+"<br>"
                    +data.data[i][4]+"/"+data.data[i][5]+"<br>"+data.data[i][6]+"<br>"+data.data[i][3]+"分<br>"+data.data[i][7]+"人评价</p></li>"
                $(".ul_show").append(appendUlBody);
            }
        }
    },
    error: function (xhr, type, errorThrown) {
    }
})
```



### 03 读文件并插入数据库

```Python
import pymysql
import pandas as pd
#读取文件
def read_file():
    conn, cursor = get_conn()  # 连接mysql
    if (conn != None):
        print("数据库连接成功！")

    sheet =pd.read_excel("1617241934831197.xlsx",sheet_name=None,keep_default_na=False)
    # print(df.columns.values)
    print(list(sheet.keys()))
    # print(type(sheet))
    # print(type(sheet['财经']))
    for item in sheet.values():
        print("sheet:\n", len(item.index.values))
        print("*******************************************")
        for i in range(len(item.index.values)):
            print(item.loc[i].values)
            SQL="insert into news values(%s,%s,%s)"
            values = (str(item.loc[i].values[0]),
                      str(item.loc[i].values[1]),
                      str(item.loc[i].values[2]) )
            try:
                cursor.execute(SQL, values)  # 执行sql语句
                conn.commit()  # 提交事务
                print("插入成功:\n", item.loc[i].values[0], "，第" + str(i + 1) + "条数据！")
                print("--------------------------------------------------")
            except:
                print("插入失败:\n", item.loc[i].values[0])
    close_conn(conn,cursor)
    # for j1 in sheet.values():
    #     j1 = j1.to_dict(orient='records')
    #     print(j1)
    return
```



### 04 url传参

```python
return render_template("content.html",content=res_sql[0][0])

    <div style="width: 100% ;" >
        <p>{{ content }}</p>
    </div>
```

