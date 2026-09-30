---
aliases:
  - qr
tags:
  - Python
  - QRcode
---
python QRcode.py 'URL'

```python
import sys
import os
import re
import qrcode

def main():
    # 1. 인자값(URL) 확인
    if len(sys.argv) < 2:
        print("사용법: python QRcode.py [URL]")
        print("예시: python QRcode.py https://www.google.com/")
        sys.exit(1)
        
    url = sys.argv[1]

    # 2. 저장할 바탕화면(Desktop) 경로 설정
    desktop_path = os.path.expanduser("~/Desktop")
    
    # 3. URL을 바탕으로 안전한 파일 이름 생성
    # 예: https://www.google.com/ -> google_com.png
    clean_name = re.sub(r'https?://(www\.)?', '', url)  # 프로토콜 및 www 제거
    clean_name = re.sub(r'[^a-zA-Z0-9]', '_', clean_name)  # 특수문자를 언더바로 변경
    clean_name = clean_name.strip('_')  # 앞뒤 언더바 제거
    
    if not clean_name:
        clean_name = "qrcode"
        
    file_name = f"{clean_name}.png"
    full_path = os.path.join(desktop_path, file_name)

    # 4. QR 코드 생성 및 저장
    try:
        print(f"QR 코드를 생성 중입니다: {url}")
        img = qrcode.make(url)
        img.save(full_path)
        print(f"성공! 파일이 다음 경로에 저장되었습니다:\n{full_path}")
    except Exception as e:
        print(f"오류가 발생했습니다: {e}")

if __name__ == "__main__":
    main()
```
