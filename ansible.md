- [Ansible](#ansible)
  - [概念](#概念)
    - [Ansible 总括](#ansible-总括)
    - [control node](#control-node)
    - [managed node](#managed-node)
    - [inventory](#inventory)
    - [playbook](#playbook)
  - [Playbook详解](#playbook详解)
    - [play](#play)
    - [tasks](#tasks)
    - [roles](#roles)
      - [Role的一些基本概念](#role的一些基本概念)
      - [当Role被加入Play](#当role被加入play)
  - [Other](#other)
    - [Template Module](#template-module)
  - [memo](#memo)
  - [Reference](#reference)
# Ansible
## 概念
### Ansible 总括
Ansible时一个自动化运维工具，可以批量服务器配置，批量部署，批量执行任务等。
> If Ansible modules are the tools in your workshop, playbooks are your instruction manuals, and your inventory of hosts are your raw material.

如果拿做菜对Ansible进行比喻的话，module是你做菜的工具，playbook就是你的菜单(料理步骤)，inventory就是你需要用工具和菜单进行加工的材料。

### control node
用于发布配置指令，运行command，playbook等来配置managed node

### managed node
被配置的服务器，也称host。自身不需要安装ansible。通过ssh协议与control node通信，根据control node的指令被配置。

### inventory
主机列表。定义host list的文件，内容包含host id，host ip等

### playbook
任务集，指令集。用于配置managed node的一连串任务

## Playbook详解
### play
> Each playbook is composed of one or more ‘plays’ in a list.  
The goal of a play is to map a group of hosts to some well defined roles, represented by things ansible calls tasks. At a basic level, a task is nothing more than a call to an ansible module.

### tasks
```
tasks:
  - name: make sure apache is running
    service:
      name: httpd
      state: started
```
task必须要包含一个name，如果未命名则用module: action.  
多数service module是通过key-value进行配置.  
另外还有command，shell module。以上两个module不是通过key-value配置.

### roles
#### Role的一些基本概念
> Docs » User Guide » Working With Playbooks » Creating Reusable Playbooks » Roles  
Roles are ways of automatically loading certain vars_files, tasks, and handlers based on a known file structure. Grouping content by roles also allows easy sharing of roles with other users.

可以将Roles理解为特殊的Playbook，它至少包含以下特定的目录中的一个 (每个目录必须包含main.yml)。

个人认为，可以被理解为可以被复用的playbook。
```
webservers.yml
fooservers.yml
roles/
    common/
        tasks/
        handlers/
        files/
        templates/
        vars/
        defaults/
        meta/
    webservers/
        tasks/
        defaults/
        meta/
```

#### 当Role被加入Play
让Role被加入Play中后，会按以下顺序执行：

When you use the roles option at the play level, Ansible treats the roles as static imports and processes them during playbook parsing. Ansible executes your playbook in this order:

* Any pre_tasks defined in the play.
* Any handlers triggered by pre_tasks.
* Each role listed in roles:, in the order listed. Any role dependencies defined in the role’s meta/main.yml run first, subject to tag filtering and conditionals. See Using role dependencies for more details.
* Any tasks defined in the play.
* Any handlers triggered by the roles or tasks.
* Any post_tasks defined in the play.
* Any handlers triggered by post_tasks.

## Other
become: 特权升级。允许用户使用sudo等指令

when: 是通过Jinja2语法进行操作。变量的引用不需要双引号或者双括号。

### Template Module
https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html#id1

src: j2 file

dest: 根据j2 file生成配置文件的地方

## memo
```bash
ssh-agent bash
ssh-add ~/.ssh/id_rsa
```

ssh-agent用于管理ssh钥匙的软件。ssh-agent bash打开对应的bash指令平台，然后，可以对ssh钥匙进行管理。ssh-add是将对应目录下的秘钥放入ssh-agent的管理范围内。

## Reference

1. <https://qiita.com/iwaseasahi/items/a2b00b65ebd06785b443>