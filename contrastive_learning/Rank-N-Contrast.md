
that learns continuous representations for regression by contrasting samples against each other based on their rankings in the target space. We demonstrate, theoretically and empirically, that R N C guarantees the desired order of learned representations in accordance with the target orders, enjoying not only better performance but also significantly im- proved robustness, efficiency, and generalization.


이 프레임워크는 
목표 공간에서의 순위에 따라 샘플들을 서로 대비시켜 연속적인 표현을 학습합니다. 
-> contrasting samples against each other based on their rankings in the target space
우리는 이론적으로나 실험적으로 RNC가 목표 순서에 따라 학습된 표현의 원하는 순서를 보장하며, 더 나은 성능뿐만 아니라 크게 향상된 강건성, 효율성, 일반화를 제공함을 입증합니다
-> guarantees the desired order of learned representations in accordance with the target orders, enjoying not only better performance but also. ...

- ground-truth : 우리가 정한 정답, 우리의 모델이 우리가 원하는 답으로 예측해주길 바라는 답이다. 우리가 "답이 이렇게 나오길 원한다"로 생각하면 되겠다. 


-> exhibiting the continuous ground-truth temperatures, learned representations are grouped by different webcams in a fragmented manner. 
(단편화된 상태에서 서로 다른 웹캠 이미지로 그룹화되어 학습된 애들이다)
- 무질서, 무관련성은 성능을 저해하는 원인이라고 할 수 있다. (Unordered, fragmented representations)


목표 공간에서의 순위에 따라 샘플들을 서로 대비시켜 연속적인 표현을 학습했다. 


![[Pasted image 20240711134328.png]]


- uncover intrinsic properties of learnign regression-aware representations
- simple & effectvie method that learns continuous representations for regression.


A few recent papers propose to adapt SupCon to tackle
ordered labels in specific downstream applications, including gaze estimation [42], medical imaging
[9, 10], and neural behavior analysis [37]. Different from prior works that directly adapt SupCon, we
==incorporate the intrinsic property of label continuity for designing representation learning scheme tailored for regression, which offers a simple and principled approach for generic regression tasks.==


#### Approach - Rank N Contrast Loss

-> first ranks the samples according to their target distances, and then contrasts them against each other based on their relative rankings


#### ==Q. 수식에서 vi, vk 각각의 의미하는 바가 궁금

하나의 샘플이라고 생각하고, vi, j는 S i,j 즉 target_distance가 vi 보다 더 긴 경우를 의미한다.  


![[Pasted image 20240711143712.png]]

![[Pasted image 20240711145945.png]]

label 간의 관계를 학습하자는 뜻
Anchor를 기준으로 다른 대상을 하나 잡아둔다 - positive 하게 잡아두고
label distance 를 기준으로 negative pair를 고려하면 된다


#### Implementation

- Broadcasting 조건 알고 있기
	- 뒤쪽 차원에서 비교 - 차원의 크기가 같아야 하고 - 비교할 때 두 텐서의 크기가 같아야 한다
	- 차원의 크기 중 하나가 1이다 - 그래야 다른 텐서의 크기에 맞춰 확장한다
	- 차원의 수가 다른 경우에는 차원이 적은 쪽의 앞부분을 1로 채워서 맞춘다. 