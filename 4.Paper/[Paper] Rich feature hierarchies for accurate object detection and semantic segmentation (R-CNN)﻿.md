



## Rich feature hierarchies for accurate object detection and semantic segmentation (R-CNN)
### 2021-09-12 (일)

출처 : https://arxiv.org/abs/1311.2524



### 0. Intro



\- Obejct Detection : 이미지가 무엇인지 판단하는 Classification과 이미지 내의 물체의 위치 정보를 찾는 Localization을 수행하는 것을 말한다. 영상 내의 객체가 사람인지 동물인지 물건인지 등을 구별하여 각 객체가 어디에 위치하는지 표시하는 것이 가능하다.



\- R-CNN은 object detection 모델에서 2-stage detector의 대표적인 R-CNN계열의 시초가 되는 모델이며, R-CNN계열로는 R-CNN, fast R-CNN, faster R-CNN 등이 있다.



- - - 

### 1. Abstract

\- 지난 몇 년 동안 *PASCAL VOC* 데이터셋에서 Object Detection의 가장 좋은 성능을 내는 것은 high-level context의 복잡한 **앙상블 모델**이었다.



\- 이 논문에서는 VOC 2012 데이터를 기준으로 이전 모델에 비해 mean average precision(mAP)가 30%이상 향상된 더 간단하고 확장 가능한 detection 알고리즘을 소개하였다.



\- **detection 알고리즘의 2가지 인사이트** 



```javascript
1. 객체 localize 및 segment하기 위해 bottom-up(상방향)방식의 region proposal(지역 제안)에 
Convolutional Neural Network(CNN)를 적용

2. labeled data가 부족할 때, 성능 향상을 위해서 domain-specific fine-tuning과
보조작업(auxiliary task)으로 supervised pre-training 적용
```

- - -


### 2. Introduction

- 이 논문에서 R-CNN의 의미 : Regions with CNN features (CNN + Region Proposals 의 결합)



