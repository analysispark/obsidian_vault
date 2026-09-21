```python
import time
from tensorflow import keras
from tensorflow.keras import layers
from kerastuner.tuners import RandomSearch

def build_model(hp):
    model = keras.Sequential()

    model.add(layers.LSTM(units=hp.Int('units_1', min_value=128, max_value=512, step=32),
                          return_sequences=True,
                          activation='relu',
                          input_shape=(1821, 33 * 2)))
    model.add(layers.Dropout(hp.Float('dropout_1', min_value=0.2, max_value=0.5, step=0.1)))

    model.add(layers.LSTM(units=hp.Int('units_2', min_value=64, max_value=256, step=32),
                          return_sequences=True,
                          activation='relu'))
    model.add(layers.Dropout(hp.Float('dropout_2', min_value=0.2, max_value=0.5, step=0.1)))

    model.add(layers.LSTM(units=hp.Int('units_3', min_value=32, max_value=128, step=32),
                          return_sequences=True,
                          activation='relu'))
    model.add(layers.LSTM(units=hp.Int('units_4', min_value=16, max_value=64, step=16),
                          return_sequences=False,
                          activation='relu'))

    model.add(layers.Dense(3, activation='softmax'))

    model.compile(optimizer='adam',
                  loss='categorical_crossentropy',
                  metrics=['accuracy'])

    return model

tuner = RandomSearch(
    build_model,
    objective='val_accuracy',
    max_trials=5,  # 튜닝할 모델 수
    executions_per_trial=1,  # 각 모델을 실행하는 데 필요한 에포크 수
    directory='my_tuner_dir',  # 튜너에 대한 저장 경로
    project_name='Archery_Tuner'  # 튜너에 대한 프로젝트 이름
)

# 튜닝된 모델 학습
tuner.search(x_train, y_train, epochs=10, validation_split=0.1)

# 가장 좋은 하이퍼파라미터 출력
best_hps = tuner.get_best_hyperparameters(num_trials=1)[0]
print(f"Best Hyperparameters: {best_hps}")

# 가장 좋은 모델 빌드 및 학습
best_model = tuner.hypermodel.build(best_hps)
history = best_model.fit(x_train, y_train, epochs=10, validation_split=0.1)

# 학습된 모델 저장
best_model.save('Archery_model_tuned.h5')


```
