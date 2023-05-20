# 一、数据库

新建名为ry-vue的MySQL数据库，运行sql文件导入数据库。

图数据库，import文件夹里的文件就是数据源，可以直接运行下面的语句完成导入。

## Neo4j图数据库操作笔记

![image-20230427213320610](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202304281543870.png)

### 1、导入省份：

```
LOAD CSV WITH HEADERS FROM 'file:///ry_map_province.csv' AS line CREATE (:Province { province_id: line.province_id, province_name: line.province_name})
```

### 2、导入学科：

```
LOAD CSV WITH HEADERS FROM 'file:///ry_map_subject.csv' AS line CREATE (:Subject { subject_id: line.subject_id, subject_name: line.subject_name})
```



**删除**一个节点：

```
MATCH (n:Lesson { name: '模拟电子技术基础' })
DELETE n
```



### 3、导入单位：

```
LOAD CSV WITH HEADERS FROM 'file:///ry_map_company.csv' AS line CREATE (:Company { company_id: line.company_id, company_name: line.company_name})
```



### 4、导入项目：

```
LOAD CSV WITH HEADERS FROM 'file:///ry_map_project.csv' AS line CREATE (:Project { project_id: line.project_id, project_name: line.project_name})
```

### Relationship_1

> ==项目-学科：==ry_project_subject【项目-从属-学科】

```
LOAD CSV WITH HEADERS FROM 'file:///ry_project_subject.csv' AS line MATCH (from:Project { project_id: line.projectId }),(to:Subject {subject_id : line.subjectAreaId })MERGE (from)-[r:从属]->(to)
```

### Relationship_2

> ==单位-项目：==ry_company_project【单位-承担-项目】

```
LOAD CSV WITH HEADERS FROM 'file:///ry_company_project.csv' AS line MATCH (from:Company { company_id: line.companyId }),(to:Project {project_id : line.projectId })MERGE (from)-[r:承担]->(to)
```

### Relationship_3

> ==省份-学科：==ry_province_subject【地域-研究方向-学科】

![image-20230427211125530](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202304281543874.png)

```
LOAD CSV WITH HEADERS FROM 'file:///ry_province_subject.csv' AS line MATCH (from:Province { province_id: line.provinceId }),(to:Subject {subject_id : line.subjectAreaId })MERGE (from)-[r:研究方向]->(to)
```

# 二、前端

```bash
# 进入项目目录
cd ruoyi-ui

# 安装依赖
npm install

# 强烈建议不要用直接使用 cnpm 安装，会有各种诡异的 bug，可以通过重新指定 registry 来解决 npm 安装速度慢的问题。
npm install --registry=https://registry.npmmirror.com

# 本地开发 启动项目
npm run dev
```

# 三、后端

修改数据库连接，编辑`resources`目录下的`application-druid.yml`，需要同时启动Redis和MySQL服务。

```yml
# 数据源配置
spring:
    datasource:
        type: com.alibaba.druid.pool.DruidDataSource
        driverClassName: com.mysql.cj.jdbc.Driver
        druid:
            # 主库数据源
            master:
                url: 数据库地址
                username: 数据库账号
                password: 数据库密码
```

# 四、其他

启动flaskChat，nlp是模型训练的源码。

数据集的分类名称顺序和序号一一对应。