![img](https://blogfiles.pstatic.net/MjAyMTA5MTJfMTg5/MDAxNjMxNDQ1MTk1MzUx.9hq_JjmcXNX--o_ucJIWLU_rqVNVgeuaqwlYE6QGR5og.Fd0hW-410dQjif-MC2aB1ObluQRBBh7pDXmqkc9D5lYg.PNG.nm1lee/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2021-09-12_%EC%98%A4%ED%9B%84_8.13.09.png?type=w1)



[ **R-CNN의 프로세스 ]**



```javascript
1. 이미지를 입력한다. (Input Image)
2. 약 2000개(2k)의 독립적인 bottom-up region proposals를 추출한다.
3. CNN을 이용하여 각각 region proposal마다 고정된 길이의 feature vector를 추출한다.
  - 위 사진 중간의 'warped region'은 각각의 regioin proposal의 크기가 각각 다르므로 이를 CNN의 
  입력으로 넣어주기 위해서 크기를 맞춰주는 역할을 한다.
4. 각 region 마다 category-specific linear SVM을 적용하여 label을 classification(분류)한다. 
```

- - -

### 3. Object detection with R-CNN 

R-CNN은 위의 프로세스를 수행하기 위해서 3가지 모듈로 나뉜다.

```javascript
1. Region Proposals : detector가 이용가능한 영역 후보의 집합
2. CNN : 각각의 region에 대한 고정된 크기의 feature vector를 추출
3. Linear SVM : classification 진행
```

**1. Region Proposals**



**- 독립적인 region proposal을 생성**하기 위한 방법은 여러가지가 있다. 

\- 해당 논문에서는 이전 detection 작업들과 비교하기 위하여 **Selective Search**라는 최적의 region proposal 기법을 사용하였다. 



**[ selective search 프로세스 ]**



![img](https://blogfiles.pstatic.net/MjAyMTA5MTJfMjU2/MDAxNjMxNDQ2NDU5NTE4.9AtZjFQEs_K32f-w5uGypjZXLrqF0JoKd4G8ppChLq4g.VkJiv_2amYJwaf-GmURWYuF-ekDs8uikW07ktz6G6u0g.PNG.nm1lee/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2021-09-12_%EC%98%A4%ED%9B%84_8.34.15.png?type=w1)



```javascript
- bottom-up : 왼쪽 사진을 보면 하단부터 상향으로 유사한 영역끼리 결합하는 것을 확인할 수 있음 -

1. 이미지의 초기 세그먼트를 정하여, 수많은 region 영역을 생성한다.
2. greedy 알고리즘을 이용하여 각 region을 기준으로 주변의 유사한 영역을 결합한다.
3. 결합되어 커진 region을 최종 region proposal로 제안하기.
```

**2. CNN - Feature Extraction**

![img](https://blogfiles.pstatic.net/MjAyMTA5MTJfMTEy/MDAxNjMxNDQ2NjE1MDU0._MSWygDFmHwLGfYQWGKOEmPj_tOHfxUMyuUs6dknXJQg.P3-wokEnjHidbxemIjIC-nglNi4-P4L2EjrMSrpPFqkg.PNG.nm1lee/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2021-09-12_%EC%98%A4%ED%9B%84_8.36.49.png?type=w1)



```javascript
1. Selective Search를 통해 도출 된 각 region proposal로부터 CNN을 사용하여 4096차원의 
feature vector를 추출한다.

2. feature들은 5개의 convolutional layer와 2개의 fully connected layer로 전파되는데, 
이때 CNN의 입력으로 사용되기 위해 각 region은 227x227 RGB의 고정된 사이즈로 변환되게 된다.
```

**3. Linear SVM**



![img](https://blogfiles.pstatic.net/MjAyMTA5MTJfMjEy/MDAxNjMxNDQ3Njg0MDI1.IFclxan0WyQuWiSfk9we9d85xhYObZmlbm_1tTbkIlYg.88rxqfR5CdsKhuObruM8PNZ85xcqkfD4T93ry7aOyvgg.PNG.nm1lee/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2021-09-12_%EC%98%A4%ED%9B%84_8.54.40.png?type=w1)



- SVM 학습을 위한 라벨로서 IoU를 활용하였고 IoU 가 0.5이상인 것들을 positive 객체로 보고 나머지는 negative로 분류하여 학습하게 된다. 각 SGD iteration마다 32개의 positive window와 96개의 backgroud window 총 128개의 배치로 학습이 진행된다.


- 이후는 fine-tuning과 마찬가지로 positive sample 32개 + negative sample 96개 = 128개의 미니배치를 구성한 후 fine-tuning된 AlexNet에 입력하여 4096 dimensional feature vector를 추출한다. 



- 추출된 벡터를 이용해 linear SVMs를 학습한다. 

  \- SVM은 2진 분류를 수행하므로 분류하려는 객체의 종류만큼 SVM이 필요하다. 

  \- 학습이 한 차례 끝난 후, ***hard negative mining*** 기법을 적용하여 재학습을 수행한다.



- R-CNN에서는 단순히 N-way softmax layer를 통해 분류를 진행하지 않고, SVMs통해서 하게 된다.

  \- SVM을 사용했을 때 성능이 더 좋기 때문이다. 

  \- 성능 차이의 이유는 SVM을 학습시킬 때 positive sample를 더 엄밀하게 정의하며, SVM이 hard negative를 이용해 학습하기 때문이다.



- linear SVM에서는 output으로 class와 confidence score를 반환한다.






**4. Traning**

- 학습에 사용되는 CNN 모델의 경우 ILSVRC 2012 데이터 셋으로 미리 학습된 pre-trained CNN(AlexNet) 모델을 사용한다.

- Classification에 최적화된 CNN 모델을 VOC 데이터셋에 적용하기 위해, VOC의 region proposals을 통해서 SGD방식으로 CNN 파라미터를 업데이트 한다.

- CNN을 통해 나온 feature map은 SVM을 통해 classification 및 bounding regreesion이 진행되게 되는데, 여기서 SVM 학습을 위해 NMS(non-maximum suppresion)과 IoU(inter-section-over-union)이라는 개념이 활용된다.


\NMS(Non-maximum suppresion)
\1. 예측한 bounding box들의 예측 점수를 내림차순으로 정렬
\2. 높은 점수의 박스부터 시작하여 나머지 박스들 간의 IoU를 계산
\3. IoU값이 지정한 threhold보다 높은 박스를 제거
\4. 최적의 박스만 남을 떄까지 위 과정을 반복



- - -

### 4. Results on PASCAL VOC 2010-12



![img](https://blogfiles.pstatic.net/MjAyMTA5MTJfOTcg/MDAxNjMxNDQ2Njc0OTAw.hEavBq5x8WbOzAF2G05wyS3i8r6D406B4TbgWW49m28g.TuCyggKEO-Zi8kLEp_4DxBHoGfzyfXTSoD7wOsZcm5Mg.PNG.nm1lee/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2021-09-12_%EC%98%A4%ED%9B%84_8.37.50.png?type=w1)



\- 위 테이블은 VOC 2010 테스트 데이터에 대한 각 모델별 결과이다.

\- 맨 오른쪽에서 mAP를 확인할 수 있는데, 논문에서는 같은 region proposal 알고리즘을 적용한 UVA모델과 R-CNN의 각각 mAP의 결과를 비교한다.

\- 위 표를 보면 UVA 모델의 mAP는 **35.1%**이고, R-CNN의 mAP는 **53.7%**인 것을 확인할 수 있으며 이것은 높은 증가율이라고 저자는 말한다. 

\- 추가로, VOC 2011/12 데이터 셋 또한 **53.3% mAP로** 높은 성능을 나타냈다.




- - -


### 5. Problems

- R-CNN의 가장 큰 문제는 복잡한 프로세스로 인한 과도한 연산량

최근에는 고성능 GPU가 많이 보급 되었기 때문에 deep한 neural net이라도 GPU연산을 통해 빠른 처리가 가능하다. 하지만 R-CNN은 selective search 알고리즘를 통한 region proposal 작업 그리고 NMS 알고리즘 작업 등은 CPU 연산에 의해 이루어 지기 때문에 굉장히 많은 연산량 및 시간이 소모된다.



- SVM 예측 시 real-time 분석의 어려움

region에 대한 classification 및 bounding box에 대한 regression 작업이 함께 작동하다 보니 모델 예측 부분에서도 연산 및 시간이 많이 소모된다.



- 새로운 모델의 등장

R-CNN의 위와 같은 한계점들로 인해, 추후 프로세스 및 연산 측면에서 Fast R-CNN과 Faster R-CNN 모델이 나오게 된다.



![img](https://blogfiles.pstatic.net/MjAyMTA5MTJfMTYw/MDAxNjMxNDQ3NTk1MDc1.WnZfJzVWRavEJcG42tXgRR4uYlIfBYkEdkpamZAHA94g.AWnNXo_ZH4dgVPEleznTGlwthsoduuEHVtSfBT8vQ30g.PNG.nm1lee/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2021-09-12_%EC%98%A4%ED%9B%84_8.53.09.png?type=w1)

- Faster R-CNN이 이전 모델보다 비교가 안될 정도로 더 빠르다는 것을 알 수 있으며, 성능 또한 향상되었다고 한다. 



- - -

### 6. References

[1] R-CNN 논문(Rich feature hierarchies for accurate object detection and semantic segmentation)

[2] https://velog.io/@skhim520/R-CNN-논문-리뷰

[3] https://velog.io/@jaehyeong/R-CNNRegions-with-CNN-features-논문-리뷰



﻿
