# Python cache 캐시삭제  
  
// 터미널  
find . -type d -name "__pycache__" -exec rm -r {} +  
find . -type f -name "*.pyc" -delete  
  
  
find /mnt -type d -name "__pycache__" -exec rm -r {} +  
find /mnt -type f -name "*.pyc" -delete  
  
  
