# Backup.sh  
  
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
rsync -avz --delete --inplace --no-whole-file --partial \  
  --exclude=".*" \  
  --exclude="_*" \  
  --exclude="lost+found" \  
  --exclude="*.tmp" \  
  --exclude="*.temp" \  
  --exclude="*.swp" \  
  --exclude="*.swo" \  
  --exclude="*.bak" \  
  --exclude="*.~*" \  
  --exclude="*.zone.identifier" \  
  --exclude="*~" \  
  --exclude="Thumbs.db" \  
  --exclude=".DS_Store" \  
  -e "ssh -p 9922" /mnt/DATA/ jayden@192.168.0.49:/mnt/DATA/  
