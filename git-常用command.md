# git常用command
1. 只加入已经加入git的文件
```
git add -u
```

2. 撤销head
```
git reset --hard HEAD
```

3. 从add中撤销文件
```
git reset <file>
```

4. 省略set-upstream和branch名直接推送
```
git push -u origin HEAD
```

5. git新建worktree(类似于工作空间的概念）。
每个工作空间共用branch，但是，相互独立，因此，在不同的工作空间中可以独立的修改不同的branch。
而同一个工作空间若想同时修改不同的branch，则需要stash后checkout到其他branch上。
```
git worktree add ../new_path <branch>
```

6. 添加upstream，追加fork项目的原始地址
```
git remote add upstream <url>
```