1. 添加upstream，追加fork项目的原始地址
```
git remote add upstream <url>
```

2. git add无法正常显示中文文件名
```
git config –global core.quotepath false
```

3. 查看gitconfig设置
```
// 设置全局
git config --global user.name "Author Name"
git config --global user.email "Author Email"

// 或者设置本地项目库配置
git config user.name "Author Name"
git config user.email "Author Email"

// 查看
git config --get user.name

// 查看所有设置
git config -l
```
5. 修改上条提交的author
```
git commit --amend --author="NewAuthor <NewEmail@address.com>"
```

6. 通过shell脚本修改所有过去提交中的author信息。
```
#!/bin/sh

git filter-branch --env-filter '

an="$GIT_AUTHOR_NAME"
am="$GIT_AUTHOR_EMAIL"
cn="$GIT_COMMITTER_NAME"
cm="$GIT_COMMITTER_EMAIL"

if [ "$GIT_COMMITTER_EMAIL" = "[Your Old Email]" ]
then
    cn="[Your New Author Name]"
    cm="[Your New Email]"
fi
if [ "$GIT_AUTHOR_EMAIL" = "[Your Old Email]" ]
then
    an="[Your New Author Name]"
    am="[Your New Email]"
fi

export GIT_AUTHOR_NAME="$an"
export GIT_AUTHOR_EMAIL="$am"
export GIT_COMMITTER_NAME="$cn"
export GIT_COMMITTER_EMAIL="$cm"
'
```