## 信息
- mysql版本 5.5
- 引擎 innodb
- 两表结构如下
- 表B 305785行 字符集 uft8mb4

名称 | 类型|长度
---|---|---
price | varchar | 100
timestampFromEP | varchar | 100
PendingTxnHash（unique索引） | varchar | 180
Nonce | varchar | 180
Year | varchar | 180
Month | varchar | 180
Day | varchar | 180
Time | varchar | 180
GasLimit | varchar | 180
GasPrice | varchar | 180
From | varchar | 180
To | varchar | 180
Value | varchar | 180
timestampFromPeding | varchar | 100
    

- 表A 274475行 字符集 uft8mb4
    
名称 | 类型|长度
---|---|---
StaticBlockReward | varchar | 200
TxnFeeForMiner | varchar | 200
UncleReward | varchar | 200
Difficulty | varchar | 200
TotalDifficulty | varchar | 200
Size | varchar | 200
GasUsed | varchar | 200
GasLimit | varchar | 200
PreviousHash | varchar | 200
ParentHash | varchar | 200
Sha3Uncles | varchar | 200
Blocks | varchar | 150
TxnHash（unique索引） | varchar | 180
TransactionValue | varchar | 180
TxnFeeFromTr | varchar | 180
Reverted | varchar | 180



- sql语句
```sql
CREATE TABLE finaldata5 SELECT
StaticBlockReward,
TxnFeeForMiner,
UncleReward,
Difficulty,
TotalDifficulty,
Size,
GasUsed,
a.GasLimit AS gaslimitfromTransaction,
PreviousHash,
ParentHash,
Sha3Uncles,
Blocks,
TxnHash,
TransactionValue,
TxnFeeFromTr,
Reverted,
price,
timestampFromEP,
PendingTxnHash,
Nonce,
b.GasLimit AS gaslimitfrompending,
GasPrice AS pendingGasPrice,
`From`,
`To`,
`Value` AS pendingValue,
timestampFromPeding 
FROM
	transactionandblocks AS a
	INNER JOIN epandpendingdistinct AS b ON a.TxnHash = b.PendingTxnHash 
```
- 新表 258105行
- sql语句explain

id | select_type | table |type|possible_key|key|ken_len|ref|rows|extra
---|---|---|---|---|---|---|---|---|---|
1 | SIMPLE|A|ALL|Null|Null|Null|Null|274475|Using temporary; Using filesort
1 | SIMPLE|B|ALL|Null|Null|Null|Null|518612|Using where; Using join buffer


- sql执行时间 722.198s

- mysql 慢查询日志
```sql

from transactionandblocks as a INNER JOIN epandpendingdistinct as b on a.TxnHash = b.PendingTxnHash;
# Time: 191210  0:17:30
# User@Host: ether[ether] @  [10.22.11.104]
# Query_time: 722.197764  Lock_time: 0.123007 Rows_sent: 0  Rows_examined: 532250
SET timestamp=1575908250;
create table finaldata5 select StaticBlockReward,
TxnFeeForMiner,
UncleReward,
Difficulty,
TotalDifficulty,
Size,
GasUsed,
a.GasLimit as gaslimitfromTransaction,
PreviousHash,
ParentHash,
Sha3Uncles,
Blocks,
TxnHash,
TransactionValue,
TxnFeeFromTr,
Reverted,
price,
timestampFromEP,
PendingTxnHash,
Nonce,
b.GasLimit as gaslimitfrompending,
GasPrice as pendingGasPrice,
`From`,
`To`,
`Value` AS pendingValue,
timestampFromPeding
```
