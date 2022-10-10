# 데이터 베이스에서 찾기
  <img width="50%" src="https://user-images.githubusercontent.com/81895293/194905933-918820d5-88dc-4f48-a43d-0d5533e87bc9.png"><img width="50%" src="https://user-images.githubusercontent.com/81895293/194905947-12c9736a-fa21-497d-bea2-ff1f37263cbe.png"/>


  ## 정형데이터
  실종장소, 실종 날짜, 견종을 입력하면 데이터베이스에서 조건에 가장 부합하는 유기견의 정보를 알려준다
  
  ## 비정형 데이터
  실종견의 이미지를 넣어주면 5가지 지표로 유사도가 가장 높은 객체의 정보를 알려준다
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

-------
# 지도 시각화
***QGIS 3.22.10***
1. 데이터 가공: 수집한 80개의 유기견 데이터 중, 구별 유기견 수 정리
  <img width="25%" src="https://user-images.githubusercontent.com/81895293/194907774-5367640e-b288-4645-ac15-c7f3395ce2d6.png">

2. [qgis 다운로드](https://qgis.org/ko/site/forusers/download.html)
<img width="10%" src="https://user-images.githubusercontent.com/81895293/194907417-8e72161e-746d-4f1d-963d-445ea347fbec.png">

3. 서울시 구별 경계 데이터를 삽입하고, 앞서 정리한 location_count.csv 파일과 join한 뒤, 단계구분도 작성
<img width="40%" src="https://user-images.githubusercontent.com/81895293/194907471-fcb42338-fea0-4946-b1e0-51e85d7948be.png">

4. 플러그인 qgis2leaf 설치
<img width="8%" src="https://user-images.githubusercontent.com/81895293/194908220-cb29f5c8-219a-4691-99cf-26b6434d43bb.png">

5. 설치 후, leaflet으로 설정한 후, export 클릭
