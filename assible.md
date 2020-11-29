# Ansible

## 简介

用于对一组远程服务器进行软件预安装，配置和部署的软件

## memo

```bash
ssh-agent bash
ssh-add ~/.ssh/id_rsa
```

ssh-agent用于管理ssh钥匙的软件。ssh-agent bash打开对应的bash指令平台，然后，可以对ssh钥匙进行管理。ssh-add是将对应目录下的秘钥放入ssh-agent的管理范围内。

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

    added `source ~/.bash_profile` into config file.

## Reference

1. <https://qiita.com/iwaseasahi/items/a2b00b65ebd06785b443>