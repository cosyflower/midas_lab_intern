
#### SimCLR에 대해서

기본적으로 구현 진행 - Masking,  torch.diag(), 양의 샘플을 앞에 두는 관례, label 만들기
InfoNCE 손실 함수를 적용, 이외에도 다른 손실 함수를 적용했을 때 어떤 차이가 발생하는지 생각하고, 관련 reference 참고해보기

-> 손실 함수 별 특징
	- superCon loss로 다시 구현해보기 
-> supervised 한 경우도 생각해보기
	-> 현재 모델은 자신의 짝이 하나만 존재하는 상황임 (Image에 대한 Augmentation 만 반영, class 살피지 않았음) - old self-supervised learning을 고려하는 상황임
	-> cosine_similarity에 대해서 고차원에는 어떻게 동작해야 하는지 (동일한 방식으로 동작함)
	-> 왜 unsqueeze() 하는지 더 생각해보기 - cosine_similarity() 기본으로 진행해야 함


-> 추가적으로 직접 모델 돌려보면서 어떻게 동작하는지
-> 관련 자료 찾아보기 (+CLIP, Rank-n-contrast)


##### code 구현시

- F.cosine_similarity() - cos similarity 계산할 때 사용하는 함수
	- Pair-wise 한 방식으로 유사도를 구한다
	- unsqueeze(1), unsqueeze(0) 
	- (x1, x2, dim=2)
- 벡터 간, 하나의 벡터 쌍에 대해서 유사도를 계산할 수 있다. 


- F.cross_entropy( pos + neg , label)
- class로 모델 구현시 __init... 함수의 경우 super 인자에 아무것도 명시하지 않아도 됨

- :.4f : 소수점 4자리까지 표시하기


