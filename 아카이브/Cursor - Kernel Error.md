# Cursor - Kernel Error  
  
The file '.local/lib/python3.10/site-packages/typing_extensions.py'   
seems to be overriding built in modules and interfering with the startup of the kernel.  
  
에러 발생 시,  
  
원인  
* Conda 환경이 Cursor에서 완전히 활성화되지 않아서 Python이 잘못된 경로를 참조하고 있습니다.  
* 이 때문에 ~/.local/lib/python3.10/site-packages 안에 있는 typing_extensions.py가 표준 라이브러리 모듈을 덮어쓰는 문제가 발생합니다.  
* 해당 파일이 현재 Conda 환경의 Python 버전과 맞지 않거나, 오래된 버전일 수 있습니다.  
  
해결 방법  
1. Conda 환경 재활성화  
    * Cursor에서 Conda 환경이 제대로 잡히도록 설정해야 합니다.  
    * 터미널에서:  
  
*conda activate park_sci*  
  
    * Cursor 설정에서 Python Interpreter를 해당 Conda 환경의 Python 경로로 지정하세요.  
  
2. 문제 파일 제거 또는 이름 변경  
    * 경고에서 안내한 대로 ~/.local/lib/python3.10/site-packages/typing_extensions.py를 삭제하거나 이름을 변경합니다.  
  
  
*mv ~/.local/lib/python3.10/site-packages/typing_extensions.py \*  
*   ~/.local/lib/python3.10/site-packages/typing_extensions.py.bak*  
  
3. Conda 환경 내 typing_extensions 재설치  
    * Conda 환경이 활성화된 상태에서 다음 명령 실행:  
  
*conda install typing_extensions*  
