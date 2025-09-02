# 🏛️ JJIKGEO - 경복궁 문화유산 체험 플랫폼  
> AI 기반 문화유산 해설과 실시간 날씨 정보를 제공하는 스마트 관광 플랫폼  

![React](https://img.shields.io/badge/Frontend-React-blue?logo=react)  
![Node.js](https://img.shields.io/badge/Backend-Node.js-green?logo=node.js)  
![PostgreSQL](https://img.shields.io/badge/DB-PostgreSQL-blue?logo=postgresql)  
![AWS](https://img.shields.io/badge/Infra-AWS-orange?logo=amazonaws)  
![License](https://img.shields.io/badge/License-MIT-lightgrey)  

---

## 📋 프로젝트 개요  
**JJIKGEO**는 경복궁을 중심으로 한 문화유산 체험 플랫폼으로, GPS 기반 위치 서비스와 AI 해설, 실시간 날씨 정보를 통해 관광객에게 몰입형 문화 체험을 제공합니다.  

### 🎯 주요 기능  
- 📍 **GPS 기반 위치 추적** - 실시간 위치 기반 건물 정보 제공  
- 🤖 **AI 문화유산 해설** - OpenAI GPT 기반 맞춤형 해설 서비스  
- 🌦️ **실시간 날씨 정보** - 기상청 API 연동 (8시간 예보)  
- 💬 **커뮤니티 기능** - 방문 후기 및 사진 공유  
- 🏆 **스탬프 수집** - 게임화된 관광 체험  
- 🌐 **다국어 지원** - 한국어/영어 지원  

---

## 🏗️ 시스템 아키텍처  

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   React SPA     │────│   ALB Ingress    │────│  EKS Cluster    │
│  (Frontend)     │    │   (nginx)        │    │                 │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                                         │
                       ┌─────────────────────────────────┼─────────────────┐
                       │                                 │                 │
                ┌──────▼──────┐  ┌──────────────┐  ┌────▼─────┐  ┌────────▼────┐
                │   Backend   │  │   Weather    │  │   RDS    │  │ Monitoring  │
                │  (Node.js)  │  │   Server     │  │ (PostgreSQL)│ │ (Prometheus)│
                └─────────────┘  └──────────────┘  └──────────┘  └─────────────┘
```

---

## 🛠️ 기술 스택  

### Frontend  
- React 18, JavaScript (ES6+), CSS3  
- Geolocation API (GPS 서비스)  
- PWA (모바일 최적화)  

### Backend  
- Node.js + Express.js  
- Sequelize (ORM)  
- Multer (파일 업로드)  
- Axios (외부 API 호출)  

### Database  
- PostgreSQL (AWS RDS)  
- Redis (세션 관리, 캐싱)  

### Infrastructure  
- AWS EKS, ALB, ECR, RDS  
- Docker & Kubernetes  
- Helm  

### DevOps & Monitoring  
- Prometheus + Grafana (모니터링)  
- Trivy (보안 스캔)  

### External APIs  
- OpenAI GPT-4 (문화유산 해설)  
- 기상청 API (날씨 정보)  
- Google Maps API (지도 서비스)  

---

## 🚀 배포 환경  

- **메인 서비스**: [https://jjikgeo.com](https://jjikgeo.com)  
- **Grafana**: [https://grafana.jjikgeo.com](https://grafana.jjikgeo.com)  
- **Prometheus**: [https://prometheus.jjikgeo.com](https://prometheus.jjikgeo.com)  

### Kubernetes Namespaces  
- `jjikgeo` - 메인 애플리케이션  
- `monitoring` - 모니터링 스택  
- `trivy-system` - 보안 스캔  

---

## 📦 서비스 구성  

### Core Services  
- **Frontend (React SPA)** → Nginx 리버스 프록시 + API 라우팅  
- **Backend (Node.js)** → REST API, DB 연동, 파일 업로드  
- **Weather Server (Node.js)** → 기상청 API 연동, 좌표 변환, 예보 제공  

### Monitoring Stack  
- **Prometheus** → 메트릭 수집, 알림 규칙  
- **Grafana** → 대시보드 및 시각화  
- **Trivy Operator** → 컨테이너 보안 스캔  

---

## 🔧 로컬 개발 환경  

### Prerequisites  
- Node.js 18+  
- Docker & Docker Compose  
- kubectl, AWS CLI  

```bash
# 저장소 클론
git clone <repository-url>
cd kakao

# 환경 변수 설정
cp .env.example .env

# 의존성 설치
cd finalfront && npm install
cd ../finalbackend && npm install
cd ../weather-server && npm install
```

### 실행 방법  
```bash
# Frontend
cd finalfront && npm start

# Backend
cd finalbackend && npm start

# Weather Server
cd weather-server && npm start
```

---

## 🐳 Docker & ☸️ Kubernetes  

```bash
# Frontend 이미지 빌드
docker build -t jjikgeo-frontend ./finalfront

# Backend 이미지 빌드
docker build -t jjikgeo-backend ./finalbackend

# Weather 서버 이미지 빌드
docker build -t weather-server ./weather-server
```

```bash
# 네임스페이스 생성
kubectl create namespace jjikgeo

# ConfigMap & Secret
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/secrets.yaml

# 서비스 배포
kubectl apply -f k8s/frontend-deployment.yaml
kubectl apply -f k8s/backend-deployment.yaml
kubectl apply -f k8s/weather-deployment.yaml
```

---

## 📊 모니터링 & 보안  

- Pod 상태 / 리소스 사용량 / 취약점 현황 모니터링  
- Grafana 대시보드 제공  
- Trivy Operator 보안 스캔 적용  

---

## 🔒 보안  

- Trivy 취약점 스캔 (모든 컨테이너 이미지)  
- 최소 권한 원칙 적용  
- HTTPS (TLS) 암호화 & WAF 적용  
- Kubernetes NetworkPolicy로 네트워크 격리  

---

## 🤝 기여  

1. 새로운 Feature 브랜치 생성  
2. 코드 작성 및 테스트  
3. Pull Request 제출  
4. 코드 리뷰 → 승인 → 메인 브랜치 병합  

📌 **코드 컨벤션**  
- JavaScript → ESLint + Prettier  
- CSS → BEM 방법론  
- Git → Conventional Commits  

---

## 📞 지원  

- **프로젝트 관리자**: [이메일]  
- **기술 지원**: [이메일]  
- **버그 리포트**: GitHub Issues  

---

## 📄 라이선스  
이 프로젝트는 **MIT License** 하에 배포됩니다.  

---

✨ **JJIKGEO - 문화유산과 기술의 만남** ✨  
