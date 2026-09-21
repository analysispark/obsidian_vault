

```bash
sudo apt update
```

```bash
sudo apt-get install zsh
```

Install “Oh-my-zsh”

```bash
sh -c "$(curl -fsSL <https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh>)"
```

Install “powerlevel10k”

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Install “zsh-autosuggestions”

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

Install “zsh-syntax-highlighting”

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```

Init plugin

```bash
code ~/.zshrc
```

```bash
ZSH_THEME="powerlevel10k/powerlevel10k"

plugins=(git zsh-autosuggestions zsh-syntax-highlighting)

#wsl alias
alias work="cd /mnt/d/Work/"
alias pydir="cd /mnt/d/Work/Python/"
alias icloud="cd /mnt/c/Users/analy/iCloudDrive/Projects/Python/"
alias chrome="/mnt/c/Program\ Files/Google/Chrome/Application/chrome.exe"
alias firefox="/mnt/c/Program\ Files/Mozilla\ Firefox/firefox.exe"
alias open="explorer.exe"
```
