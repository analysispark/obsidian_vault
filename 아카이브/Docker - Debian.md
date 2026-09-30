# Docker - Debian  
  
//0. Docker 및 Debian 기본 세팅  
  
sudo apt update && sudo apt upgrade -y  
sudo apt install git curl wget gnupg zsh tmux unzip openssh-server sshfs build-essential -y  
  
sudo systemctl status ssh  
sudo vi /etc/ssh/sshd_config  
  
  
// Oh-my-zsh  
sh -c "$(wget -O- [https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"](https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)  
  
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting  
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions  
  
  
// zshrc  
plugins=(git  
        zsh-syntax-highlighting  
        zsh-autosuggestions  
)  
  
git clone https://github.com/dylanaraps/neofetch.git  
cd neofetch  
sudo make install  
  
  
// 1. NVIDIA GPG 키 등록  
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | \  
  sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg  
  
// 2. 저장소 리스트 생성 (서명 키 포함)  
curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \  
  sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \  
  sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list  
  
// 3. 패키지 목록 업데이트  
sudo apt-get update  
  
// 4. 툴킷 설치  
sudo apt-get install -y nvidia-container-toolkit  
  
sudo nvidia-ctk runtime configure --runtime=docker  
// sudo systemctl restart docker #wsl2  
  
Restart-Service -Name com.docker.service      #powershell  
  
sudo usermod -aG docker $USER  
  
// wsl --shutdown  
  
1. 컨테이너 다운받기  
  
docker pull analysispark/tf-gpu-custom:latest  
	  
2. 컨테이너 실행하기  
  
docker run -d --restart=always --privileged \  
  --gpus all \  
  --ipc=host \  
  --ulimit memlock=-1 \  
  --ulimit stack=67108864 \  
  -v /mnt/DATA:/workspace \  
  -p 2222:22 \  
  --name tf_container \  
  analysispark/tf-gpu-custom:latest \  
  /usr/sbin/sshd -D  
  
docker exec -it tf_container bash  
  
echo 'root:charles#' | chpasswd  
  
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config  
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config  
  
service ssh start  
  
3. 컨테이너 중지하기  
  
docker stop <컨테이너이름 또는 ID>  
  
// 모든 실행 중인 컨테이너 한 번에 중지  
docker stop $(docker ps -q)  
  
4. 이미지로 저장하기  
  
docker commit <컨테이너이름 또는 ID> analysispark/tf-gpu-custom:latest  
docker tag 기존이미지명 사용자명/레포지토리명:태그  
docker login  
docker push jihoon/my-image:latest  
  
// 이미 pull 한 이미지라면 tag 만 하면 됨  
docker tag analysispark/tf-gpu-custom analysispark/tf-gpu-custom:latest  
  
docker login  
docker push analysispark/tf-gpu-custom:latest  
  
  
  
  
  
  
// DATA 자동 마운트 및 컨테이너 자동실행  
  
mkdir -p /mnt/DATA  
sudo chown charles:charles /mnt/DATA  
ssh-keygen -t rsa -b 4096 -C "alaysispark@gmail.com"  
ssh-copy-id -p 8888 [charles@yswrc.iptime.org](mailto:charles@yswrc.iptime.org)  
  
// Server의 /etc/ssh/sshd_confg 확인  
  
sudo nvim /etc/ssh/sshd_config  
  
//// PubkeyAuthentication yes  
//// AuthorizedKeysFile .ssh/authorized_keys  
  
sudo systemctl restart ssh  
  
  
# SSHFS 자동 마운트 (포트 8888 사용)  
MOUNT_POINT="/mnt/DATA"  
REMOTE="charles@yswrc.iptime.org:/mnt/DATA"  
PORT=8888  
  
if ! mountpoint -q "$MOUNT_POINT"; then  
  echo "🔗 마운트되지 않음: $MOUNT_POINT → 마운트 시도 중..."  
  sshfs -p $PORT "$REMOTE" "$MOUNT_POINT"  
else  
  echo "✅ 이미 마운트됨: $MOUNT_POINT"  
fi  
  
  
neofetch  
  
  
# Docker 컨테이너 자동 실행  
ZSH_DISABLE_COMPFIX=true  
autoload -Uz compinit  
compinit -i  
  
docker start tf_container 2>/dev/null  
