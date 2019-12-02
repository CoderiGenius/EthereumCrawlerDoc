# 爬虫
## 共五个分支
- master 根据区块号获取transaction数据
- pending 实时获取pending数据
- block 根据区块号获取区块数据
- GetEtherprice 获取分钟级Etherprice数据
- CheckStatus 检查爬虫状态（废弃）


# 数据库表结构
### Blocks

名 | 类型 | 长度 |解释
---|---|---|---|
id | int | 10 | 主键
Time | varchar | 255 | 区块生成时间 05:37:11 （不准确，会有时区差别）
TransactionNum | varchar | 255 |此区块所包含transaction数量
MinedBy | varchar | 255 | 被哪个矿工挖取
StaticBlockReward | varchar | 255 | 静态区块奖励，每挖到一个区块获得的奖励
TxnFee | varchar | 255 | 挖矿所获手续费奖励
UncleReward | varchar | 255 | 叔区块奖励
Difficulty | varchar | 255 | 挖矿难度
TotalDifficulty | varchar | 255 | 总挖矿难度
Size | varchar | 255 | 区块大小
GasUsed | varchar | 255 | 用了多少gas
GasLimit | varchar | 255 | gas限制为多少，等于所有transaction的gaslimit之和
PreviousHash | varchar | 255 | 前一个区块hash
ParentHash | varchar | 255 | 区块hash
Sha3Uncles | varchar | 255 | webjs所用参数
Nonce | varchar | 255 | 顺序
Blocks | varchar | 255 | 区块号
Month | varchar | 255 | 月份
Day | varchar | 255 | 日
Year | varchar | 255 | 年


### Transactions
名 | 类型 | 长度 |解释
---|---|---|---|
id | int | 11|主键
TxnHash | varchar | 255|transaction唯一标识
Block | varchar | 255|所在区块号
Month | varchar | 255|月
Year | varchar | 255|年
From | varchar | 255|从那个account发起
To | varchar | 255|到哪个account
Value | varchar | 255| 涉及的eth金额
TxnFee | varchar | 255| 手续费
Time | varchar | 255|生成时间 05:37:11 （不准确，会有时区差别）
Day | varchar | 255|日
Reverted | varchar | 255|错误信息，例如out of gas

### etherprice
名 | 类型 | 长度 |解释
---|---|---|---|
id | int | 10|主键
price | varchar | 100|etherprice价格
timestamp | varchar | 100|时间戳（颗粒度为一分钟）


### pendingtransactions
名 | 类型 | 长度 |解释
---|---|---|---|
PendingTxnHash | varchar | 255|等待transaction唯一标识
id | int | 11|主键
Nonce | varchar | 255|排序号
Year | varchar | 255|年
Month | varchar | 255|月
Day | varchar | 255|日
Time | varchar | 255|进队列时间（不准确）
GasLimit | varchar | 255|gas花费限制
GasPrice | varchar | 255|gas价格
From | varchar | 255|从那个account发起
To | varchar | 255|到哪个account
Value | varchar | 255|涉及的eth金额
timestamp | varchar | 100|时间戳


## 数据处理
需要将多个爬虫的语句爬取后合并join在一起

### 合并blocks和transactions表，生成BloksAndTransaction表
```
CREATE table BloksAndTransaction SELECT	StaticBlockReward,a.TxnFee as TxnFeeForMiner,UncleReward,Difficulty,TotalDifficulty,Size,GasUsed,GasLimit,PreviousHash,ParentHash,Sha3Uncles,Nonce	Blocks,TxnHash,`Value` as TransactionValue,b.TxnFee as TxnFeeFromTr,Reverted FROM `Blocks` as a right join `Transactions` as b on a.`Blocks` like b.`Block`  

```

### 合并bloksandtransaction和epandpending表，生成finaldata
```
create table FinalData SELECT StaticBlockReward,TxnFeeForMiner,UncleReward,Difficulty,TotalDifficulty,Size,substring(GasUsed,1,8),PreviousHash,ParentHash,Sha3Uncles,Blocks,TxnHash,TransactionValue,TxnFeeFromTr,Revert,price,timestampFromEP,PendingTxnHash,Nonce,GasLimit,GasPrice,`Value`,timestampFromPeding FROM `epandpending` as a INNER JOIN `bloksandtransaction` as b on a.TxnHash = b.TxnHash limit 0,10
```
