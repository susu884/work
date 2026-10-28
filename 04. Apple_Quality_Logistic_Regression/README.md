# 사과의 특성 7종으로 사과 품질(good/bad)을 구분할 수 있는가

![Python](https://img.shields.io/badge/Python-3.14.3-3776AB?logo=python&logoColor=white) ![pandas](https://img.shields.io/badge/pandas-3.0.5-150458?logo=pandas&logoColor=white) ![NumPy](https://img.shields.io/badge/NumPy-2.5.2-013243?logo=numpy&logoColor=white) ![statsmodels](https://img.shields.io/badge/statsmodels-0.14.6-4051B5) ![scikit-learn](https://img.shields.io/badge/scikit--learn-1.9.0-F7931E?logo=scikitlearn&logoColor=white)


## 요약 (Executive Summary)

- **데이터:** Apple Quality (Kaggle) / https://www.kaggle.com/datasets/nelgiriyewithana/apple-quality
- **규모:** 4,000 관측치 × 9 변수 (식별자 `A_id` 제거 후 8개 변수)
- **문제 유형:** 분류 (이항 로지스틱 회귀)

> 사과 4,000개의 품질(`Quality`, `good`/`bad`)을 크기·무게·단맛·아삭함·과즙·숙성도·산도 7개 특성으로 분류했다.
> Box-Tidwell 검정으로 로짓 선형성을 점검해 **7종 중 5종의 위배**를 확인했고, 위배 변수에 제곱항을 추가하는 처방을 **진단 → 처방 → 검증 → 재진단 루프**로 반복해 위배를 해소했다.
> 전처리를 하나씩 누적한 체크포인트 8종(가정 미반영 4종 + 가정 반영 4종)을 비교했고, 자동 선택이 1순위로 지목한 `5_가정반영+후진소거`는 제곱항의 짝(`Crunchiness_c`)을 지워 계층 원칙에 어긋나므로
> **`4_가정반영`(독립변수 13개)** 을 최종 채택했다.
> 정확도 0.792, AUC 0.868, Pseudo R² = 0.342로, 기준선 모형(독립변수 7개, 정확도 0.749, AUC 0.824)보다 정확도는 0.043, AUC는 0.044 높다.
> 가정 미반영 4종의 AUC는 0.823~0.824, 가정 반영 4종은 모두 0.868로, **성능을 바꾼 것은 전처리가 아니라 가정 검정에 따른 처방**이었다.
> 사과가 `good`일 확률을 가장 크게 좌우하는 요인은 **크기(`Size`, βstd = 1.214)** 였고, 단맛(`Sweetness`, 1.105)과 과즙(`Juiciness`, 0.937)이 뒤를 이었다.
> 즉 품질은 크기·단맛·과즙이 좌우하며, 단독 검정에서 무관해 보이던 `Weight`·`Acidity`·`Crunchiness`도 다른 변수를 통제하거나 제곱항을 넣으면 유의해졌다.

## 핵심 결과

| 항목 | 내용 |
|---|---|
| 최종 모형 | `Quality(good = 1) ~ Sweetness + (Size, Weight, Juiciness, Acidity, Ripeness, Crunchiness 각각의 중심화 1차항 + 제곱항)` (독립변수 13개) |
| 적합도 | LLR χ²(13) = 1,899.19, p < .001, Pseudo R² = 0.342 |
| 분류 성능 | 정확도 0.792 / 정밀도 0.785 / 재현율 0.804 / 특이성 0.779 / F1 0.795 / AUC 0.868 (학습 데이터 기준) |
| 임계값 | 정확도는 0.5에서, F1은 0.4에서 최대 (0.3 → 재현율 0.932, 0.7 → 정밀도 0.857) |
| 유의 변수 | 13개 중 12개 유의 (`Crunchiness_c`만 비유의, p = 0.787) |
| 가정 검토 | 선형성 (7종 중 5종 위배 → 제곱항 6개 추가로 해소) / 독립성 (충족, DW = 2.022) / 다중공선성 (최대 VIF = 1.61) |
| 이번에 연습한 기법 | Welch T검정·Mann-Whitney U, VIF 점검, Box-Tidwell 로짓 선형성 검정, 진단 → 처방 → 검증 → 재진단 루프, 제곱항(중심화), 계층 원칙, Durbin-Watson, 오즈비·표준화계수(βstd), 혼동행렬·AUC, 임계값 조정 |

## 참고자료

### 데이터 출처

- Kaggle 데이터셋: Apple Quality (`nelgiriyewithana/apple-quality`)
- URL: https://www.kaggle.com/datasets/nelgiriyewithana/apple-quality
- 원출처: 미국의 한 농업 회사(American agriculture company)가 제공한 데이터를 업로더 Nidula Elgiriyewithana가 Kaggle에 게시 (Kaggle 설명란 기준, 값은 표준화·정제된 상태로 제공됨)
- 라이선스: Other (specified in description) — Kaggle 표기 기준이며, 설명란에는 별도의 이용 조건이 적혀 있지 않음
- DOI: https://doi.org/10.34740/kaggle/dsv/7384155
- 분석에 사용한 라이브러리: `jussam` (데이터 로딩·분석 보조 함수) — https://juyounga.kr/

## 회고

- **새로 익힌 기법:** **Box-Tidwell 검정으로 로짓 선형성을 점검**하고, 위배가 나온 변수에 **제곱항을 추가한 뒤 재진단하는 루프**를 위배가 없어질 때까지 반복하는 방법이다. 제곱항을 넣을 때는 변수를 **중심화**하고, 1차항을 함께 남기는 **계층 원칙**을 지켜야 한다는 점도 익혔다. 그리고 제곱항이 있는 변수는 오즈비를 한 줄로 해석할 수 없어 **표준화계수(βstd)로 영향력을 비교**하는 방식이다.
- **막혔던 지점과 해결 방법:**
  - ① EDA(T검정·Mann-Whitney U)에서 품질과 무관하게 나온 `Weight`(p = 0.928)·`Acidity`(p = 0.797)·`Crunchiness`(p = 0.985)가 모형에서는 유의하게 나와 의아했다 → **다른 변수를 통제하면 효과가 드러나는 것**임을 확인했고, `Crunchiness`는 1차항이 끝까지 비유의지만 **제곱항이 유의**해 Box-Tidwell 검정에서만 잡히는 관계였다.
  - ② `compare_models()`가 변수가 하나 적은 `5_가정반영+후진소거`를 1순위로 지목했으나, 지워진 변수가 제곱항의 짝(`Crunchiness_c`)이었다 → **계층 원칙에 따라 4번을 채택**했다. AIC 차이는 1.93에 불과했다.
  - ③ 값이 이미 표준화되어 제공되어 "크기 1점"이 몇 mm인지 알 수 없었다 → 오즈비를 현장 단위로 환산하지 않고 **표준화 점수 1점 증가 기준으로만 해석**했다.
- **다음 데이터셋에서 보완할 점:** 훈련·검증 데이터를 나누지 않고 전체 데이터로 적합해 정확도 0.792와 AUC 0.868이 **낙관적인 값**이므로, 처음부터 학습/검증 분할(교차검증)을 설계해 일반화 가능한 성능으로 보고하고 싶다. 또 7종 모두 음수를 포함해 로그 변환이 후보에서 빠져 **사실상 제곱항 외의 선택지가 없었으므로**, 곡선 관계를 처방할 대안을 더 준비해 두고, 사용 목적(정밀도 우선 / 재현율 우선)을 먼저 정해 그에 맞는 임계값으로 평가하고 싶다.
