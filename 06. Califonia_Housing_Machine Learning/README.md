# 지역 특성으로 캘리포니아 주택 중위가격을 예측할 수 있는가

![Python](https://img.shields.io/badge/Python-3.14.7-3776AB?logo=python&logoColor=white) ![pandas](https://img.shields.io/badge/pandas-3.0.5-150458?logo=pandas&logoColor=white) ![NumPy](https://img.shields.io/badge/NumPy-2.5.2-013243?logo=numpy&logoColor=white) ![statsmodels](https://img.shields.io/badge/statsmodels-0.14.6-4051B5) ![scikit-learn](https://img.shields.io/badge/scikit--learn-1.9.0-F7931E?logo=scikitlearn&logoColor=white)


## 요약 (Executive Summary)

- **데이터:** California Housing Prices (Kaggle) / https://www.kaggle.com/datasets/camnugent/california-housing-prices
- **규모:** 20,640 관측치 × 10 변수 (결측 207건 제거 후 20,433건)
- **문제 유형:** 예측(회귀) — 머신러닝 지도학습

> 1990년 미국 인구조사 기반 캘리포니아 20,433개 구역의 주택 중위가격(`median_house_value`)을 소득·위치·주거 구조 특성으로 예측했다.
> 원본 변수 대신 **비율형 파생변수**(1인당 소득·가구당 방 수 등)와 해안/내륙 구분(`ocean_area`)을 만들고, 위·경도는 Moran's I(0.790)로 공간자기상관을 확인한 뒤 **K-means(k=5) 공간 군집**(`special_cluster`)으로 바꿔 반영했다. 가격은 로그 변환해 모델링했다.
> 선형 4종 · 비선형 2종 · 트리 1종 · 앙상블 4종, 총 11종을 RMSE 기준으로 비교하고 `GridSearchCV`로 튜닝한 결과, 튜닝 후 1위인 **`CatBoost`** 를 최종 채택했다. Feature Importance로 변수를 9개에서 7개로 줄여 재학습·튜닝해도 성능은 같았다.
> 검증 데이터(4,087개 구역)에서 R² = 0.785, RMSE = 0.266(로그 척도, 가격으로 환산하면 약 ±30%)으로, 선형회귀(R² 0.713)보다 R²가 0.072 높다.
> 가격을 가장 크게 좌우하는 요인은 **1인당 소득(`income_per_person`)** 이었고, 해안/내륙 구분(`ocean_area`)과 공간 군집(`special_cluster`)이 뒤를 이었다. 세 변수가 SHAP 기여의 77%를 차지했다.
> 다만 튜닝 후에도 Train ↔ CV Gap이 15.53%로 기준(15%)을 넘어 **과대적합** 판정이 해소되지 않았다.
> 즉 집값은 주거 구조보다 **소득과 입지**가 좌우한다. 주거 구조 변수 4개를 모두 합쳐도 SHAP 기여가 23%로, 소득 하나(33%)에 미치지 못했다.

## 핵심 결과

| 항목 | 내용 |
|---|---|
| 최종 모델 | `CatBoostRegressor` (`depth` 8, `iterations` 1000, `learning_rate` 0.03), 독립변수 7개 |
| 검증 성능 | R² = 0.785 / RMSE = 0.266 / MAE = 0.194 (로그 변환한 `median_house_value` 기준, 검증 데이터 4,087건) |
| 과적합 판정 | Train RMSE 0.224 / CV 0.265(±0.002) / Test 0.266, Gap 15.53% → 과대적합 (기준 15%, 튜닝 전 16.17%) |
| 모델 비교 | 튜닝 후 RMSE 상위 5개는 `CatBoost` 0.266, `LightGBM` 0.270, `XGBoost` 0.272, `RandomForest` 0.273, `KNN` 0.274. 선형 4종은 R² 0.713에 머묾 |
| 영향력 상위 | `income_per_person`(SHAP 0.228, 누적 33%) > `ocean_area`(0.185, 59%) > `special_cluster`(0.126, 77%) |
| 이번에 연습한 기법 | 비율형 파생변수·상한 절단 플래그·범주 병합, Moran's I, K-means 공간 군집, Spearman 상관·Welch ANOVA·Games-Howell, VIF, 11종 모델 비교(RMSE 1순위 + 보조지표 결함 점검), GridSearchCV, 과적합 판정, Feature Importance, SHAP(Bar · Beeswarm · Dependence) |

## 참고자료

### 데이터 출처

- Kaggle 데이터셋: California Housing Prices (`camnugent/california-housing-prices`)
- URL: https://www.kaggle.com/datasets/camnugent/california-housing-prices
- 원출처: Pace, R. K., & Barry, R. (1997). Sparse spatial autoregressions. *Statistics & Probability Letters*, 33(3), 291–297. Kaggle 데이터는 Luís Torgo(University of Porto) 페이지의 California Housing 데이터를 Aurélien Géron이 수정한 버전임 (Kaggle 설명란 기준)
- 라이선스: CC0: Public Domain
- 분석에 사용한 라이브러리: `jussam` (데이터 로딩·분석 보조 함수) — https://juyounga.kr/

## 회고

- **새로 익힌 기법:** 파생변수를 만들고 **효과크기로 채택 여부를 검증**하는 방법(비율형 파생변수·상한 절단 플래그·범주 병합), 위·경도를 그대로 쓰지 않고 **Moran's I로 공간자기상관을 확인한 뒤 K-means 군집 변수로 바꾸는 방법**, 11종 모델을 **RMSE 1순위 + 보조지표 결함 점검**으로 고르는 방법, **Train ↔ CV Gap으로 과적합을 판정**하는 방법, 그리고 Feature Importance와 SHAP으로 변수의 영향력과 비선형 패턴(임계효과·U자)을 읽는 방법이다.
- **막혔던 지점과 해결 방법:**
  - ① 공간 군집 수의 자동 판정(실루엣)은 k=2였는데 두 덩어리로 양분하면 지역을 구분하는 의미가 작았다 → 엘보우와 파생변수로서의 활용성을 고려해 **k=5로 정했다.**
  - ② 튜닝이 항상 성능을 올리지는 않았다. `Lasso`·`ElasticNet`은 기본값(`alpha`)이 너무 강해 R²가 0.000이었다가 튜닝으로 0.713이 되었지만, `SVR`은 3개 조합으로 줄인 탐색에서 오히려 RMSE가 0.266 → 0.285로 나빠졌다 → **튜닝 전후를 같은 표에서 비교**해 확인했다.
  - ③ 튜닝 후에도 과적합 Gap이 15.53%로 기준(15%)을 넘어 과대적합 판정이 해소되지 않았다 → 한계로 정리하고 다음 과제로 남겼다.
- **다음 데이터셋에서 보완할 점:** 이웃한 구역이 훈련·검증에 함께 들어갔을 수 있어(Moran's I = 0.790) **군집(지역) 단위 교차검증(GroupKFold)** 으로 새로운 지역에 대한 성능을 다시 재고, 탐색 조합을 늘린 튜닝과 규제 강화로 과적합 Gap을 줄이고 싶다. 또 가격 상한(500,001$)에서 잘린 구간의 과소예측을 구간별로 확인하지 못했으므로 평가 단계에서 함께 점검하고, 군집 수(k)별 성능도 비교해 보고 싶다.
