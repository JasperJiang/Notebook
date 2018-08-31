# Install And use nvm

```bash
brew update
brew install nvm
mkdir ~/.nvm
vi ~/.bash_profile
```

In your `.bash_profile` file \(you may be using an other file, according to your shell\), add the following :

```bash
export NVM_DIR=~/.nvm
source $(brew --prefix nvm)/nvm.sh
```

Install node

```bash
nvm install 0.12
```

To have a node activated by default \(not to have to `nvm use` on each new shell\), run this \(stable being the id of the version\):

```bash
nvm alias default stable
```

show list remote

```bash
nvm ls-remote
```



