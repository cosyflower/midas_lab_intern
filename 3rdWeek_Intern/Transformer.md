> Attention is all you need

- [ ] 구조나 원리 이해 - 다만 Q K V 의 파라미터가 학습되는 과정이 궁금. 이 파트는 직접 구현해야 확인 가능할 듯 
- [ ] 차원 인지하는 방법에 대해서

#### Background - RNN 

연속적인 특징을 반영하기 위해서 이전의 학습된 인공신경망 내 neuron을 그대로 가지고 오고, sum한다.
Activation function으로 주로 tanh() 함수를 적용. 범위가 -1 ~ 1, 기울기 범위는 0 ~ 1로 진행함. Forward propagation, Back propagation 상 한계가 발생 (점점 값이 작아지는 문제점이 발생하기 때문). 단, 이런 상황을 가지고 Vanishing Gradient라고 말하면 안됨. 점점 Gradient값이 작아지는 것을 말하는게 아니라, 더해지는 값들이 작아지는 문제점을 가지고 있다라고 이야기 할 수 있어야함.

#### Seq2Seq 모델 - Teacher forcing

1. 멀수록 잊혀지는 문제점
2. Context vector에는 마지막 단어의 정보가 뚜렷하게 담긴다는 점. decoder에서 해당 context vector를 가지고 번역을 진행하다보니, 마지막 단어를 제일 열심히 보게 된다 


#### Context vector를 서로 다르게 줘보자

매번 같은 정보를 참고한다는 문제점을 해결하려면 -> 다른 정보를 볼 수 있게끔 만들면 된다. 
-> 이전에 Encoder에서 final output만을 가지고 학습을 했다면, 그렇지 않게 학습해보자
-> Sentence embedding으로 생각하지 말고 Word embedding으로 바라보자 (관점 변화) - Main role
-> 다른 단어들을 참고하자는 뜻으로 생각하자는 것(Additional한 Role)

###### 특정 단어만 보는것이 아니라 모든 단어를 바라봐야만 한다

모든 단어에 대한 정보, 즉 embedding vector를 모두 가지고 온 상황에서 생각을 해보자는 뜻. 

- 모두 동일하게 Add 한다면?? - 가중치 개념이 들어가 있지 않음. 동일한 시각으로 단어를 바라본다는 것. (어울리지 못한 시각) 
- Weighted sum! - 각각의 임베딩 벡터에 가중치를 곱하자는 뜻으로 생각해보자

###### 어떤 가중치를 반영해야 할까??

앞에 가중치로 단순한 값만 붙인다면? 그것은 "거리"에 대한 개념만 반영한 셈이다. 위치반 반영하는 꼴이라고 할 수 있다. 위치가 아니라 가장 중요한 것은 해당 단어의 값이 중요하다. 따라서 가중치는 function of h 가 되어야 겠다 *h는 encoding of single word로 생각하면 되겠다*

###### 여기서 끝이 아니라

문제는 가중치를 또 동일하게 줘야 하나는 것인데, 현실적으로 불가능하다. 매번 단어를 번역할 때마다 src 문장에 있는 단어들에 대해서 0.7 0.2 0.1로 바라보는 것은 원활하지 못한 생각이다. 

현재 시점의 정보도 반영할 수 있어야겠다! -> 함수로 표현을 할 때 decoder에서 도출된 단어도 같이 학습시키자는 것으로 생각하며 된다. 

##### Linear Space에서 생각을 한다면 

어떤 벡터와 유사할 지를 학습하는 과정이라고 생각하면 된다 (geometrical info)
예로 an 인 벡터를 embedding 하는 벡터로 가정한다면 강사를 embedding 한 벡터와 가까워진다고 생각하면 된다(다른 벡터와 가까워진다 = 다른 벡터와 유사하다)
(여기서 an과 강사가 가까워지는 이유를 궁금할 수 있다. 현재 decoder는 next-token-prediction 모델이기 때문에 이런 결과가 발생하는 것이다)

#### RNN + Attention의 문제점

구조 내 변화가 있었다. word embedding으로 생각하자는 것과 weighted sum에서 가중치를 어떻게 부여할 것인지에 대해서 생각해보았다. 가중치에서는 현재 정보와 그리고 관계를 확인할 다른 h와 구성된 함수를 적용한다. 현 정보와 encoder 에서의 token 정보를 모두 가지고 응용하는 방식으로 변화를 가한 셈이다. 하지만 여전히

"거리"의 영향을 받고 있다. 거리의 영향을 받고 있는 원인을 생각해야 한다. 

#### 거리의 영향을 받지 않게 하기 위해서는

