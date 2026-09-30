# Linux-mint 22.2 - RTX 4080  
  
  
  
## SSH 및 기본세팅  
  
sudo apt update && sudo apt upgrade  
  
sudo apt install openssh-server sshfs -y  
sudo systemctl status ssh  
  
sudo vi /etc/ssh/sshd_config  
—  
  
sudo apt install zsh tmux neofetch unzip build-essential -y  
sudo add-apt-repository ppa:git-core/ppa -y  
sudo apt-get update  
sudo apt-get install git -y  
—  
##   
git config *--global user.name jihoonpark*  
git config *--global user.email analysispark@gmail.com*  
git config credential.helper store  
  
  
## Oh-my-zsh & Oh-my-posh  
  
sh -c "$(wget -O- [https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"](https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)  
curl -s https://ohmyposh.dev/install.sh | bash -s  
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting  
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions  
  
// zshrc  
plugins=(git  
        zsh-syntax-highlighting  
        zsh-autosuggestions  
)  
  
—  
  
  
## node & rpm  
  
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash  
  
// 터미널 재실행  
  
export NVM_DIR="$HOME/.nvm"  
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm  
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion  
  
nvm --version  
  
  
nvm install 14  
  
  
nvm ls  
  
node --version  
—  
  
  
## Lazyvim  
  
# Neovim 최신버전 다운  
curl -LO [https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.appimage](https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.appimage)  
  
chmod u+x nvim-linux-x86_64.appimage  
  
sudo mkdir -p /opt/nvim  
sudo mv nvim-linux-x86_64.appimage /opt/nvim/nvim  
  
echo 'export PATH="$PATH:/opt/nvim/"' >> ~/.zshrc  
source ~/.zshrc  
  
  
git clone https://github.com/LazyVim/starter ~/.config/nvim  
rm -rf ~/.config/nvim/.git  
  
  
  
// DaddyTimeMono Nerd Font  
mkdir -p ~/.local/share/fonts/NerdFonts  
cd ~/.local/share/fonts/NerdFonts  
wget https://github.com/ryanoasis/nerd-fonts/releases/download/v3.0.2/DaddyTimeMono.zip  
  
sudo apt install unzip fontconfig  
  
unzip DaddyTimeMono.zip  
  
fc-cache -fv  
  
rm DaddyTimeMono.zip  
  
---  
  
  
  
## Miniforge  
  
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"  
bash Miniforge3-$(uname)-$(uname -m).sh  
source ~/.zshrc  
  
// My nvim Lua Setting  
pip install isort pylint black  
  
  
//Miniforge  
conda create -n park_sci python=3.10  
conda activate park_sci  
  
conda update -n base -c conda-forge conda  
  
pip install tensorflow==2.14.0  
pip install nvidia-cudnn-cu11==8.6.0.163  
pip install nvidia-cublas-cu11  
pip install nvidia-cufft-cu11  
pip install nvidia-cuda-runtime-cu11  
pip install nvidia-cusolver-cu11  
pip install nvidia-cusparse-cu11  
pip install nvidia-nccl-cu11  
pip install nvidia-cuda-nvcc-cu11==11.8.89  
  
  
pip install ipykernel  
pip install pandas numpy==1.24.3 matplotlib scikit-learn opencv-contrib-python imbalanced-learn seaborn  
  
python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"  
