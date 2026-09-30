# Archery - Requirement Library  
  
-- 2025-09-25 WSL2에서 더 이상 Cuda 와 cuDNN 따로 설치 불필요  
  
//Miniforge  
conda create -n park_sci python=3.10  
conda activate park_sci  
  
conda update -n base -c conda-forge conda  
  
pip install tensorflow==2.14.0  
pip install nvidia-cudnn-cu11==8.6.0.163  
pip install nvidia-cublas-cu11  
pip install nvidia-cufft-cu11  
pip install nvidia-cuda-runtime-cu11  
pip install nvidia-cusolver-cu11  
pip install nvidia-cusparse-cu11  
pip install nvidia-nccl-cu11  
pip install nvidia-cuda-nvcc-cu11==11.8.89  
  
  
pip install ipykernel  
pip install pandas numpy==1.24.3 matplotlib scikit-learn opencv-contrib-python imbalanced-learn seaborn  
  
python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"  
