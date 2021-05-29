# Windows 基本命令

标签（空格分隔）： Windows

---

## 网络服务

### 查看监听端口的进程

```netstat -ao | findstr 80```

### 查看操作系统限制的端口

```netsh interface ipv4 show excludedportrange protocol=tcp```

参考：
<https://ardalis.com/attempt-made-to-access-socket/>
<https://www.cnblogs.com/zsmumu/p/13389816.html>
