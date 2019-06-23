# Pytorch를 사용한 딥 Q 러닝

The implementation of Deep Q Learning with Pytorch. 
Pytorch로 딥 Q 러닝 구현하기. 

* Replay Memory 
* Simple Deep Q Learning (not using A3C or Dueling) 
* Support for original DQN (the paper in Nature published by DeepMind) and LSTM-based DQN
* Used Pytorch
* Frame Skipping 
* Target Network (for stability when training)
* Python 3.x (I used Python 3.6)

* Replay Memory 
* (A3C나 Dueling을 사용하지 않는) 간단한 딥 Q 러닝 
* Support for original DQN (the paper in Nature published by DeepMind) and LSTM-based DQN
* Pytorch 사용
* Frame Skipping 
* Target Network (for stability when training)
* 파이썬 3.x (I used Python 3.6)

### DQN Algorithm
The linked article explains how DQN works in detail.<br>
다음 링크에 DQN이 어떻게 동작하는지 자세히 설명합니다. 
[http://andersonjo.github.io/artificial-intelligence/2017/06/03/Deep-Reinforcement-Learning/](http://andersonjo.github.io/artificial-intelligence/2017/06/03/Deep-Reinforcement-Learning/)

### Actual Playing game 실제 게임 실행
The below image is actual result of the code here. 
여기 있는 코드를 돌리면 다음과 같은 결과물이 나옵니다. 
![alt text](./images/flappybird.gif?raw=true)

[![Watch the video](http://img.youtube.com/vi/MkE6bnK7_DE/0.jpg)](https://youtu.be/MkE6bnK7_DE)

[![Snake Game](https://img.youtube.com/vi/cBxXIII4qRM/0.jpg)](https://www.youtube.com/watch?v=cBxXIII4qRM)


# 설치하기

Requirements 

1. 파이썬 3
2. Pytorch 
3. TorchVision
4. gym
5. opencv2 
6. Scipy
7. ffmpeg (optional. if you want to record the gameplay)


PyGame 설치하기

```
sudo pip3 install pygame
```

PyGame-Learning-Environment 설치하기

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
| LSTM-based DQN | FlappyBird | 83 |

* Best Score is the average value of 10 times of games. 
* 최고 점수는 게임을 10번 한 후 구한 평균값입니다. 

# How to use 사용법

## Training 학습하기

Before training, you need to make a "dqn_checkpoints" directory for saving model automatically. 
학습하기 전에, 모델을 자동 저장하기 위해서는 "dqn_checkpoints"라는 디렉토리를 생성해야합니다. 

```
mkdir dqn_checkpoints
python3 dqn.py --mode=train
```

Training LSTM-based DQN

```
mkdir dqn_checkpoints
python3 dqn.py --mode=train --model=lstm
```

## Playing 게임하기

It automatically loads the latest checkpoint (it loads saved model parameters). <br>
자동적으로 가장 최근 체크포인트를 로드합니다. 
But first, you need to train it.<br>
하지만 우선 모델을 학습시켜야합니다. 
If there is no checkpoint (You might have not trained it yet), the play is just simply random walk. 
만약 체크포인트가 없다면, 게임은 무작위로 실생됩니다

```
python3 dqn.py --mode=play
```

playing LSTM-based DQN is like.. 

```
python3 dqn.py --mode=play --model=lstm
```

## Recoding 녹화하기

If you want to record game play, just do like this. 
게임 실행을 녹화하고 싶다면, 다음과 같이 실행하세요.

```
python3 dqn.py --mode=play --record 
```

## 영상을 GIF 파일로 전환하는 법

```
mkdir frames
ffmpeg -i flappybird.mp4 -qscale:v 2  -r 25 'frames/frame-%05d.jpg'
cd frames
convert -delay 4 -loop 0 *.jpg flappybird.gif
```

FFMpeg and Imagemagic(Convert command) have the following options. 

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
