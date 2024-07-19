we split an image into patches and provide the sequence of linear embeddings of these patches as an input to a Transformer. Image patches are treated the same way as tokens (words) in an NLP application. We train the model on image classification in supervised fashion.

However, the picture changes if the models are trained on larger datasets (14M-300M images). We find that large scale training trumps inductive bias. Our Vision Transformer (ViT) attains excellent results when pre-trained at sufficient scale and transferred to tasks with fewer datapoints.


Most related to ours is the model of Cordonnier et al. (2020), which extracts patches of size 2 × 2 from the input image and applies full self-attention on top. This model is very similar to ViT, but our work goes further to demonstrate that large scale pre-training makes vanilla transformers competitive with (or even better than) state-of-the-art CNNs. Moreover, Cordonnier et al. (2020) use a small patch size of 2 × 2 pixels, which makes the model applicable only to small-resolution images, while we handle medium-resolution images as well.


Figure 1: Model overview. We split an image into fixed-size patches, linearly embed each of them, add position embeddings, and feed the resulting sequence of vectors to a standard Transformer encoder. In order to perform classification, we use the standard approach of adding an extra learnable “classification token” to the sequence. The illustration of the Transformer encoder was inspired by . . .

-> 이미지를 고정 패치로 나누고, 각 패치에 대해서 Linear embedding을 진행한다. 위치 임베딩을 추가해서 결과 벡터 시퀀스를 트랜스포머 인코더에 입력합니다. 
-> 시퀀스에 추가 학습이 가능한 분류 토큰을 추가하는 표준 접근 방식을 사용합니다. 


![[Pasted image 20240715151352.png]]

#### Classification token의 추가에 대해서 with BERT???


##### Residual connection 효과 


###### concat() 했을 때 차원의 변화도 