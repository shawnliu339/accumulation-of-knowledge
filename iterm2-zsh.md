# ITerm 2 Instal and Setting

## Instal

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

    # Erase dup history
    setopt histignorealldups
    ```
    
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

## Reference

1. <https://qiita.com/iwaseasahi/items/a2b00b65ebd06785b443>
2. <https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md>
3. <https://xiaozhou.net/learn-the-command-line-iterm-and-zsh-2017-06-23.html>
4. history-peco: https://suwaru.tokyo/history%E4%B8%8D%E8%A6%81%EF%BC%81peco%E3%81%A7linux%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E5%B1%A5%E6%AD%B4%E3%82%92%E7%88%86%E9%80%9F%E3%81%A7%E6%A4%9C%E7%B4%A2/
5. cdr: https://wada811.blogspot.com/2014/09/zsh-cdr.html
