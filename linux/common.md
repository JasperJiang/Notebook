# Common

### 批量修改文件夹名字

```bash
find . -depth -type d -name OriginalName -exec sh -c 'mv "${0}" "${0%/OriginalName}/NewName"' {} \;
```

### 查看CPU信息（型号）

```bash
cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c
```

### 查看运行情况

```bash
top
```

### 查看磁盘大小

```bash
df -h
```

### 查看文件或文件夹大小

```bash
du -sh directory/
```

### 创建用户

```bash
adduser username
passwd username
usermod -G xxx username
```

### 更改自己的密码

```bash
passwd
```

### 用scp下载文件

```bash
scp root@1.2.3.4:/root/pcfilename.rar ./
```

### 打包文件

```bash
tar cvf tecmint-14-09-12.tar /home/tecmint/
```

### 打包并且压缩文件

```bash
tar cvzf MyImages-14-09-12.tar.gz /home/MyImages
```

### 拆包文件

```bash
tar xvf public_html-14-09-12.tar
```

### 拆包文件到制定文件夹

```bash
tar xvf public_html-14-09-12.tar -C /home/public_html/videos/
```

### 查看十名空间最大的

```bash
du -hsx * | sort -rh | head -10
```

### 软连接move

move docker lib path  
1\) Stop docker: service docker stop. Verify no docker process is running ps faux  
2\) Double check docker really isn't running. Take a look at the current docker directory: ls /var/lib/docker/  
2b\) Make a backup - tar -zcC /var/lib docker &gt; /mnt/pd0/var\_lib\_docker-backup-$\(date +%s\).tar.gz  
3\) Move the /var/lib/docker directory to your new partition: mv /var/lib/docker /mnt/pd0/docker  
4\) Make a symlink: ln -s /mnt/pd0/docker /var/lib/docker  
5\) Take a peek at the directory structure to make sure it looks like it did before the mv: ls /var/lib/docker/ \(note the trailing slash to resolve the symlink\)  
6\) Start docker back up service docker start  
7\) restart your containers

### 安装vim

```bash
apt-get install vim
```

### 检查网络

```bash
netstat -rn
```

### 查看端口使用情况

```bash
netstat -tulpn | grep 8080
```

### 查看进程cpu占用情况

```bash
ps -eo pcpu,pid,user,args | sort -k 1 -r | head -16
```

### static network 静态IP设置

```bash
vi /etc/network/interfaces
```

添加内容：

```text
auto eth0
iface eth0 inet static
address 192.168.8.100    
```

