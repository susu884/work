# 진단 측정값으로 피마 인디언 여성의 당뇨병 여부를 예측할 수 있는가

![Python](https://img.shields.io/badge/Python-3.14.7-3776AB?logo=python&logoColor=white) ![pandas](https://img.shields.io/badge/pandas-3.0.5-150458?logo=pandas&logoColor=white) ![NumPy](https://img.shields.io/badge/NumPy-2.5.2-013243?logo=numpy&logoColor=white) ![statsmodels](https://img.shields.io/badge/statsmodels-0.14.6-4051B5) ![scikit-learn](https://img.shields.io/badge/scikit--learn-1.9.0-F7931E?logo=scikitlearn&logoColor=white)


## 요약 (Executive Summary)

- **데이터:** Pima Indians Diabetes Database (Kaggle) / https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database
- **규모:** 768 관측치 × 9 변수 (0값을 결측으로 처리·제거한 뒤 724건)
- **문제 유형:** 분류 (이항 로지스틱 회귀)

> 21세 이상 피마 인디언 혈통 여성 768명 가운데 결측(0값)을 처리한 724명의 당뇨병 여부(`Outcome`)를 임신횟수·포도당·혈압·BMI·당뇨 내력 가중치·나이 등 진단 측정값으로 분류했다.
> 의료 데이터에서 0일 수 없는 값을 결측으로 보고, 결측률이 높은 `SkinThickness`(29.6%)·`Insulin`(48.7%)은 변수를 제거했으며 나머지 3개 변수의 결측 행 44건(5.7%)은 제거했다.
> Box-Tidwell 검정에서 6종 중 3종(`Age`·`DiabetesPedigreeFunction`·`BMI`)의 **로짓 선형성 위배**를 확인했고, `Age`와 `DiabetesPedigreeFunction`에 제곱항을 추가하는 처방을 **진단 → 처방 → 검증 → 재진단 루프**로 반복해 위배를 해소했다(`BMI`는 저절로 해소).
> 전처리를 하나씩 누적한 체크포인트 6종(가정 미반영 3종 + 가정 반영 3종)을 비교했고, 자동 선택이 1순위로 지목한 `1_후진소거`(독립변수 4개) 대신
> **`4_가정반영+후진소거`(독립변수 6개)** 를 최종 채택했다.
> Pseudo R² = 0.305, 정확도 0.782, AUC 0.854로, 기준선 모형(독립변수 6개, Pseudo R² 0.278, 정확도 0.776, AUC 0.843)보다 AUC는 0.011, Pseudo R²는 0.027 높다.
> 다만 **재현율은 0.594로 낮아** 당뇨 환자 249명 중 101명을 놓친다. 임계값을 0.3으로 낮추면 놓친 환자는 45명으로 줄지만, 잘못 잡는 정상은 57명에서 127명으로 늘어난다.
> 당뇨병 여부에 가장 큰 영향을 주는 요인은 **포도당(`Glucose`, βstd = 1.111)** 이었고, 2위는 `Age`(βstd = 0.988)였다.
> 즉 포도당이 가장 강하게 작용하지만, 직선 모형에서는 비유의(p = 0.085)해 제거되던 `Age`가 **제곱항을 넣자 2위 변수**가 되었다. 나이는 곡선 관계라 직선 모형으로는 놓칠 수 있다.

## 핵심 결과

| 항목 | 내용 |
|---|---|
| 최종 모형 | `Outcome(1 = 당뇨) ~ Glucose + BMI + (Age_c + Age_c²) + (DiabetesPedigreeFunction_c + DiabetesPedigreeFunction_c²)` (독립변수 6개 항, `_c`는 중심화한 값) |
| 적합도 | LLR χ²(6) = 284.63, p < .001, Pseudo R² = 0.305 |
| 분류 성능 | 정확도 0.782 / 정밀도 0.722 / 재현율 0.594 / 특이성 0.880 / F1 0.652 / AUC 0.854 (학습 데이터 기준) |
| 임계값 | 정확도는 0.4에서 최대(0.783), F1은 0.3에서 최대(0.703) (0.3 → 재현율 0.819, 0.7 → 정밀도 0.831) |
| 유의 변수 | 6개 항 전부 유의 (p < .01) — `Glucose`·`BMI`·`Age`(1차항, 제곱항)·`DiabetesPedigreeFunction`(1차항, 제곱항) |
| 가정 검토 | 선형성 (6종 중 3종 위배 → 제곱항 2종 추가로 해소) / 독립성 (충족, DW = 1.906) / 다중공선성 (최대 VIF = 2.02) |
| 이번에 연습한 기법 | 0값 결측 처리, Mann-Whitney U 검정, VIF 점검, Box-Tidwell 로짓 선형성 검정, 진단 → 처방 → 검증 → 재진단 루프, 제곱항(중심화), 계층 원칙, Durbin-Watson, 오즈비·표준화계수(βstd), 혼동행렬·AUC, 임계값 조정 |

## 참고자료

### 데이터 출처

- Kaggle 데이터셋: Pima Indians Diabetes Database (`uciml/pima-indians-diabetes-database`)
- URL: https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database
- 원출처: 미국 국립 당뇨병·소화기·신장질환 연구소(National Institute of Diabetes and Digestive and Kidney Diseases, NIDDK). 더 큰 데이터베이스에서 21세 이상 피마 인디언 혈통 여성만 선택한 자료
- 인용: Smith, J.W., Everhart, J.E., Dickson, W.C., Knowler, W.C., & Johannes, R.S. (1988). Using the ADAP learning algorithm to forecast the onset of diabetes mellitus. *Proceedings of the Symposium on Computer Applications and Medical Care* (pp. 261–265). IEEE Computer Society Press.
- 라이선스: CC0: Public Domain
- 분석에 사용한 라이브러리: `jussam` (데이터 로딩·분석 보조 함수) — https://juyounga.kr/

## 회고

- **새로 익힌 기법:** 의료 데이터에서 **0일 수 없는 변수만 골라 0값을 결측으로 바꾸고**, 결측률이 높은 변수는 제거하고 낮은 변수는 행을 제거하는 방식으로 나누는 처리다. 그리고 **Box-Tidwell 검정으로 로짓 선형성을 점검**하고 제곱항을 추가한 뒤 재진단하는 루프(제곱항은 중심화하고 1차항을 함께 남기는 **계층 원칙**), 임계값을 바꿔 **정밀도·재현율의 트레이드오프**를 확인하는 방법이다.
- **막혔던 지점과 해결 방법:**
  - ① `Age`가 단독 검정에서는 유의(p < .001)했는데 직선만 허용한 기준선 모형에서는 비유의(p = 0.085)가 되어 후진소거로 제거되었다 → **Box-Tidwell 검정(z = −4.46)에서 곡선 관계를 확인**하고 제곱항을 넣자 영향력 2위 변수가 되었다.
  - ② `compare_models()`가 변수가 적은 `1_후진소거`를 1순위로 지목했다 → 정밀도를 제외한 모든 지표가 앞서고 AIC가 24.9 낮은 **`4_가정반영+후진소거`를 직접 검토해 채택**했다.
  - ③ 기준선 모형에서 p < .001이던 `Pregnancies`가 `Age`의 곡선을 반영하자 비유의(p = 0.2347)가 되어 제거되었다 → `Age`와의 상관계수(0.617)를 확인하고 **후진소거 결과를 그대로 받아들였다**.
- **다음 데이터셋에서 보완할 점:** 학습/검증 분할 없이 전체 데이터로 적합해 정확도 0.782와 AUC 0.854가 **학습 데이터 기준 값**이므로, 처음부터 분할(교차검증)을 설계해 일반화 가능한 성능으로 보고하고 싶다. 또 재현율 0.594가 낮았으므로 **재현율 목표를 먼저 정한 뒤 임계값을 평가**하고, 통째로 제거한 `SkinThickness`·`Insulin`을 **대체 방식으로 되살려 성능을 비교**해 보고 싶다.
