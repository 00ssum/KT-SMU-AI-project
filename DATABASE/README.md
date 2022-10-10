# 데이터 베이스 설계 

### 데이터베이스 생성
1. [AWS 접속](https://aws.amazon.com/ko/?nc2=h_lg) 및 로그인
2. RDS 콘솔 클릭 후, 왼쪽 탐색 창에서 데이터베이스 클릭
3. 데이터베이스 생성 클릭 및, 엔진 옵션 MySQL로 설정 후, 데이터베이스 생성

-----
### 데이터베이스 연결
1. [mysql workbench 설치](https://dev.mysql.com/downloads/workbench/)
2. "+"클릭
<br><p align="center"><img width="70%" src="https://user-images.githubusercontent.com/81895293/194904251-269972a9-209b-44d6-8051-439c6c6f4590.png"><br>
3. connenction name-원하는 이름으로 설정
~~~
  -Hostname: database-2.cdv8ov56qedi.ap-northeast-2.rds.amazonaws.com 

  -Username: admin

  -Password-> store in vault,,,클릭-> sjw04050 입력

  -Ok 클릭
~~~
