## oh-my-zsh
```zsh
sudo apt update -y && sudo apt upgrade -y

sudo apt install zsh -y
sudo add-apt-repository ppa:git-core/ppa -y
sudo apt-get update
sudo apt-get install git -y

sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"


// zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

// zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

// fzf (Fuzzy Finder )
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf

~/.fzf/install

//
plugins=(git
zsh-syntax-highlighting
zsh-autosuggestions
fzf
)


```

## Oh-my-zsh Theme
```zsh
vim .oh-my-zsh/custom/themes/mrp.zsh-theme

//paste

PROMPT="%(?:%F{#46b5d1} :%F{#d16246} )"
PROMPT+=' %{%F{#46b5d1}%c%{$reset_color%} $(git_prompt_info) %F{#58C456}'

ZSH_THEME_GIT_PROMPT_PREFIX="%F{#89C9D9}\ue727 -> (%F{#89C9D9}"
ZSH_THEME_GIT_PROMPT_SUFFIX="%{$reset_color%} "
ZSH_THEME_GIT_PROMPT_DIRTY="%F{#89C9D9}) %{$fg[yellow]%}✗"
ZSH_THEME_GIT_PROMPT_CLEAN="%F{#89C9D9})"

// theme="mrp"

vim ~/.zshrc
source ~/.zshrc


```
### NAS 드라이브 마운트
```zsh
sudo mkdir /mnt/DATA
sudo -S mount -t drvfs Y: /mnt/DATA
```

## WSL 삭제
```power shell
wsl --unregister Ubuntu-20.04
```


?????????????????????????????????????????????????????????????????????????????????????????????????
## RTX4080 Super - cuda 11.8
## RTX4080 Super - cuDNN 8.5.0
?????????????????????????????????????????????????????????????????????????????????????????????????

// Cuda 11.8

```zsh
sudo apt update && sudo apt install build-essential -y

wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run
chmod +x cuda_11.8.0_520.61.05_linux.run
sudo ./cuda_11.8.0_520.61.05_linux.run

```


// cuDNN 8.9.0
```zsh
sudo rsync -a /mnt/DATA/Software/Linux/cudnn-linux-x86_64-8.9.0.131_cuda11-archive.tar.xz ~
tar -xvf cudnn-linux-x86_64-8.9.0.131_cuda11-archive.tar.xz
cd cudnn-linux-x86_64-8.9.0.131_cuda11-archive
sudo cp include/cudnn* /usr/local/cuda-11.8/include
sudo cp lib/libcudnn* /usr/local/cuda-11.8/lib64/
sudo chmod a+r /usr/local/cuda-11.8/lib64/libcudnn*


// CUDA path 설정
sudo vim ~/.zshrc

export PATH="/usr/local/cuda-11.8/bin:$PATH"
export LD_LIBRARY_PATH="/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH"

source ~/.zshrc

nvcc --version
```















## RTX2060 Super - cuda 10.1
## RTX2060 Super - cuDNN 7.6.5

// Cuda 10.1
```zsh
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/10.1/Prod/local_installers/cuda-repo-ubuntu1804-10-1-local-10.1.243-418.87.00_1.0-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1804-10-1-local-10.1.243-418.87.00_1.0-1_amd64.deb
sudo apt-key add /var/cuda-repo-10-1-local-10.1.243-418.87.00/7fa2af80.pub
sudo apt-get update && sudo apt-get -y install cuda

// Cudnn 7.6.5

scp -P 9003 cudnn-10.1-linux-x64-v7.6.5.32.tgz jayden@analysispark.iptime.org:/home/jayden

sudo rsync -a /mnt/c/Users/dev.jihoonpark/Downloads/cudnn-10.1-linux-x64-v7.6.5.32.tgz ~

tar -zxvf cudnn-10.1-linux-x64-v7.6.5.32.tgz

cd ~/cuda 
sudo cp include/cudnn* /usr/local/cuda-11.8/include
sudo cp lib64/libcudnn* /usr/local/cuda-11.8/lib64/
sudo chmod a+r /usr/local/cuda-11.8/lib64/libcudnn*



// CUDA path 설정
sudo vim ~/.zshrc

export PATH="/usr/local/cuda-10.1/bin:$PATH"
export LD_LIBRARY_PATH="/usr/local/cuda-10.1/lib64:$LD_LIBRARY_PATH"

source ~/.zshrc

nvcc --version

```





## node, npm 설치

```zsh 
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash

// 터미널 재실행

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

nvm --version


nvm install 14


nvm ls

node --version

```








