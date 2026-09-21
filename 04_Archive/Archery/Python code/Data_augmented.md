```python

import os
import cv2
import numpy as np

def video_augmentation(input_path, output_dir, num_augmentations, shift_pixels=50):
    # 동영상을 읽어옴
    cap = cv2.VideoCapture(input_path)
    
    # 동영상의 속성을 가져옴
    frame_width = int(cap.get(3))
    frame_height = int(cap.get(4))
    fps = cap.get(5)

    # 만약 output_dir 디렉토리가 존재하지 않으면 생성
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)

    # 주어진 횟수만큼 동영상을 읽어오고 증식
    for augmentation_id in range(num_augmentations):
        cap.set(cv2.CAP_PROP_POS_FRAMES, 0)  # 프레임 위치 초기화

        # 랜덤하게 변형 방식 결정
        random_shift_direction = np.random.choice(['left', 'right', 'flip'])
        random_flip = np.random.choice([True, False])

        # 랜덤하게 회전 각도 결정
        fixed_rotation_angle = np.random.uniform(-10, 10)

        # 증식된 동영상을 저장할 파일명 설정
        output_path = os.path.join(output_dir, f"augmented_video_{augmentation_id + 1}.mp4")
        
        # 동영상 저장을 위한 VideoWriter 객체 생성
        fourcc = cv2.VideoWriter_fourcc(*'mp4v')
        out = cv2.VideoWriter(output_path, fourcc, fps, (frame_width, frame_height))

        while True:
            ret, frame = cap.read()

            if not ret:
                break

            # 랜덤하게 변형 적용
            if random_shift_direction == 'left':
                frame = shift_image(frame, shift_pixels=-shift_pixels, direction='left')
            elif random_shift_direction == 'right':
                frame = shift_image(frame, shift_pixels=shift_pixels, direction='right')

            if random_flip:
                frame = cv2.flip(frame, 1)

            # 랜덤하게 회전 각도 적용
            M = cv2.getRotationMatrix2D((frame_width / 2, frame_height / 2), fixed_rotation_angle, 1)
            frame = cv2.warpAffine(frame, M, (frame_width, frame_height))

            # 결과 동영상에 프레임 쓰기
            out.write(frame)

        # 사용한 자원 해제
        out.release()

    cap.release()
    cv2.destroyAllWindows()

def shift_image(image, shift_pixels, direction='right'):
    # 이미지의 높이와 너비 가져오기
    height, width = image.shape[:2]

    # 이동할 픽셀 수 계산
    if direction == 'left':
        shift_matrix = np.float32([[1, 0, -shift_pixels], [0, 1, 0]])
    elif direction == 'right':
        shift_matrix = np.float32([[1, 0, shift_pixels], [0, 1, 0]])
    else:
        raise ValueError("Invalid direction. Use 'left', 'right', or 'flip'.")

    # 이미지 이동
    shifted_image = cv2.warpAffine(image, shift_matrix, (width, height))

    return shifted_image

# 테스트를 위해 사용할 동영상 파일 경로 설정
current_dir = os.path.join(os.getcwd(), "sample_video/")
input_video_path = os.path.join(current_dir, "Sample_test_video/Sample_1.mp4")
output_dir = os.path.join(current_dir, "augmented_videos")

# 동영상 증식 실행
video_augmentation(input_video_path, output_dir, num_augmentations=5)



```
