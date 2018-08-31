# Brew

安装

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

卸载

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
```

替换brew.git

```bash
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
```

替换homebrew-core.git

```bash
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
```

重置brew.git

```bash
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git
```

重置homebrew-core.git

```bash
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```



