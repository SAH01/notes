

- 备份表
- 【用户_文件名】
  - 表头字段
- 操作表
- 【用户_文件名 _文件类型】
  - 表头字段
- 字段状态表
- 【用户_文件名 _state】
  - 字段名、各种状态（停用 _ 未清洗_ 已清洗）
  - 【field_name、field_state】

---



- 表操作日志表
- 【table_log】
  - 序号、**用户**、**表名**、**时间**、操作（增加_删除等编号）
  - 【num、**user_name**、**table_name**、**date_time**、 operation_num】
- 表操作类型表：
- 【table_operation】
  - **编号**、操作类型名
  - 【**operation_num**、operation_name】						
- 字段操作日志表
- 【field_log】
  - 序号、**表名**、**用户**、**字段名**、**时间**、操作（清洗_脱敏 _添加描述等）、操作细节（行业维度 _其他维度等）
  - 【num、**table_name**、**user_name**、**field_name**、**date_time**、operation_num、operation_detail】
- 字段操作类型表
- 【field_operation】
  - **编号**、操作类型名
  - 【**operation_num**、operation_name】
- 字段描述映射表
- 【field_map】
  - **用户**、**字段名**、字段描述
  - 【**user_name**、**field_name**、field_desc】
- 字段维度映射表
- 【dimension_map】
  - **用户**、**字段名**、维度描述
  - 【**user_name**、**field_name**、dimension_desc】

---



- 用户表
- 【user】
  - **用户**、密码、用户状态（权限）
  - 【**user_name**、user_pass、user_state】
- 管理表
- 【admin】
  - **用户**、**备份表名**、操作表名、字段状态表名、表状态（处理中_处理完成）、文件路径
  - 【**user_name**、**table_name_1**、table_name_2、field_state_table、table_state、file_path】

