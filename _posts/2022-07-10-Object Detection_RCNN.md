# Part5.(Object Detection_RCNN)

### RCNN

1. Region Proposal : 카테고리와 무관하게 물체의 영역을 찾는 모듈
2. CNN : 각각의 영역으로부터 고정된 크기의 Feature Vector를 생성
3. Classification : 분류를 위한 선형 지도학습 모델 SVM
- Selective Search : 객체와 주변간의 색감(Color), 질감(Texture) 차이, Enclosed여부를 파악해서 물체의 위치 파악

→ Bounding Box들을 random하게 많이 생성하고 조금씩 Merge하며 물체 인식

- Warping Image

![](../images/2022-07-10-Object%20Detection_RCNN/58be508293d5e0eb25e3ed381f433f8a9245df6a.png)

- CNN : Warping 된 Image를 CNN에 넣어줌, AlexNet을 Detection class에맞게 변형한 구조를 이용
- Classification : SVM을 이용해서 분류
- Bounding Box Regression : x, y(좌표위치), w(너비), h(높이) P는 선택된 BoundingBox이고 G는 Ground Truth BoundingBox이다.

![Untitled 1.png](../images/2022-07-10-Object%20Detection_RCNN/410f659242aa58e0b474664637f4435178a66113.png)

[Bounding box regression](https://better-tomorrow.tistory.com/entry/Bounding-box-regression)

단점

1. 오래걸린다 : Training Time 84시간
2. 복잡하다 : CNN, SVM, BoundingBox Regressiong총 세가지 모델을 필요로하는 복잡한 구조
3. Back Propagation이 안된다. : CNN을 업데이트 시키지 못한다.

### Fast RCNN, Faster RCNN

![Untitled 2.png](../images/2022-07-10-Object%20Detection_RCNN/6d0f914bbd850905b19f7d7fde7dc64f0570a296.png)

### Fast RCNN

![Untitled 3.png](../images/2022-07-10-Object%20Detection_RCNN/3bee2c37d189e32898caa587ec0bde3d655285c1.png)

Unified Framework

1. Feature Extractor
2. Classifier
3. Regressor

Selective Search ← 너무 오래걸림

Spatial Pyramid Pooling : Warping에서 일어나는 정보손실을 막으며, 이미지의 차원을 맞추기 위한 방법 일정 개수의 지역으로 나눈 뒤, 각 지역에 BoW(Bags-of-words)를 적용

Region of Interest Pooling : feature map에서 region proposals에 해당하는 Region of Interest를 지정한 크기의 grid로 나눈 후 max pooling을 수행하는 방법 channel별 독립 수행하며, 고정된 크기의 feature map을 출력하는 것이 가능

SPP/RoI pooling사용

![Untitled 4.png](../images/2022-07-10-Object%20Detection_RCNN/821017eb7fc86ab647365e3bd23c3098b1673a36.png)

### Faster RCNN

Region proposal network(RPN)를 학습해보자

![Untitled 5.png](../images/2022-07-10-Object%20Detection_RCNN/b22d4d7acdc97aaf4d94d920f1f3c78bdce38578.png)

Region Proposal Network

![Untitled 6.png](../images/2022-07-10-Object%20Detection_RCNN/fab9578518c39cd43bec3b14713396c2cfe11925.png)

object의 크기와 비율을 모르므로 k개의 anchor box를 미리 정의했다. (128, 256, 512) / 3가지 비율 (2:1, 1:1, 1:2) 총 9개의 anchor box를 사용했다.

![Untitled 7.png](../images/2022-07-10-Object%20Detection_RCNN/af8f4dc2fc0f8f1ccc7315b356919cc9a981d2e1.png)

9개의 anchor box를 이용하여 classification과 bounding box regression을 구한다.