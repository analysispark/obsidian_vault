### NAS 드라이브 마운트
```zsh
sudo -S mount -t drvfs E: /mnt/e/
```


### Backup.sh
```sh
#!/bin/bash

SRC="/mnt/DATA/"  # 백업할 디렉토리
DEST="/mnt/webdav/DATA_backup/"  # 백업을 저장할 WebDAV의 경로

# rsync 옵션
# -a: 아카이브 모드 (모든 파일 속성 유지)
# -v: 과정 표시
# -z: 데이터 압축
# --delete: 대상에 존재하지 않는 파일 삭제
# --exclude: .으로 시작하는 파일 및 디렉토리 제외
# rsync -avz --delete --inplace --no-whole-file --partial --exclude=".*, _*" "$SRC" "$DEST"

# backup-server
rsync -avz --delete --inplace --no-whole-file --partial --exclude=".*, _*, lost+found" -e "ssh -p 9922" /mnt/DATA jayden@192.168.0.49:/mnt/DATA/
```

### SMB
```sh
sudo apt-get install samba smbfs

sudo smbpasswd -a [사용자이름]

sudo nvim /etc/samba/smb.conf
```

Samba 설정 파일 수정:
```conf
[공유폴더이름]
comment = 설명
path = /공유할/디렉토리/경로
read only = no
writable = yes
browsable = yes
guest ok = no

## Charles-Ubuntu Examplee
[sambashare]
comment = Samba on Ubuntu
path = /mnt/DATA
read only = no
browsable = yes
veto files = /lost+found/

```

Samba 서비스 재시작
```sh
sudo systemctl restart smbd
```

추가확인 사항
- 방화벽 설정: Samba 포트(기본적으로 445 및 139)가 열려 있는지 확인
```sh
sudo ufw allow samba
```
