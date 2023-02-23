# Database Related Things


## 1. Gorm
- 数据库相关，目前支持, MySQL, SQLServer, PostgreSQL, SQLite
- 什么是 `dsn` ?

```go
type TableRow struct{
	Id uint64 	 `gorm:"primarykey"`
	Name string `gorm:"colume: name"`  
}

db, err =: gorm.open( mysql.Open(dsn), &qorm.Config{} )
if err!=nil{
	panic(err)
}

db.Create(&TableRow{...})

var row TableRow
db.First(&row, index) // find first one
db.Model(&row).Update(..)

```

- 事务
```go
# 声明区间
tx := db.Begin() // begin transactions
...
tx.Commit()

# 回调函数
if err:= db.Transaction( func(tx* gorm.DB) error{
	if .......{
		return err
	}
	if  ......{
		tx.Rollback()
		return err
	}
	return nil // nil 即为 commit
}); err!=nil{
	return 
}

```
- 性能提高，关闭默认事务
对于写操作（create, update, delete) , 为确保数据完整性， GORM会默认启用事务
关掉然后在需要的地方手动启动
```go
db, err:=  gorm.Open(
	mysql.Open( "username:password@tcp(localhsot:3306)/database?charset=utf8"),
	&grom.Config{
		SkipDefaultTransaction: true, // close default tx
		PreparseStmt: true , // 缓存预编译语句
	}	
)
```

- **Additional**
![](attachments/Pasted%20image%2020230120161742.png)

## 2. Kitex (RPC)
![](attachments/Pasted%20image%2020230120161901.png)

- IDL 
interface define language
![](attachments/Pasted%20image%2020230120162229.png)

- 服务注册与发现
![](attachments/Pasted%20image%2020230120162854.png)

## 3. Hertz (Http)
![](attachments/Pasted%20image%2020230120163105.png)

- 参数、命名、通配路由
 ![](attachments/Pasted%20image%2020230120163220.png)
![](attachments/Pasted%20image%2020230120163442.png)
![](attachments/Pasted%20image%2020230120164110.png)