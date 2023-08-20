python整理1992、2009国家标准学科分类及代码数据并存入MySQL数据库【资源分享】

# 文件内容
![image-20230105125834745](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202301051258226.png)

# 处理结果

![image-20230105125850090](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202301051258786.png)

 

# 代码

```
  1 import pandas as pd
  2 import pymysql
  3 
  4 
  5 def get_subject_1992():
  6     res={}
  7     the_former_code = ""
  8     layer1_code = ""  # 一位
  9     layer1_name = ""
 10     layer2_code = ""  # 三位
 11     layer2_name = ""  # 三位
 12     layer3_code = ""  # 五位
 13     layer3_name = ""
 14     layer4_code = ""  # 七位
 15     layer4_name = ""  # 七位
 16     df = pd.read_excel("std_subject_1992.xlsx")
 17     for i in range(len(df.values)):
 18         item=df.values[i]
 19         # print(item[0],item[1])
 20         if (len(str(item[0])) == 1):
 21             layer1_code = str(item[0])
 22             layer1_name = item[1]
 23             # print(layer1_code,layer1_name)
 24         if (len(str(item[0])) == 3):
 25             layer2_code = str(item[0])
 26             layer2_name = item[1]
 27             # print(layer2_code, layer2_name)
 28         if (len(str(item[0])) == 5):
 29             layer3_code = str(item[0])
 30             layer3_name = item[1]
 31             if(i!=(len(df.values)-1)):
 32                 if(len(str(df.values[i+1][0]))!=7):
 33                     # print(layer1_code + layer3_code,layer1_name + "·" + layer2_name + "·" +layer3_name)
 34                     res.update({layer1_code + layer3_code+"00":layer1_name + "·" + layer2_name + "·" +layer3_name})
 35             # print(layer3_code, layer3_name)
 36         if (len(str(item[0])) == 6):
 37             layer4_code = str(item[0])+"0"
 38             layer4_name = item[1]
 39             # print(layer4_code, layer4_name)
 40             if (layer4_code[:5] == layer3_code):
 41                 # print(layer1_code + layer4_code,layer1_name + "·" + layer2_name + "·" + layer3_name + "·" + layer4_name)
 42                 res.update({layer1_code + layer4_code:layer1_name + "·" + layer2_name + "·" + layer3_name + "·" + layer4_name})
 43         if (len(str(item[0])) == 7):
 44             layer4_code = str(item[0])
 45             layer4_name = item[1]
 46             # print(layer4_code, layer4_name)
 47             if (layer4_code[:5] == layer3_code):
 48                 # print(layer1_code + layer4_code,layer1_name + "·" + layer2_name + "·" + layer3_name + "·" + layer4_name)
 49                 res.update({layer1_code + layer4_code:layer1_name + "·" + layer2_name + "·" + layer3_name + "·" + layer4_name})
 50     return res
 51 
 52 """
 53 ---------------------------------------------------------------------------------------
 54 """
 55 def get_subject_2009():
 56     res={}
 57     the_former_code = ""
 58     layer1_code = ""  # 一位
 59     layer1_name = ""
 60     layer2_code = ""  # 三位
 61     layer2_name = ""  # 三位
 62     layer3_code = ""  # 五位
 63     layer3_name = ""
 64     layer4_code = ""  # 七位
 65     layer4_name = ""  # 七位
 66     df = pd.read_excel("std_subject_2009.xlsx")
 67     for i in range(len(df.values)):
 68         item=df.values[i]
 69         # print(item[0],item[1])
 70         if (len(str(item[0])) == 1):
 71             layer1_code = str(item[0])
 72             layer1_name = item[1]
 73             # print(layer1_code,layer1_name)
 74         if (len(str(item[0])) == 3):
 75             layer2_code = str(item[0])
 76             layer2_name = item[1]
 77             # print(layer2_code, layer2_name)
 78         if (len(str(item[0])) == 5):
 79             layer3_code = str(item[0])
 80             layer3_name = item[1]
 81             if(i!=(len(df.values)-1)):
 82                 if(len(str(df.values[i+1][0]))!=7):
 83                     # print(layer1_code + layer3_code,layer1_name + "·" + layer2_name + "·" +layer3_name)
 84                     res.update({layer1_code + layer3_code+"00":layer1_name + "·" + layer2_name + "·" +layer3_name})
 85         if (len(str(item[0])) == 7):
 86             layer4_code = str(item[0])
 87             layer4_name = item[1]
 88             # print(layer4_code, layer4_name)
 89             if (layer4_code[:5] == layer3_code):
 90                 # print(layer1_code + layer4_code,layer1_name + "·" + layer2_name + "·" + layer3_name + "·" + layer4_name)
 91                 res.update({layer1_code + layer4_code:layer1_name + "·" + layer2_name + "·" + layer3_name + "·" + layer4_name})
 92     return res
 93 """
 94 ---------------------------------------------------------------------------------------------------------------
 95 """
 96 def get_conn():
 97     """
 98     :return: 连接，游标
 99     """
100     # 创建连接
101     conn = pymysql.connect(host="127.0.0.1",
102                     user="root",
103                     password="000429",
104                     db="data_cleaning",
105                     charset="utf8")
106     # 创建游标
107     cursor = conn.cursor()  # 执行完毕返回的结果集默认以元组显示
108     return conn, cursor
109 
110 def close_conn(conn, cursor):
111     if cursor:
112         cursor.close()
113     if conn:
114         conn.close()
115 
116 
117 def into_mysql():
118     global conn, cursor
119     res=get_subject_2009()
120     for k,v in res.items():
121         print(k,v)
122         try:
123             conn,cursor=get_conn()
124             SQL="insert into std_subject_2009 (year,subject_code,subject_name) values (2009,'"+k+"','"+v+"')"
125             cursor.execute(SQL)
126             conn.commit()
127         except:
128             print(k,v+" 插入失败！")
129     conn,cursor.close()
130     return None
131 if __name__ == '__main__':
132     into_mysql()
```
 **获取标准学科分类表 请关注公众号【靠谱杨的挨踢生活】回复【学科】获取**（整理不易，fu费获取，请博主喝杯奶茶，谢谢理解！）

![image-20230105125813388](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202301051258298.png)