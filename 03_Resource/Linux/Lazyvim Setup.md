### nvim cache 삭제
```zsh

-- 백업
mv ~/.config/nvim{,.bak}

mv ~/.local/share/nvim{,.bak}
mv ~/.local/state/nvim{,.bak}
mv ~/.cache/nvim{,.bak}


-- 삭제
sudo rm -rf ~/.config/nvim

sudo rm -rf ~/.local/share/nvim
sudo rm -rf ~/.local/state/nvim
sudo rm -rf ~/.cache/nvim

```
### Parkjihoon
```zsh
git clone https://github.com/analysispark/Neovim-setting.git ~/.config/nvim --depth 1
```


### Lzieniew/nvim_config
```zsh

git clone https://github.com/lzieniew/nvim_config.git ~/.config/nvim_config --depth 1

mv ~/.config/nvim_config ~/.config/nvim
```
