# Windows Settings(Powershell)  
  
  
# PowerShell 7 설치  
  
winget source reset --force  
winget install Microsoft.PowerShell --source winget  
  
#PowerShell 관리자 권한 실행  
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser  
  
# Git 설치  
winget install --id Git.Git -e  
  
# 에러발생시 msstore 초기화  
winget source remove msstore  
winget source update  
  
git config --global user.name "Park Jihoon"  
git config --global user.email "analysispark@gmail.com"  
  
git -v  
  
#PSReadLine 설치 (자동완성 + 하이라이팅)  
Install-Module PSReadLine -Force  
  
#Oh-My-Posh 설치  
winget install JanDeDobbeleer.OhMyPosh  
  
#Nerd Font 설치  
oh-my-posh font install  
  
Windows Terminal → Settings → PowerShell → Font → 원하는 Nerd Font 선택  
  
  
  
# 자동 디렉토리 이동(fasd/zoxide 같은 기능)  
  
winget install ajeetdsouza.zoxide  
  
  
  
#fzf 설치(강력한 fuzzy finder)  
winget install fzf  
  
# git repository 설정  
git clone [https://github.com/analysispark/dotfiles.git](https://github.com/analysispark/dotfiles.git)  
  
cd dotfiles  
ii .  
  
  
  
  
--- 참고용  
notepad $PROFILE  
  
<<<<<아래 내용을 추가>>>>>  
Import-Module PSReadLine  
  
# Syntax Highlighting  
Set-PSReadLineOption -Colors @{  
    "Command" = "Green"  
    "Parameter" = "Yellow"  
    "String" = "Cyan"  
}  
  
# Autosuggestion (zsh-style)  
Set-PSReadLineOption -PredictionSource History  
Set-PSReadLineOption -PredictionViewStyle ListView  
  
# History 寃??(Ctrl + R 濡?fzf 媛숈? 諛⑹떇)  
Set-PSReadLineKeyHandler -Key Ctrl+r -Function ReverseSearchHistory  
  
# Oh-my-Posh  
oh-my-posh init pwsh --config "C:\Users\Charles\dotfiles\WindowsPowerShell\m365princess.omp.json" | Invoke-Expression  
---  
