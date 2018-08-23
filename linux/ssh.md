# SSH

安装ssh服务 

```bash
sudo apt-get install openssh-server
```

可以对 openssh server进行配置 

```bash
sudo vi /etc/ssh/sshd_config
```

生成ssh-keygen 复制到另一台主机 

```bash
scp file username@ip:file
```

```bash
cat id_rsa.pub >> authorized_keys
```

Remove the cached key for ip on the local machine ssh-keygen -R ip