연속적으로 영향을 미치는 관계를 제거해야 한다. 기존 RNN에서 기존의 정보를 다시 가지고 와서 활용하는 방식을 배제하면 거리의 영향을 받지 않는 학습이 가능하다.

#### Encoder-Decoder attention, Self-attention에 대해서 

==벡터를 구분하면 context-vector, encoding-vector, decoding-vector로 구분이 가능하다.==


###### About masking

파악가능한 info만 확인해야 한다. 확인이 불가능한 정보를 활용해서는 안되기 때문. 


###### 정리

거리의 영향을 받지 않기 위해서 거리를 아예 끊었다. 거리의 영향을 더 이상 받지 않는다.
C(context), H(encoding), S(decoding), 각각의 벡터에 대해서 update를 진행한다. 


----

## Attention is all you need 시작 (Transformer)

> Attention + positional embedding + optional masking

- 단어를 숫자로 잘 변환했고
- Self-attention : 내적을 활용한 weighted sum (닮은 정도를 의미하는 내적. 내적을 통해 단어의 관계를 파악하자는 것이 되겠다.
- Attention을 활용해서 어떤 단어를 주목할지를 학습했다. 
- ==내적과 가중합을 활용해서 어떤 단어에 주목할지를 학습했다. ==


#### Inputs embedding - input data feature를 반영할 수 있어야 한다.

> 단어를 embedding. 정확히 따지면 문장 내 단어를 embedding 하고 있으니, 위치 그리고 단어의 정보를 같이 반영하면 좋겠다! 라고 전달하는 셈임.

nn.Linear( ? , 512 ) - 512 차원의 embedding space로 변환을 먼저 진행한다. 
기존의 input이 one-hot encoding 형태이기 때문에 변환을 하게 되면 

결과적으로 행 뽑기가 되는 상황이다. 

-> 각각의 뜻이 one-hot encoding으로 반영된 상황
-> 거리가 정보가 없다. 따라서 거리를 반영해야 한다 - positional embedding 벡터 생각하기
	-> 마찬가지로 거리에 대한 정보 역시 one-hot encoding 으로 전달하게 된다
	-> .linear(max_len, 512)
	-> 정보 그리고 거리 모두 512 차원으로 embedding 하게 되었다. 이 2개의 정보를 더했다고 생각하자. 


<- 실제 트랜스포머 논문과는 달리 코드 상으로는 Positional Encoding을 진행하고 있다. 따로 학습을 시킨 것이 아님을 유의하자 
- Encoding - fixed vector 형식으로
- Embedding - 학습 파라미터 형식으로 위치를 표현하는 방식을 마찬가지로 학습을 하게 된다. 
- 요새는 모두 Embedding, 학습하는 형식으로 변하고 있으니 참고할 수 있도록 



##### Transformer 내 학습하는 레이어는??? 

> 단어 간 관계를 학습하기 위해서 내적을 사용한다 

단어 간 관계를 파악, 어떤 단어를 주목해야 하는지를 인공신경망이 직접 학습하는 방식을 말한다. 
그렇다면 대체 학습 파라미터는 무엇일까? 라는 생각을 해야 한다.

#### 학습 파라미터 - Q, K, V

input-data를 토대로 Q, K, V를 얻을 수 있다. 
nn.Linear(512, 64) 를 통과 시켜 하나의 단어(token)당 3개의 파라미터를 구한다 (Q, K, V)
ex) 저는q, 저는k, 저는v

각 파라미터 별로 역할을 나누고, 각 역할별로 독립적으로 학습하자는 형태를 유도한다. 

Q - 관계를 물어볼 기준이 되는 단어의 벡터
K - Query와의 관계를 물어볼 단어 벡터
K - 키 단어의 의미를 담은 벡터 

##### Attention 수식 이해하기 

QK 행렬곱을 진행한 다음 - dk 로 scaling을 진행하고 - V 곱한다

1. Q, K를 먼저 곱해주면서 서로 다른 token 간의 유사도를 먼저 비교한다.
2. Root dk로 scaling을 진행한다
3. dim = 1 방향으로 softmax 함수를 통과한다

scaling을 하는 이유 - softmax 함수에 input할 때 입력 데이터의 값이 커지게 되면 분산값이 커진다. 분산값이 커지면 미분 값이 작아지는 문제가 있다. 따라서 scaling을 진행한 다음에 softmax 함수를 통과하게 되는 것이다. 

