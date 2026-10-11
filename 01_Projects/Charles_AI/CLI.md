
## 예시

```zsh
./run.sh \
  "독립표본 Student t-test와 Welch t-test의 차이, 가정, Python 예제 코드를 포함한 짧은 Markdown 문서를 작성하라" \
  --output outputs/custom/t_test_guide.md \
  --debug-dir outputs/debug_t_test
```

## 직접입력

```zsh
./run.sh "사용자 목표"
```

## 목표 파일 입력

```zsh
./run.sh --goal-file prompts/task.md
```

## 출력 위치 지정

```zsh
./run.sh \

  "사용자 목표" \

  --output outputs/custom/result.md
```

## 실행별 디버그 폴더 분리

```zsh
./run.sh \

  "사용자 목표" \

  --output outputs/custom/result.md \

  --debug-dir outputs/debug_custom
```

## 옵션 확인

```zsh
./run.sh --help
```

