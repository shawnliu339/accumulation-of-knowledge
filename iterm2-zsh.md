- [ITerm 2 Install and Setting](#iterm-2-install-and-setting)
  - [Install](#install)
  - [Recomand plugin](#recomand-plugin)
    - [Install plugin](#install-plugin)
    - [peco](#peco)
    - [cdr](#cdr)
  - [常用指令](#常用指令)
  - [Reference](#reference)

# ITerm 2 Install and Setting

## Install

1. Download ITerm 2. <https://iterm2.com/downloads.html>

2. Install homebrew in terminal

    ```bash
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```

3. Follow reference to config zsh.

4. Added original $PATH into zsh

    ```bash
    vim ~/.zshrc
    ```

    added below command into config file.
    ```
    # Add bash command
    source ~/.bash_profile
    ```

5. 设置指令检索去重
    ```bash
    vim ~/.zshrc
    ```

    added below command into config file.
    ```
    # Erase dup history
    setopt histignorealldups
    ```

6. 设置左移一个单词快捷键
   iTerm2 -> Preferences -> Profiles -> Keys -> Key Mappings -> add new -> Action:Send Escape Sequence -> esc + b(向左) -> esc + f (向右)
    
## Recomand plugin
* zsh-syntax-highlighting
* zsh-completions
* zsh-autosuggestions
* peco 
* cdr 

### Install plugin
1. Install plugins
```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

git clone https://github.com/zsh-users/zsh-completions ~/.oh-my-zsh/custom/plugins/zsh-completions

git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
2. Added to ~/.zshrc
```
plugins=(git zsh-syntax-highlighting zsh-completions zsh-autosuggestions)
```

3. 启动kube-ps1显示k8s信息
```
    plugins=(
        kube-ps1
    )
```
直接修改candy theme的prompt
```
vim ~/.oh-my-zsh/themes/candy.zsh-theme
PROMPT='$(kube_ps1)'$PROMPT
```

### peco
交互式过滤工具。
```
brew install peco
```
vim ~/.zshrc 在最后加入
```
# ⌃ r で peco で history 検索
function peco-history-selection() {
    local tac
    if which tac > /dev/null; then
        tac="tac"
    else
        tac="tail -r"
    fi
 
    BUFFER=$(\history -n 1 | \
        eval $tac | \
        peco --query "$LBUFFER")
    CURSOR=$#BUFFER
    zle clear-screen
}
zle -N peco-history-selection
bindkey '^r' peco-history-selection
```

### cdr
1. 首先创建文件夹
```
mkdir -p $HOME/.cache/shell/
```
2.  vim ~/.zshrc 在最后加入
```
# cdr, add-zsh-hook を有効にする
autoload -Uz chpwd_recent_dirs cdr add-zsh-hook
add-zsh-hook chpwd chpwd_recent_dirs

# cdr の設定
zstyle ':completion:*' recent-dirs-insert both
zstyle ':chpwd:*' recent-dirs-max 500
zstyle ':chpwd:*' recent-dirs-default true
zstyle ':chpwd:*' recent-dirs-file "$HOME/.cache/shell/chpwd-recent-dirs"
zstyle ':chpwd:*' recent-dirs-pushd true

function peco-cdr() {
    local selected_dir=$(cdr -l | awk '{ print $2 }' | peco)
    if [ -n "$selected_dir" ]; then
        BUFFER="cd ${selected_dir}"
        zle accept-line
    fi
    zle clear-screen
}
zle -N peco-cdr
bindkey '^q' peco-cdr
```
3. source ~/.zshrc


## 常用指令
光标移动
Ctrl + e　　　　移动到行尾(end)

Ctrl + a　　　　移动到行首(ahead)

Ctrl + 左/右　　向左/右移动一个单词

删除
Ctrl + w　　　　删除光标左边一个单词(word)

Ctrl + u　　　　删除光标左边全部


## Reference

1. <https://qiita.com/iwaseasahi/items/a2b00b65ebd06785b443>
2. <https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md>
3. <https://xiaozhou.net/learn-the-command-line-iterm-and-zsh-2017-06-23.html>
4. history-peco: https://suwaru.tokyo/history%E4%B8%8D%E8%A6%81%EF%BC%81peco%E3%81%A7linux%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E5%B1%A5%E6%AD%B4%E3%82%92%E7%88%86%E9%80%9F%E3%81%A7%E6%A4%9C%E7%B4%A2/
5. cdr: https://wada811.blogspot.com/2014/09/zsh-cdr.html
