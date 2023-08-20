python [pymysql] 操作MySQL数据库

# 连接、关闭数据库

```python
def get_conn_():
    """
    :return: 连接，游标
    """
    # 创建连接
    conn = pymysql.connect(host="",
                    user="",
                    port=3306,
                    password="",
                    db="",
                    charset="utf8")
    # 创建游标
    cursor = conn.cursor()  # 执行完毕返回的结果集默认以元组显示
    return conn, cursor
"""
关闭数据库连接
"""
def close_conn(conn, cursor):
    if cursor:
        cursor.close()
    if conn:
        conn.close()
```

# 插入数据

```python
INSERT_SQL = "insert into table_name (database_name," \
                     "table_name,layout_code,ds_name,ds_type,field_name" \
                     ",field_desc,value,value_desc) " \
                     "values(%s,%s,%s,%s,%s,%s,%s,%s,%s)" \
                     "              " % ("\"" + save_item[0] + "\"",
                                         "\"" + table_flag + "\"",
                                         "\"" + save_item[2] + "\"",
                                         "\"" + save_item[3] + "\""
                                         , "\"" + save_item[4] + "\""
                                         , "\"" + save_item[5] + "\""
                                         , "\"" + save_item[6] + "\""
                                         , "\"" + save_item[7] + "\""
                                         , "\"" + save_item[8] + "\""
                                         )
        print(INSERT_SQL)
        cursor.execute(INSERT_SQL)  # 执行sql语句
        conn.commit()  # 提交事务
```

# 查询数据

```python
def query(sql,*args):
    """
    通用封装查询
    :param sql:
    :param args:
    :return:返回查询结果 （（），（））
    """
    conn , cursor= get_conn()
    print(sql)
    cursor.execute(sql)
    res = cursor.fetchall()
    close_conn(conn , cursor)
    return res
```

