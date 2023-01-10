# git
1. 只加入已经加入git的文件
```
git add -u
```
2. 撤销head
```
git reset --hard HEAD
```
3. 省略set-upstream和branch名直接推送
```
git push -u origin HEAD
```
4. git新建worktree(类似于工作空间的概念）。
每个工作空间共用branch，但是，相互独立，因此，在不同的工作空间中可以独立的修改不同的branch。
而同一个工作空间若想同时修改不同的branch，则需要stash后checkout到其他branch上。
```
git worktree add ../new_path <branch>
```
5. 添加upstream，追加fork项目的原始地址
```
git remote add upstream <url>
```

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
-H 'X-CLIENT-API-KEY: xxxx' \
-H 'X-User: xxxxx' \
-H 'X-BTS: xxxxx' \
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

9.  

