



# neo4j

---

![image-20230427213320610](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202304281543870.png)

# 1、导入省份：

```
LOAD CSV WITH HEADERS FROM 'file:///ry_map_province.csv' AS line CREATE (:Province { province_id: line.province_id, province_name: line.province_name})
```

# 2、导入学科：

```
LOAD CSV WITH HEADERS FROM 'file:///ry_map_subject.csv' AS line CREATE (:Subject { subject_id: line.subject_id, subject_name: line.subject_name})
```



**删除**一个节点：

```
MATCH (n:Lesson { name: '模拟电子技术基础' })
DELETE n
```



# 3、导入单位：

```
LOAD CSV WITH HEADERS FROM 'file:///ry_map_company.csv' AS line CREATE (:Company { company_id: line.company_id, company_name: line.company_name})
```



# 4、导入项目：

```
LOAD CSV WITH HEADERS FROM 'file:///ry_map_project.csv' AS line CREATE (:Project { project_id: line.project_id, project_name: line.project_name})
```

# Relationship_1

> ==项目-学科：==ry_project_subject【项目-从属-学科】

```
LOAD CSV WITH HEADERS FROM 'file:///ry_project_subject.csv' AS line MATCH (from:Project { project_id: line.projectId }),(to:Subject {subject_id : line.subjectAreaId })MERGE (from)-[r:从属]->(to)
```

# Relationship_2

> ==单位-项目：==ry_company_project【单位-承担-项目】

```
LOAD CSV WITH HEADERS FROM 'file:///ry_company_project.csv' AS line MATCH (from:Company { company_id: line.companyId }),(to:Project {project_id : line.projectId })MERGE (from)-[r:承担]->(to)
```

# Relationship_3

> ==省份-学科：==ry_province_subject【地域-研究方向-学科】

![image-20230427211125530](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202304281543874.png)

```
LOAD CSV WITH HEADERS FROM 'file:///ry_province_subject.csv' AS line MATCH (from:Province { province_id: line.provinceId }),(to:Subject {subject_id : line.subjectAreaId })MERGE (from)-[r:研究方向]->(to)
```



# 存储路径：

> C:\Users\reliable.yang\.Neo4jDesktop\relate-data\dbmss\dbms-f3ce351f-a159-4c6c-a987-135ad7ae299b\import

# 密码：

密码1：neo4jneo4j

密码2：neo4jneo4j2



----



修改知识图谱ID字段名：

```sql
UPDATE ry_map SET subjectAreaId = CONCAT('subject_', subjectAreaId);
UPDATE ry_map SET companyId = CONCAT('company_', companyId);
UPDATE ry_map SET provinceId = CONCAT('province_', provinceId);
```

# Cypher查询语句：

单位【承担】项目【从属】学科【研究方向】地域

```Cypher
MATCH p1=(c:Company)-[:承担]->(p:Project)-[:从属]->(s:Subject)<-[:研究方向]-(pr:Province)
RETURN c,p,s,pr limit 27
```

