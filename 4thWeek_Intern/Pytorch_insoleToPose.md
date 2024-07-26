
#### DataLoader

- 기본 len 그리고 getitem 메서드르 구현해야 함
	- CustomDataset을 구축하고 싶다면 \_\_getitem__() 함수를 정의해야 한다
- dataset 객체를 입력으로 받아서 미니 배치로 나누어 제공해야 한다 
	- dataset
	- batch_size
	- shuffle
	- drop_last (True - 마지막 미니 배치가 batch_size 보다 작은경우에는 버리게 된다)
- 내부 동작은 인덱스를 생성하고 - 배치를 생성하고 - getitem() 메서드를 활용해서 데이터를 로드한다.

#### nn.BatchNorm1d(input_channel)

FC layer를 통과한 다음에 batch normalization을 통과하고 Activation 함수를 통과한다 - 이후에 dropout을 활용하는 것을 확인할 수 있다.


#### nn.Dropout(p=0.5)

드롭 아웃될 확률을 p로 전달한다.

![[Pasted image 20240723101752.png]]


- 뉴런의 비활성화
- 출력되는 뉴런의 scaling(0.5 -> 출력이 2배가 된다)
- 평가시에는 모든 뉴런이 활성된다. 추가적인 scaling은 필요하지 않다. 


![[Pasted image 20240723101914.png]]




---

### MLP ver

- input : 16 차원의 깔창 데이터
- output: 2차원 좌표 6개의 관절 데이터를 추출하게 된다



모델 간단히 설명하고


통계도 간단하게 설명하기 



### Q. BlazePose 

해당 모델 자체에서 output을 도출할 때 이미지 별 정규화를 이미 진행한 것으로 알고 있다. 근데 모델 내에서 또 다시 정규화를 진행하고 있어서, 이 과정을 제거했다. (RMSE 상으로 큰 차이는 없었음)

정규화에서 다시 정규화를 해야 하는 의미를 모르겠다 -> 정규화 하는 과정을 삭제했다 


---
