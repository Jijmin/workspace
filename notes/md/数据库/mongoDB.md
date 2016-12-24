# MongoDB入门篇
## 数据库基本概念
### MongoDB的概念
1. MongoDB
2. mongo
3. 索引
4. 集合
5. 复制集
6. 分片
7. 数据均衡

### 学会MongoDB的搭建
#### 部署数据库服务
1. 搭建简单的单机服务
2. 搭建具有冗余容错功能复制集
3. 搭建大规模数据集群
4. 完成集群的自动部署

### 使用MongoDB的使用
1. 最基本的文档的读写更新删除
2. 各种不同类型的索引的创建与使用
3. 复杂的聚合查询
4. 对数据集合进行分片，在不同分片间维持数据均衡
5. 数据备份与恢复
6. 数据迁移

### 简单运维
- 部署MongoDB集群
- 处理多种常见的故障
    + 单节点失效，如何恢复工作
    + 数据库意外被杀死如何进行数据恢复
    + 数据库发生拒绝服务时如何排查原因
    + 数据库磁盘快满时如何处理

### 几个重要的网站
1. [MongoDB官网](https://www.mongodb.com/)
2. [MongoDB国内官网网站](http://www.mongoing.com/)
3. [中文MongoDB文档地址](http://docs.mongoing.com/manual-zh/)
4. [MongoDB的github--源码下载](https://github.com/mongodbs)
5. [MongoDB的jira--bug提交及回复](https://jira.mongodb.org/secure/Dashboard.jspa)
6. 两个google groups:mongodb-user与mongo-cn--用户交流

### 数据库功能
1. 有组织地存放数据
2. 按照不同的需求进行查询

### NoSQL数据库去掉
1. 实时一致性
2. 事务
3. 多表联合查询

### MongoDB优点
1. 无数据结构的限制
- 没有表结构的概念，每条记录可以有完全不同的结构
- 业务开发便捷
- sql数据库需要事先定义好表结构再使用
2. 完全的索引支持
- redis内存数据库，只提供按键查询
- hbase写入速度快，但是是单索引，二级索引需要自己实现
- mongodb中有单键索引、多键索引、数组索引、全文索引、地理位置索引
3. 方便的冗余与扩展
- 复制集保证数据安全
- 分片扩展数据规模
4. 良好的支持
- 完善的文档支持
- 完全的驱动支持
