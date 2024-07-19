
The common approach is to produce heatmaps for each joint along
with refining offsets for each coordinate. 
-> 관절마다 히트맵 생성 - 오프셋을 정제하는게 일반적인 접근 방법

While this choice of heatmaps scales to multiple people with minimal over-
head, it makes the model for a single person considerably larger than is suitable for real-time inference on mobile phones. 

In this paper, we address this particular use case and demonstrate significant speedup of the model with little to no quality degradation.
-> 품질의 저하 없이 모델의 속도를 크게 높이는 방법을 제시합니다.

---

인체에서의 중요한 지점의 위치를 찾는다 (머리, 어깨, 팔꿈치, 무릎, 발목)

자세 추정의 형태 3가지

- skeleton-based : 주요 부위를 중심으로 찾기 때문에 신체 위치를 찾기에 유용합니다.
- contour-based : 신체의 모양을 알 수 있다 
- volume-based : 3D 자세 추정에 사용하는 모델이다


##### Top-down 방식 그리고 Bottom-up 방식

![[Pasted image 20240718164722.png]]

TD : 사람을 탐지하고 탐지 결과를 바탕으로 주요 부위를 찾아 자세 추정을 하는 방식
DT : 전체 이미지에서 인체 주요 부위들을 먼저 찾은 뒤, 각 부위를 연결해서 자세를 추정하는 방식 


![[Pasted image 20240718165321.png]]



#### 2D single-person pose estimation

> Regression methods and heatmap-based methods

2D single-person pose estimation is used to localize human body joint positions when the input is a single-person image
-> several people - input image is cropped first (this can be processed automatically by an upper-body detector or a full-body detector)

Regression methods apply an end-to-end framework to learn a mapping from
the input image to the positions of body joints or parameters of human body models [233]. 

The goal of heatmap-based methods is to predict approximate locations of body parts and joints which are supervised by heatmaps representation [231, 254]. Heatmap-based frameworks are now widely used in 2D HPE tasks. The general frameworks of 2D single-person HPE methods are depicted in Fig. 2

![[Pasted image 20240718171606.png]]

-> Regression - 운동학적 신체 모델로의 매핑을 직접 학습 - 관절 좌표 생성
-> 2D 자세에서 각 관절의 위치에 가우시안 커널을 적용하여 실제 히트맵 생성. 각 관절의 히트맵을 예측하기 위해 모델을 활용하게 된다. 

##### 2.1.1 Regression methods - predict joint coordinates from the images

- [ ] DeepPose - learning keypoints from images(Impressive performance)
- [ ] structure-aware regression method (compositional pose regression)
	- re-parameterized and bone-based representation (body information or pose structure)
- E2E regression for HPE using soft-argmax function to convert feature maps into joint coordinates in a fully diffe... framework.
	- Pose Recognition with Cascade Transformers
	- Human pose regression with residual log-likelihood estimation. In ICCV
	- Heterogeneous multi-task learning for human pose estimation 
	  with deep convolutional neural network. In CVPR Workshops.
	- Combining local appearance and holistic view:
		Dual-source deep neural networks for human pose estimation. In CVPR.


#### 2.1.2 Heatmap-based methods

