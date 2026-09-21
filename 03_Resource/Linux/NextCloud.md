## Ubuntu 24.04-Server
Nextcloud 는 snap 으로 설치하되, 29버전으로 설치 (30.x 버전은 외부저장소 인증에서 비밀번호 오류 발생)

```zsh
sudo apt update && sudo apt upgrade -y
```
```zsh
sudo apt install net-tools
```
```zsh
sudo snap run nextcloud.mysql-client
```
#### MySQL 설정
```sql
CREATE USER ‘CloudAdmin’@‘localhost' IDENTIFIED BY ‘charles’;

GRANT ALL PRIVILEGES ON nextcloud.* TO ‘CloudAdmin’@‘localhost'; 

FLUSH PRIVILEGES;

EXIT;
```

#### /mnt/photo 폴더 연결
```zsh
sudo snap stop nextcloud

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
```

#### 신규사용자 추가
```zsh
sudo nextcloud.occ user:add <사용자이름>
```

#### 등록된 사용자 확인
```zsh
sudo nextcloud.occ user:list
```

#### 사용자 삭제
```zsh
sudo nextcloud.occ user:delete <사용자 ID>
```


```
sudo nextcloud.occ user:resetpassword <사용자 ID>
```