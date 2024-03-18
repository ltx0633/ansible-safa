#### [目录结构](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)

```
|
roles
├── os # 目录顶层
│   ├── handlers 
│   │   └── main.yml # notify ，ansible会寻找目录下的main.yml 可在全局引用
│   ├── run.yml #入口文件，可在此处include roles
│   ├── tasks #任务文件，所有yml都在main.yaml 注册
│   │   ├── audit.yml
│   │   ├── close-service.yml
│   │   ├── file-permission.yml
│   │   ├── issue.yml
│   │   ├── limit.yml
│   │   ├── main.yml
│   │   ├── no-login.yml
│   │   ├── ntp.yml
│   │   ├── pam.yml
│   │   ├── profile.yml
│   │   ├── rsyslog.yml
│   │   ├── sshd.yml
│   │   └── update-password.yml
│   ├── templates #模板文件，ansible会寻找目录下的main.yml 可在全局引用
│   │   └── issus.conf.j2
│   └── vars # 变量，ansible会寻找目录下的main.yml 可在全局引用
│       └── main.yml
└── soft-install
    ├── handlers
    ├── tasks
    │   ├── linux-base.yml
    │   ├── main.yml
    │   └── openresty.yml
    └── vars
        └── main.yml
```

#### 功能介绍
使用 `ansible-playbook` 对服务器进行安全加固，包含以下功能
- 入侵防范
    - 密码强度
    - 禁止非必要用户登陆权限
    - 登陆失败锁定
- 访问控制
    - 对关键文件最小权限
    
- 安全审计
    - 对关键操作监察(audit)
    - 记录系统详细日志(rsyslog)
- 系统管理
    - ntp同步
    - 禁止无用程序启动
    - 终端超时
    - 系统limit
    - 终端登陆欢迎消息格式化

#### 版本
`ansible-playbook` 2.16.4


#### 运行
[如使用`inventory`记录远程信息，请使用`values-pass`对敏感文件加密](https://docs.ansible.com/ansible/2.9/user_guide/playbooks_vault.html)
```
ansible-vault create inventory.txt 
```


指定模块路径中的`run.yml`即可执行该任务组

```
# 全部初始化
ansible-playbook -i inventory.txt  --ask-vault-pass   -b -e host_ip=10.10.2.10 -v os/run.yml

#通过tag运行指定
ansible-playbook -b -e host_ip=10.10.2.10 -v run.yml -t audit
```