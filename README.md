# 2022_DM_Project

2조 김원웅, 강효준, 양정열, 오손빈
<br>
<br>
### 데이터 정보 
* X 변수 = 자치구별 특징
'세금 비율', '차량대수 비율', '인구(여)비율', '외국인 비율', '면적 비율',
'0대 비율', '10대 비율', '20대 비율', '30대 비율', '40대 비율', '50대 비율', '60대 비율',
'70대 비율', '80대 비율', '90대 비율', '100대 비율', '도소매 종사자 비율', '숙박 및 음식점 비율',
'도시가스 비율', '석유 비율', '공무원 1인당 담당인구', '관광지당 관광객', '흡연율'
<br>

* y변수 = '가정생활 폐기물 비율'
<br>

### 파일 정보
* garbage_data.csv.xlsx - 수집한 데이터와 그 데이터를 가지고 비율로 변환한 데이터들<br>
* trial_error.ipynb - 비율로 변환하기 전의 모델과 여러가지 시도<br>
* garbage_regression_code.ipynb - 최종 모델 <br>
* EDA.ipynb - 유의미한 변수로 선정한 x변수들의 시각화<br>
<br>
<br>

### 분석 목적 및 배경
[쓰레기 대란 예고 기사] <https://www.yna.co.kr/view/AKR20210719145400501> 

-> Regression을 이용해 가정생활폐기물 양에 많은 영향을 주는 요인을 분석하고 이를 줄이기 위한 방안 제시, 폐기물 양 예측 모델을 통해 다음해 양 예측, 줄이기 위한 제도 제안
<br>

### 데이터 수집

#### 선행 연구

정성영·배수호, 2018,  ‘지방공공서비스의 효율성 및 영향요인 분석: 생활페기물 처리서비스를 중심으로’, 정책분석평가학회보, 28(2), 55-92

현승현, 2018, ‘지방정부 생활폐기물 관리서비스의 효율성 변화 분석 - 경기도 기초자치단체를 중심으로’, 지방정부연구, 22(3), 91-113

##### 선행 연구 분석을 통한 가설 

* 지방세 납부액: **세금**을 많이 내는 지역에서 쓰레기 배출량이 많을 것
* 1인당 공무원 수: **1인당 공무원 수**가 많은 지역에서 쓰레기 배출량이 적을 것 (관리 가능)
* 구별 면적: **면적**이 넓은 지역에서 쓰레기 배출이 많을 것 (많은 유동인구, 회사, 처리시설)
* 인구 규모·밀도 1: **영유아**(기저귀), **여성**(화장품, 생리대), **외국인**(관광지 활성화)이 많은 지역은 쓰레기 배출량이 많을 것
* 인구 규모·밀도 2: **차량대수**, **도시가스 사용량**, **석유 사용량**이 높은 지역에서 쓰레기 배출량이 많을 것 (유동인구, 거주인구)

##### 선행 연구 이외의 추가 가설

* 관광지당 관광객: 지역별 **지당 관광객**많은 지역에서 쓰레기 배출량이 많을 것 (유동인구)
* 흡연율: **흡연율**이 높은 지역(담배꽁초)에서 쓰레기 배출량이 많을 것
* 도소매업 종사자 수: **도소매업 종사자**가 많은 지역에서 쓰레기 배출량이 많을 것
* 숙박 및 음식점 종사자 수: **숙박 및 음식점 종사자 수**(유동인구 및 거주 인구 많음)가 많은 지역에서 쓰레기 배출량이 많을 것

#### 데이터 수집
* [서울시 인구수 통계] <https://data.seoul.go.kr/dataList/419/S/2/datasetView.do> 
-> 서울시 구별 인구 합계, 한국인/외국인, 남자/여자 수 
* [서울시 흡연율] <https://data.seoul.go.kr/dataList/10668/S/2/datasetView.do>
-> 남녀 흡연비율 합계로 서울시 구별 흡연율 추출
* [서울시 주요 관광지점 입장객(자치구별) 통계] <https://data.seoul.go.kr/dataList/10668/S/2/datasetView.do>
-> 관광지별 관광객 수 추출, 관광지가 없는 구는 0으로 변수 설정
* [서울시 지방세 징수(구별) 통계] <https://data.seoul.go.kr/dataList/177/S/2/datasetView.do>
-> 서울시 구별 세금 총합
* [서울시 구별 차량 여부] <https://data.seoul.go.kr/dataList/255/S/2/datasetView.do>
-> 서울시 구별 차량 총 합계
* [서울시 구별 면적 및 행정동 개수] <https://data.seoul.go.kr/dataList/412/S/2/datasetView.do> 
-> 인구 데이터를 사용해 면적당 인구비율 추출
* [공무원 1인당 담당 인구] <https://data.seoul.go.kr/dataList/294/S/2/datasetView.do>
-> 서울시 구별 공무원 1인당 담당인구
* [서울시 주민등록인구 통계(나이별)] <https://data.seoul.go.kr/dataList/10718/S/2/datasetView.do>
-> 10세 단위로 나누어 연령대별 인구 추출
* [서울시 도시가스 이용현황] <https://data.seoul.go.kr/dataList/125/S/2/datasetView.do>
-> 도시가스 이용현황 중 가정용 합계 추출
* [서울시 석유류 소비량] <https://data.seoul.go.kr/dataList/10789/S/2/datasetView.do>
-> 구별 석유류 사용량 합계 추출
* [서울시 사업체 현황(산업대분류별/동별) 통계] <https://data.seoul.go.kr/dataList/104/S/2/datasetView.do>
-> 여러 산업체 중 도소매업/숙박 및 음식점업 선택
* [폐기물 배출량] <https://www.recycling-info.or.kr/rrs/stat/envStatList.do?menuNo=M13020201>
-> 가정생활 폐기물 발생량을 중점으로 2014년부터 2019년까지 y변수 추출

