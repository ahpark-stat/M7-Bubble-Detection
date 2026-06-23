# M7-Bubble-Risk-Analysis

## Overview
본 연구는 미국 M7(Magnificent Seven) 기업의 급등 현상을 분석하여 전통적인 평균회귀(Mean Reversion) 가설이 실제로 성립하는지 검증하고, 급등 이후 발생할 수 있는 버블 붕괴 위험을 정량적으로 예측하는 것을 목표로 한다.

초기 가설은 "과열된 주가는 결국 평균으로 회귀한다"는 것이었으나, 분석 결과 M7 종목에서는 평균회귀가 관찰되지 않았다. 이에 따라 연구 방향을 전환하여, "모든 급등이 버블은 아니다" 라는 새로운 관점에서 급등을
Justified Growth
Bubble Growth
로 구분하고, 머신러닝을 활용한 버블 탐지 모델과 조건부 리스크 분석을 수행하였다.

## Research Questions
1. 급등한 주가는 이후 평균회귀 현상을 보이는가?
2. 모든 급등은 버블인가?
3. 버블성 급등과 정당화된 상승은 어떻게 구분할 수 있는가?
4. 현재 시장이 버블 상태인지 탐지할 수 있는가?
5. 버블 상태에서 투자 위험은 얼마나 증가하는가?

## Data
### Target Assets
- AAPL, GOOGLE, META, MSFT, TSLA

## Phase 1. Mean Reversion Test
### Hypothesis: 급등한 주가는 시간이 지나면 평균으로 회귀할 것이다.
### Result
Mean Reversion Test
T-statistic = 0.137
p-value = 0.891
Breakout Persistence Test
T-statistic = 0.679
p-value = 0.498
### Conclusion
평균회귀 현상은 통계적으로 확인되지 않았다.
또한 상단 돌파 역시 추가 상승 또는 하락을 예측하는 유의한 신호가 아니었다.

### Regime Analysis
시장 환경이 섞여 있어 결과가 왜곡되었을 가능성을 검토하기 위해 Regime 분리를 수행하였다.
그러나 Regime을 분리한 이후에도 평균회귀 현상은 확인되지 않았다.

### Interpretation
M7 종목은 전통적인 기술적 과열 지표가 잘 작동하지 않는 특수한 자산군으로 해석된다.

가능한 원인
- 대규모 자사주 매입
- ETF 자금의 구조적 유입
- 초대형 기술주에 대한 투자자 선호
- AI 및 기술 혁신에 대한 장기 성장 기대
  
## Phase 2. Bubble vs Justified Growth
### Motivation
평균회귀는 존재하지 않았다.
그러나 일부 급등 사례는 이후 큰 폭의 하락을 경험하였다.
따라서 연구 방향을 다음과 같이 수정하였다.
급등 자체보다 "어떤 급등이 위험한가?"를 식별한다.

### Event Definition
급등 이벤트 정의
- Upper Bollinger Band Breakout
- RSI ≥ 70
- 거래량 증가
총 512건의 급등 이벤트 발견, 정당화된 급등 203건 & 버블형 급등 296건

분석 결과 MFI와 Disparity의 조합이 Bubble과 Justified를 구분하는 핵심 변수로 나타났다.
Bubble 유형은 최대낙폭(MDD) 분포가 왼쪽으로 크게 치우쳐 있었으며,
특히 -40% 이상의 대규모 폭락 사례가 Bubble 그룹에서 집중적으로 발생하였다.

## Phase 3. Bubble Detection Model
### Model
- Random Forest Classifier

### Performance
Metric	Score
ROC-AUC	0.786
Precision	0.90
Recall	0.61
Classification Report
Class	Precision	Recall
Normal	0.89	0.97
Bubble	0.78	0.45

### Key Insight
모델이 Bubble이라고 경고한 경우 약 90% 수준의 높은 신뢰도를 보였다.
이는 단순 가격 돌파가 아닌 "위험한 돌파" 를 식별할 수 있음을 의미한다.

## Phase 4. Fear Regime Analysis
### Hypothesis: 공포장(VIX 상승)에서 발생하는 하락 신호는 더 위험할 것이다.

### Result
VIX > 20
p-value > 0.8
VIX > 30
p-value > 0.8

### Conclusion
공포장에서도 M7 종목은 약세를 보이지 않았다.
오히려 공포 국면은 M7 종목의 상대적 강세 구간으로 나타났다.
이는 투자자들이 M7을 "안전자산에 가까운 구조적 피난처"로 인식하고 있음을 시사한다.

## Phase 5. Conditional Risk Analysis
버블 여부에 따라 조건부 위험도를 계산하였다.
Conditional VaR
VaR | Bubble
VaR | Normal
### Regime Comparison
2020~2023
Bubble VaR ≈ -22% ~ -24%
2024~2025
Bubble VaR = -35.2%
### Interpretation
최근 버블은 과거보다 훨씬 깊은 손실을 유발할 가능성이 있다. 

###Backtesting
- Goal: 리스크 탐지 모델이 실제 투자 성과를 개선할 수 있는지 검증
### Result
- Low Risk
Metric	Value
평균 수익률	10%
손절 적용 후	8%
손절 이후에도 수익성이 유지되었다.

- High Risk
Metric	Value
평균 수익률	14.19%
손절 적용 후	-3.72%
수익률 변동성이 매우 커 리스크 관리가 사실상 불가능하였다.

### Final Conclusion
본 연구는 M7 종목이 전통적인 평균회귀 자산이 아님을 통계적으로 확인하였다.
또한 모든 급등을 동일하게 취급하는 접근법의 한계를 발견하였으며, Bubble과 Justified Growth를 구분하는 프레임워크를 제안하였다.

핵심 결론은 다음과 같다.
평균회귀 현상은 관찰되지 않았다.
M7은 공포장에서 오히려 강한 모습을 보였다.
Bubble 유형은 -40% 이상의 폭락 위험을 내포한다.
머신러닝 모델을 통해 위험한 급등을 사전에 탐지할 수 있었다.
버블은 관리하는 것이 아니라 회피해야 한다.
"거품은 제어하는 것이 아니라 회피하는 것이다."
