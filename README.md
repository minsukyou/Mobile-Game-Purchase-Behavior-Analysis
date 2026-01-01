# 📊 데이터 사이언스 프로젝트  
## 모바일 게임 사용자 구매 행동 분석  
**Mobile Game In-App Purchases Dataset**

---

## 1. 프로젝트 개요

본 프로젝트는 **모바일 게임 사용자 행동 데이터를 분석하여 인앱 구매(In-App Purchase)에 영향을 미치는 요인을 도출하고**,  
이를 기반으로 **구매 가능성이 높은 잠재 고객을 식별하는 데이터 기반 의사결정 시나리오를 제안**하는 것을 목표로 합니다.

- **문제 정의**
  - 사용자 구매 여부를 예측하는 **이진 분류(Binary Classification)** 문제
- **활용 데이터**
  - Mobile Game In-App Purchases Dataset (2025)

---

## 2. 데이터 설명

- **총 사용자 수**: 3,024명
- **주요 변수**
  - 인구통계: `Age`, `Gender`, `Country`
  - 디바이스 / 게임 정보: `Device`, `GameGenre`
  - 행동 데이터: `SessionCount`, `AverageSessionLength`
  - 결제 정보: `InAppPurchaseAmount`, `FirstPurchaseDaysAfterInstall`, `SpendingSegment`

### 타겟 변수
- `is_payer`
  - `InAppPurchaseAmount > 0` → 1 (구매 유저)
  - 그 외 → 0 (비구매 유저)

> 전체 사용자 중 약 **95.5%가 구매 경험이 있는 사용자**

---

## 3. 탐색적 데이터 분석 (EDA)

### 3.1 구매 유저 vs 비구매 유저 행동 비교

- **세션 수(SessionCount)**와 **평균 세션 길이(AverageSessionLength)**는  
  구매 여부에 따른 차이가 크지 않음
- 단순 플레이 빈도나 플레이 시간만으로는  
  구매 여부를 명확히 구분하기 어려움

---

### 3.2 첫 구매까지 걸리는 시간

- 첫 구매는 **설치 후 0~30일 구간에 비교적 고르게 분포**함
- 특정 초반 시점에 구매가 집중되기보다는,  
  **30일 동안 점진적으로 구매가 발생하는 패턴**을 보임
- 다만 **30일 시점에서 첫 구매 빈도가 다른 날짜 대비 약 2배 이상 증가**하는 현상이 관찰됨

→ 이는 게임 플레이 누적, 콘텐츠 소진, 혹은  
   **30일 전후의 이벤트·보상·결제 유도 설계**가  
   구매 전환을 촉진했을 가능성을 시사함

→ 따라서 초기 온보딩뿐 아니라  
   **30일 시점 전후를 타겟으로 한 리텐션·결제 유도 전략** 또한  
   중요한 마케팅 포인트로 활용 가능

---

### 3.3 Spending Segment 분석

| Spending Segment | 평균 결제 금액 |
|------------------|---------------|
| Minnow           | 약 10         |
| Dolphin          | 약 246        |
| Whale            | 약 2,787      |

- SpendingSegment는 구매 금액과 **강한 상관관계**
- 행동 변수보다 **유저의 결제 성향 자체가 핵심 Feature**임을 확인

---

### 3.4 장르 및 인구통계 특성

- 게임 장르별 구매 비율 차이는 **미미**
- 일부 국가(Country), 성별(Gender) 변수는  
  구매 확률에 의미 있는 영향

---

## 4. 모델링

### 4.1 모델 설정

- **Logistic Regression**
  - 구매 확률 예측 및 **변수 영향력 해석** 목적
  - 클래스 불균형 보정: `class_weight = 'balanced'`

### 사용 Feature
- **수치형**
  - `SessionCount`, `AverageSessionLength`, `Age`
- **범주형**
  - `Gender`, `Country`, `Device`, `GameGenre`, `SpendingSegment`

### 전처리
- 결측치 처리 (중앙값 / `Unknown`)
- One-Hot Encoding
- Standard Scaling

---

### 4.2 모델 해석

- **계수 > 0**
  → 해당 feature 값이 증가할수록 구매 확률 증가
- **계수 < 0**
  → 구매 확률 감소

주요 인사이트:
- `SpendingSegment`, 일부 `Country`, `Gender` 변수가  
  구매 여부 예측에 상대적으로 큰 영향
- 플레이 시간/횟수보다 **유저 특성 기반 Feature의 영향이 더 큼**

---

## 5. 구매 확률 기반 타겟팅

- `predict_proba`를 활용해 **사용자별 구매 확률(purchase_prob)** 산출
- 구매 확률 상위 **20% 구간** 중 아직 구매하지 않은 사용자 식별

→ **구매 가능성은 높지만 아직 결제하지 않은 잠재 고객 그룹 도출**

---

## 6. 프로젝트 결론

- **설치 후 30일째**는 구매 전환을 위한 핵심 구간
- 단순 행동 지표보다 **유저 특성 및 결제 성향 Feature**가  
  구매 여부 예측에 더 효과적

---

## 7. 기술 스택

- Python
- pandas, numpy
- matplotlib, seaborn
- scikit-learn
- Kaggle Notebook
