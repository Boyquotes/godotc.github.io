1. 导入数据
- 选择一个database后，source+文件

2. 创建表
**CR**EATE **TA**BLE <表名> ([表定义选项])[表选项][分区选项];****
```mysql
mysql> USE test_db;
Database changed
mysql> CREATE TABLE tb_emp1
    -> (
    -> id INT(11),
    -> name VARCHAR(25),
    -> deptId INT(11),
    -> salary FLOAT
    -> );
Query OK, 0 rows affected (0.37 sec)
```