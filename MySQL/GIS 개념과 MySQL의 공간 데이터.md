# 지리정보시스템(GIS) 

지도와 그에 따른 텍스트(속성) 정보를 컴퓨터에 입력한 후에, 그 입력된 전자지도를 교통, 환경, 농업, 생활, 시설물 관리 등의 다양한 곳에 응용하는 분야

ex) 구글맵, 카카오맵, 자동차 내비게이션(공간 데이터 + GPS), 구글 어스 등

구글 어스 : 지형에 대한 인공위성 영상, 각 주요 건물에 대한 사진 정보, 해당 위치에 대한 지명 정보 등을 모두 함께 관리하는 통합된 GIS의 사례
https://www.google.co.kr/intl/ko/earth/

---
### Q) 다양한 GIS를 구축하기 위해 필요한 것?
속성 데이터(텍스트 기반 데이터),  공간 데이터(지도에 표현되는 데이터) 

![](https://images.velog.io/images/hyojeong_sss/post/21a919f0-49ed-4362-9c8f-4d1b4e1ac339/image.png)

과거 :  공간 데이터(지도)는 따로 파일로 가져오고 DBMS에서 테이블의 속성을 가져와서 2개를 조합해서 사용하는 기법 이용
- 데이터의 이중 관리 문제  (지도 데이터 따로, 속성 데이터 따로) 
- 지도 데이터까지 mysql 데이터베이스 안에서 사용하고 싶다!!
-> mysql 5.0부터 지도 데이터를 저장할 수 있는 데이터 타입을 지원 (geometry)

---

## 공간 데이터 
지구상에 존재하는 지형정보를 표현한 데이터 (강, 도로, 나무 등 모든 표현물을 디지털 지도 상에 옮겨 놓은 데이터)  *"디지털 지도 = 수치지도"*

### 공간 데이터 구성
점, 선, 면  3개의 개체로서 표현

![](https://images.velog.io/images/hyojeong_sss/post/76dd9ec3-b868-407c-a36e-ccaf8663e342/image.png)

---

### mysql에서 공간 데이터의 저장 
![](https://images.velog.io/images/hyojeong_sss/post/049b686a-2e17-45d6-9cb4-823f635484eb/image.png)

geometry 타입 : 지도가 파일 단위로 통째로 들어가는 게 아니라, 지도 안에 있는 점, 선, 면이 각각 저장

![](https://images.velog.io/images/hyojeong_sss/post/3275d4f3-5f8c-4e55-a5c8-baaff872a1cd/image.png)

강, 건물 각각이 공간 데이터로 각각 들어감. DB안에서 조회하고 처리 가능

Q) 우리나라에서 가장 큰 건물? 
A) 건물 크기 알려주는 공간 쿼리 사용, 실제 고급 어플리케이션 없이도 mysql안에서 처리 가능

---

```sql
CREATE TABLE StreamTbl (
	MapNumber CHAR(10), 	-- 지도 일련번호
	StreamName CHAR(20), 	-- 하천이름
	Stream GEOMETRY );		-- 공간 데이터 (하천 개체)
INSERT INTO StreamTbl VALUES ( '330000001', '한류천', ST_GeomFromText('LINESTRING (-10 30, -50 70, 50 70)', 0));
```
하천 테이블 생성 및 하천 데이터 추가
mysql 5.0에서 지원하는 공간 함수 ST_GeomFromText() 사용하여 공간 데이터 입력
<br>


```sql
SELECT 하천이름 FROM 하천테이블 WHERE 하천길이 > 10KM
```
mysql에서 공간 데이터 개체가 데이터 형식으로 저장됨으로써 공간 쿼리 사용 가능