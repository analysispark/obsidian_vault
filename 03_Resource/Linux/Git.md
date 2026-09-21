---
id: Git
aliases: []
tags: []
---

### 사용자, 이메일 등록

```zsh
git config --global user.name "analysispark"
git config --global user.email "analysispark@gmail.com"
```

### git 등록
```git
git init
```

### git 원격저장소와 연결
```git
git remote add origin [원격 저장소 URL]

# example: git remote add origin https://github.com/analysispark/corne-zmk.git
```

### add & commit
```git
git add .
git commit -m "initial commit"
```


### 아이디, 패스워드 캐싱
```zsh
git config credential.helper store
```

### Token


### 인자 생략하기
```zsh
git push -u origin main
```
- `-u` 옵션으로 명령어를 실행한 이후 부터는 `git push` 명령어만 날려도 됨

### 코드 변경 이력 덮어쓰기
```zsh
git push -f origin main
```

