| 단축키                                 | 의미                   |
| ----------------------------------- | -------------------- |
| <Ctrl> + b                          | tmux 명령 접두어          |
| tmux                                | tmux 세션 시작           |
| tmux -new -s session_name           | 세션명 지정하여 tmux 세션 시작  |
| <Ctrl> + b + $                      | tmux 현재 세션 이름 변경     |
| tmux ls                             | 실행중인 tmux 세션 확인      |
| tmux attach-session -t session_name | 세션에 연결               |
| <Ctrl> + b + d                      | Tmux 세션 detached(분리) |
| <Ctrl> + b + (                      | 이전(num) 세션으로 전환      |
| <Ctrl> + b + L                      | 이전 세션으로 전환           |
| <Ctrl> + b + )                      | 다음(num) 세션으로 전환      |
| <Ctrl> + b + c                      | 새 창 만들기(쉘 사용)        |
| <Ctrl> + b + n                      | 다음 창으로 이동            |
| <Ctrl> + b + ,                      | 창 이름 변경              |
| <Ctrl> + b + p                      | 이전 창으로 이동            |
| <Ctrl> + b + &                      | 현재 창 종료              |
| <Ctrl> + b + w                      | 목록에서 창 선택            |
| <Ctrl> + b + l                      | 이전 창으로 전환(되돌리기)      |
| <Ctrl> + ‘                          | 선택할 창 인덱스 묻기(숫자입력)   |
| <Ctrl> + b + <num>                  | 숫자로 창 전환             |
| <Ctrl> + b + %                      | 현재 창 좌우로 나누기(탭)      |
| <Ctrl> + b + o                      | 다음 나뉜 탭으로 이동         |
| <Ctrl> + b + “                      | 현재 창 상하로 나누기(탭)      |
| <Ctrl> + b + ;                      | 이전에 있었던 탭으로 이동(되돌리기) |
| <Ctrl> + b + !                      | 현재 나뉜 탭 새 창으로 분리     |
| <Ctrl> + b + m                      | 현재 탭 확인              |
| <Ctrl> + b + z                      | 현재 탭 크게보기 ↔ 원래대로     |
| <Ctrl> + b + M                      | 탭 확인 취소              |
| <Ctrl> + b + x                      | 현재 탭 닫기              |
| <Ctrl> + b + :                      | tmux command line 입력 |
| <Ctrl> + b + t                      | 시계 보기                |
| <Ctrl> + b + [                      | copy mode            |
| <Ctrl> + b + q                      | 탭 인덱스 보기             |
| <Ctrl> + b + i                      | 창 정보 보기              |
| Page up / down                      | copy mode 전환 후 스크롤   |
| <Ctrl> + b + f                      | 열린 창에서 텍스트 검색하기      |
| <Ctrl> + b + 방향키                    | 창이동                  |
| <Ctrl> + b + ?                      | 바인딩 리스트 확인           |
