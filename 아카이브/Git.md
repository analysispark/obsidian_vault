# Git  
  
git init   #워킹폴더 지정  
  
git add .  #모두 추가  
  
git status   #업로드 상태 보기  
  
git commit -m “first commit”   #히스토리 만들기  
  
git remote add origin [https://github.com/analysispark/SA_BSP.git](https://github.com/analysispark/SA_BSP.git)  
  
git remote -v  
  
git push origin main     #or  
git push -f origin main  
  
  
  
  
## Github created branch   
  
git checkout -b “New_branch”  
  
git add .  
git commit -m “first commit”  
git push origin New_branch  
  
  
## Git repository disconnect  
  
git remote remove origin  
git remote -v  
  
## Git remote rm  
git checkout main  
rm -rf .git  
git status  
  
  
# 마스터브렌치에서 소스 가져오기(pull)  
  
git pull origin main  
  
  
## Obsidian Memo  
  
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
  
  
### 인자 생략하기  
```zsh  
git push -u origin main  
```  
- `-u` 옵션으로 명령어를 실행한 이후 부터는 `git push` 명령어만 날려도 됨  
  
### 코드 변경 이력 덮어쓰기  
```zsh  
git push -f origin main  
```  
  
