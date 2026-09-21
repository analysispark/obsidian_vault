
![[Getting Started with tensorflow-metal.png]]
**어느날 애플의 텐서플로 공식 깃헙에 들어가보니 archived 되어있었고  
[이곳**](https://developer.apple.com/metal/tensorflow-plugin/) 으로 안내된다.  
  
이제 apple silicon용 텐서플로가 알파 버전으로 지원되는것이 아니라 텐서플로 v2.5부터 공식 지원된다.**

![[OS Requirements.png]]

[https://developer.apple.com/metal/tensorflow-plugin/**](https://developer.apple.com/metal/tensorflow-plugin/)  
공식 페이지에 들어가보면 초장부터 조금 황당한 소리를 하고 있다.  
macOS 12 라니 지금 현재는 베타버전인 OS를 설치하라고 한다.**

**그래도 혹시 모르니 한번 해보도록 한다.  
프로시져는 공식 페이지의 것을 그대로 따라하도록 한다.**

  
**Download and install** [**Conda env**](https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh)**:**

  

#chmod +x ~/Downloads/Miniforge3-MacOSX-arm64.sh

#sh ~/Downloads/Miniforge3-MacOSX-arm64.sh

#source ~/miniforge3/bin/activate

# Homebrew install miniforge.  <- 가능

  

**Install the Tensorflow dependencies:**

  

conda install -c apple tensorflow-deps

  

**Install base tensorflow:**

  

python -m pip install tensorflow-macos

  

**Install metal plugin:**

  

python -m pip install tensorflow-metal

  

**이제 따로 환경을 만들어서 테스트 해본다:**

  

conda create --clone base -n tensorflow

  

**만든 환경을 활성화 하고:**

  

conda activate tensorflow

  

**벤치마크용 코드를 실행하기 위해 이것도 설치한다:**

  

pip install tensorflow_datasets

  

  

  

**vscode던 아무 코드에디터로 test.py를 만들고 다음 코드를 넣는다:  
**[**이곳**](https://github.com/apple/tensorflow_macos/issues/25) **에서 벤치마크용으로 쓰이던 코드를 가져왔다.**

  

  

  

import tensorflow.compat.v2 as tf

import tensorflow_datasets as tfds

tf.enable_v2_behavior()

  

from tensorflow.python.framework.ops import disable_eager_execution

disable_eager_execution()

  

  

(ds_train, ds_test), ds_info = tfds.load(

    'mnist',

    split=['train', 'test'],

    shuffle_files=True,

    as_supervised=True,

    with_info=True,

)

  

def normalize_img(image, label):

  """Normalizes images: `uint8` -> `float32`."""

  return tf.cast(image, tf.float32) / 255., label

  

batch_size = 128

  

ds_train = ds_train.map(

    normalize_img, num_parallel_calls=tf.data.experimental.AUTOTUNE)

ds_train = ds_train.cache()

ds_train = ds_train.shuffle(ds_info.splits['train'].num_examples)

ds_train = ds_train.batch(batch_size)

ds_train = ds_train.prefetch(tf.data.experimental.AUTOTUNE)

  

  

ds_test = ds_test.map(

    normalize_img, num_parallel_calls=tf.data.experimental.AUTOTUNE)

ds_test = ds_test.batch(batch_size)

ds_test = ds_test.cache()

ds_test = ds_test.prefetch(tf.data.experimental.AUTOTUNE)

  

  

model = tf.keras.models.Sequential([

  tf.keras.layers.Conv2D(32, kernel_size=(3, 3),

                 activation='relu'),

  tf.keras.layers.Conv2D(64, kernel_size=(3, 3),

                 activation='relu'),

  tf.keras.layers.MaxPooling2D(pool_size=(2, 2)),

#   tf.keras.layers.Dropout(0.25),

  tf.keras.layers.Flatten(),

  tf.keras.layers.Dense(128, activation='relu'),

#   tf.keras.layers.Dropout(0.5),

  tf.keras.layers.Dense(10, activation='softmax')

])

model.compile(

    loss='sparse_categorical_crossentropy',

    optimizer=tf.keras.optimizers.Adam(0.001),

    metrics=['accuracy'],

)

  

model.fit(

    ds_train,

    epochs=12,

    validation_data=ds_test,

)

  

  

  

**실행한다:**

python test.py

**실행되는 동안 활성상태를 보면**

  
![[image.png]]
  

  

**GPU가속을 잘 활용하는것을 볼 수 있다.**

**본인은 다음과 같이 결과가 나왔는데, 다른 시스템하고 비교해보는것도 재밌을 것이다.**

Init Plugin

Init Graph Optimizer

Init Kernel

Metal device set to: Apple M1

2021-07-20 17:32:24.807252: I tensorflow/core/common_runtime/pluggable_device/pluggable_device_factory.cc:305] Could not identify NUMA node of platform GPU ID 0, defaulting to 0. Your kernel may not have been built with NUMA support.

2021-07-20 17:32:24.807441: I tensorflow/core/common_runtime/pluggable_device/pluggable_device_factory.cc:271] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:0 with 0 MB memory) -> physical PluggableDevice (device: 0, name: METAL, pci bus id: <undefined>)

2021-07-20 17:32:24.859240: I tensorflow/core/common_runtime/pluggable_device/pluggable_device_factory.cc:305] Could not identify NUMA node of platform GPU ID 0, defaulting to 0. Your kernel may not have been built with NUMA support.

2021-07-20 17:32:24.859261: I tensorflow/core/common_runtime/pluggable_device/pluggable_device_factory.cc:271] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:0 with 0 MB memory) -> physical PluggableDevice (device: 0, name: METAL, pci bus id: <undefined>)

