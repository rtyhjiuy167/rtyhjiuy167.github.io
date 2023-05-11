```bash
yum install -y keepalived
```

修改`/etc/keepalived`配置文件：

```
! Configuration File for keepalived

global_defs {
   router_id haha # 自己命名，匹配时两个应相同
}

# vrrp 是 keepalived 在内网中通讯的协议
vrrp_instance haha {
    state MASTER # 配对与 backup
    interface eth0 # 需要对应自己的网卡名字
    virtual_router_id 51 # 匹配时两个应相同
    priority 100 # 竞选时的优先级
    advert_int 1 # 间隔检测时间

# 匹配时两个应相同
    authentication {
        auth_type PASS
        auth_pass 1111
    }

# 虚拟地址，一个即可
# 匹配时两个应相同
    virtual_ipaddress {
        192.168.200.16
    }
}

```

