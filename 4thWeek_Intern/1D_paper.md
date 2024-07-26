
> self-supervised feature learning of 1d convolutional neural networks with contrastive loss... (제목)

In this work, we adapt ideas from self-supervised image
classification to 1D convolutional neural networks (CNN)
with the goal to train a chewing-detection model on audio
from an in-ear microphone, and focus on training the feature
extractor using only unlabeled data. The model is a deep
neural network (DNN) that includes convolutional and max-
pooling layers for feature extraction


### nn.Conv1d vs nn.Conv2d

- 시계열 데이터라고 생각하기
- kernel_size (연속된 N개의 값에 대해서 convolution 연산을 진행하나고 생각하면 된다)
	- (3, 5) 
- padding 연산의 경계 효과를 조절하는 방식을 말한다.



#### input and output



#### 2가지 sub-network로 구성한 상황

1. Feature network
2. Classifier network 

각각의 network를 어떻게 학습시키는지 확인해보자