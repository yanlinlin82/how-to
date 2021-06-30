# How-To

---

## SSH

### 如何查询当前SSH连接的客户端IP地址

使用环境变量`$SSH_CLIENT`：
```sh
$ echo $SSH_CLIENT  # 其结果将会分别列出客户端IP、客户端端口、服务端端口（以空格隔开）
```

参考：<https://stackoverflow.com/questions/996231/find-the-ip-address-of-the-client-in-an-ssh-session>

### 如何配置自动加载ssh-agent

在`~/.bashrc`中，加入：
```sh
if [ -z "$SSH_AUTH_SOCK" ] ; then
    eval `ssh-agent -s`
    ssh-add
fi
```
 
参考：<https://snippets.khromov.se/automatically-start-ssh-agent-and-add-your-keys-in-windows-subsystem-for-linux-wsl-ubuntu/>

---

## WSL

### 如何启动WSL的SSH服务，并允许远程登录？

1. 需要重新安装`openssh-server`：
    ```sh
    sudo apt remove openssh-server
    sudo apt install openssh-server
    ```

2. 然后，修改配置文件`/etc/ssh/sshd_config`：

    将`PasswordAuthentication`设置为`yes`

    并增加一行（注意设置允许登录的用户名）：
    ```
    AllowUsers yourusername
    ```

3. 最后，重启服务：
    ```sh
    sudo service ssh start
    ```
    或
    ```sh
    service ssh --full-restart
    ```

    查看服务状态：
    ```sh
    service ssh status
    ```

参考：<https://www.illuminiastudios.com/dev-diaries/ssh-on-windows-subsystem-for-linux/>

### 如何修改WSL终端启动的缺省路径？

两种方法：

1. 修改`.bashrc`，加入命令`cd ~`
2. WSL启动命令行，改为：`wsl -d Ubuntu sh -c "cd ~;exec $SHELL"`

参考：<https://github.com/microsoft/terminal/issues/3286>