2021-07-20 17:32:24.865391: W tensorflow/core/platform/profile_utils/cpu_utils.cc:128] Failed to get CPU frequency: 0 Hz

2021-07-20 17:32:24.956177: I tensorflow/core/grappler/optimizers/custom_graph_optimizer_registry.cc:112] Plugin optimizer for device_type GPU is enabled.

2021-07-20 17:32:24.965837: I tensorflow/core/grappler/optimizers/custom_graph_optimizer_registry.cc:112] Plugin optimizer for device_type GPU is enabled.

2021-07-20 17:32:25.012885: I tensorflow/core/grappler/optimizers/custom_graph_optimizer_registry.cc:112] Plugin optimizer for device_type GPU is enabled.

2021-07-20 17:32:25.026958: I tensorflow/core/grappler/optimizers/custom_graph_optimizer_registry.cc:112] Plugin optimizer for device_type GPU is enabled.

2021-07-20 17:32:25.090473: I tensorflow/core/grappler/optimizers/custom_graph_optimizer_registry.cc:112] Plugin optimizer for device_type GPU is enabled.

2021-07-20 17:32:25.104164: I tensorflow/core/grappler/optimizers/custom_graph_optimizer_registry.cc:112] Plugin optimizer for device_type GPU is enabled.

2021-07-20 17:32:25.125363: I tensorflow/core/grappler/optimizers/custom_graph_optimizer_registry.cc:112] Plugin optimizer for device_type GPU is enabled.

2021-07-20 17:32:25.142313: I tensorflow/core/grappler/optimizers/custom_graph_optimizer_registry.cc:112] Plugin optimizer for device_type GPU is enabled.

2021-07-20 17:32:25.159820: I tensorflow/compiler/mlir/mlir_graph_optimization_pass.cc:176] None of the MLIR Optimization Passes are enabled (registered 2)

Train on 469 steps, validate on 79 steps

2021-07-20 17:32:25.175285: I tensorflow/core/grappler/optimizers/custom_graph_optimizer_registry.cc:112] Plugin optimizer for device_type GPU is enabled.

Epoch 1/12

2021-07-20 17:32:25.186795: I tensorflow/core/grappler/optimizers/custom_graph_optimizer_registry.cc:112] Plugin optimizer for device_type GPU is enabled.

468/469 [============================>.] - ETA: 0s - batch: 233.5000 - size: 1.0000 - loss: 0.1561 - accuracy: 0.9540/Users/qone/miniforge3/envs/tensorflow-test/lib/python3.9/site-packages/tensorflow/python/keras/engine/training.py:2426: UserWarning: `Model.state_updates` will be removed in a future version. This property should not be used in TensorFlow 2.0, as `updates` are applied automatically.

  warnings.warn('`Model.state_updates` will be removed in a future version. '

2021-07-20 17:32:35.514217: I tensorflow/core/grappler/optimizers/custom_graph_optimizer_registry.cc:112] Plugin optimizer for device_type GPU is enabled.

469/469 [==============================] - 11s 22ms/step - batch: 234.0000 - size: 1.0000 - loss: 0.1559 - accuracy: 0.9541 - val_loss: 0.0478 - val_accuracy: 0.9848

Epoch 2/12

469/469 [==============================] - 11s 22ms/step - batch: 234.0000 - size: 1.0000 - loss: 0.0437 - accuracy: 0.9866 - val_loss: 0.0416 - val_accuracy: 0.9861

Epoch 3/12

469/469 [==============================] - 11s 22ms/step - batch: 234.0000 - size: 1.0000 - loss: 0.0269 - accuracy: 0.9917 - val_loss: 0.0353 - val_accuracy: 0.9878

Epoch 4/12

469/469 [==============================] - 11s 22ms/step - batch: 234.0000 - size: 1.0000 - loss: 0.0187 - accuracy: 0.9941 - val_loss: 0.0306 - val_accuracy: 0.9898

Epoch 5/12

469/469 [==============================] - 11s 22ms/step - batch: 234.0000 - size: 1.0000 - loss: 0.0133 - accuracy: 0.9958 - val_loss: 0.0389 - val_accuracy: 0.9885

Epoch 6/12

469/469 [==============================] - 11s 22ms/step - batch: 234.0000 - size: 1.0000 - loss: 0.0097 - accuracy: 0.9968 - val_loss: 0.0431 - val_accuracy: 0.9876

Epoch 7/12

469/469 [==============================] - 11s 22ms/step - batch: 234.0000 - size: 1.0000 - loss: 0.0104 - accuracy: 0.9966 - val_loss: 0.0334 - val_accuracy: 0.9899

Epoch 8/12

469/469 [==============================] - 11s 22ms/step - batch: 234.0000 - size: 1.0000 - loss: 0.0047 - accuracy: 0.9984 - val_loss: 0.0359 - val_accuracy: 0.9897

Epoch 9/12

469/469 [==============================] - 11s 22ms/step - batch: 234.0000 - size: 1.0000 - loss: 0.0060 - accuracy: 0.9981 - val_loss: 0.0414 - val_accuracy: 0.9890

Epoch 10/12

469/469 [==============================] - 11s 22ms/step - batch: 234.0000 - size: 1.0000 - loss: 0.0053 - accuracy: 0.9983 - val_loss: 0.0366 - val_accuracy: 0.9906

Epoch 11/12

469/469 [==============================] - 11s 22ms/step - batch: 234.0000 - size: 1.0000 - loss: 0.0042 - accuracy: 0.9987 - val_loss: 0.0415 - val_accuracy: 0.9899

Epoch 12/12

469/469 [==============================] - 11s 22ms/step - batch: 234.0000 - size: 1.0000 - l