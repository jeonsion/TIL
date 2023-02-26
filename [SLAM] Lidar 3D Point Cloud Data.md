# [SLAM] Lidar 3D Point Cloud Data

### What is 3D Point Cloud?

:`라이다 센서`를 통해 수집한, 위치정보를 가진 수많은 점들의 집합. 그렇게 공간 정보를 가진 점들의 집합을 `Point Cloud`라고 한다.

Usage : 자율주행차량,  로봇의 이동을 위한 학습 데이터셋에 주로 사용된다.

Purpose : 특정 장소에서, 얼만큼 떨어진 곳에, 어떤 크기의 물체가 있는지를 파악하는 목적

### Point Cloud Data

Point cloud Data는 정형화되어 있지 않은 데이터. 

- 2D 이미지 데이터의 경우 정해진 격자 구조의 형태 안에 픽셀 형식으로 정보가 저장
- Point Cloud 데이터는 3D 공간상의 수 많은 점들을 순서없이 기록하는 방식 → 딥러닝 모델에 있어서는 상당히 치명적이다. 객체의 형상, 점들 간의 상호작용 등 데이터가 가지고 있는 기하학적인 특서을 파악하기 어려움

### Solution

Point Cloud Data를 `Voxel` 형태의 정형화된 데이터로 `전처리`(Preprocessing)하는 방법이 존재

→ 하지만 위 방법 역시 `Sparse`한 성질을 지닌 Point Cloud의 특성으로 주어진 3D공간 안에 빈공간 상당히 많음

## Point Cloud Deep Learning Model

Point Cloud 데이터를 기반으로 좋은 성과를 보인 대표적인 `딥러닝 모델`

![Untitled](%5BSLAM%5D%20Lidar%203D%20Point%20Cloud%20Data%203305f35dbd1345dd8902db94141056ad/Untitled.png)

### 1) PointNet

![Untitled](%5BSLAM%5D%20Lidar%203D%20Point%20Cloud%20Data%203305f35dbd1345dd8902db94141056ad/Untitled%201.png)

- **PointNet**은 `데이터 형태의 변환 없이` Point Cloud `데이터를 그대로 입력`하여 학습하는 모델
- 비정형 구조의 데이터를 `Max Pooling`, `Spatial Transformer Network`와 같은 데이터의 기하학적 성질을 유지할 수 있는 딥러닝 네트워크를 소개하며 PCD 데이터를 직접적으로 해석하는데 성공한 모델
- 초기 PointNet 모델은 데이터 Classification, Segmentation 수행가능

### 2) VoxelNet

![Untitled](%5BSLAM%5D%20Lidar%203D%20Point%20Cloud%20Data%203305f35dbd1345dd8902db94141056ad/Untitled%202.png)

> 먼저 Voxel이란 Volume + Pixel의 줄임말로 부피를 가진 픽셀을 의미한다.
> 
> 
> ![Untitled](%5BSLAM%5D%20Lidar%203D%20Point%20Cloud%20Data%203305f35dbd1345dd8902db94141056ad/Untitled%203.png)
> 

- **VoxelNet**은 Point Cloud 데이터로부터 `Voxel Feature`를 추출한 다음 이를 해석해 물체를 검출하는 `3D Object Detection` 모델이다.
- Voxel Feature는 데이터의 3차원 공간을 `Voxel` 단위로 나눈 후 각 Voxel 안의 점들을 `Voxel Feature Encoding Layer`(VFE)라는 딥러닝 네트워크를 거쳐 Feature Map을 얻어낸 것이다.

### 3) SECOND

![Untitled](%5BSLAM%5D%20Lidar%203D%20Point%20Cloud%20Data%203305f35dbd1345dd8902db94141056ad/Untitled%204.png)

- VoxelNet과 기본적인 구조는 동일하지만 `Convolution Middle Layer`를 `CNN` 대신 `Sparse Conv Layer`라는 레이어를 사용하는 것이 특징이다.
- SPCONV는 반복되는 패턴에 대해 Rule을 설정하는 방식을 사용해 기존의 CNN에서 요구되는 계산량을 대폭 낮췄다.

### 4) PointPillars

![Untitled](%5BSLAM%5D%20Lidar%203D%20Point%20Cloud%20Data%203305f35dbd1345dd8902db94141056ad/Untitled%205.png)

- PointPillars는 Pillar라는 새로운 형태의 `Point Cloud Encoder`를 사용해 Point Cloud 데이터로부터 `격자 형태`의 Feature Map을 얻은 다음, 이를 해석해 물체를 검출하는 3D Object Detection 모델.
- 얻어낸 Feature Map은 `이미지 데이터와 동일하게` `2D CNN`을 통한 데이터 해석이 가능해지므로 처리 속도가 상당히 빠르다.

### reference

[https://arxiv.org/abs/1612.00593](https://arxiv.org/abs/1612.00593)

[https://www.mdpi.com/1424-8220/18/10/3337](https://www.mdpi.com/1424-8220/18/10/3337)

[https://arxiv.org/pdf/1812.05784.pdf](https://arxiv.org/pdf/1812.05784.pdf)

[https://arxiv.org/abs/1711.06396](https://arxiv.org/abs/1711.06396)