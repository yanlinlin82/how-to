# How-To

---

## Tmux终端

### 如何翻看tmux屏幕历史

按下组合键`<Ctrl>` + `b`，放开，再按 `[`，即可进入滚屏模式，通过方向键或翻页键进行上下滚动，并可使用`<ESC>`键退出。

参考：<https://www.geek-share.com/detail/2766041673.html>

### 设置允许tmux屏幕显示颜色

修改`~/.tmux.conf`配置文件，增加一行：
```
set -g default-terminal "screen-256color"
```

修改后，再次启动`tmux`，可以看到`TERM`环境变量已更新（此前是`screen`）：
```sh
$ echo $TERM
screen-256color
```

参考：<https://unix.stackexchange.com/questions/1045/getting-256-colors-to-work-in-tmux>

### 解决tmux中Ctrl+Left/Right不按单词移动光标的问题

修改`~/.tmux.conf`文件，加入：
```
set-window-option -g xterm-keys on
```

或者直接操作：
```
C-b :set-window-option xterm-keys on
```
即按下`<Ctrl>`+`b`后，直接输入冒号（:）及之后的命令，然后回车。

参考：<https://stackoverflow.com/questions/29474794/use-ctrl-left-right-to-move-forward-back-one-word-in-tmux-within-mobaxterm>

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

---

## Perl

### 读取并解析JSON

```perl
use JSON;
use Data::Dumper;
my $s = '{"a":[1,2],"b":"xyz"}';
my $x = decode_json $utf8_json_text;
print Dumper $x;
```

结果如下：

```
$VAR1 = {
          'a' => [
                   1,
                   2
                 ],
          'b' => 'xyz'
        };
```

参考：<https://metacpan.org/pod/JSON>

### 宽字符print警告

对于提示警告：`Wide character in print at ...`

可以在perl脚本一开始加入：

```perl
use utf8;
binmode(STDOUT, "encoding(UTF-8)");
```

进行解决。

参考：<https://stackoverflow.com/questions/47940662/how-to-get-rid-of-wide-character-in-print-at>
