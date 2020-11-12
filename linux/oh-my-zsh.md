# oh my zsh

输入以下命令安装 oh my zsh

```bash
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

启动iTerm 2 默认使用dash改用zsh解决方法

```bash
chsh -s /bin/zsh
```

如果想切换回原来的dash

```bash
chsh -s /bin/bash
```

主题

```text
ZSH_THEME="af-magic"
```

插件

```text
plugins=(git z extract zsh-autosuggestions vscode)
```

安装zsh-sutosuggestions

{% embed url="https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md\#oh-my-zsh" %}

按照vscode

```text
git clone https://github.com/valentinocossar/vscode.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/vscode
```



