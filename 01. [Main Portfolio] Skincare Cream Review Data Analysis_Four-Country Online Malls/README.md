# CreamyView: 리뷰 속 소비자는 무엇을 말하고, 분석가는 무엇을 들어야 하는가

4개국 쇼핑몰의 스킨케어 크림 리뷰 텍스트마이닝·머신러닝 분석으로 제안하는 K-Beauty 글로벌 전략

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white) ![Selenium](https://img.shields.io/badge/Selenium-43B02A?logo=selenium&logoColor=white) ![BeautifulSoup](https://img.shields.io/badge/BeautifulSoup-4B8BBE) ![Kiwi](https://img.shields.io/badge/Kiwi-형태소분석-6C757D) ![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?logo=scikitlearn&logoColor=white) ![XGBoost](https://img.shields.io/badge/XGBoost-189AB4)


## 요약 (Executive Summary)

- **프로젝트:** 2026 빅데이터 분석 프로젝트 (4인 팀)
- **데이터:** 4개국 대표 온라인 쇼핑몰의 스킨케어 크림 리뷰를 직접 크롤링 (2026-04-24 ~ 04-28) — 한국 올리브영 · 미국 Ulta · 일본 라쿠텐 · 러시아 Wildberries
- **규모:** 8,000건 수집 → 3점 리뷰 제외 7,230건 → 속성 미언급 474건 제거 후 **6,756건 × 9개 속성**
- **문제 유형:** 분류 (만족 / 불만족) — 통계 분석(EDA · 로지스틱 회귀)과 머신러닝을 함께 사용

> 4개국 쇼핑몰의 크림 리뷰에서 **단어를 9개 제품 속성으로 분류하고, 각 속성의 언급 여부가 만족·불만족과 어떻게 관련되는지, 그 관계가 국가마다 다른지** 분석했다.
> 리뷰는 한국어로 번역해 Kiwi로 형태소 분석한 뒤, TF-IDF와 로지스틱 회귀 계수로 후보 단어를 찾아 **9개 속성 · 811개 키워드의 속성 사전**을 만들었고, 리뷰 1건에 속성 언급 여부를 0/1로 라벨링했다(리뷰 1건당 평균 2.4개 속성). 별점 4·5점은 만족(1), 1·2점은 불만족(0)으로 두었다.
> ① **EDA**에서는 9개 속성이 모두 만족도와 유의하게 관련되었고(p < .001), **자극**이 가장 뚜렷했다(Cramér's V = 0.292, 언급 시 만족률 −27.9%p). 국가별 언급률도 크게 달랐다(자극: 한국 62.5% vs 러시아 14.9%, 향: 러시아 39.2% vs 한국 2.5%).
> ② **로지스틱 회귀**에서는 국가 × 속성 상호작용 27개를 추가하자 모형 적합도가 유의하게 좋아져(LR χ² = 175.15, df = 27, p < .001, Pseudo R² 0.172 → 0.192) **같은 속성도 국가에 따라 효과가 달랐다.** 자극(OR 0.28~0.45)과 배송(OR 0.21~0.45)은 전 국가에서 만족을 낮췄고, 보습은 전 국가에서 만족을 높였다(미국 OR 4.23으로 최대).
> ③ **머신러닝**(XGBoost)은 같은 변수로 Test MCC 0.438, ROC-AUC 0.813을 보여 전부 만족으로 찍는 기준선(MCC 0)보다 확실히 구분했고, 처음 보는 제품에서도 MCC 0.370 ± 0.040이었다. 예측을 가장 크게 움직인 변수는 **자극**(SHAP 20.8%)이었다.
> 즉 **불만족 요인(자극·배송)은 국가를 가리지 않고 공통**이고, **만족 요인은 방향은 비슷해도 효과 크기와 중요 요인이 국가마다 다르다.** 공통 리스크는 표준 품질로 관리하고 만족 요인은 국가별로 공략해야 한다.

## 핵심 결과

| 분석 단계 | 확인한 질문 | 주요 결과 |
|---|---|---|
| 1. EDA | 속성과 만족도는 전체적으로 관련이 있는가? | 9개 속성 모두 유의(p < .001). 언급 시 7개는 만족률 상승, 자극(−27.9%p)·배송(−16.8%p)은 하락. 효과크기는 자극(0.292) > 기능(0.194) > 향(0.123) > 보습(0.101) |
| 2. 로지스틱 회귀 | 동일한 속성이라도 국가별로 만족도에 미치는 영향이 달라지는가? | 상호작용 27개 블록 전체가 유의(LRT p < .001). 유의한 국가 × 속성 상호작용 10건. 제품 통제 모형에서도 가설 판정이 모두 유지 |
| 3. 머신러닝 | 동일한 변수들이 실제 만족/불만족 리뷰를 얼마나 잘 구분하는가? | Test Accuracy 0.753 / F1(만족) 0.819 / F1(불만족) 0.614 / Macro-F1 0.716 / MCC 0.438 / ROC-AUC 0.813. Group Holdout(20회) F1 0.794 ± 0.035, MCC 0.370 ± 0.040, ROC-AUC 0.767 ± 0.025 |
| 가설 검증 | 국가별 속성 가설 4개 | 지지 2 · 미지지 2 |
| 이번에 연습한 기법 | | 크롤링(Selenium · BeautifulSoup), 다국어 번역 정제, 형태소 분석(Kiwi), TF-IDF 키워드 탐색, 속성 사전 기반 다중 라벨 분류, 카이제곱 · Cramér's V, 상관분석, 국가 × 속성 상호작용 파생변수, 이항 로지스틱 회귀 · 우도비검정(LRT) · 오즈비, 제품 고정효과 통제, 11개 모델 비교(Train 내부 5-Fold CV) · GridSearchCV, XGBoost, Feature Importance(Gain), SHAP, Group Holdout |

### 가설 검증 결과

| 가설 | 판정 | 근거 |
|---|---|---|
| H1. 한국에서 가격을 언급한 리뷰는 만족일 가능성이 높다 | 미지지 | 한국 OR 1.26 (n.s.), 일본에서 대신 유의(OR 1.83) |
| H2. 미국에서 제품 기능을 언급한 리뷰는 만족일 가능성이 높다 | 지지 | 미국 OR 2.85 (p < .05) |
| H3. 일본에서 향을 언급한 리뷰는 만족일 가능성이 높다 | 미지지 | 일본 OR 1.32 (n.s.), 러시아에서 대신 유의(OR 2.32) |
| H4. 러시아에서 보습을 언급한 리뷰는 만족일 가능성이 높다 | 지지 | 러시아 OR 2.10 (p < .05), 4개국 모두 유의 |

### 국가별 만족을 끌어올리는 속성 (유의한 오즈비)

| 국가 | 속성(OR) |
|---|---|
| 한국 | 기능 2.13 · 보습 1.32 · 발림성 1.28 |
| 미국 | 보습 4.23 · 기능 2.85 · 브랜드충성도 1.84 |
| 일본 | 브랜드충성도 2.58 · 보습 2.53 · 가격 1.83 |
| 러시아 | 향 2.32 · 브랜드충성도 2.15 · 보습 2.10 · 발림성 1.97 |

- 공통 불만 요인은 **자극**과 **배송**이며 SHAP에서도 가장 큰 감점 요인이었다.
- 브랜드충성도는 한국에서는 유의하지 않지만(OR 0.86) 해외 3개국에서는 모두 유의했다.
- 용기는 전 국가에서 유의한 효과가 확인되지 않았다.

## 참고자료

### 데이터 출처

- 직접 수집 (Selenium · BeautifulSoup), 수집 기간 2026-04-24 ~ 04-28
- 수집 대상 플랫폼: 한국 올리브영 · 미국 Ulta · 일본 라쿠텐(Rakuten) · 러시아 Wildberries
- 수집 방식: 국가별 스킨케어·크림 카테고리 인기순 상위 10개 제품 × 제품별 리뷰 200건(별점 높은 순 100건 + 낮은 순 100건) = 국가별 2,000건
- 국가 선정 기준: 한국 화장품 수출 실적 상위국과 K-Beauty 잠재력 (중국은 데이터 보안 및 규제로 인한 웹 크롤링 제한으로 제외)

### 참고문헌

1. 권태흥. (2024, April 28). 가격비교·가성비 소비패턴에 화장품 기업들 다이소로 GOGO. CNC News. https://www.cncnews.co.kr/news/article.html?no=9211
2. 김동현. (2025, December 29). '미국판 올영' 얼타뷰티…불황에도 주가 훨훨. 한국경제. https://www.hankyung.com/article/2025122908611
3. 대한화장품산업연구원. (2026). 2025년 국가별 화장품 수출 실적 분석. https://www.kcii.re.kr/trade/total
4. 서소명. (2022). 텍스트 마이닝 기법을 활용한 고객 리뷰 감성분석 [박사학위논문, 우송대학교].
5. 식품의약품안전처. (2026, July 2). '26년 상반기 화장품 수출 70억 달러, 역대 상반기 실적 중 최대치 달성 [보도자료]. https://www.mfds.go.kr/brd/m_99/view.do?seq=50161
6. 심은혜. (2026, January 13). 미국 스킨케어 시장, 스키니멀리즘 확산…'단계형 관리'보다 '효능 중심'. 우먼타임스. https://www.womentimes.co.kr/news/articleView.html?idxno=93965
7. 원종수, & 배장원. (2026). 토픽모델링 기반 고객 불만 분류 및 서비스 개선 방법 연구. 한국정보통신학회논문지, 30(1), 1–12. https://doi.org/10.6109/jkiice.2026.30.1.1
8. 이한진, 이예은, & 박수현. (2024). 구매후기 토픽모델링 분석 기반 고객경험 증진 제안 연구. 융복합지식학회논문지, 12(1), 77–88.
9. 정혜영. (2025, October 29). 올리브영, 온라인 뷰티 플랫폼 3사중 관심도 압도적 1위…쿠팡·무신사 순. 빅데이터뉴스. https://www.thebigdata.co.kr/view.php?ud=2025102712243731234aca46cea9_23
10. 허강우. (2023, August 2). 日 열도에 부는 K-뷰티 스킨케어 바람. 코스모닝. https://cosmorning.com/news/article.html?no=46122
11. 허강우. (2026a, January 10). 식약처가 분석한 2025년 대한민국 화장품 수출 실적. 코스모닝. https://www.cosmorning.com/news/article.html?no=51971
12. 허강우. (2026b, February 3). 2025년 국가별 화장품 수출실적 분석 ①. 코스모닝. https://www.cosmorning.com/news/article.html?no=52127
13. 홍태호, 나우한잉, 임강, & 박지영. (2018). LDA를 이용한 온라인 리뷰의 다중 토픽별 감성분석: TripAdvisor 사례를 중심으로. 한국정보시스템학회지, 27(1), 89–110. https://koreascience.or.kr/article/JAKO201811648108694.page
14. KOTRA 로스앤젤레스무역관, Choi, J., & 김서원. (2025). 미국 화장품 시장 트렌드 및 K-뷰티 진출전략 (Global Market Report No. 25-051). KOTRA. https://dl.kotra.or.kr/pyxis-api/2/digital-files/0add9e23-d133-448e-90f3-88b3c2f5db86
15. KOTRA 모스크바무역관. (2026, March 6). 변화하는 러시아 뷰티 시장, 2026년 주목할 트렌드는? KOTRA 해외시장뉴스. https://dream.kotra.or.kr/dream/kotra/actionKotraShortUrl/iXiEJH6PTbg3.do
16. Andrei, F. (2025). Sensory science in cosmetics. In S. P. Kartal (Ed.), *Cosmetic industry - Trends, products and quality control*. IntechOpen. https://doi.org/10.5772/intechopen.1009804
17. IMARC Group. (2026). *Japan flavors and fragrances market report by product type (nature-identical, artificial, natural), form (liquid, dry), application (flavors, fragrances), ingredient (natural, synthetic), and region 2026-2034* (Report No. SR112026A19521). https://www.imarcgroup.com/japan-flavors-fragrances-market
18. IndexBox. (2026). *Russia moisturizing skincare products: Market analysis, forecast, size, trends and insights*. https://www.indexbox.io/store/russia-moisturizing-skincare-products-market-analysis-forecast-size-trends-and-insights/
19. Названы страны-лидеры по поставкам косметики в Россию [Leading countries supplying cosmetics to Russia named]. (2025, December 13). News.ru. https://news.ru/society/nazvany-strany-lidery-po-postavkam-kosmetiki-v-rossiyu
20. PowerReviews. (2021). *The ever-growing power of reviews*. https://www.powerreviews.com/the-ever-growing-power-of-reviews/

## 회고

- **새로 익힌 기법:** 외국어 리뷰를 번역·정제해 **형태소 분석(Kiwi) → TF-IDF + 로지스틱 회귀 계수로 후보 단어를 찾고 속성 사전(9개 속성 · 811개 키워드)으로 구조화**하는 방법, **국가 × 속성 상호작용 파생변수를 우도비검정(LRT)으로 블록 단위로 검증**하는 방법(한국 기준 국가 더미, 국가별 총 오즈비 계산), 그리고 머신러닝을 **기존 제품군 내 새 리뷰(메인 Test)와 처음 보는 제품(Group Holdout 20회 반복)으로 이중 검증**하는 방법이다.
- **막혔던 지점과 해결 방법:**
  - ① 별점 높은 순 100건 + 낮은 순 100건의 1:1 수집이 일본·러시아에서는 설계대로 되지 않아(낮은 순 수집분에 4·5점이 섞임) 국가별 만족률 차이를 소비자 만족도 차이로 볼 수 없었다 → 국가 자체의 효과 대신 **"국가에 따라 속성 효과가 달라지는가(국가 × 속성 상호작용)"** 로 질문을 바꿨다.
  - ② 국가 효과에 제품 구성 차이와 플랫폼 차이가 섞여 있었다 → **제품 고정효과를 통제한 모형을 따로 적합**해 가설 판정 4개가 모두 유지됨을 확인했다.
  - ③ 보안 정책(`llvmlite.dll` 차단)으로 `shap` 패키지가 실행되지 않았다 → **XGBoost Native TreeSHAP으로 계산**하고 SHAP 합이 모델 출력과 일치함을 검증했다(최대 오차 2.6 × 10⁻⁶).
  - ④ EDA, 로지스틱 회귀, SHAP의 결과가 서로 달라 보였다 → 세 분석이 **서로 다른 질문**(전체적 관련성 / 독립적 효과 / 예측 기여도)을 검정하고 있음을 정리하고, 효과 크기와 국가 비교는 로지스틱 회귀를 기준으로 삼았다.
- **다음 프로젝트에서 보완할 점:** 한계로 정리한 다섯 가지를 보완하고 싶다. ① 별점 분포를 반영하지 못한 수집 설계는 **표본을 추가 수집**하고, ② 국가별 플랫폼이 1곳이라 국가 효과와 플랫폼 효과를 분리할 수 없었으므로 **국가별 다중 플랫폼**을 수집하고 수출 순위국(중국) 데이터를 확보하며, ③ 국가별 상위 제품이 서로 달라 차이를 문화 효과로 단정할 수 없었으므로 **글로벌 동일 브랜드·제품 라인 비교**를 시도하고, ④ 일본어·러시아어를 영어를 거쳐 한국어로 옮기는 2단계 번역으로 뉘앙스가 손실될 수 있어 **원어 기반 NLP**를 적용하며, ⑤ 배송·가격은 언급이 적어 불확실성이 크고 SHAP도 Test 200건 기준이라 **표본을 늘리고 성분·뷰티 트렌드 같은 제품 특성 변수를 추가**하고 싶다.
