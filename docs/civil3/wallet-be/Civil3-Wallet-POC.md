# Civil3 Wallet POC BE

> https://github.com/Civil3Labs/TaskBook/issues/208

## References:

- Repositories:
FE Website ： [https://github.com/Civil3Labs/official-site/](https://github.com/Civil3Labs/official-site/)  
BE Wallet-Service : [https://github.com/Civil3Labs/wallet-be](https://github.com/Civil3Labs/wallet-be)


## Task

1. 登录成功后拿到相关token信息  
2. 列出当前用户的钱包账户相关内容  
3. 在测试网络进行货币的创建  
4. 在测试网络进行货币的认购交易

> Actually is 3:
1. Connect to network (test), then call contract address
2. User account info on networks (or and store more details on our service)
3. Deploy `CIVIL3` contract

## 1. Core-Features

> https://app.optimism.io/bridge/deposit

#### 1 . Login the homepage by `wallet` , get account information

- Wallet Types:
	- `Rainbow`
	- `Coinbase Wallet`
	- `MetaMask` ( Prepare to support for this )
	- `WalletConnect`

![](attachments/Pasted%20image%2020230210233438.png)

#### 2. ~~Add our network to wallet account (On MetaMask)~~

> Add a network on eth wallet, but our project depends on public chain

![](attachments/Pasted%20image%2020230210233354.png)

#### 3. Interact with CIVIL3 contract

- Convert from other tokens like `ETH`, `USDT` to `CIVIL` 
- Buy  `CIVIL` by manual pay methods (PAYPAL)

#### 4. Transfer and Transactions histroy 

- Transfer between accounts
- From blockchain
	- Get all/self transactions 
	- Transaction infomations 


## 2. Interfaces analyze

- Under `/wallet` 

| mapping         | description                                    |
| --------------- | ---------------------------------------------- |
| `/login`        | Connect to chain networks, return initial info |
| `/account`      | Get self info(tokens)                          |
| `/transfer`     | Transfer action                                |
| `/transactions` | Return the transactions of specific address    |
| `/exchange`     | From other token to `CIVIL`                    |

## 3. Services interaction

- Sperate `wallet`  and  `account`
- A eth acoount will bind to our account `@User-Service`


## 4. IDL

### 1. Login
```protobuf
message UserInfo{
	uint256 Address =1;
	string Profile =2;
	string UserName =3;
}

message LoginRequest {
	uint256 Address = 2;
}
message LoginRespone {
	bool bRegistered  =1;
	optional UserInfo UserInfo =2;
}
```

### 2. Account
```protobuf
message Token{
	string Symbol =1;
	uint256 Amount = 2;
}
message AccountRequest {
	uint256 Address = 1;	
}
message AccountRespone {
	UserInfo UserInfo = 1;
	repeated Token = 2;
}
```

### 3. Transfer
```protobuf
message TransferRequest {
	uint256 From =1;
	uint256 To = 2;
	string Symbol = 3;
	uin256 amount = 4;
}

message TransferRespone {
	bool bSuccess =1;
	uint256	TransactionId = 2;
	uint256 Noce = 3;
}
```
### 4. Transactions
```protobuf
message Transaction {
	uint256 TransactionId =1;
	uint256 From = 2;	
	uint256 To = 3;	
	uint256 Amount = 4;
	string  Data = 5;	
}


message TransactionsRequest {
	uint256 Address =1; // 0 for all transactions	
}
message TransactionsRespone {
	repeated uin256 TransactionsId = 1;
}
```