QK를 곱하면 단어 간 유사도를 구할 수 있다. 그리고 나서 V를 곱해준다. 
먼저 내적을 활용해서 유사도를 구했고 - softmax 함수를 통과해서 분포를 구한 상황이다. 그리고 난 다음에 V 즉 해당 단어의 정보를(가치 or 정보) 곱해줌으로써 유사도 x value 형태가 되는 것이다. 

해당 단어가 가진 value를 반영하는게 main 거기에 additional 하게 다른 단어들을 참고하는 형태를 이렇게 완성시켰다고 생각하면 되겠다. 

#### Multi-Head Attention

판단을 한 명이 내리는게 아니라 여러 명이 판단을 내린다 - 더 객관적이고 종합적인 결론을 유도한다. 
nn.Linear(512, 64) 통과시킨 8명을 concat() 하는 셈. 

정리하자면 512 -> 64 x 8 명으로 구성 - 자리에 앉혀서 서로의 의견을 교류할 수 있도록 만드는 것이 concat()이 되겠다. 그리고 이 과정이 Multi-head attention 이라고 할 수 있는 것이다. 



#### Q? 차원의 변화로 기대할 수 있는 효과 (inc - dec vs dec - inc)




#### Masked MHA(Multi-head attention)

입력 들어가는 방식은 Encoder와 동일하지만, 학습 시엔 지도 학습, test 땐 출력 나온 것을 입력을 사용한다. 이 때 정답 문장을 그대로 넣으면 안 된다. 미래의 정답 데이터를 미리 주면 안되는 것이다.

뒷단어를 볼 수 없도록 Masking 해야 한다. softmax 통과하기 직전에 엄청 작은 음수로 바꿔야 한다. 

-> 현재 단어를 포함해서 이전의 단어들만 참고할 수 있도록 변형하는 것이다. 
-> Q는 encoder의 embedding 하는 벡터를 , K와 V는 decoder의 embedding output을 의미한다. 
-> 질문은 어떤 걸 해야하고, 그에 따른 적절한 답변이 무엇인지를 생각해볼 필요가 있는 것이다. 

self attention - encoder and decoder - FF

---

#### 관련 영상 혹은 Reference

https://aistudy9314.tistory.com/63

#### Implementation

-> nn.Parameter() - learnable parameters + automatically included in model's parameters 

`nn.Parameter()`는 모델 내에서 학습 가능한 파라미터를 정의하고 관리하는 데 중요한 역할을 합니다. 이를 통해 파라미터가 자동으로 모델의 파라미터 목록에 포함되고, 역전파 동안 기울기가 자동으로 계산됩니다. 이러한 기능은 모델을 구성하고 학습하는 데 매우 유용합니다.


#### 전체에서 부분으로 접근하자

Input Sentence -> Model -> Output Sentence 

### Model - Encoder + Decoder 

> 부수적인 요소들도 존재하지만 크게 생각했을 때 2가지로 구분할 수 있다고 생각하면 된다.

- Encoder는 input으로 sentence를, output으로 context vector를 만든다. 이 과정을 "Encoding" 이라고 말한다. 문맥을 함축한 vector를 말하며 문장의 정보들을 추상적으로 반영할 수 있는 것을 목표로 학습하게 된다. 
- Decoder는 input으로 some sentence(?) 그리고 context 를, output으로 결과인 output sentence를 형성하게 된다. 여기서 말하는 some sentence란 Decoder가 output으로 생성하는 sentence를 right shift한 sentence도 함께 입력을 받는다. 

###### 정리하자면

Model 은 인코더 그리고 디코더로 구성되어 있다. 인코더, 디코더도 일종의 함수다. 함수의 개념처럼 "무언가"를 입력으로 받아야 하고, "무언가"를 도출(결과를 도출)해야 한다. 

-> 인공신경망도 일종의 Function 임을 잊지 말자.

### Encoder

encoder block이 N개 구성되어 있다고 생각하면 된다. 이 때 input 그리고 output의 shape이 동일한 matrix를 encoder block이 유지한다. 따라서 encoder block은 shape에 대해 멱등해야 한다. 

이는 encoder 전체도 shape에 대해 멱등하다라고 말할 수 있다. Encoder 내부를 살펴보면 같은 encoder block이 N개로 구성되어 있다(논문상에는 N=6). 하나의 encoder block은 input sentence를 입력받을 때 sentence에 대한 추상적인 정보를 담은 context vector를 형성하게 된다. 같은 encoder block을 계속해서 지나가면서 더 고차원적인 context vector를 만들 수 있는 것이다(더 중요한 정보를 많이 담은 context vector를 형성하게 된다)


###### 정리하자면

