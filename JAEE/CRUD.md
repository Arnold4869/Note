[TOC]
# 插入
使用插入语句时，主键一定要写，用来**唯一确定**插入位置

```sql
insert into dept(deptno,dname,loc)values('50','教研部'，'Beijing');
```
如果插入**全字段数据**，字段名可以**省略不写**
```sql
insert into dept values('60','人事部'，'Beijing');
```

### commit 和rollback
插入后记得提交

```sql
commit;
```

失败则回滚

```sql
rollback;
```

# 更新
update

```sql
update dept set dname = '教学部' where deptno = '70';
```

# 删除
整个清空表数据有两种方式：
- delete 表名
- truncate table 表明（这个不能回滚，不过推荐使用）

```sql
delete dept where deptno = '70'; //删除指定数据库
delete dept; //清空表数据
truncate table dept;//清空表数据
```