# Nextcloud  
  
  
—— # 서버를 이용한 설치 ver.29  
  
sudo apt update && sudo apt upgrade -y  
  
sudo apt install net-tools  
  
  
sudo snap run nextcloud.mysql-client  
  
CREATE USER ‘CloudAdmin’@‘localhost' IDENTIFIED BY ‘charles’;  
GRANT ALL PRIVILEGES ON nextcloud.* TO ‘CloudAdmin’@‘localhost';   
FLUSH PRIVILEGES;  
EXIT;  
  
  
  
sudo snap stop nextcloud  
#   
~~‘# 기본 폴더를 /mnt/DATA 로 이동~~  
~~sudo rsync -avz /var/snap/nextcloud/common/nextcloud/data/ /mnt/DATA/~~  
~~sudo mv /var/snap/nextcloud/common/nextcloud/data /var/snap/nextcloud/common/nextcloud/data_backup~~  
~~sudo ln -s /mnt/DATA /var/snap/nextcloud/common/nextcloud/data~~  
  
  
## /mnt/photo 폴더를 연결  
  
sudo chown -R root:root /mnt/photo  
sudo chmod -R 750 /mnt/photo  
  
sudo snap start nextcloud  
  
sudo snap connect nextcloud:removable-media  
  
sudo snap set nextcloud ports.http=9980  
  
sudo snap restart nextcloud  
  
sudo snap set nextcloud php.memory-limit=1024M  
sudo snap set nextcloud php.opcache-enable=1  
sudo snap set nextcloud php.opcache-memory-consumption=128  
sudo snap set nextcloud php.opcache-interned-strings-buffer=16  
sudo snap set nextcloud php.opcache-max-accelerated-files=10000  
sudo snap set nextcloud php.opcache-revalidate-freq=1  
sudo snap set nextcloud php.opcache-save-comments=1  
sudo snap set nextcloud php.upload-max-filesize=100G  
sudo snap set nextcloud php.post-max-size=100G  
  
sudo snap restart nextcloud.php-fpm  
  
sudo nextcloud.occ files:scan --all  
  
  
  
# 신규사용자 추가  
  
sudo nextcloud.occ user:add <사용자 ID>  
  
  
# 등록된 사용자 확인  
  
sudo nextcloud.occ user:list  
  
  
## 사용자 삭제  
  
sudo nextcloud.occ user:delete <사용자 ID>  
  
  
# 사용자 비밀번호 초기화  
  
sudo nextcloud.occ user:resetpassword <사용자 ID>  
