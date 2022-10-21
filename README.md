# 한국은행의 기준금리 결정 예측

## 개발 동기
* 한국은행은 물가 안정 등의 목적으로 기준금리를 결정한다.
* 한국은행의 기준금리 인하/인상/동결 결정은 경제주체의 행동에 큰 영향을 준다.
* 한국은행의 기준금리 변동을 예측할 필요성이 존재한다.

## 데이터 설정 및 배경

<img width="80%" src="https://user-images.githubusercontent.com/106813090/197179994-10e7cb62-b017-4c43-a29e-808e981a1161.png"/>


## 데이터 전처리
* 설명변수 데이터 특성 차이 존재 : 데이터 전월 대비 증가율 변환
* 설명변수 데이터 단위 차이 존재 : MinMaxScaler 변환
* 종속변수 데이터 수 불균형 : SMOTE 오버 샘플링으로 데이터 추가
  
## 분석

### 모형 1 : Multi classfication regression model
* Keras : softmax function, cross entropy cost function 활용
* Y 설정 : 0 - 기준금리 동결, 1 - 기준금리 인상, 2 - 기준금리 인하

#### 분석 결과
<img width="80%" src="https://user-images.githubusercontent.com/106813090/197180038-52a8523f-d4a9-41ed-b523-5c8b60b70d80.png"/>

* Cost 약 0.80 수준에 수렴
* Train_data Report, Test_data Report 간 차이 미미, 과적합 가능성 적음
* Test_data에서 기준금리 인상/인하/동결 순으로 예측력 높음
* Accuracy Train_data, Test_data 모두 0.69 수준
* Train_data, Test_data 모두 기준금리 동결에 대한 예측력 낮음


### 모형 2 : Deep Neural Network classfication model
* Keras : Sequential, categorical_crossentropy 활용
* Y 설정 : 0 - 기준금리 동결, 1 - 기준금리 인상, 2 - 기준금리 인하

#### 분석 결과

<img width="80%" src="https://user-images.githubusercontent.com/106813090/197180066-be528c33-cf12-4c43-be00-d845ec9c4c8b.png"/>

- Cost 약 0.80 부근에서 진동하나 0.8로 수렴하는 경향.
- Train_data Report, Test_data Report 간 차이 미미, loss, val_loss function 그래프 비교 시 과적합 가능
성 적음. 
- Accuracy Train_data, Test_data는 각각 0.83, 0.76으로 모형 1에 비해 예측력이 월등히 높음

## 한계 및 배운 점

### 1. 데이터 수집
* 기준금리 예측 시 필요한 x_data는 일정 시간이 지난 후 작성 및 발표 된다.
* 따라서 한국은행의 기준금리 결정이 발표될 시점에 예측에 필요한 데이터가 불충분한 상황이 발생한다. 즉, 현재 모델은 실효성이 떨어진다.
* 데이터에 접근 및 활용 가능성의 여부가 프로그램의 효율성에 큰 영향을 미치는 것을 깨달았다.

### 2. 데이터 처리
* Y 변수로 설정한 기줌금리는 월 데이터(연 12회)를 사용했으나 실제 한국은행의 기준금리는 연 8회 금융통화위원회에서 결정한다. 이러한 차이로 기준금리 동결(Y=0) 데이터가 과생성되었고, 이것이 Y=0에 대한 저조한 예측력 혹은 과적합의 원인 중 하나인 것으로 판단할 수 있다.
* 한국은행의 통화정책은 기준금리 결정 외에도 다양한 정책이 존재한다. 이러한 정책이 거시경제변수에 영향을 미철 것으로 예상된다. 그러나 이를 분석에 반영하지 못하였다.
* 종속변수 데이터 수 불균형 문제를 SMOTE로 해결하였다. 정확한 예측을 위해서는 다른 방법을 고려하는 것이 타당할 것이다.
* 정확한 데이터 분석을 위해서는 보다 정교한 작업이 필수적임을 깨달았다.