<br>

### 분석 과정

#### Trial and Error 1

* 각 feature를 인구 대비 비율로 설정하지 않고 원래 데이터 숫자대로 분석, feature 별 상관분석하지않음.
-> R-square 값이 너무 낮게 나옴
* k-means, DBSCAN 등의 clustering 진행
-> clustering에서 유의미한 분석 결과를 얻을 수 없다고 판단, clustering은 이후 진행하지 않음

#### Trial and Error 2

* PCA 후 Regression 진행
-> PCA 했을 경우 성능이 좋지 않고, 가설로 선정한 feature를 뺄 경우 가설 검증이 불가능하기 때문에 PCA로 feature를 빼는 건 옳지 않다고 판단, 이후 모델에서는 PCA를 사용하지 않음. (이후 상관성 분석을 이용해 불필요한 feature를 제외하는 방식으로 진행하였다)

#### Trial and Error 3

* PCA를 하지 않고 Regression에서 Ridge와 Lasso 둘 다 사용해보고 hyperparameter를 바꿔가며 다양한 시도 진행
-> 두 방법 모두 R-square 값이 높지 않음, 이후 분석에서는 모든 데이터셋의 숫자를 인구 대비 비율로 변환
<br>

### 분석 진행

#### Dataset

<img src=https://user-images.githubusercontent.com/100409757/170096094-ec0f46df-805d-473f-b436-232e96e27045.png width=600 height=400>


모든 데이터를 인구별 비율로 설정 -> 구별 인구 합은 feature에서 제외

#### EDA

<img src=https://user-images.githubusercontent.com/100409757/170095637-eb460b27-5d1a-4ad7-a247-a5462d94a183.png width=800 height=400>

#### Correlation을 이용한 Preprocessing

<img src=https://user-images.githubusercontent.com/100409757/170095889-4ada5155-f6a2-4f6d-ada3-d6dac678ed83.png width=600 height=600>
* x 변수 중 '숙박 및 음식점 비율'과 '도소매 종사자 비율'이 상관성이 높고 같은 의미를 공유하고 있다고 판단 -> 제거

* x 변수 중 '세금' 또한 '도소매 종사자'와 높은 상관성을 갖기 때문에 제거

####  Data split, scaling, modeling

* 2014년부터 2018년까지의 데이터를 train data로, 2019년의 데이터를 test data로 설정
* Standard Scaler를 이용해 data scaling
* Linear Regression을 이용해 Modeling

#### Model Performance

<img src=https://user-images.githubusercontent.com/100409757/170096734-1ed6431a-54da-47bc-8601-c08e1de762f9.png width=600 height=400>

#### 예측 모델 시각화

<img src=https://user-images.githubusercontent.com/100409757/170096787-1f8f5873-3c73-4f65-a673-de8b038e4694.png width=600 height=400>

####

<img src=https://user-images.githubusercontent.com/100409757/170096827-7bd16b21-d32e-4b6e-ad9a-41e637675719.png width=600 height=400>

* 전체 feature 분석에서 p-value가 0.05보다 낮은 동시에 vif가 10보다 높은 값 4개 선정
-> **'외국인 비율', '면적 비율', '도소매 종사자 비율', '석유 비율'**

#### 위에서 선택한 4가지 feature를 이용해 다시 모델링

<img src=https://user-images.githubusercontent.com/100409757/170097295-26c887f1-94ad-45e6-af37-cc22970e79de.png width=600 height=400>

#### 4가지 최종 feature를 활용한 모델의 performance 및 summary

<img src=https://user-images.githubusercontent.com/100409757/170097378-c1a6b4ba-baae-4ae4-9d4f-18f79573b8f1.png width=800 height=400>

<br>

### 분석 결과













<br>

### 기대 효과 및 한계점











