# HBase常用command
1. 取前10行的数据（查看rowkey的形式）
```
scan 'table', {LIMIT => 10}
```

2. rowkey filter和翻转字符
```
scan 'table', {ROWPREFIXFILTER => '1089018'.reverse}
```

3. 根据rowkey取数据
```
get 'table', 'rowkey'
```

4. 根据row prefix选取区间
```
scan 'table', {STARTROW => '100', STOPROW => '200'}
```
选取rowkey前缀从100到200的区间的数据。  

5. HBase可以进行字符串拼接
```
scan 'bot_follower_stats', {STARTROW => '1089018' + '-20220323'}
```

