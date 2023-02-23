#### 消息格式
- id
 - payload
  - ...
   
#### 大致的功能：
1. 向 `kafka` push& pull
	 -  从kfka订阅，序列化 (ws 二进制) 返回给用户
2. 用户消息反序列化
	- 得到command content 
	- 放到对应的kafka里面
 
#### Additional
- 使用`protobuf`
- 前端使用 `json` ， socket-gateway 解析json binary
 
