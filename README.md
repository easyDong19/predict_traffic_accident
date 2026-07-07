# predict_traffic_accident

> **대구 교통사고 피해 예측 AI 경진대회** — 사고 정보를 바탕으로 인명피해 심각도(ECLO)를 예측하는 머신러닝 프로젝트

## 📌 프로젝트 개요

대구광역시에서 발생한 교통사고 데이터를 활용해 각 사고의 **인명피해 심각도 지표(ECLO, Equivalent Casualty Loss Only)** 를 예측하는 데이터 분석·머신러닝 프로젝트입니다.

- **주제**: 대구 교통사고 피해 예측 AI 경진대회
- **예측 대상**: `ECLO` (사망·중상·경상·부상 정보를 종합한 인명피해 심각도)
- **평가 지표**: RMSLE (Root Mean Squared Log Error)
- **형태**: 팀 프로젝트 (3인) — 각자 EDA / 베이스라인 / 실험 노트북 작성

ECLO는 `사망자수`, `중상자수`, `경상자수`, `부상자수`를 종합해 산출되는 값으로, 이를 단일 모델로 직접 예측하거나 각 피해 항목을 개별 예측한 뒤 합산하는 두 가지 접근이 가능합니다. 본 프로젝트에서는 두 접근을 모두 실험했습니다.

## 🗓️ 작업 기간

**2023년 11월 20일** (하루간 집중 작업)

| 시각 | 커밋 | 내용 |
|------|------|------|
| 2023-11-20 13:18 | `7ae453e` "EDA" | 프로젝트 초기 세팅 및 EDA 노트북 |
| 2023-11-20 23:24 | `4703c3b` | 베이스라인 모델링 노트북 업로드 |

## 👥 팀 구성 및 담당

| 담당자 | 파일 | 역할 |
|--------|------|------|
| **유동연** | [`유동연/EDA.ipynb`](유동연/EDA.ipynb) | 탐색적 데이터 분석(EDA) — 가해운전자 성별·연령대 분포 등 시각화 |
| **심규일** | [`심규일/baseline.ipynb`](심규일/baseline.ipynb) | 베이스라인 파이프라인 — 파생변수 생성, 외부 데이터 결합, 전국 사고 데이터 증강, 모델 학습 및 제출 |
| **최은학** | [`최은학/Practice.ipynb`](최은학/Practice.ipynb) | 피해 항목별(사망·중상·경상·부상) 개별 예측 실험, 이상치 제거, 유효성 검증 |

## 🔧 주요 작업 내용

### 1. 탐색적 데이터 분석 (EDA)
- 가해운전자 성별 / 연령대 분포 분석 및 시각화
- 한글 폰트 설정(AppleGothic / MaruBuri) 등 시각화 환경 구성
- 학습·테스트 데이터의 기간 및 컬럼 불일치(기상상태, 시군구 등) 점검

### 2. 특성 공학 (Feature Engineering)
- **`사고일시`** → 연·월·일·시간 파생변수 추출 (`str.extract`, `to_datetime`)
- **`시군구`** → 도시·구·동 공간 정보 분리
- **`도로형태`** → 도로형태1·도로형태2 로 분리
- 범주형 변수 인코딩 (`LabelEncoder`, `get_dummies`)

### 3. 외부 데이터 결합
동 단위로 집계해 병합한 대구광역시 공공 데이터:
- 🔦 대구 보안등 정보 (설치개수)
- 🚸 대구 어린이 보호구역 정보
- 🅿️ 대구 주차장 정보 (급지구분)

### 4. 데이터 증강
- 전국 교통사고 데이터(`countrywide_accident.csv`)를 대구 데이터에 결합해 학습 데이터 규모 확대

### 5. 모델링
- **베이스라인**: `DecisionTreeRegressor`, `RandomForestRegressor`, `XGBoost` 비교
- **개별 예측 접근**: 사망/중상/경상/부상자수를 각각 `RandomForestRegressor`로 예측한 뒤 ECLO 합산
- 이상치 제거(IQR 기반), 검증셋 분리(`train_test_split`) 및 RMSLE 평가

## 🛠️ 기술 스택

- **언어/환경**: Python, Jupyter Notebook
- **데이터 처리**: pandas, numpy
- **시각화**: matplotlib, seaborn
- **머신러닝**: scikit-learn (RandomForest, DecisionTree), XGBoost

## 📁 디렉터리 구조

```
predict_traffic_accident/
├── README.md
├── 유동연/
│   └── EDA.ipynb          # 탐색적 데이터 분석
├── 심규일/
│   └── baseline.ipynb     # 베이스라인 모델링 파이프라인
└── 최은학/
    └── Practice.ipynb     # 피해 항목별 개별 예측 실험
```

> ⚠️ 원본 데이터(`train.csv`, `test.csv`, `external_open/`, `countrywide_accident.csv` 등)는 리포지토리에 포함되어 있지 않으며, 대회 페이지에서 별도로 내려받아야 합니다.
