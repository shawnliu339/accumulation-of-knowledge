# git
1. 只加入已经加入git的文件
```
git add -u
```
2. 省略set-upstream和branch名直接推送
```
git push -u origin HEAD
```
3. terminal上显示剪贴板内容
```
pbpaste
```
4. terminal快速查询json字符串
```
jq
```
5. curl
```
curl \
-H 'X-CLIENT-API-KEY: xxxx' \
-H 'X-User: xxxxx' \
-H 'X-BTS: xxxxx' \
'url'
```
6. git grep 检查git目录下的文件
7. grep查询当前目录下的包含关键字的文件位置
```
grep . 'kakyoin' -R
```