#### 一、获取某数据库下的所有数据表名

```sql
SELECT 
  TABLE_NAME,
  TABLE_COMMENT 
FROM
  information_schema.TABLES 
WHERE TABLE_SCHEMA = 'file_insert' ;
```

![image-20211122151631890](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211122151631890.png)

---



#### 二、获取某数据表下的所有字段信息

```sql
SELECT 
  COLUMN_NAME,
  DATA_TYPE,
  COLUMN_COMMENT 
FROM
  information_schema.COLUMNS 
WHERE table_name = 'admin' 
  AND table_schema = 'file_insert' ;

```

![image-20211122151716432](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211122151716432.png)



---

