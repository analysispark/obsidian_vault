---
aliases:
  - lecture
tags:
  - 수업자료
  - Python
  - 표준편차
---
python lecture.py

```python
import math

while True:
    numbers = []

    for i in range(5):
        while True:
            try:
                value = float(input(f"{i+1}번째 숫자 입력: "))
                numbers.append(value)
                break
            except ValueError:
                print("잘못된 숫자입니다")

    # 평균
    mean = sum(numbers) / len(numbers)

    # 모표준편차 (population std)
    variance_population = sum((x - mean) ** 2 for x in numbers) / len(numbers)
    std_population = math.sqrt(variance_population)

    # 표본표준편차 (sample std)
    variance_sample = sum((x - mean) ** 2 for x in numbers) / (len(numbers) - 1)
    std_sample = math.sqrt(variance_sample)

    print("\n=== 결과 ===")
    print(f"평균: {mean}")
    print(f"모표준편차: {std_population}")
    print(f"표본표준편차: {std_sample}\n")
```
