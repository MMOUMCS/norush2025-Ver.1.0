# 🚇 NoRush (졸업작품)

> 서울교통공사 및 SK OpenAPI 기반 지하철 칸별 혼잡도 예측 및 경로 안내 시스템
팀 NoRush
---

## 💡 프로젝트 소개

<div align="center">
<img src="./assets/norushapp.png" width="30%" />
</div>

**NoRush**는 "가장 빠른 경로"가 아닌  
**덜 붐비는 쾌적한 경로**를 찾고 싶은 사용자를 위한  
**지하철 혼잡도 기반 경로 추천 서비스**입니다.

공공 데이터와 혼잡도 데이터를 활용하여  
**시간 · 역 · 호선 · 칸별 혼잡도를 예측**하고  
이를 기반으로 경로의 혼잡도를 계산합니다.

사용자는

- 출발역
- 도착역
- 출발 시간

을 입력하면

- **최소 혼잡 경로**
- **균형 추천 경로 (시간 + 혼잡도)**
- **최단 시간 경로**

를 비교하여 추천받을 수 있습니다.


## 📅 Project Overview

| 항목 | 상세 내용 |
| :--- | :--- |
| **개발 인원** | 팀 프로젝트 (총 6인 개발) |
| **주요 역할** | AI 모델링, 데이터 엔지니어링 및 백엔드 API(엔드포인트) 구현 |
| **서비스 환경** | 모바일 앱 (Android / iOS 지원 환경) |
| **소스 코드** | [[Backend Repository](https://github.com/MMOUMCS/norush2025-Ver.1.0) / [Frontend Repository](https://github.com/MMOUMCS/NoRush_Front-Ver.1.0) / [AI Repository](https://github.com/MMOUMCS/norush2025-ai-Ver.1.0)] |

*이후 AI의 실제 모델.apk는 huggingface로 이전했습니다. AI 모델에 관한 설명은 AI 레포지토리에 저장하였습니다.*

# 🛠️ 담당 업무 및 성과
**다기능 데이터 전처리 및 데이터셋 구축** <br>
서울교통공사(역별 데이터)와 SK 행정 데이터(칸별 데이터)를 수집하여 데이터 결합 및 정제 수행. <br>
평균 제곱근 오차 평가 기반(예측 결과의 평균오차 평가)의 Random Forest 모델(조건부 데이터를 의사결정에 반영하기 위함)을 활용하여 역별/호선별 승하차 인원을 예측하고, 범주형 변수(호선, 역 이름)의 수치 인코딩 처리. <br> 
혼잡도 예측 정확도를 높이기 위해 독립변수(시간대, 역 정보, 노선 더미)와 가중치 타깃 변수(평균 승객수)를 정의하고, 환승역·업무 지구 여부 및 피크 타임(07-08시, 18-19시) 가중치 피처 엔지니어링 적용.<br>

**예측 모델 서빙 및 백엔드 엔드포인트 구현**<br>
출발역, 도착역, 노선, 탑승 시간을 입력받아 1~10호차의 칸별 승객수 및 혼잡도를 산출하는 AI 예측 API 엔드포인트 구현.<br>
Kakao Maps API를 연동하여 지도 상에 최적 경로 및 역별 실시간 혼잡도 시각화 데이터를 프론트엔드에 정합성 있게 서빙.<br>

**Full-Stack 연동 및 데이터 흐름 최적화**<br>
Postman을 활용하여 백엔드 REST API의 HTTP 메서드(GET/POST)별 데이터 정합성을 검증하고 프론트엔드-백엔드 간 밀결합 연동 총괄.<br>
별도의 무거운 DB 트랜잭션 없이 실시간성을 보장하기 위해, 사용자가 지정한 출발지/목적지 기반의 실시간 재검색 방식의 경량화된 즐겨찾기 라우팅 로직 구현.<br>

**Render 서버 배포**<br>

---

# 🦾 주요 기능

## 로그인/회원가입
<img width="380" height="824" alt="GIF_20251117_012724_776 (1)" src="https://github.com/user-attachments/assets/b397ac41-dfe9-4f57-accd-55277354ae05" />
<img width="380" height="824" alt="GIF_20251117_012753_076 (1)" src="https://github.com/user-attachments/assets/c7c19c01-7242-4951-a465-1cf1c6bb26b1" />

## 🧭 혼잡도 기반 경로 추천
<img width="380" height="824" alt="GIF_20251117_012853_591 (1)" src="https://github.com/user-attachments/assets/84c3f98b-e394-47ee-9e1f-282f00e7f7a1" />

대중교통 API와 AI 예측 서버를 활용하여

- 시간대별 혼잡도
- 역별 혼잡도
- 칸별 혼잡도

를 계산하고 **혼잡도 + 소요 시간 기반 추천 경로**를 제공합니다.

<img width="380" height="824" alt="GIF_20251117_013318_514 (1)" src="https://github.com/user-attachments/assets/a98a7b2f-e22d-4b57-89a1-8588b16267f5" />

---

## 📊 혼잡도 예측 모델

공공 데이터를 활용하여 다음 특징을 기반으로 학습합니다.

- 연도 / 월 / 요일 / 시간대
- 호선 / 역
- 상행 / 하행
- 칸 번호

FastAPI 기반 **AI 서버**에서 예측 모델을 제공하며  
Backend에서 실시간으로 호출합니다.

---

## 예외처리
<img width="380" height="824" alt="GIF_20251117_012834_563 (1)" src="https://github.com/user-attachments/assets/1180744c-c63f-4bfb-b04c-f2b4d0e0f5e7" />


## 🗺️ 경로 및 혼잡도 시각화

프론트엔드에서는 모바일 환경에서 사용자가 직관적으로 확인할 수 있도록

- 지도 기반 경로 표시
- 혼잡도 색상 표시
- 예상 소요 시간
- 경로별 혼잡도 점수

를 제공합니다.

---

### ⭐ 사용자 맞춤 기능 (확장 가능)
- 자주 이용하는 경로를 저장하는 **즐겨찾기 기능**
- 시간대별 혼잡도 패턴을 기반으로 한 **출근·퇴근 추천 시간대 안내**
- 장애인·노약자 등 교통 약자를 위한 **혼잡도 최소화 모드**  
  등으로 확장 가능하며, 추후 다양한 대중교통 수단(버스, 환승 연동 등)과의 통합도 고려하고 있습니다.

---

# 🔄 시스템 아키텍처

### Backend - AI 서버 구조

<img src="./assets/system.png" width="60%" />

- **Main Backend (Spring Boot)**  
  사용자 인증, 즐겨찾기 관리, 서비스 로직

- **AI Predict Server (Python/FastAPI)**  
  혼잡도 예측 모델 API 제공

AI 서버는 **Render**를 통해 독립 배포됩니다.

---

### 경로 조회 데이터 흐름

<img src="./assets/route.png" width="60%" />

---

## 🚀 CI/CD

NoRush 프로젝트는 **GitHub Actions와 EC2 SSH 배포**를 이용해  
코드 변경 시 자동 빌드 및 배포가 이루어지도록 구성했습니다.

### 배포 과정

1. `main` 브랜치에 코드 Push
2. **GitHub Actions** 워크플로우 실행
3. GitHub Actions에서 **EC2 서버로 SSH 접속**
4. 서버에서 프로젝트 빌드 진행
5. `deploy.sh` 실행
6. 애플리케이션 자동 재배포

### 사용 기술

- **GitHub Actions** : CI/CD 파이프라인 관리
- **EC2** : 애플리케이션 서버
- **SSH** : 원격 배포 실행
- **deploy.sh** : 서버 빌드 및 서비스 재시작 스크립트

---

# 🚇 혼잡도 모델

## 1️⃣ 칸별 혼잡도 모델

### 데이터 수집

<img src="./assets/cogmodel1.png" width="60%" />

퍼즐(Puzzle) 데이터 기반 혼잡도 수집

---

### 데이터 전처리

<img src="./assets/cogmodel2.png" width="60%" />

Puzzle 응답을 CSV 형태로 변환 후 가공

---

### 모델 학습

<img src="./assets/cogmodel3.png" width="60%" />

칸별 혼잡도 예측 모델 학습 및 API 제공

---

## 2️⃣ 역별 혼잡도 모델

### 데이터 수집

<img src="./assets/cogmodel4.png" width="60%" />

서울교통공사 데이터 기반 수집

---

### 데이터 학습

<img src="./assets/cogmodel5.png" width="60%" />

승하차 데이터를 기반으로 학습

---

### 예측 모델

<img src="./assets/cogmodel6.png" width="60%" />

지하철 승하차 인원 예측 모델

---

## ⚠️ 주요 에러 및 향후 개선방안

<img src="./assets/cogmodel7.png" width="60%" />

데이터 처리 및 모델 학습 과정에서 발생한 주요 오류 사례

<img src="./assets/feedback.png" width="60%" />

향후 개선방안 

---

## 🧰 개발 IDE & 기술 스택

NoRush 프로젝트는 **Frontend / Backend / AI / Deploy / 공통 환경**으로 구성되어 있으며, 모든 팀원이 동일한 개발 환경을 유지할 수 있도록 아래와 같이 정리했습니다.


### 🖥 Frontend

| 역할 | 구성 |
|------|------|
| **Language** | ![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=white) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white) |
| **IDE** | ![VSCode](https://img.shields.io/badge/VSCode-007ACC?style=for-the-badge&logo=visualstudiocode&logoColor=white) |
| **Framework** | ![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=white) |



### 🖥 Backend

| 역할 | 구성 |
|------|------|
| **Language** | ![Java](https://img.shields.io/badge/Java-007396?style=for-the-badge&logo=openjdk&logoColor=white) |
| **Framework** | ![SpringBoot](https://img.shields.io/badge/SpringBoot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white) |
| **IDE** | ![IntelliJ](https://img.shields.io/badge/IntelliJ_IDEA-000000?style=for-the-badge&logo=intellijidea&logoColor=white) |
| **Build Tool** | ![Gradle](https://img.shields.io/badge/Gradle-02303A?style=for-the-badge&logo=gradle&logoColor=white) |
| **DB** | ![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white) |
| **API** | ![REST API](https://img.shields.io/badge/REST_API-000000?style=for-the-badge&logo=swagger&logoColor=white) |



### 🖥 AI Server

| 역할 | 구성 |
|------|------|
| **Language** | ![Python](https://img.shields.io/badge/Python_3.10-3776AB?style=for-the-badge&logo=python&logoColor=white) |
| **IDE** | ![PyCharm](https://img.shields.io/badge/PyCharm-000000?style=for-the-badge&logo=pycharm&logoColor=white) |
| **Framework** | ![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white) |
| **AI Library** | ![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white) |
| **Serving** | ![Render](https://img.shields.io/badge/Render-00979D?style=for-the-badge&logo=render&logoColor=white) |
| **Environment** | CUDA / cuDNN / Ubuntu |



### 🛠 Deployment 환경

| 역할 | 구성 |
|------|------|
| **Backend Deploy** | ![Amazon EC2](https://img.shields.io/badge/Amazon_EC2-FF9900?style=for-the-badge&logo=amazonec2&logoColor=white) ![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white) ![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white) ![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white) |
| **Frontend Deploy** | (필요 시) S3 + CloudFront 또는 기타 배포 플랫폼 |
| **AI Deploy** | ![Render](https://img.shields.io/badge/Render-00979D?style=for-the-badge&logo=render&logoColor=white) |
| **CI/CD** | ![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2496ED?style=for-the-badge&logo=githubactions&logoColor=white) |



### 🧩 공통 협업 환경

| 역할 | 구성 |
|------|------|
| **Communication** | ![Notion](https://img.shields.io/badge/Notion-000000?style=for-the-badge&logo=notion&logoColor=white) ![Discord](https://img.shields.io/badge/Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white) |
| **Design** | ![Figma](https://img.shields.io/badge/Figma-F24E1E?style=for-the-badge&logo=figma&logoColor=white) |
| **Version Control** | ![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white) ![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white) |
