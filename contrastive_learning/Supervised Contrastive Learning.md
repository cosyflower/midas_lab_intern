
#### Unknown

- self-supervised contrastive learning
- Representation learning




#### 3. Method

Given an input batch of data, 
we first apply data augmentation twice to obtain two copies of the batch. 

Both copies are forward propagated through the encoder network to obtain a 2048-dimensional normalized embedding. 


-> 2048 차원 정규화된 임베딩을 위한
-> encoder network를 통과하도록 forward propagated 함

During training, this representation is further propagated through a projection network that is discarded at inference time.

-> projection network 로 전파
-> 학습 시에만 전파되고 추론하는 경우에는 무시됨

The supervised contrastive loss is computed on the outputs of the projection network. 

To use the trained model for classification, we train a linear classifier on top of the frozen representations using a cross-entropy loss. 

-> ????


Fig. 1 in the Supplementary material provides a visual explanation.
-> Fig1이 도움을 줄거야~



##### Why do normalize??? Role of normalization

- Inner product
- cosine similarity by making norm = 1
- unit hypershpere - 벡터의 크기가 1인 모든 점들의 집합 (고차원 공간에서의 구체로 생각)


#### train에서만 사용하고 추론시에는 프로젝션 네트워크를 사용하지 않는 이유

결국 프로젝션을 사용하는 이유는 contrastive loss를 계산하기 위한 것. Backpropagation 과정을 통해서 가중치가 업데이트 된다. 따라서 추론하는 경우에는 projection network 를 구성하지 않아도 되고, 이는 비교하려는 모델의 파라미터 수를 일치시켜주는 장점도 존재한다. 

Cross-entropy를 활용한 모델 간 비교도 원활하게 가능하다 



![[Pasted image 20240709163353.png]]



#### Encoder를 학습시킨다는 의미

Representation을 학습시킨다?? 이 의미가 궁금
	강력한 일반화를 시킨다 - 다양한 downstream task에도 강건한 동작이 가능하다



