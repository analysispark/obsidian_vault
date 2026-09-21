**Mac Setting - 2025**

## 홈브루 설치

```{zsh}
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile

eval "$(/opt/homebrew/bin/brew shellenv)"
```


## oh-my-zsh 설치
```{zsh}
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

#### zsh 설정
```zsh
sudo vi ~/.zshrc
```


  

  

#Miniforge 설치

curl -L https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh -o Miniforge3-MacOSX-arm64.sh

bash Miniforge3-MacOSX-arm64.sh

  

conda init zsh

source ~/.zshrc

  

  

conda create -n tf_env python=3.10.12 -y

  

conda activate tf_env

  

conda install -c apple tensorflow-deps -y

  

pip install tensorflow-macos==2.12.0 tensorflow-metal==0.8.0 #optimizer issue

  

######## 현재 해결 안됨 - 커널 충돌 ########### 

pip install tensorflow-macos==2.10 tensorflow-metal==0.5

conca install scikit-learn==1.5.2

#####################################

  

  

pip install opencv-python==4.10.0.84

  

  

  

#Tensorflow TEST

python3

  

```{python3}

import tensorflow as tf

  

cifar = tf.keras.datasets.cifar100

(x_train, y_train), (x_test, y_test) = cifar.load_data()

model = tf.keras.applications.ResNet50(

    include_top=True,

    weights=None,

    input_shape=(32, 32, 3),

    classes=100,)

  

loss_fn = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=False)

model.compile(optimizer="adam", loss=loss_fn, metrics=["accuracy"])

model.fit(x_train, y_train, epochs=5, batch_size=64)

```

  

  

#HomeBrew 설치 리스트

1. 아크브라우저:  arc
2. 크롬 : google-chrome
3. 파이어폭스 : firefox
4. 옵시디안 : obsidian
5. 데이터분석 : cursor
6. 압축해제 : keka
7. 동영상 플레이어 : iina
8. 프로노트 : pronotes
9. 단축키 보기 : cheatsheet
10. 스크린샷 도구: shottr
11. 서지관리 : zotero
12. 터미널 : ghostly
13. 폰트 : font-meslo-lg-nerd-font
14. etc : xquartz

• 15. zsh-support : zsh-autosuggestions zsh-syntax-highlighting  
  
source /opt/homebrew/share/zsh-autosuggestions/zsh-autosuggestions.zsh  
source /opt/homebrew/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh