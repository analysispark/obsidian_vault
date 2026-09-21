#### 주제: #Mac #Homebrew #Setting

## Install

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## Basic Command

#### \- Homebrew를 이용한 어플리케이션 설치,삭제 등

`brew update` : brew를 최신버전으로 업데이트

`brew search <패키지명>` : 프로그램이 있는지 검색

`brew install <패키지명>[@버전]` : 프로그램 설치(최신버전으로)  
ex) `brew install mysql`, `brew install mysql@5.5`

#### \- 확인

`brew list` : 깔려있는 패키지 확인  
`brew info <패키지명>` : 패키지 정보보기

#### \-업데이트

`brew outdated` : 업그레이드 필요한 프로그램 찾기

`brew upgrade <패키지명>`: 패키지 업그레이드

`brew upgrade` : 모드 패키지 업그레이드

#### \-삭제

`brew cleanup <패키지명>` : 버전을 여러개 깔았는데 최신버전 이외의 버전들 전부 삭제  
`brew uninstall <패키지명>` : 특정 패키지 삭제

`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"`  
: Homebrew 삭제하기

