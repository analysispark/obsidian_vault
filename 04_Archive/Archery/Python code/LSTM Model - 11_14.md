```python
import pandas as pd
import numpy as np
import os
import json
from tensorflow import keras
from tensorflow.keras import layers
from tensorflow.keras.utils import to_categorical
from tensorflow.keras.preprocessing.sequence import pad_sequences

def filter_rows_by_column_value(df, column_name, value):
    filtered_series = df[df[column_name] == value]['label']
    return filtered_series

def get_filename(file_path):
    file_name, file_extension = os.path.splitext(os.path.basename(file_path))
    return file_name

def gen_y_train():
    # Training data file list
    file_list = os.listdir(os.path.join(os.getcwd(), "sample/x_train/"))
    file_list_json = [file for file in file_list if file.endswith('.json')]

    name_list = []

    # Preprocessing: Extract file names without extension
    for i in file_list_json:
        name_list.append(get_filename(i))

    # Read the example video list
    data = pd.read_csv(os.path.abspath(os.path.join(os.getcwd(), os.pardir, 'example_video_list.csv'))) 

    y_train_labels = []
    for i in name_list:
        try:
            # Filter rows based on sample name
            filtered_data = filter_rows_by_column_value(data, 'sample_name', i).to_frame()
            if not filtered_data.empty and 'label' in filtered_data.columns:
                y_train_labels.extend(filtered_data['label'].values)
            else:
                print(f"No 'label' column in data for {i}")
                # Add a placeholder label for cases where 'label' is missing
                y_train_labels.append(-1)
        except json.JSONDecodeError as e:
            print(f"Error decoding JSON in file {i}: {e}")
            continue

    # Convert labels to one-hot encoding if needed
    if y_train_labels:
        # Check for placeholder labels (-1) and remove them
        y_train_labels = [label - 1 for label in y_train_labels if label != -1]
        num_classes = len(set(y_train_labels))
        y_train_encoded = to_categorical(y_train_labels, num_classes=num_classes)
        return y_train_encoded
    else:
        return np.array([])


## Gen x_train
def extract_landmarks(frame):
    landmarks_data = frame.get("landmarks", [])
    x_values = [landmark.get("x", 0.0) for landmark in landmarks_data]
    y_values = [landmark.get("y", 0.0) for landmark in landmarks_data]
    return np.column_stack((x_values, y_values))

def pad_list_data(list_data, max_frame_length=1821, num_keypoints=33):
    # Ensure the list has the correct structure
    if not all(isinstance(frame, np.ndarray) and frame.shape == (num_keypoints, 2) for frame in list_data):
        raise ValueError("Invalid structure for the list data")

    # Pad or truncate frames to ensure there are exactly max_frame_length
    frames_padded = pad_sequences([list_data], maxlen=max_frame_length, padding='post', truncating='post', dtype='float32')[0]

    # Reshape the padded frames to the desired shape
    frames_padded = frames_padded.reshape((max_frame_length, num_keypoints * 2))  # Change this line

    return frames_padded

def gen_x_train():
    path_dir = os.path.join(os.getcwd(), 'sample/x_train/')
    file_list = os.listdir(path_dir)
    file_list_json = [file for file in file_list if file.endswith('.json')]

    frame_data = []

    for i in file_list_json:
        with open(os.path.join(path_dir, i), 'r', encoding='latin-1') as json_file:
            try:
                data = json.load(json_file)
            except json.JSONDecodeError as e:
                print(f"{i} 파일에서 JSON 디코딩 오류 발생: {e}")
                continue

        single_video_data = [extract_landmarks(frame) for frame in data]

        padded_data = pad_list_data(single_video_data)
        frame_data.append(padded_data)

    return frame_data

def park_LSTM():
    # LSTM model
    model = keras.Sequential([
        layers.LSTM(64, return_sequences=True, activation='relu', input_shape=(1821, 33 * 2)),
        layers.LSTM(128, return_sequences=True, activation='relu'),
        layers.LSTM(64, return_sequences=False, activation='relu'),  # Change this line
        layers.Dense(3, activation='softmax')
    ])

    # Compile the model
    model.compile(optimizer='adam',
                  loss='categorical_crossentropy',
                  metrics=['accuracy'])

    # Train the model
    model.fit(x_train, y_train, epochs=10, validation_split=0.1)

# Example usage:
x_train = np.array(gen_x_train())
y_train = gen_y_train()


print("x_train의 샘플 수:", len(x_train))
print("y_train의 샘플 수:", len(y_train))

# Check if y_train is not empty before training the model
if y_train.size > 0:
    # Train the LSTM model
    park_LSTM()
else:
    print("훈련 데이터가 없습니다. 데이터 로딩 프로세스를 확인하십시오.")

```
