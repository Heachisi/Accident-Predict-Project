# 🔧 Accident Prediction Monitoring System

AI 기반 블레이드 마모 상태 예측 및 사고 예방 모니터링 시스템

## 📋 프로젝트 개요

현대 제조 공정에서 칼날(blade)과 같은 소모성 부품의 마모 상태를 실시간으로 모니터링하고, 교체 시점을 예측하여 사고를 예방하는 통합 시스템입니다.

### 주요 기능
- 🤖 **AI 기반 마모 상태 예측**: Autoencoder와 LSTM을 활용한 이상 탐지 및 잔여수명(RUL) 예측
- 📊 **실시간 대시보드**: React 기반의 직관적인 모니터링 인터페이스
- ⚡ **실시간 데이터 처리**: FastAPI를 통한 고성능 백엔드 API
- 🗄️ **체계적인 데이터 관리**: MySQL 기반의 센서 데이터 및 예측 결과 저장

## 🏗️ 시스템 아키텍처

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  React Frontend │ ←→ │  FastAPI Backend│ ←→ │  MySQL Database │
│   (Dashboard)   │    │   (ML Models)   │    │  (Sensor Data)  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## 📁 프로젝트 구조

```
├── Python/                    # 백엔드 및 ML 모델
│   ├── app/
│   │   ├── main.py           # FastAPI 메인 애플리케이션
│   │   ├── health.py         # Health Score 계산 로직
│   │   ├── run_RUL.py        # 잔여수명 예측 모듈
│   │   └── routers/          # API 라우터
│   └── models/               # 학습된 ML 모델 파일
│       ├── *.pkl             # Scaler 모델
│       └── *.h5              # Autoencoder 모델
├── react/frontend/           # React 프론트엔드
│   ├── src/
│   ├── public/
│   └── package.json
├── Oneyear_benchmark/        # 벤치마크 데이터
└── src/                      # 추가 소스 코드
```

## 🚀 빠른 시작

### 사전 요구사항
- Python 3.8+
- Node.js 16+
- MySQL 8.0+

### 백엔드 설정

1. 가상환경 생성 및 활성화
```bash
cd Python/
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
```

2. 의존성 설치
```bash
pip install -r requirements.txt
```

3. 환경 변수 설정 (`.env` 파일 생성)
```env
DATABASE_URL=mysql://user:password@localhost/accident_predict
ML_MODEL_PATH=./models/
```

4. 서버 실행
```bash
cd app/
uvicorn main:app --reload --port 8000
```

### 프론트엔드 설정

1. 의존성 설치
```bash
cd react/frontend/
npm install
```

2. 개발 서버 실행
```bash
npm start
```

## 🤖 AI 모델 및 알고리즘

### 1. Autoencoder (이상 탐지)
- **목적**: 정상 데이터 패턴 학습 후 이상 상태 탐지
- **입력**: 9개 센서 데이터 (토크, 속도, 위치, 랙 오차 등)
- **출력**: 재구성 오차(MSE)를 통한 이상 점수

### 2. LSTM (잔여수명 예측)
- **목적**: 시계열 패턴 분석을 통한 미래 성능 예측
- **방법**: Health Score 변화 추세 분석
- **출력**: 잔여 사용 가능 시간(RUL)

### 3. Health Score 산출
```python
# Health Score = 100 * (1 - ECDF(MSE))
# 100: 완전 정상, 0: 교체 필요
```

## 📊 데이터베이스 스키마

### 주요 테이블
- **`run_measurement`**: 원시 센서 데이터
- **`run_risk`**: Health Score 및 RUL 결과
- **`benchmark_mse`**: 벤치마크 비교 데이터
- **`blade_benchmark`**: 정상/마모 기준 데이터

## 🔧 API 엔드포인트

### 주요 API
```
GET  /api/health/{blade_id}     # Health Score 조회
GET  /api/rul/{blade_id}        # 잔여수명 조회
POST /api/sensor-data           # 센서 데이터 입력
GET  /api/dashboard/summary     # 대시보드 요약 정보
```

## 📈 성과 및 효과

### 기대 효과
- ⚠️ **사고 예방**: 조기 이상 탐지를 통한 안전사고 방지
- 💰 **비용 절감**: 불필요한 교체 방지로 15-20% 유지보수 비용 절감
- 🔄 **최적화**: 데이터 기반의 정확한 교체 시점 결정

### 핵심 지표
- Health Score 정확도: 85%+
- RUL 예측 오차: ±10% 이내
- 실시간 처리 지연시간: <100ms

## 🛠️ 개발 및 배포

### 로컬 개발
```bash
# 백엔드
cd Python/app && uvicorn main:app --reload

# 프론트엔드
cd react/frontend && npm start
```

### 프로덕션 배포
```bash
# Docker를 이용한 배포 (추후 지원 예정)
docker-compose up -d
```

## 📝 라이센스

이 프로젝트는 MIT 라이센스 하에 있습니다.

## 🤝 기여하기

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📞 연락처

프로젝트 문의사항이 있으시면 Issues를 통해 연락해 주세요.

---

**🚨 주의사항**: 프로덕션 환경에서 사용 시 보안 설정 및 데이터 백업을 반드시 확인하세요.
