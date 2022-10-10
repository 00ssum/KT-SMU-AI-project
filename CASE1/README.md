# CASE1

## 먼저 Detection 폴더에 있는 압축파일들을 압축해제를 먼저 해야합니다.

### 객체탐지
<br><p align="center"><img width="70%" src="https://user-images.githubusercontent.com/79439483/194917802-c84af394-095e-4390-9e4a-c28fcbeaf743.jpg"></br>
</br>
<br><p align="center"><img width="70%" src="https://user-images.githubusercontent.com/79439483/194918489-2de7e29a-31d8-40bd-9b4a-b881f64b9ba4.jpg"></br>
</br>
1. main.py 실행 시 DATA 폴더가 있는 경로에 있는 이미지를 INPUT으로 받음 

2. 객체탐지 후 정확도는 엑셀 파일에 기록 후, 개가 나와 있는 이미지만 Crop 하여 저장

-----
### 크기 추출 및 배경색 제거 
<br><p align="center"><img width="70%" src="https://user-images.githubusercontent.com/79439483/194922172-9f2dab0f-7b1d-4a23-94aa-0bdb42ca8d55.jpg"></br>
</br>

1. OUTPUT 내 객체탐지가 이뤄진 이미지를 대상으로 배경색 제거를 실행 

-----

### 모색 클러스터링
<br><p align="center"><img width="70%" src="https://user-images.githubusercontent.com/79439483/194923075-42e40e95-b3ca-4f85-9fb7-9ed2948cd821.jpg"></br>
</br>

1. K-MEANS 알고리즘 이용. k=5 일 경우 가장 클러스터링이 잘 되어, k=5로 선택
2. 같은 모색끼리 군집화 해주는 알고리즘

출처
https://moding.tistory.com/entry/Project-6-dataset%EC%9D%84-matrix-%ED%98%95%ED%83%9C%EB%A1%9C-%EA%B5%AC%EC%84%B1%ED%95%B4%EB%B3%B4%EA%B8%B0
-----

### 견종 분류
<br><p align="center"><img width="70%" src="https://user-images.githubusercontent.com/79439483/194923804-7d3928c8-447a-405c-b52a-8c7a1cba5af8.jpg"></br>
</br>

1.Efficient Net으로 견종 분류

2.OUTPUT 크기가 추출되고 배경색이 제거된 이미지를 대상으로 견종 분류 실행

3.TOP3 안에 드는 견종 데이터가 엑셀에 %와 함께 저장

-----
### 데이터베이스 업로드
<br><p align="center"><img width="70%" src="https://user-images.githubusercontent.com/62383521/194911859-c9457dab-ba27-4f9e-88ba-78e6520e92a7.JPG"></br>
</br>
~~~
conn = pymysql.connect(
    host='database-2.cdv8ov56qedi.ap-northeast-2.rds.amazonaws.com',
    user='admin',
    password='sjw04050',
    db='KTDB',
    charset='utf8')

curs=conn.cursor()

sql = """
    INSERT INTO puppy (imagenum,filename,date,location,breed,shelter,width,height,accuracy) values(%s, %s, %s,%s,%s,%s,%s,%s,%s)
"""

f=open('output.xlsx','r',encoding='utf-8')
rd=csv.reader(f)

for line in rd:
     curs.execute(sql,(line[0],line[1],line[2],line[3],line[4],line[5],line[6],line[7],line[8],line[9],line[10]))

conn.commit()
conn.close()
f.close()

s3 = s3.s3_create()

for i in file_list:   
    s3.upload(file_path/+file_list,file_list)

~~~
1.OUTPUT에 최종 엑셀파일이 저장되고, 엑셀 내용이 AWS의 DB에 저장된다.

2.최종 이미지는 S3를 통해 업로드 된다.

