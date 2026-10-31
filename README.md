
<p align='center'>
    <img src="https://github.com/susu884/file/blob/main/jisu_github_welcome_transparent.gif?raw=true"/>
</p>


## 🙋‍♂️ About Me

| 구분 | 내용 |
| :--- | :--- |
| **Role** | Data Analyst / Data Collector |
| **Focus** | 데이터 전처리 · 데이터 시각화 · 데이터 분석 |
| **Education** | 수원대학교 미디어커뮤니케이션학과 (졸업) |
| **Contact** | `j_susuu@naver.com` |
| **Career** | 리얼투데이 (2021.10-23.04)<br/>쌩끄프로덕션(2023.07-2026.01)|
| **Training** | 아이티윌 데이터 분석가 과정 수료 |
| **License** | `SQLD` `ADsP` `컴퓨터활용능력1급` |
| **Notion**| https://app.notion.com/p/3f7d7aae1dba806ca27bf1aab55b6c0f|


## 🛠 Tech Stack

**Data Analysis**  
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?logo=numpy&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?logo=mysql&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?logo=jupyter&logoColor=white)

**Statistics & Machine Learning**  
![statsmodels](https://img.shields.io/badge/statsmodels-4051B5)
![scipy](https://img.shields.io/badge/scipy-blue?logo=scipy&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-189AB4)
![LightGBM](https://img.shields.io/badge/LightGBM-2E8B57)
![CatBoost](https://img.shields.io/badge/CatBoost-FFCC00)
![SHAP](https://img.shields.io/badge/SHAP-FF0051)

**Text Mining & Geospatial**  
![kiwipiepy](https://img.shields.io/badge/kiwipiepy-brightgreen)
![GeoPandas](https://img.shields.io/badge/GeoPandas-139C5A)
![PySAL](https://img.shields.io/badge/PySAL-3B7EA1)

**Collection & Automation**  
![Selenium](https://img.shields.io/badge/Selenium-43B02A?logo=selenium&logoColor=white)
![BeautifulSoup](https://img.shields.io/badge/BeautifulSoup4-3776AB?logo=python&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?logo=googlesheets&logoColor=white)

**Visualization**  
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?logo=python&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-4C72B0)

**Tools**  
![Git](https://img.shields.io/badge/Git-F05032?logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?logo=github&logoColor=white)
![Notion](https://img.shields.io/badge/Notion-000000?logo=notion&logoColor=white)


## 📂 Portfolio

데이터 생애주기를 **한 노트북 안에서** 검증하는 것을 원칙으로 합니다.
품질 점검 → EDA → 모델링 → 진단 → 해석까지, 모든 코드 셀 아래에 **판단이 담긴 해석**을 남깁니다.

| # | 프로젝트 | 기법 | 핵심 결과 |
|:--:|---|:--:|---|
| 01 | [[Main Portfolio] 4개국 쇼핑몰의 스킨케어 크림 리뷰 분석](https://github.com/susu884/work/tree/main/01.%20[Main%20Portfolio]%20Skincare%20Cream%20Review%20Data%20Analysis_Four-Country%20Online%20Malls) | 텍스트마이닝 · 로지스틱 회귀 · 머신러닝(분류) | 리뷰 6,756건 × 9개 속성, 국가 × 속성 상호작용 유의(LRT p < .001), XGBoost Test MCC 0.438 · ROC-AUC 0.813 |
| 02 | [다이아몬드 가격 예측](https://github.com/susu884/work/tree/main/02.%20Diamond_Price_Prediction_linear_regression) | 선형회귀 | 중량 1% ↑ → 가격 약 1.88% ↑, RMSE 796.88달러, 원본 척도 R² 0.960 |
| 03 | [보스턴 하우징 가격 결정 요인](https://github.com/susu884/work/tree/main/03.%20Boston_Housing_Linear_Regression) | 선형회귀 | 저소득층 비율(`LSTAT`)이 영향 1위, 조정 R² 0.748, RMSE 4.393천 달러 |
| 04 | [사과 품질 분류](https://github.com/susu884/work/tree/main/04.%20Apple_Quality_Logistic_Regression) | 로지스틱 회귀 | 정확도 0.792, AUC 0.868, 크기(`Size`)가 영향 1위 |
| 05 | [피마 인디언 여성 당뇨병 여부 분류](https://github.com/susu884/work/tree/main/05.%20Pima_Indians_Diabetes_Logistic_Regression) | 로지스틱 회귀 | AUC 0.854, 재현율 0.594, 포도당(`Glucose`)이 영향 1위 |
| 06 | [캘리포니아 하우징 중위 가격 예측](https://github.com/susu884/work/tree/main/06.%20Califonia_Housing_Machine%20Learning) | 머신러닝(예측) | R² 0.785, 소득 + 입지가 SHAP 기여의 77% |
| 07 | [SQL 문제 풀이](https://github.com/susu884/work/blob/main/07.%20SQL_portfolio.pdf) | SQL | - |

**공통 모듈** — [`helpers/`](./helpers/)
품질 점검(`my_qtcheck`) · 시각화(`my_plot`) · 통계 검정(`my_stats`) · 전처리(`my_prep`) · 선형회귀(`my_ols`) · 로지스틱 회귀(`my_logit`) · 머신러닝(`my_ml`) · 군집(`my_cluster`)


<div align="center">

### 📮 Contact

![PHONE](https://img.shields.io/badge/PHONE-010.4277.5856-brightgreen?style=for-the-badge)
<br/>
![Email](https://img.shields.io/badge/Email-j_susuu@naver.com-red?style=for-the-badge)

<sub>Data Collection · Data Preprocessing · Exploratory Data Analysis · Modeling · Machine Learning</sub>

</div>

