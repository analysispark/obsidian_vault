#### 주제: #Deeplearning #Archery #Python #Keras #LSTM

```python
## Keras-LSTM
## Jihoon, Park
## Enviroment: Park38
## 2023-10-07

import cv2
import numpy as np
import tensorflow as tf
from tensorflow.keras import layers, models

# 비디오 파일 경로 리스트 설정
video_paths = ['video1.mp4', 'video2.mp4', 'video3.mp4']

# 비디오에서 프레임 추출 함수
def extract_frames(video_path):
    cap = cv2.VideoCapture(video_path)
    frames = []

    while True:
        ret, frame = cap.read()
        if not ret:
            break
        frames.append(frame)

    cap.release()
    return frames

# 프레임 간의 유사성을 측정하는 모델 정의
def create_similarity_model():
    model = models.Sequential()
    model.add(layers.Conv2D(32, (3, 3), activation='relu', input_shape=(frame_height, frame_width, 3)))
    model.add(layers.MaxPooling2D((2, 2)))
    model.add(layers.Flatten())
    model.add(layers.Dense(64, activation='relu'))
    model.add(layers.Dense(1, activation='sigmoid'))  # 0과 1 사이의 유사성 점수 출력

    model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy'])
    return model

# 프레임 간의 유사성을 평가하는 함수
def evaluate_similarity(frames1, frames2, model):
    similarity_scores = []
    for i in range(len(frames1)):
        frame1 = frames1[i]
        frame2 = frames2[i]

        # 프레임 크기를 모델 입력 크기에 맞게 조정
        frame1 = cv2.resize(frame1, (frame_width, frame_height))
        frame2 = cv2.resize(frame2, (frame_width, frame_height))

        # 이미지를 넘파이 배열로 변환하고 정규화
        frame1 = np.array(frame1) / 255.0
        frame2 = np.array(frame2) / 255.0

        # 모델에 입력을 전달하여 유사성 점수 계산
        similarity_score = model.predict(np.array([frame1, frame2]))[0][0]
        similarity_scores.append(similarity_score)

    return similarity_scores

# 비디오 프레임 추출
all_frames = []
frame_height, frame_width = None, None

for video_path in video_paths:
    frames = extract_frames(video_path)
    if frame_height is None:
        frame_height, frame_width, _ = frames[0].shape
    all_frames.append(frames)

# 유사성 모델 생성 및 훈련
model = create_similarity_model()

# 예제 데이터와 레이블을 사용하여 모델을 훈련합니다.
# 이 예제에서는 비디오 데이터에서 움직임을 감지하는 목적이므로 적절한 데이터 및 레이블을 사용해야 합니다.

# 여러 동영상 간의 유사성 평가
for i in range(len(video_paths) - 1):
    video1_frames = all_frames[i]
    video2_frames = all_frames[i + 1]
    similarity_scores = evaluate_similarity(video1_frames, video2_frames, model)
    
    print(f'동영상 {i+1}과 동영상 {i+2}의 평균 유사성 점수: {np.mean(similarity_scores)}')
```
