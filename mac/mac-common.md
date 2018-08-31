# Mac Common

Hide Folder

```bash
chflags hidden/nohidden <folder>
```

Set Finder show hidden folder

```bash
defaults write com.apple.finder AppleShowAllFiles TRUE/FALSE
killall Finder
```

任何来源

```bash
sudo spctl --master-disable
```

通过终端命令调整 Dock 栏的隐藏速度

```bash
defaults write com.apple.dock autohide-delay -int 0（时间设为最短）
defaults write com.apple.dock autohide-delay -int 0.5（时间设为 0.5s）
killall Dock
```

截图保存路径

```bash
defaults write com.apple.screencapture location /path/
killall SystemUIServer
```