## lazyvim 설치
```zsh
sudo snap install nvim --classic
export PATH=$PATH:/snap/bin

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


// My nvim Lua Setting
cd ~/.config
sudo mv nvim{,.bak}
git clone https://github.com/analysispark/Neovim-setting.git
sudo mv Neovim-setting nvim


echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
sudo apt install python3-pip -y
pip3 install isort pylint black

```





## SSH
sudo apt update
sudo apt install openssh-server -y
sudo systemctl status ssh

sudo vi /etc/ssh/sshd_config

# port, X11, Password 접속 등 설정

sudo service ssh restart




## Cmake 빌드(20.04는 불필요)

```zhs
## 20.04

sudo apt install cmake-gui

```


//사전 설치된것 삭제: sudo apt purge cmake-qt-gui
//sudo apt-get install qtbase5-dev
//Cmake 사이트에서 리눅스용 다운로드


```zsh
sudo rsync -a /mnt/e/parkjihoon/Download/WSL_setup/cmake-3.30.8.tar.gz ~ && cd ~
sudo tar zxvf cmake-3.30.8.tar.gz
sudo chown -R jayden ~/cmake-3.30.8
cd cmake-3.30.8/

sudo apt-get update
sudo apt-get install libgflags-dev libgoogle-glog-dev libprotobuf-dev protobuf-compiler libboost-all-dev libhdf5-dev libatlas-base-dev libssl-dev

sudo ./configure --qt-gui

sudo chown -R $USER:$USER /home/jayden/cmake-3.30.8
sudo chmod -R u+rwX /home/jayden/cmake-3.30.8

sudo ./bootstrap && make -j`nproc` 
sudo make install -j`nproc`
```


```zsh

sudo apt install python3-pip -y
pip3 install scikit-build
sudo apt-get install libopencv-dev -y
```




// GUI
sudo apt install x11-apps
xeyes
//(눈동자가 나온다면 정상 출력)


## OpenPose 빌드  (4080은 use cuDNN 체크해제)

```zsh

cd ~ && git clone https://github.com/CMU-Perceptual-Computing-Lab/openpose

sudo -S mount -t drvfs Z: /mnt/z/
sudo cp -R /mnt/z/parkjihoon/Download/WSL_setup/models ~/openpose

cd ~/openpose/

// Caffe
sudo bash ./scripts/ubuntu/install_deps.sh


//build
mkdir build && cd build
cmake-gui ..


//cmake settings
configure
"Unix Makefiles"

Check 'BUILD_PYTHON'

configure
generate


//build
make -j`nproc`


//run
cd ..

./build/examples/openpose/openpose.bin --video examples/media/video.avi

```






## tensorflow 설치
```zsh
pip3 install --upgrade pip
pip3 install tensorflow-gpu==2.3.0

// RTX 4080
pip3 install --upgrade pip
pip3 install tensorflow==2.13.1

//python3

python3
import tensorflow as tf
tf.__version__

from tensorflow.python.client import device_lib
device_lib.list_local_devices()

```

 


## OpenCV build

sudo apt-get update
sudo apt-get install libgtk2.0-dev pkg-config
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install ffmpeg


sudo pip uninstall opencv-python --root-user-action=ignore

pip install pybind11


# OpenCV 소스 코드 다운로드
git clone https://github.com/opencv/opencv.git
cd opencv
git checkout 4.x

# 빌드 디렉토리 생성 및 이동
mkdir build && cd build

# CMake를 사용하여 빌드 설정
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local ..

# 빌드 및 설치
make -j8
sudo make install

## 바인딩 추가
vi ~/.zshrc
export PYTHONPATH=$PYTHONPATH:/usr/local/lib/python3.8/site-packages/cv2/python-3.8/



#### WSL Backup

```powershell

wsl -l -v

// RTX 2060
wsl --export Ubuntu-18.04 Z:\parkjihoon\Download\WSL_setup\Backup_image\Ubuntu-18.04-backup.tar

// RTX 4080
wsl --export Ubuntu-20.04 Z:\parkjihoon\Download\WSL_setup\Backup_image\Ubuntu-20.04-backup.tar
```


# 기존 배포판 제거 (필요한 경우)
```powershell
wsl --unregister Ubuntu-22.04
```

# 기본 경로에 배포판 가져오기

```powershell

wsl --import Ubuntu-20.04 "" Z:\parkjihoon\Download\WSL_setup\Backup_image\Ubuntu-20.04-backup.tar
```





