#### 2024-01-22 16:22
```python
# /project/module/test_preprocess.py

import json
import os

import numpy as np
import pandas as pd
from sklearn.preprocessing import MinMaxScaler

def preprocess_json(input_json, max_frame_length=1821, num_keypoints=11):
    output_json = []

    with open(input_json, "r") as file:
        json_data = json.load(file)
        keypoints_data = json_data["annotation"]["2d_keypoints"]
        score = json_data["info"]["score"]

    for frame in keypoints_data:
        new_frame = []
        for keypoints in frame:
            # 처음 11개 포인트만 추출
            new_keypoints = [[point[0], point[1]] for point in keypoints[:num_keypoints]]
            new_frame.append(new_keypoints)
        
        flattened_data = np.array(
            [item[1::-1] for sublist in new_frame for item in sublist]
            ).reshape(max_frame_length, num_keypoints * 2)
        scaler = MinMaxScaler()
        scaled_data = scaler.fit_transform(flattened_data)

        output_json.append(scaled_data)  # 수정된 부분

    # 입력 데이터에 따라 빈 프레임 추가
    while len(output_json) < max_frame_length:
        output_json.append([[[0.0, 0.0] for _ in range(num_keypoints)]])

    return output_json, score

## 주어진 JSON 데이터
#json_data = data  # data는 5개의 프레임으로 구성된 데이터
#
## 전처리 함수 호출
#preprocessed_data = preprocess_json(json_data)
#
## 결과 출력
#print(json.dumps(preprocessed_data, indent=4))
```

#### 2024-01-22 17:30

```python
# /project/module/test_preprocess.py

import json
import os

import numpy as np
import pandas as pd
from sklearn.preprocessing import MinMaxScaler

def preprocess_json(input_json, max_frame_length=1821, num_keypoints=11):
    output_json = []

    with open(input_json, "r") as file:
        json_data = json.load(file)
        keypoints_data = json_data["annotation"]["2d_keypoints"]
        score = json_data["info"]["score"]

    for frame in keypoints_data:
        new_frame = []
        for keypoints in frame:
            # 처음 11개 포인트만 추출
            new_keypoints = [[point[0], point[1]] for point in keypoints[:num_keypoints]]
            new_frame.extend(new_keypoints)
        
        flattened_data = np.array(new_frame).reshape(-1)
        output_json.append(flattened_data)

    # 정규화를 수행하기 전에 데이터를 (None, 1821, 22) 형태로 변환
    output_json = np.array(output_json).reshape(-1, max_frame_length, num_keypoints * 2)

    # MinMaxScaler를 사용하여 데이터를 정규화합니다.
    scaler = MinMaxScaler()
    output_json_reshaped = output_json.reshape(-1, num_keypoints * 2)
    output_json_normalized = scaler.fit_transform(output_json_reshaped)
    output_json_normalized = output_json_normalized.reshape(-1, max_frame_length, num_keypoints * 2)

    return output_json_normalized, score
```


- 정규화가 되고 있지 않음 (Jupyter_test.ipynb) 참조 - 2024-01-22 17:33
- 