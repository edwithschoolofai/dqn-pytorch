# Pytorch를 사용한 딥 Q-러닝

Pytorch로 딥 Q-러닝 구현하기. 

* 재생 메모리
* (A3C나 Dueling을 사용하지 않는) 간단한 딥 Q-러닝 
* (디마인드에서 네이처지에 출판한 논문의) DQN 과 LSTM을 사용한 DQN 지원
* Pytorch 사용
* 프레임 스키핑 
* Target Network (학습할 때 안정성을 위한) 
* 파이썬 3.x (I used Python 3.6)

### DQN 알고리즘 
다음 글에 DQN이 어떻게 동작하는지 자세히 설명되어 있습니다. <br>
[http://andersonjo.github.io/artificial-intelligence/2017/06/03/Deep-Reinforcement-Learning/](http://andersonjo.github.io/artificial-intelligence/2017/06/03/Deep-Reinforcement-Learning/)

### 실제 게임 실행 
여기 있는 코드를 실행하면 다음 그림과 같은 결과물이 나옵니다.  <br>
![alt text](./images/flappybird.gif?raw=true)

[![Watch the video](http://img.youtube.com/vi/MkE6bnK7_DE/0.jpg)](https://youtu.be/MkE6bnK7_DE)

[![Snake Game](https://img.youtube.com/vi/cBxXIII4qRM/0.jpg)](https://www.youtube.com/watch?v=cBxXIII4qRM)


# 설치

필요 조건: 

1. 파이썬 3
2. Pytorch 
3. TorchVision
4. gym
5. opencv2 
6. Scipy 
7. ffmpeg (선택 사항. 게임 실행을 녹화하기 위해 필요함)


PyGame 설치하기

```
sudo pip3 install pygame
```

PyGame-Learning-Environment (PyGame 학습 환경) 설치하기

```
git clone https://github.com/ntasfi/PyGame-Learning-Environment.git
cd PyGame-Learning-Environment/
sudo pip3 install -e .
```

Gym-Ple 설치하기

```
git clone https://github.com/lusob/gym-ple.git
cd gym-ple/
sudo pip3 install -e .
```
## 비교 

| 알고리즘 | 게임 | 최고 점수 | 
|:----------|:-----|:-----------|
| DQN       | FlappyBird | 65   |
| LSTM을 사용한 DQN | FlappyBird | 83 |

* 최고 점수는 게임을 10번 실행한 한 후 구한 평균값입니다. 

# 사용법

## 학습하기

학습하기 전, 모델을 자동 저장하기 위해서 "dqn_checkpoints"라는 디렉토리를 생성해야합니다. 

```
mkdir dqn_checkpoints
python3 dqn.py --mode=train
```

LSTM을 사용한 DQN으로 학습하기 

```
mkdir dqn_checkpoints
python3 dqn.py --mode=train --model=lstm
```

## 게임하기

자동적으로 가장 최근 체크포인트를 로드합니다. (저장된 모델의 매개 변수를 로드해옵니다.) <br>
우선, 학습시킨 모델이 있어야 합니다. <br>
만약 (아직 모델을 학습시키지 않아) 체크포인트가 없다면, 게임은 무작위식으로 실행됩니다.

```
python3 dqn.py --mode=play
```

LSTM을 사용한 DQN으로 게임하기 

```
python3 dqn.py --mode=play --model=lstm
```

## 녹화하기

게임 실행을 녹화하려면 다음과 같이 실행하세요.

```
python3 dqn.py --mode=play --record 
```

## 영상을 GIF 파일로 전환하기

```
mkdir frames
ffmpeg -i flappybird.mp4 -qscale:v 2  -r 25 'frames/frame-%05d.jpg'
cd frames
convert -delay 4 -loop 0 *.jpg flappybird.gif
```

FFMpeg와 Imagemagic(변환 명령어)에는 다음과 같은 실행 옵션이 있습니다.

```
-r 5 stands for FPS value
    for better quality choose bigger number
    adjust the value with the -delay in 2nd step
    to keep the same animation speed

-delay 20 means the time between each frame is 0.2 seconds
   which match 5 fps above.
   When choosing this value
       1 = 100 fps
       2 = 50 fps
       4 = 25 fps
       5 = 20 fps
       10 = 10 fps
       20 = 5 fps
       25 = 4 fps
       50 = 2 fps
       100 = 1 fps
       in general 100/delay = fps

-qscale:v n means a video quality level where n is a number from 1-31, 
   with 1 being highest quality/largest filesize and 
   31 being the lowest quality/smallest filesize.

-loop 0 means repeat forever
```
