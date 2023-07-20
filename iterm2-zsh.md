- [ITerm 2 Install and Setting](#iterm-2-install-and-setting)
  - [Install](#install)
  - [Recomand plugin](#recomand-plugin)
  - [常用指令](#常用指令)
  - [Reference](#reference)

# ITerm 2 Install and Setting

## Install

1. Download ITerm 2. <https://iterm2.com/downloads.html>

2. Install homebrew in terminal

3. install zsh.
   ```
   brew install zsh
   ```

4. install oh-my-zsh
   ```
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
   ```

5. Install plugins
    ```
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

    git clone https://github.com/zsh-users/zsh-completions ~/.oh-my-zsh/custom/plugins/zsh-completions

    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

    brew install peco

    # cdr用目录
    mkdir -p $HOME/.cache/shell/

    # 安装kube-ps1，并将brew提示的source和指令添加到下一步覆盖后的.zshrc文件的最后。
    # 需要安装kubectl，kube-ps1依赖kubectl
    brew install kube-ps1
    ```

6. 用本目录下的 `zshrc` 覆盖 `~/.zshrc`。注意文件名要以.开头

7. 设置左移一个单词快捷键
   iTerm2 -> Preferences -> Profiles -> Keys -> Key Mappings -> add new -> Action:Send Escape Sequence -> esc + b(向左) -> esc + f (向右)

8. source ~/.zshrc
    
## Recomand plugin
* zsh-syntax-highlighting
* zsh-completions
* zsh-autosuggestions
* peco 
* cdr 
* kube-ps1

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
