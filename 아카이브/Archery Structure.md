# Archery Structure  
  
archery-score-prediction/  
│  
├── data/  
│   ├── raw/                    # 원본 JSON 파일 저장  
│   ├── processed/              # 전처리된 데이터 저장  
│   ├── split/                  # 훈련/검증/테스트 데이터셋 저장  
│   ├── feedback/               # 시각화를 위한 피드백 데이터  
│  
├── models/  
│   ├── weights/                # 선수별 학습된 가중치 저장  
│   ├── checkpoints/            # 학습 중간 체크포인트 저장  
│  
├── scripts/  
│   ├──data_collect.py        # 데이터 수집 스크립트  
│   ├── preprocess.py           # 데이터 전처리 스크립트  
│   ├── train.py                # 모델 학습 스크립트  
│   ├── evaluate.py             # 모델 평가 스크립트  
│   ├── visualize.py            # 피드백 시각화 스크립트  
│   ├── utils.py                # 공통 유틸리티 함수  
│  
├── notebooks/  
│   ├── data_analysis.ipynb     # 데이터 분석 및 시각화 노트북  
│   ├── model_tuning.ipynb      # 하이퍼파라미터 튜닝 노트북  
│  
├── configs/  
│   ├── archers.json            # 선수별 메타데이터 및 설정 파일  
│   ├── train_config.yaml       # 학습 관련 설정 파일  
│  
├── tests/  
│   ├── test_preprocessing.py   # 전처리 코드 테스트  
│   ├── test_training.py        # 학습 코드 테스트  
│   ├── test_visualization.py   # 시각화 코드 테스트  
│  
├── results/  
│   ├── logs/                   # 학습 로그 저장  
│   ├── plots/                  # 학습 과정 시각화 (예: 손실, 정확도)  
│   ├── feedback_images/        # 피드백 시각화 이미지 저장  
│  
├── main.py                     # 프로젝트 실행 메인 스크립트  
├── requirements.txt            # 필요한 Python 패키지 목록  
├── README.md                   # 프로젝트 설명 파일  
└── .gitignore                  # Git 무시 파일 목록  
  
———————————————————————————————————————  
**각 파일의 역할**  
1. **data/**:  
    * **raw/**: OpenPose에서 추출한 JSON 데이터를 저장.  
    * **processed/**: 전처리된 데이터를 CSV나 Numpy 형태로 저장.  
    * **split/**: 훈련, 검증, 테스트 데이터셋 분리 저장.  
    * **feedback/**: 피드백 시각화를 위한 데이터를 저장.  
2. **models/**:  
    * **weights/**: 선수별로 독립적으로 학습된 모델 가중치를 저장.  
    * **checkpoints/**: 학습 중간 저장용 체크포인트.  
3. **scripts/**:  
    * **preprocess.py**: JSON 데이터를 로드하고 전처리.  
    * **train.py**: 선수별 모델 학습 및 가중치 저장.  
    * **evaluate.py**: 모델 평가 및 테스트 점수 계산.  
    * **visualize.py**: 피드백 시각화를 위한 스크립트.  
    * **utils.py**: 파일 로드, 데이터 변환 등 공통 함수.  
4. **notebooks/**:  
    * **data_analysis.ipynb**: JSON 데이터 통계 및 시각화.  
    * **model_tuning.ipynb**: 모델 하이퍼파라미터 조정 및 성능 비교.  
5. **configs/**:  
    * **archers.json**: 선수 ID, 이름, JSON 파일 경로 등의 메타정보.  
    * **train_config.yaml**: 학습 설정 (배치 크기, 학습률 등).  
6. **tests/**:  
    * 전처리, 학습, 시각화 코드의 단위 테스트.  
7. **results/**:  
    * 학습 결과물(로그, 시각화 그래프, 피드백 이미지) 저장.  
8. **main.py**:  
    * 프로젝트 전체 파이프라인을 실행하는 메인 스크립트.  
9. **requirements.txt**:  
    * 필요한 Python 패키지 목록 (예: TensorFlow, NumPy, Matplotlib 등).  
10. **README.md**:  
    * 프로젝트 소개, 사용법, 파일 구조 설명.  
