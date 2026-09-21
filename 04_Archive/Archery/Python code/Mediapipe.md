---
tags:
  - Archery
  - Deeplearning
  - Python
  - Mediapipe
---
``` python
import cv2
import mediapipe as mp
import csv
import os

# Define Functions

def write_landmarks_to_csv(landmarks, frame_number, csv_data):
	print(f"Landmark coordinates for frame {frame_number}:")
	for idx, landmark in enumerate(landmarks):
		print(f"{mp_pose.PoseLandmark(idx).name}: (x: {landmark.x}, y: {landmark.y}, z: {landmark.z})")
		csv_data.append([frame_number, mp_pose.PoseLandmark(idx).name, landmark.x, landmark.y, landmark.z])
	print("\n")


file_name = 'Sample_55'
 

# Define Input and Output Paths

video_path = f'/Users/parkjihoon/Documents/Projects/Python/Archery/양궁 파일럿/Sample_test_video/{file_name}.mp4'
output_csv = f'/Users/parkjihoon/Documents/Projects/Python/Archery/{file_name}.csv'

  
  

# Initialize Libraries
# Initialize MediaPipe Pose and Drawing utilities
mp_pose = mp.solutions.pose
mp_drawing = mp.solutions.drawing_utils
pose = mp_pose.Pose()
#pose = mp_pose.Pose('/Users/parkjihoon/Documents/Projects/Python/Mediapipe/pose_landmarker_lite.task')
#pose = mp_pose.Pose('/Users/parkjihoon/Documents/Projects/Python/Mediapipe/pose_landmarker_full.task')
#pose = mp_pose.Pose('/Users/parkjihoon/Documents/Projects/Python/Mediapipe/pose_landmarker_heavy.task')
  

# Open the video file
cap = cv2.VideoCapture(video_path)


# Process Each Frame of the Video

frame_number = 0
csv_data = []

while cap.isOpened():
	ret, frame = cap.read()
	if not ret:
		break
	
	# Convert the frame to RGB
	frame_rgb = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
	
	# Process the frame with MediaPipe Pose
	result = pose.process(frame_rgb)
	
	# Draw the pose landmarks on the frame
	if result.pose_landmarks:
		mp_drawing.draw_landmarks(frame, result.pose_landmarks, mp_pose.POSE_CONNECTIONS)
		
		# Add the landmark coordinates to the list and print them
		write_landmarks_to_csv(result.pose_landmarks.landmark, frame_number, csv_data)

	# Display the frame
	cv2.imshow('MediaPipe Pose', frame)
	
	# Exit if 'q' keypyt
	if cv2.waitKey(1) & 0xFF == ord('q'):
		break
	
	frame_number += 1

  
# Release the video capture and close the CSV file
cap.release()
cv2.destroyAllWindows()

# Write the collected pose landmarks data to the CSV file

with open(output_csv, mode='w', newline='') as file:
	writer = csv.writer(file)
	writer.writerow(['Frame Number', 'Landmark Name', 'X', 'Y', 'Z'])
	for data in csv_data:
		writer.writerow(data)

print("Processing completed. CSV file saved.")
```
