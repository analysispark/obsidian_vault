
```bash
sudo apt-get install unzip
```

```bash
cd /usr/local/share/fonts
```

```bash
wget <https://github.com/naver/d2codingfont/releases/download/VER1.3.2/D2Coding-Ver1.3.2-20180524.zip>
unzip D2Coding-Ver1.3.2-20180524.zip
```

폰트 캐시 지우고 다시 생성

```bash
fc-cache -f -v
```

설치폴더 삭제(청소)

```bash
rm -rf D2Coding*
```
