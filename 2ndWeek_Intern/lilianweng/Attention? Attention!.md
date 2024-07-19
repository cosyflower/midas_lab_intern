
### High resolution with certain region, not all of it

In images or sentences, we do not focus on every pixel or word(or token)
Purpose is to explain the relatioship btween words in one sentence or close context

EX) She is eating a green apple -> eating - green <<<<< eating - apple

In a nutshell, attention in deep learning can be broadly interpreted as a vector of importance weights: in order to predict or infer one element, such as a pixel in an image or a word in a sentence, we estimate using the attention vector how strongly it is correlated with (or “_attends to_” as you may have read in many papers) other elements and take the sum of their values weighted by the attention vector as the approximation of the target.

#### Problem of Seq2Seq Model


- context vector - thought vector (compresses the information, or embedding)
- from context vector to emit the transformed output - using the las state of the encoder network as the decoder initial state

A critical and apparent disadvantage of this fixed-length context vector design is incapability of remembering long sentences. Often it has forgotten the first part once it completes processing the whole input. The attention mechanism was born ([Bahdanau et al., 2015](https://arxiv.org/pdf/1409.0473.pdf)) to resolve this problem.

![[Pasted image 20240709092147.png]]


- incapability of remembering long sentences
- vanishing gradient - initial state loses their impacts on processing gradually

#### Rather than

building a single context vector out of the encoder's last hidden state, the secret sauce invented by attention is to create shortcuts (지름길) between the context vector and entire source input 

-> weights of these shortcut connections are customizable for each output element


##### Alignment between the source and target 

The alignment between the source and target is learned and controlled by the context vector.

-> context vector에 학습이 됨 


#### NMT 관련 구현

torch.manual_seed(num) : 난수를 고정하고 싶은 경우

- torch.dot() - 내적 (하나의 스칼라 값을 유도할 수 있음)
- 일반 행렬곱 진행할 때 - (n,m) 각각의 원소 간 곱으로 생각하기

2가지 상황 구분해서 구현해야 할 듯

** 구현 추가적으로 더 살펴볼 것 - definition 50프로 정도 이해하고 PASS


### Attention mechanisms

----

https://bigdaheta.tistory.com/67


##### Attention 관련 설명



