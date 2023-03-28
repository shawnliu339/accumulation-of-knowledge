# 其他
1. terminal上显示剪贴板内容
```
pbpaste
```
2. terminal快速查询json字符串
```
jq
```
3. curl
```
curl \
-X POST \
-H 'Content-Type: application/json; charset=UTF-8' \
-H 'X-CLIENT-API-KEY: xxxx' \
-H 'X-User: xxxxx' \
-H 'X-BTS: xxxxx' \
-d '{
}' \
'url'
```
4. git grep 检查git目录下的文件
5. grep查询当前目录下的包含关键字的文件位置
```
grep . 'kakyoin' -R
```
6. csvq 使用sql语句对csv进行检索
没有header，分割为tab，检索example文件中前十行
```
csvq -n -d '\t' 'select c1 from example limit 10
```
第一行为header，分割为tab，检索example文件中前十行
```
csvq -d '\t' 'select c1 from example limit 10
```
7. make https://www.ruanyifeng.com/blog/2015/02/make.html

8. chmod 777
三个数字分别为给owner(文件拥有者), group(群组), other(其他用户)分别赋予可读可写可执行权限

9. 查看本占用端口的pid
```
lsof -i tcp:8080
```
10. 



