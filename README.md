# Ani -Time 
**2022 KT와 함께하는 상명 AI 경진대회**은 AI를 활용해 객체검출, 견종분류, 배경제거, 크기추출, 모색 구분등을 통해 유기견의 이미지를 하나의 통합 된 데이터베이스로 구축한다. 

이미지, 견종, 위치등의 장소가 입력되면 데이터베이스에서 AI를 활용해 유사도가 가장 높은 객체 정보를 출력한다. 
<br><img width="50%" src="https://user-images.githubusercontent.com/81895293/194898621-681931f0-03b5-4580-a2e4-b0d1dcce5f47.png"><img width="50%" src="https://user-images.githubusercontent.com/81895293/194898934-d587520b-28e9-4944-8b96-0049fb736f87.png"/>
  <br>

## 시나리오
<br><p align="center"><img width="70%" src="https://user-images.githubusercontent.com/81895293/194900610-cc90ed38-60c4-42f4-a014-0af54589b111.png"><br>
----
## 구현 기능
### 0. 데이터 셋 구축
- 출처: 동물 보호 관리 시스템
- 기간: 2022.03.02 022.08.12
- 수집 내용
    1. 정형데이터: 날짜, 장소, 견종, 보호소 정보
    2. 비정형 데이터: 유기견 이미지

### 1. 객체 탐지

- 제보 받은 영상은 차, 사람 등 다양한 객체가 공존하기 때문에 유기견만 탐지해내는 과정이 필요함
- 사용한 모델: YOLOv5
- 학습 데이터: [25,000장의 고양이와 개의 이미지](https://www.kaggle.com/competitions/dog-breed-identification/data)
  
> input: 제보 받은 영상
> output: 영상 내 객체의 종류를 박스로 분류해낸 영상
  <br><img width="40%" src="https://user-images.githubusercontent.com/109662803/194903955-1ed45367-ac82-4cf7-b204-e6de762cb401.png"><img width="40%" src="https://user-images.githubusercontent.com/109662803/194904048-d1b7b4b0-53bb-40fa-9004-7644ac6af35b.png"/>
  <br>
  
### 2. 배경 제거

- 유기견의 정확한 특징 추출을 위해 배경을 제거함
- 사용한 모델: cv2.Canny() - 물체 외곽선 추출 / cv2.grapCut() - 배경 제거
> input: 객체 탐지 된 이미지
> output: 크기가 추출된 이미지
    <br><img width="40%" src="https://user-images.githubusercontent.com/109662803/194904048-d1b7b4b0-53bb-40fa-9004-7644ac6af35b.png"><img width="20%" src="https://user-images.githubusercontent.com/81895293/194910990-d502ce8e-9d97-4e81-a93f-305433a006f1.png"/>
  <br>

### 3. 견종 구분

- 이미지 내의 유기견의 견종을 구분해내는 과정
- 사용한 모델: EfficientNet
- 학습 데이터: [20,581장의 이미지](https://www.kaggle.com/competitions/dog-breed-identification/data)
> input: 배경이 제거된 이미지
> output: 가장 유사한 상위 3가지 종 추출
      <br><img width="20%" src="https://user-images.githubusercontent.com/81895293/194911705-79c2a362-0a8c-46e2-96cc-1c6ec1e7ee42.png"><img width="30%" src="https://user-images.githubusercontent.com/81895293/194911723-8c3c146b-395a-48d5-b513-2c37956c83c5.png"/>
  <br>

### 4. 모색 구분

- 유기견의 모색을 구분해내는 과정으로, 밝기에 영향을 받지 않는 HSV 색상 채널을 사용
- 사용한 모델: K-means
> input: 배경이 제거된 이미지
> output: 가장 유사한 상위 5가지의 색상과 비율 벡터를 추출
  ![image](https://user-images.githubusercontent.com/81895293/194911888-a07a8b8d-77f0-4d06-b15a-9dc2f933e27d.png)

  
### 5. 데이터베이스 구축

- 시스템의 결과를 통합된 데이터베이스로 구축
- MySQL: 정형 데이터 업로드 
<br><p align="center"><img width="70%" src="https://user-images.githubusercontent.com/81895293/194912073-7fa12e6b-2f1b-4f26-a86a-3eb78d3c1ac5.png"><br>
- AWS 기반 S3: 비정형 데이터 업로드 
<br><p align="center"><img width="70%" src="https://user-images.githubusercontent.com/81895293/194912106-6c8d999d-9b5a-4b02-a91c-0d03530c36ed.png"><br>
  
### 6. 유사도 분석
  7가지 지표로 유사도 분석
  ~~~
  # 0에 가까울 수록 일치도가 높다
CORREL : 상관 분석
INTERSECT : 교집합

# 1에 가까울 수록 일치도가 높다
CHISQR: 카이 제곱 검정
BHATTACHARYYA: 바차타야 거리
EMD: EMD

ret = cv2.compareHist(query, histogram, index) 
  ~~~
  
### 7. 지도 시각화
  - 구별 유기견 현황을 파악하기 위해 지도 상에 시각화
  - 사용한 프로그램: QGIS
  - input: 수집한 데이터의 구별 유기견 수를 정리한 csv
  - output: 웹지도 형태의 구별 단계 구분도
<br><p align="center"><img width="70%" src="https://user-images.githubusercontent.com/81895293/194912305-7aeac841-248c-4da4-9abc-8b2cde11673b.png"><br>
  
-----
## 기대효과 & 발전 방향
<br><img width="50%" src="https://user-images.githubusercontent.com/81895293/194903368-f267cd14-4397-4457-8c7b-e50c3c402729.png"><img width="50%" src="https://user-images.githubusercontent.com/81895293/194903084-847aa845-ac01-45d1-a797-900f24d8b37e.png"/>
  <br>

## 라이센스
위 프로젝트는 KT와 함께하는 상명 AI 경진대회 출품작입니다.
