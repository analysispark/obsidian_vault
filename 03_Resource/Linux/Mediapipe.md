```python
# WSL2 Cuda & CuDnn 설치

# install numpy
conda install -c conda-forge numpy

# install Tensorflow-gpu
pip install tensorflow

# install Mediapipe
pip install mediapipe

```

```bash
python -c "import tensorflow as tf; print(tf.config.experimental.list_physical_devices('GPU'))"
```