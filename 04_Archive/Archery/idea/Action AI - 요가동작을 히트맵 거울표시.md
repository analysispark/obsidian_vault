![actionai_example.gif](https://blog.kakaocdn.net/dn/czh7kk/btqZ6klDxeZ/NaFvMB6pVveH2JGXgvdPoK/img.gif)

ActionAI는 YogAI(Smart Personal Trainer)를 위한 딥러닝 학습 기반 Python Library 이다. 요가 동작을 히트맵으로 거울에 표시하였다. 맨 아래 참고자료에 링크를 넣어두었는데, 요가 동작 인식하는 방법을 굉장히 자세하게 써놓았다. 나중에 시간될 때 따라서 만들어보면 좋을 것 같다. 

아래와 같이 유투브 크롤링을 이용하여 비디오 데이터를 수집하고, 이미지를 추출하였다고 한다. (정제는 수동으로 한듯)
/Users/parkjihoon/miniforge3/envs/archery/bin/python
```python
#!/usr/bin/env python
import os
import sys
import requests
from bs4 import BeautifulSoup as bs
from urllib.parse import urlencode
from pytube import YouTube

qstring = sys.argv[1]
out_dir = sys.argv[2]
base = "https://www.youtube.com/results?"
s = {"search_query": qstring}
s = urlencode(s)

r = requests.get(base + s)
page = r.text
soup=bs(page,'html.parser')

vids = soup.findAll('a',attrs={'class':'yt-uix-tile-link'})
videolist=[]
for v in vids:
	tmp = 'https://www.youtube.com' + v['href']   
	videolist.append(tmp)
count=0
os.chdir(out_dir)
for item in videolist:
	# increment counter:
	count+=1
	try:
		yt = YouTube(item)
		# grab the video:
		yt.streams.filter(progressive=True, file_extension='mp4').order_by('resolution').desc().first().download()    
	except: 
	    pass
```

그 다음 이미지 분류기를 학습시켜 행동 분류 모델을 만들었다고 한다.

![similar_poses_eEFCnqtzoi.jpg](https://blog.kakaocdn.net/dn/c1YyN2/btqZ7KEowwx/3G2r8vNYFLMUxDdwnUF6Nk/img.jpg)

이 때 Augmentation은 x 및 y 방향으로 이동시키고, 수직 축으로 flip 시키고, 포즈 특징 벡터에 약간의 rotation 을 가미하여 데이터 세트를 증강했다고 한다. 이런 방법을 이용하여 과적합을 방지하였고, 아래와 같이 기하학적 변환을 했다고 한다. 이를 이용해 데이터 세트가 약 35,000개로 증가하였고, 간단한 트리 기반 분류기로 정확도 57% 에서 85%로 증가했다고 한다.

자세추정 모델을 쌩으로 돌리면 FPS는 굉장히 느린편이라서 tflite 를 사용했다. 자세 추정 모델은 CPM(Convolutional Pose Machines)을 사용하였다. 

```python
y_coords = np.flip(x_coords, axis=0)   # get y coordinates only
def rand_shift(vec, v):
	return vec + v * np.random.randint(-10, 10)
def vert_swap(vec, N=96):
	def col_swap(col):       
		return (N - col) % 96   
	return (vec * x_coords) + apply_vec(col_swap, (vec * y_coords))
@np.vectorize
def apply_vec(f,x):
	return f(x)
out_lst = []
for row in range(df.shape[0]):
	x, y= df.iloc[row, :-1], df.iloc[row,-1]
	for idx in range(5):       
		v = random.choice([x_coords, y_coords]) #randomly choose an index to shift 
		vec = rand_shift(x, v)
		out_lst.append(list(vec) + [y])
	#Vertical flip
	v_vec = vert_swap(x)
	out_lst.append(list(v_vec) + [y])
out_df = pd.DataFrame(out_lst)
out_df.to_csv('./data/yoga/augmented_poses.csv')
```

LSTM을 사용하여 연속적인 동작을 인식하기도 한다.

```python
#!/usr/bin/env python3
import argparse
import numpy as np
import pandas as pd
from multiprocessing import cpu_count
from sklearn.model_selection import train_test_split
 
#import keras
 
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import LSTM, Dense, Dropout
from tensorflow.keras.optimizers import RMSprop
 
# Initializing variables
window = 3 # depends on time window
epochs = 50
batch_size = 16
pose_vec_dim = 36 # depends on pose estimation model used
cores = cpu_count()
 
class_names = ['list', 'of', 'actiions', 'here']
num_class = len(class_names)
lbl_dict = {class_name:idx for idx, class_name in enumerate(class_names)}
 
 
def load_data():
    dataset = pd.read_csv('data/data.csv',  index_col=None)
 
    y = dataset.pop('y')
    X = dataset.values
 
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
 
    y_train = tf.keras.utils.to_categorical(list(map(lbl_dict.get, y_train)), num_class)
    y_test = tf.keras.utils.to_categorical(list(map(lbl_dict.get, y_test)), num_class)
 
    X_test = X_test.reshape(X_test.shape[0], pose_vec_dim, window)
    X_train = X_train.reshape(X_train.shape[0], pose_vec_dim, window)
    return X_train, X_test, y_train, y_test
 
 
def lstm_model():
    model = Sequential()
    model.add(LSTM(32, dropout=0.2, recurrent_dropout=0.2, input_shape=(pose_vec_dim, window)))
    model.add(Dense(32, activation='relu'))
    model.add(Dropout(0.2))
    model.add(Dense(len(class_names), activation='softmax'))
    print(model.summary())
    return model
 
 
if __name__ == '__main__':
    parser = argparse.ArgumentParser(description='Training for LegDay application')
    #parser.add_argument('--data', type=str, default='./data/legday/squats_deadlifts_stand5.csv')
    parser.add_argument('--out_file', type=str, default='./models/lstm.h5')
    args = parser.parse_args()
 
    #model = lstm_model()
    model = tf.keras.models.load_model('./models/lstm.h5')
    model.compile(loss='categorical_crossentropy',
                  optimizer=RMSprop(),
                  metrics=['accuracy'])
 
    X_train, X_test, y_train, y_test = load_data()
 
    history = model.fit(X_train, y_train,
                        batch_size=batch_size,
                        epochs=epochs,
                        verbose=1,
                        validation_data=(X_test, y_test))
 
    score = model.evaluate(X_test, y_test, verbose=0)
    print('Test loss:', score[0])
    print('Test accuracy:', score[1])
 
 
    model.save(args.out_file)
    print("Saved model to disk")
```

![yogai_squat_or_not.gif](https://blog.kakaocdn.net/dn/bbL5YL/btqZ4z4q8kn/S72KllZ3mnbu7XGRKOrDnk/img.gif)

참고자료 1 : [www.hackster.io/yogai/yogai-smart-personal-trainer-f53744](https://www.hackster.io/yogai/yogai-smart-personal-trainer-f53744)

 [YogAI: Smart Personal Trainer

Pose estimation on a Raspberry Pi to guide and correct positions for any yogi. By Salma Mayorquin and Terry Rodriguez.

www.odroid.hackster.io](https://www.hackster.io/yogai/yogai-smart-personal-trainer-f53744)

참고자료 2 : [github.com/smellslikeml/ActionAI](https://github.com/smellslikeml/ActionAI)

 [smellslikeml/ActionAI

custom human activity recognition modules by pose estimation and cascaded inference using sklearn API - smellslikeml/ActionAI

github.com](https://github.com/smellslikeml/ActionAI)

728x90