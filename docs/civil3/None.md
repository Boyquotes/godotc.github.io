
- [ ] generate a NFT  and upload on mint
	搞定一个算法/流程：
		1.  盲盒, 用户开盒的时候生成 NFT 才 mint 到链上
		2. mint 的时候执行


# 分享

- [ ] NFT  -> OpenSea
- [ ] 多签钱包 & token swap
- [ ] 空投


# POC

## 2023-4-22

- [ ] 打通 NFT 合约和 OpenSea (高)
	- 生成nft metadata
	- 智能合约和OpenSea的交互形式 
	- TokenURI 与 FileCoin 的对应 
- [ ] 后端监听链合约事件
	- golang 后端监听链上合约的事件
	- 写入tranfser、approve、mint 等事件到数据库供给数据分析
- [ ] 开发第一版 Token智能合约 (普通的，暂不考虑经济模型)
	- CICD
	- DAO 前置
	-  部署到某条链上并作为前端的基础测试环境
	~~- 确认代币的发放模式: 固定并线性释放或其他形式 ~~
 - [ ] swap的形式确认
	 - Uniswap
 - [ ] 基于Token, NFT 逻辑较复杂的DAO
	 - Govornor



## 2023-3-19

1. [x] 跨链, `Bridge`, 在多条链上流通, 提取token到不同链上, 预言机与聚合器, multichain-contract `(godot)`
	未有太好的解决方案

2. [x] 测试网络与 CICD (自动部署, data: address, abi) `(曙光)`
	 - goerlin
	 - rostpen
	 - contract address
	  - abi, bin, json
	  - FE js interaction
2. [ ] 空投 `(曙光)`
	- merkle tree
 
4. [x] Vote extension & 白名单 (NFT) `(godot)`

### Token
1. [ ]   认购代币 & Token Swap `(charlie)`
	- uniswap
	- token
2.  [x]   ~~token可升级~~
3.  [x] 签名流程 & 多签钱包 `(charlie)`
	-  wallet address
	-  contract address
	-  ETH transfer to contract
 
4. [ ] 手续费 & 代币质押  `(virtual)`

5. [ ] 线性释放(或自定义释放) `等待经济模型出来`
	- totalsupply 分批次
	-  分账，回收，销毁 (参考项目计划书中的初步设想)


### NFT

1. [ ] NFT图片随机生成，json文件生成, 开图(mint 过程中) `(david)` 
2. ~~NFT交易所~~
3. ~~ERC1155~~