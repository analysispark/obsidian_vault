```python

import numpy as np
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Conv3D, MaxPooling3D, LSTM, Dense, Flatten, Reshape

# 데이터 전처리 함수
def preprocess_data(data):
    # 데이터를 적절한 형태로 전처리
    # 예: 정규화, 프레임 크기 조정 등
    # 반환된 데이터는 (샘플 수, 프레임 수, 높이, 너비, 채널)의 형태여야 함
    #ToDo 
    processed_data = data # 예시로 단순히 전처리 없이 데이터를 그대로 반환 
    return processed_data




## 데이터 준비 pass
frames = 10
height = 64
width = 64
channels = 3

def park_model():
    # CNN-LSTM 모델 구성
    model = Sequential()

    # CNN 부분
    model.add(Conv3D(32, kernel_size=(3, 3, 3), input_shape=(frames, height, width, channels), activation='relu'))
    model.add(MaxPooling3D(pool_size=(2, 2, 2)))

    model.add(Conv3D(64, kernel_size=(3, 3, 3), activation='relu'))
    model.add(MaxPooling3D(pool_size=(2, 2, 2)))

    model.add(Flatten())

    # Reshape to a 3D tensor before passing to LSTM
    #reshape_size = (frames, -1)
    #model.add(Reshape(reshape_size))
    model.add(Reshape((frames, -1)))

    # LSTM 부분
    model.add(LSTM(64, return_sequences=True))
    model.add(LSTM(64))

    # Fully connected layers
    model.add(Dense(64, activation='relu'))
    model.add(Dense(num_classes, activation='softmax')) # num_classes는 분류할 동작의 클래스 수

    # 모델 컴파일
    model.compile(optimizer='adam', loss = 'categorical_crossentropy', metrics=['accuracy'])

    # 모델 요약 확인
    model.summary()




```
