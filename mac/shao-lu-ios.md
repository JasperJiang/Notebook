# 烧录IOS

#### 查看磁盘列表

```bash
diskutil list
```

#### 取消磁盘挂载

```bash
diskutil unmountDisk /dev/disk4
```

#### 烧制ISO文件到优盘

```bash
sudo dd if=cn_windows_8_1_x64_dvd_2707237.iso of=/dev/disk1 bs=512
```

#### 弹出磁盘

```bash
diskutil eject /dev/disk4
```

command + t 查看进度