Encoder는 같은 Encoder block으로 구성되어 있다. 각각의 encoder block은 context vector를 생성한다. 여러 개의 encoder block을 통과할수록 더 고차원적인 context vector를 생성하게 된다
input - output의 shape이 동일한 matrix를 유지해야 하며 shape에 대해 멱등성을 가진다(idempotent)



##### Encoder block - multihead-attention + position-wise feed forward

### Multi-Head Attention

> 먼저 scaled dot-product attention. 넓은 범위의 전체 데이터에서 특정한 부분에 집중한다는 의미

- 같은 문장 내의 attention - self-attention (같은 문장 내 서로 다른 token)
- 다른 문장 내의 attention - cross-attention (다른 문장 간 서로 다른 token)


##### 기존 RNN 방식과의 차이점

- 거리의 영향을 받는 RNN
- 거리의 영향을 받지 않고 NxN 연산을 수행하게 되는 self-attention

![[Pasted image 20240718102800.png]]


##### Query, Key, Value

- Query : 질문 (현재 시점의 token)
- Key : 답변 (Attention을 구하고자 하는 대상 token을 의미)
- Value : token(단어)에 담긴 정보의 가치 (Key와 동일한 token)

사실 Key, Value는 같은 token 을 의미한다. 왜 그러면 따로 2개가 존재하냐면, 의미적으로는 같은 token이지만 실제 값은 서로 다르기 때문이다. 또한 이후의 Attention 계산 과정에서 별개로 사용하게 된다. 

Q - 기준이 되는 token K, V는 semantic 적으로 같은 token을 지칭한다. 다만 계산하는 과정에서 서로 다른 값을 갖게 되는, 구분되는 개념이 되는 것이다. 

공통점으로 위 3가지 모두 같은 shape을 유지한다. 다른 값이지만 같은 shape을 유지한다. input으로 받는 word embedding vector도 동일하고 동일한 shape의 output도 만들어내는 것을 인지하자.

$$ \text{Dimension} (Q, K, V) =d_k$$ 
### Scaled Dot-Product Attention

$$
\text{Query's Attention } (Q, K, V) = \text{softmax} \left( \frac{QK^T}{\sqrt{d_k}} \right) V
$$

##### Q K V 구하는 Matrix shape도 모두 동일하다

input sentence -> Q, K, V를 만들기 위한 matrix -> Q, K, V 를 가지고 self - attention 진행


![[Pasted image 20240718111319.png]]


#### code 상에서는 

q, k, v - n_batch x seq_len x d_k 로 표현할 수 있다. 

attention_score - q x transpose of k -> n_batch x seq_len x seq_len
이후 math.sqrt(d_k) 로 나누면 구할 수 있다. 

- copy.deepcopy()를 사용해야 weight matrix의 shape을 그대로 가지고 오되 서로 다른 값을 가지고 올 수 있다. 
- view() - 메모리에서 연속된 경우메나 사용한다
- contiguous() - 메모리에서 연속된 형태로 변환하는 데 사용되며 view()를 사용하기 전에 호출이 가능하다. 
- reshape() -연속되지 않은 메모리에서도 shape 변화를 만들 수 있음 

#### Encoder block을 다시 작성해보자면

이전에 코드를 작성할 때는 self_attention, position_ff 로 크게 2가지로 나눠서 구성했다. 
부분적으로 수정을 해보자면 src_mask(masking pad) 를 외부에서 인자로 받을 것이기에 EncoderBlock에서의 foward() 인자를 추가해야 한다. 

EncoderBlock -> Encoder -> Transform을 전체적으로 수정하면 되겠다.


#### Pad Mask Code in Pytorch

self 인 경우 그리고 cross 인 경우에 유의해서 작성해야 한다. 

##### Encoder block은 사실 Residual connection을 적용한다.

> nn.Dropout(), torch.norm() (Add and norm 하는 과정으로 생각하기)

Encoder block 내부에서는 self_attention을 진행한 다음 residual connection
그리고 position-wise feed forward를 진행하고 마찬가지로 residual connection 진행하도록 설정하면 되겠다


## Decoder 

- input : some sentence, context
- output : output sentence

##### Context 말고도 추가적인 info를 요구하는 이유에 대해서 

- context : encoder에서 생성한 결과. 구성된 Layer 모두 shape이 멱등함. context 역시 Encoder의  input sentence와 동일한 shape를 갖느다. 
- sentence : 실제 label data의 token??? Teacher forcing!!!

train() - 학습과정에서는 실제 label data를 사용하지만, Test나 Real-world에서는 model이 생성해낸 이전 token을 사용하게 된다. 


#### make_tgt_mask -> forward() 수정관련