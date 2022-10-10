# CASE1 

### 객체탐지
<br><p align="center"><img width="70%" src="https://user-images.githubusercontent.com/62383521/194908737-eb06af26-f556-4b54-ae3a-e457553db933.JPG"></br>
</br>

1. main.py 실행 시 DATA 폴더가 있는 경로에 있는 이미지를 INPUT으로 받음 

2. 객체탐지 후 정확도는 엑셀 파일에 기록 후, 개가 나와 있는 이미지만 Crop 하여 저장

-----
### 크기 추출 및 배경색 제거 
<br><p align="center"><img width="70%" src="https://user-images.githubusercontent.com/62383521/194909137-9d7c3e06-4a9c-4fdb-bd8c-eae5eccd4d38.JPG"></br>
</br>

1. OUTPUT 내 객체탐지가 이뤄진 이미지를 대상으로 배경색 제거를 실행 

-----

### 모색 클러스터링


-----

### 견종 분류
<br><p align="center"><img width="70%" src="https://user-images.githubusercontent.com/62383521/194910143-23e73c15-c340-4e73-bba5-802f7478318f.JPG"></br>
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

