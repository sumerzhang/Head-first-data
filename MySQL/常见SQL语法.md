# 常见的SQL语法

下面总结一下自己平时用的比较多的SQL语法，以及遇到的一些”坑"

为了方便起见，我们有如下结构的表：
```SQL
CREATE TABLE IF NOT EXISTS input_base (
id bigint(20) NOT NULL AUTO_INCREMENT,
mid bigint(20) COMMENT '用户 id',
sex varchar(50) COMMENT '性别',
age int(10) COMMENT '年龄',
degree varchar(50) COMMENT '学位',
creat_ymd varchar(50),
PRIMARY KEY (id),
INDEX mid_index(mid)
);
```

一般我们说SQL的基本语法就是四个字- **增删改查**。

## 1. 增
增，就是增加数据，可以分为全表插入，具体列插入和初始化插入。

### 1.全表插入
```SQL
insert into input_base select * from input
```

### 2. 插入具体列
```SQL
insert into input_base (id, mid, sex) select id, mid, sex from input
```


### 3. 初始化插入

```SQL
# 插入全部列可以不写列名
insert into input_base values ('0', '80', '女', '32', '本科', '2017-12-21');


#插入部分列，其他列为null
insert into input_base(id, mid, sex) values ('0', '122354', '女');


```


## 2. 删
删，就是删除数据，主要有三种方式:`DROP`、`TRUNCATE`和`DELETE`。

### 相同点:
#### 1. 都可以删除数据
#### 2. DROP、TRUNCATE都是DDL语句，执行后会自动提交

### 不同点:
#### 1. TRUNCATE和DELETE只删除数据，不删除表结构，而DROP还会删除表结构和索引等依赖
#### 2. 效率： DROP > TRUNCATE > DELETE

#### 3. 安全性：在没有备份前，小心使用DROP和TRUNCATE。如果涉及事务处理最好用delete from。
#### 4. 使用场景： 想要删除部分数据，使用delete from 结构；想要删除表，使用DROP结构;想要保留表结构，删除所有数据，使用TRUNCATE来操作。

### DROP删除表
```SQL
DROP TABLE input_base;
```

### DELETE删除部分数据
```SQL
DELETE FROM input_base WHERE sex='男';
```

### DRUNCATE清除数据
```SQL
TRUNCATE TABLE input_base;
```

## 3.改

改，包括改数据、改数据类型和改表结构。
### 1. 改数据
```SQL
UPDATE input_dabase SET num=7 WHERE sex='男';
```

### 2.改数据类型
```SQL
#把degree的数据类型由varchar(50)改为varchar(100)

ALTER TABLE input_base MODIFY COLUMN degree varchar(100);

ALTER TABLE input_base CHANGE degree degree varchar(100);
```
### 3. 改表结构
#### 1. 新增列
```SQL
#首位
ALTER TABLE input_base ADD uuid varchar(50) COMMENT '唯一标识' first;

#末尾
ALTER TABLE input_base ADD num int(10) COMMENT '文章数量';

#制定位置
ALTER TABLE input_base ADD amount int(20) COMMENT '总额' AFTER mid;
```

#### 2. 删除列
```SQL
ALTER TABLE input_base DROP update_ymd;
```

## 4. 查

查分为查表结构，查全表，制定列和条件查询。
### 1. 查表结构
```SQL
DESC input_base
```

### 2. 全表查询
```SQL
#取前10条
SELECT * FROM input_base LIMIT 10;
```

### 3. 特定列查询
```SQL
SELECT id, mid, sex FROM input_base LIMIT 10;
```

### 4.条件查询
```SQL
SELECT id, mid, sex FROM input_base WHERE sex='男' LIMIT 10;
```































