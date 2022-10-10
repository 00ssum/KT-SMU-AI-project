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
