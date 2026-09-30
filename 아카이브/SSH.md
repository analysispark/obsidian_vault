# SSH  
  
sudo systemctl status ssh  
  
sudo ufw allow ssh  
sudo ufw reload  
  
sudo ufw status  
  
sudo vim /etc/ssh/sshd_config  
- Port 9922  
  
sudo systemctl restart ssh  
  
sudo ufw allow 9922  
sudo ufw reload  
  
  
## 원격 터미널 접속  
  
ssh -X -p 9003 charles[@analysispark.iptime.org](mailto:jayden@analysispark.iptime.org)  
  
  
## ssh 기존 호스트 삭제  
  
ssh-keygen -R [analysispark.iptime.org]:9003  
  
  
  
# SSHFS  
  
brew install macfuse  
brew install gromgit/fuse/sshfs-mac  
  
sshfs -o port=9002 charles@analysispark.iptime.org:/mnt/DATA ~/mnt/DATA  
  
  
