# Project
### 실무 시계열 진동 데이터를 이용한 불량품 예측 모델 개발

# Intro 
### 📚 Course
기계학습론(ROB3006-22690) </br>
### 🗓️ Date 
Project term : 2023.11.17 ~ 2023.12.13 </br>
### :man: Professor 
  한양대학교 ERICA, 로봇공학과 윤종완 교수님 
### 👥 Team member 
  * 산업경영공학과 김윤성
  * 로봇공학과 김민표
  * 로봇공학과 최호재
  * 컴퓨터학과 설혁정

## 1.  Introduction
A. 문제 상황

K 로보틱스 회사의 스테이지 로봇 연구 및 개발 팀 팀장으로서, 현재로서 회사들은 로봇 조인트의 주요 구성 요소인 다이나믹셀(Dynamixel)의 상태를 정확하게 진단할 수 있는 기술이 필요하다. 로봇이 무대에 올라가기 전에 로봇 상태를 사전 점검하여 사고를 예방하기 위해 계속해서 다이나믹셀 작동 데이터를 양품과 불량품에 대해 수집하고 있다. 이 데이터를 사용하여 머신러닝과 딥러닝을 통해 인공지능 모델을 개발하고 정확한 성능 분석을 통해 모델을 평가해보자.


B. 데이터 전처리 – jupyter notebook환경에서 진행
- glob 라이브러리를 이용해 폴더에 있는 모든 csv 파일 업로드 후 하나의 csv 파일을 각각 중복 정도에 맞춰 분할
- 데이터셋을 기술 통계량 데이터셋과 fft의 비율을 조정한 데이터셋들로 전처리함
  
제공된 데이터는 양품 모터 40개와 불량품 모터 40개의 시간에 따른 진폭 값을 (1024*2) 크기의 csv 파일로 편집하였다. 이때 양품과 불량품 모두1~30번 데이터를 Train set으로 31~40번 데이터를 Test set으로 설정하여 학습을 진행시킨다. 데이터를 추출하는 방법은 중복의 정도에 따라 총 3가지로 나뉜다. 1024행을 256개의 행으로 분할할 떄 중복을 0%,25%,50%로 설정하고 진행한다.
  (1) 중복 0% : 1024개의 행이 256 *4 로 총 4개의 데이터 셋으로 분할
  (2) 중복 25% : 1024개의 행이 256개씩 나눌 때 25% 즉 64개씩 겹치게 추출하여 총 5개의 데이터 셋으로 분할
  (3) 중복 25% : 1024개의 행이 256개씩 나눌 때 50% 즉 128개씩 겹치게 추출하여 총 7개의 데이터 셋으로 분할


C. 통계 변수 데이터 vs fft 변환 변수 데이터를 실험을 통해 비교 분석
- 통계 변수는 min, max, mean, abs mean, skew, outlier counts, std, abs std, median, abs median, kurt, moving average를 사용
- 통계 변수 데이터: 통계 변수들을 전체 다 쓴 데이터셋과 상관관계가 높은 변수들을 feature selection한 데이터셋과 결정나무기반 모델의 feature importance로 상위 feature만 사용한 데이터셋을 비교함
- fft 변환 변수 데이터: 중복 비율을 0, 25, 50%의 데이터셋으로 나누고 비교함
- 상위 데이터셋을 비교한 결과 fft 변환 변수 데이터들이 통계 변수 데이터보다 좋은 성능을 보였으며 그 중에서도 0% 중복 비율을 보인 데이터가 가장 성능이 좋게 나옴

D. 모델 비교
- 머신러닝 기법 중 SVM, 로지스틱 회귀, 랜덤 포레스트 모델을 사용하고 딥러닝 ANN의 하이퍼 파라미터를 다양하게 설정하여 비교한 후 최적의 모델을 선정함
- 딥러닝 모델의 노드 수, dropout 값, 은닉층 활성화 함수, 출력층 활성화 함수, 손실 함수, 최적화 함수, epochs, batch size 하이퍼파라미터를 조절하여 모델들을 비교
- 머신러닝 기법들은 GridSearchCV를 사용하여 하이퍼파라미터를 설정함

## 2.	Results 
- 평가지표: AUC-Score
- 최적 모델: 딥러닝 모델
- AUC-Score: 0.9632

## 3. References
- A Study of Vibration and Current Data Characteristic Analysis for Motor Mechanical Fault Level Determination by Deep Learning(호서대학교)
- Condition Monitoring and Fault Diagnosis of Electrical Motors(S. Nandi)
