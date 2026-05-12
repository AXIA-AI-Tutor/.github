# AXIA AI Tutor

> **AI 기반 면접·발표 코칭 플랫폼**
> 

AXIA AI Tutor는 면접, 발표, 자기소개, 스피치와 같은 실제 커뮤니케이션 상황을 준비하는 사용자를 위한 AI 코칭 서비스입니다.

사용자는 AI 코치 아바타와 함께 멀티턴 연습 세션을 진행하고, 음성 인식·피드백·세션 리포트를 통해 전달력, 논리 구조, 표현 방식을 점검하고 개선합니다.

---

## 📋 목차

- [프로젝트 개요](#-프로젝트-개요)
- [핵심 가치](#-핵심-가치)
- [주요 기능](#-주요-기능)
- [기술 스택](#-기술-스택)
- [시스템 아키텍처](#-시스템-아키텍처)
- [데이터 모델](#-데이터-모델)
- [프로젝트 구조](#-프로젝트-구조)
- [Jira Workflow](#-jira-workflow)
- [설치 및 실행](#-설치-및-실행)
- [환경 변수](#-환경-변수)
- [배포](#-배포)
- [협업 규칙](#-협업-규칙)
- [Roadmap](#-roadmap)
- [Team](#-team)
- [Security](#-security)
- [Troubleshooting](#-troubleshooting)

---

## 🌟 프로젝트 개요

AXIA AI Tutor는 사용자의 답변을 음성으로 수집하고, STT·RAG·LLM 기반 분석을 통해 질문, 피드백, 리포트를 제공하는 AI 면접·발표 코칭 플랫폼입니다.

혼자 면접이나 발표를 준비할 때는 객관적인 피드백과 반복 질문을 받기 어렵습니다. 
AXIA AI Tutor는 이 과정을 AI 코치와의 대화형 세션으로 바꾸어 반복 연습과 개선을 돕습니다.

---

## 🎯 핵심 가치

### 🧭 상황 기반 코칭

백엔드·프론트엔드·AI/ML·DevOps·모바일·QA 등 직군별 코퍼스를 RAG로 활용해 지원 직무에 맞는 질문과 피드백을 생성합니다.

### 🗣️ 말하기 역량 개선

AI가 답변에서 부족한 부분을 찾아 역할, 판단 근거, 결과, 답변 구조 등 다양한 관점으로 꼬리 질문을 생성합니다.

### 📊 세션 기반 성장 관리

세션마다 항목별 점수(구조·구체성·관련성·전달력)와 총점이 기록되어 리포트 화면에서 성장 그래프로 확인할 수 있습니다.

### 🔁 반복 연습 중심 UX

AI 코치 아바타가 TTS 음성으로 질문을 읽어주고, 사용자 답변(STT)을 실시간으로 처리하는 대화형 흐름을 제공합니다.

### 🧩 확장 가능한 3-tier 구조

Frontend → Spring Boot Backend → FastAPI AI Gateway/Worker로 역할이 분리되어, AI 모델 교체나 코퍼스 확장이 프론트엔드와 독립적으로 가능합니다.

---

## 🚀 주요 기능

### 1. 홈 화면

사용자의 현재 상태를 확인하고 세션을 시작하는 진입 화면입니다.

- AI 코치 아바타 카드
- 직군·난이도·모드(면접/발표) 선택
- 이력서·포트폴리오 문서 업로드
- 지난 세션 요약 대시보드

---

### 2. 실시간 연습 화면

AI 코치 아바타와 함께 멀티턴 면접·발표 연습을 진행하는 핵심 화면입니다.

- 동적인 AI 아바타 코치
- TTS로 질문을 읽어주는 음성 가이드
- MediaRecorder 기반 STT (Whisper large-v3-turbo)
- 실시간 오디오 파형 시각화
- 멀티턴 꼬리질문 : 기본 질문과 후속 질문을 번갈아 제공하며, 답변 내용에 따라 다음 질문의 초점을 조정합니다.
- 턴별 피드백 모달 (인라인 오버레이)
- 세션 완료 후 리포트 화면으로 이동

---

### 3. 피드백 화면

답변 별 AI 피드백을 확인하는 화면입니다.

- 답변 내용 요약 및 증거(Evidence) 제시
- 구조·구체성·관련성·전달력 4가지 점수
- 개선 예시 답변 제공
- 강점 / 개선 필요 항목 분류

---

### 4. 세션 리포트 화면

세션 전체 결과를 종합적으로 확인하는 화면입니다.

- 총점 기반 성장 그래프 (세션 히스토리)
- 항목별 점수 링 차트
- 강점 및 개선 포인트 정리
- 질문별 피드백 상세 목록
- 다음 연습 목표 제안

---

### 5. AI 기반 질문 생성 및 RAG 코퍼스

- 직군별 코퍼스 (Backend / Frontend / AI-ML / Data / DevOps / Mobile / QA / Fullstack)
- BGE-M3 임베딩(1024차원) + pgvector 유사도 검색
- 꼬리질문 전략: role / tradeoff / impact / structure_repair / delivery_rehearsal / evidence / deep_dive
- 질문 편향 방지를 위한 rotation 및 blocklist 로직

---

### 6. 인증

- Google OAuth2 소셜 로그인 (Spring Security 연동)
- 세션 쿠키(JSESSIONID) 기반 인증 유지
- Next.js `AuthGate` / `AuthProvider` 클라이언트 컴포넌트 보호

---

## 🛠 기술 스택

### Frontend

| 항목 | 버전/도구 |
| --- | --- |
| Framework | Next.js 16.2.4 (App Router) |
| Runtime | React 19.2.4 |
| Language | TypeScript 5 |
| Styling | Tailwind CSS 4 |
| State | Zustand 5 |
| HTTP | Axios |
| Vision | @mediapipe/tasks-vision (mouth cue) |
| Icons | lucide-react |
| Test | Vitest + Testing Library |
| Lint/Format | ESLint + Prettier |
| Deploy | Vercel |

### Backend

| 항목 | 버전/도구 |
| --- | --- |
| Framework | Spring Boot 3.5.14 |
| Language | Java 21 |
| Database | PostgreSQL 16 + pgvector |
| ORM | Spring Data JPA |
| Migration | Flyway |
| Auth | Spring Security + Google OAuth2 |
| Storage | Google Cloud Storage Signed URL |
| API Docs | SpringDoc OpenAPI (Swagger) |
| Deploy | Google Cloud Run |
| CI/CD | GitHub Actions |

### AI Server

| 항목 | 버전/도구 |
| --- | --- |
| Framework | FastAPI (Python) |
| STT | Whisper large-v3-turbo |
| LLM | EXAONE-3.5-7.8B (Ollama / 클라우드 LLM) |
| Embedding | BGE-M3 (1024차원, Ollama) |
| RAG | pgvector + 커스텀 코퍼스 파이프라인 |
| Session Memory | Langgraph 1.1.9 |
| Architecture | Gateway / Worker 이중 구조 |
| Deploy | Google Cloud Run |

### Infra / Collaboration

| 항목 | 도구 |
| --- | --- |
| 형상관리 | GitHub (AXIA-AI-Tutor Organization) |
| 이슈 관리 | Jira |
| 문서 | Confluence |
| CI/CD | GitHub Actions + Cloud Build |
| Frontend 배포 | Vercel |
| Backend/AI 배포 | Google Cloud Run |

---

## 🏗 시스템 아키텍처
<img width="2870" height="940" alt="image" src="https://github.com/user-attachments/assets/1d0db239-63c0-4373-a7b2-9d18dd4992f6" />

> **규칙**: 프론트엔드는 AI 서버를 직접 호출하지 않습니다. 모든 AI 관련 요청은 Spring Boot 백엔드를 통해서만 전달됩니다.
> 

---

## 🗄 데이터 모델

핵심 테이블 요약 (상세 ERD: `backend/avatar-coach/docs/erd.dbml`):

| 테이블 | 역할 |
| --- | --- |
| `users` | Google OAuth2 사용자 (provider, provider_user_id) |
| `sessions` | 연습 세션 (mode, target, difficulty, status) |
| `answers` | 턴별 답변 (transcript, 발화 지표: speech_rate, silence_count, filler_word_count, eye_contact_score, posture_score) |
| `feedbacks` | 답변별 AI 피드백 (summary, evidence, improvement_example, 4가지 점수) |
| `reports` | 세션 종합 리포트 (total_score, strengths, improvements) |
| `documents` | 이력서·포트폴리오 (GCS 저장, Signed URL 업로드 관리) |
| `session_events` | 세션 이벤트 로그 (event_type, payload_json) |
| `global_corpus_*` | RAG 코퍼스 (source, chunk, embedding 벡터) |

---

## 🧱 프로젝트 구조

Organization 내 레포지토리 구성:

```
AXIA-AI-Tutor/
├── frontend/        # Next.js App Router 프론트엔드 (Vercel 배포)
├── backend/         # Spring Boot API 서버 (Cloud Run 배포)
│   └── avatar-coach/
│       ├── src/domain/  # answer, corpus, document, feedback, report, session, user
│       └── docs/        # ERD, 배포 체크리스트, API 가이드
├── ai-server/       # FastAPI AI Gateway + Worker (Cloud Run 배포)
│   ├── app/api/     # gateway.py, ai.py (STT/LLM/TTS/RAG), rest.py
│   ├── app/services/# model_adapters, rag, session_memory, text
│   └── configs/corpus/  # 직군별 코퍼스 수집 설정
└── .github/         # Organization profile
```

### Frontend 폴더 구조

```
frontend/
├── app/                    # App Router 라우트
│   ├── page.tsx            # 홈
│   ├── live/page.tsx       # 실시간 연습
│   ├── feedback/page.tsx   # 피드백
│   ├── report/
│   │   ├── list/page.tsx         # 리포트 목록
│   │   └── [sessionId]/page.tsx  # 리포트 상세
│   └── login/page.tsx      # 로그인
├── components/
│   ├── auth/               # AuthGate, AuthProvider, LoginScreen
│   ├── home/               # HomeScreen, AvatarCard, SessionOptions, SessionSummary
│   ├── live/               # LiveScreen, CoachAvatarLive, LiveAudioWaveform, LiveMetrics
│   ├── feedback/           # FeedbackScreen, FeedbackBlock, ScoreRow
│   ├── report/             # ReportScreen, ReportListScreen, ScoreRing, TurnChart
│   ├── layout/             # BottomNav, PrototypeScreenPage
│   └── ui/                 # CoachAvatar, Toast, Dropdown, Toggle, BottomSheet
├── lib/                    # API 클라이언트, 유틸
└── types/                  # 공유 타입 (user, session, answer, feedback, report, document)
```

---

## 🗂 Jira Workflow

프로젝트 관리는 Jira를 사용합니다.

### Issue Type

- **Epic**: 큰 기능 또는 작업 묶음
- **Task**: 실제 구현 단위

### Status

해야 할 일 → 진행 중 → 검토 중 →  완료

### Automation
1. 브랜치가 생성되고 원격 저장소에서 인식되면 Task의 상태가 "해야 할 일 → 진행 중"으로 자동 변경됩니다.
2. 브랜치의 PR 생성이 원격 저장소에서 인식되면 Task의 상태가 "진행 중 → 검토 중"으로 자동 변경됩니다.
3. 브랜치의 MR 완료가 원격 저장소에서 인식되면 Task의 상태가 "검토 중 → 완료"로 자동 변경됩니다.
4. Epic 하위 업무가 모두 완료되면 상위 Epic이 자동으로 완료 처리됩니다.
---

## 🚀 설치 및 실행

### Frontend

```bash
git clone https://github.com/AXIA-AI-Tutor/frontend.git
cd frontend
npm install
cp .env.example .env.local   # 환경 변수 설정
npm run dev
```

### Backend

```bash
git clone https://github.com/AXIA-AI-Tutor/backend.git
cd backend/avatar-coach
cp .env.example .env
docker compose up -d         # PostgreSQL 16 + pgvector
./gradlew bootRun --args='--spring.profiles.active=local'
```

### AI Server

```bash
git clone https://github.com/AXIA-AI-Tutor/ai-server.git
cd ai-server
cp .env.example .env
docker compose up -d         # PostgreSQL
pip install -r requirements.txt
uvicorn app.main:app --reload
```

> **로컬 E2E 서버 실행 순서**: 
 PostgreSQL →  AI Worker → AI Gateway → Spring Boot → Next.js
> 

---

## 🔧 환경 변수

### Frontend (`.env.local`)

```
NEXT_PUBLIC_API_BASE_URL=http://localhost:8080
```

### Backend (`.env`)

```
SPRING_PROFILES_ACTIVE=local
DB_URL=jdbc:postgresql://localhost:5432/avatar_coach
DB_USERNAME=
DB_PASSWORD=
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
CORS_ALLOWED_ORIGINS=http://localhost:3000
AI_GATEWAY_BASE_URL=http://localhost:8000
INTERNAL_API_KEY=
GCS_STORAGE_ENABLED=false
```

### AI Server (`.env`)

```
APP_ENV=local
AI_SERVER_ROLE=worker        # gateway 또는 worker
DATABASE_URL=postgresql://axia:axia@localhost:5432/axia_ai
MODEL_MODE=local             
STT_MODEL_NAME=large-v3-turbo
LLM_MODEL_NAME=hf.co/LGAI-EXAONE/EXAONE-3.5-7.8B-Instruct-GGUF:Q5_K_M
EMBEDDING_PROVIDER=ollama
EMBEDDING_MODEL_NAME=bge-m3
WORKER_TOKEN=
INTERNAL_API_TOKEN=
```

---

## 📦 배포

### 배포 환경

| 서비스 | 플랫폼 | 브랜치 |
| --- | --- | --- |
| Frontend | Vercel | `main` → production, `develop` → staging |
| Backend | Google Cloud Run | `main` → production |
| AI Gateway | Google Cloud Run | `main` → production |
| AI Worker | Cloudflare Tunnel | `main` → production |
| Database | PostgreSQL 16 + pgvector | Cloud SQL |

### Branch Strategy

```
main       # production
develop    # staging
feature/*  # 기능 개발
fix/*      # 버그 수정
hotfix/*   # 긴급 수정
chore/*    # 설정 및 기타 작업
```

### Frontend 배포 흐름

```
develop 브랜치 push
→ Vercel preview 배포 (staging)
→ PR → main 머지
→ Vercel production 배포
```

### Backend / AI 배포 흐름

```
main 브랜치 push
→ Cloud Build 트리거
→ Docker 이미지 빌드
→ Google Cloud Run 배포
```

---

## 🤝 협업 규칙

### Commit Convention

```
KAN-<번호> <type>(<scope>): <요약>

type: feat | fix | refactor | style | docs | test | chore
scope: home | live | feedback | report | auth | layout | common
       session | answer | corpus | document | ai | global (백엔드)
       stt | llm | tts | rag | embedding (AI 서버)
```

예시:

```bash
KAN-183 feat(live): 이미지 기반 아바타 및 mouth cue 애니메이션 구현
KAN-232 fix(live): TTS 한국어 보이스 선택 버그 수정
KAN-221 feat(session): send question plan hints
KAN-231 feat(rag): 꼬리질문 편향 및 점수 기준 보정
```

### Pull Request Rule

```markdown
## 작업 내용

- 구현 또는 수정한 내용을 요약합니다.

## 관련 이슈

- KAN-00

## 변경 사항

- 주요 변경 사항을 작성합니다.

## 테스트

- [ ] 로컬 실행 확인
- [ ] 주요 화면 렌더링 확인
- [ ] E2E 흐름 확인 (해당 시)

## 참고 사항

- 리뷰어가 확인해야 할 내용을 작성합니다.
```

### PR Base 브랜치

- 모든 PR은 **`develop`** 브랜치를 base로 지정합니다.
- `main` 직접 머지는 hotfix 또는 develop → main 릴리스 PR에만 허용합니다.

---

## 🧭 Roadmap

### ✅ 완료

- Google OAuth2 로그인 연동
- 홈 화면 (세션 옵션, AI Avatar 카드, 지난 세션 요약)
- 실시간 연습 화면 
(멀티턴, STT, TTS, AI 아바타, 오디오 파형, 아이컨택·자세 점수 실시간 측정 (MediaPipe Vision))
- 피드백 화면 (항목별 점수, 강/약점, 개선 예시)
- 세션 리포트 (성장 그래프, 질문별 피드백 상세)
- 문서 업로드 (GCS Signed URL)
- RAG 코퍼스 파이프라인 (BGE-M3 + pgvector, 8개 직군)
- 꼬리질문 전략 7종 (rotation·blocklist 포함)
- Spring Boot ↔ FastAPI 내부 API 계약 확정
- Google Cloud Run 배포 (Backend + AI Gateway/Worker)
- Vercel 배포 (Frontend)

### 🔜 고도화 과제

- [ ]  발표 모드 전용 시나리오 구체화
- [x]  코퍼스 데이터 품질 개선 및 추가 수집
- [ ]  사용자별 성장 추이 분석 고도화
- [ ]  모바일 반응형 UX 개선

---

## 🧑‍💻 Team

| Role | Name | Responsibility |
| --- | --- | --- |
| Frontend / PM | 최재명 | Next.js 화면 구현, AI 아바타 구현, Frontend Vercel 배포, Web Speech API TTS 성능 개선 및 한국어 보이스 최적화, 한국어 음절 분석 기반 Mouth Cue 애니메이션 구현, LLM 출력 피드백 텍스트 파싱 및 정규화, E2E 테스트 주도 및 5차 반복 개선 사이클 운영 / 일정 관리, 요구사항 정리, Jira 등 협업 툴 운영 |
| Backend / Frontend | 김동규 | GCP Cloud Run 배포, GCS Signed URL 문서 문서 업로드 API, Supabase PostgreSQL 연동, Document/Answer/Feedback 저장·조회 API, GCS Bucket·CORS·IAM 설정, 운영 환경변수 및 CI/CD 구성 / TTS 구현 |
| Backend / AI | 신은찬 | Google OAuth2 인증, User/Session/Report API, FastAPI AI Gateway, 멀티턴 꼬리질문, pgvector RAG context, corpus ingest/embedding 검증 |
| AI / worker | 김슬기 | Mediapipe Vision metric 평가, FastAPI, STT/LLM/RAG, 코퍼스 파이프라인, LangGraph 멀티턴 질문 구현, CloudFlare 로컬 llm 연동 |

---

## 🔐 Security

- API Key, Secret, Token은 Repository에 커밋하지 않습니다.
- 환경 변수는 `.env.local` 또는 Cloud Run Secret Manager를 사용합니다.
- 인증은 Google OAuth2 + Spring Security 세션 기반으로 운영합니다.
- 프론트엔드는 `NEXT_PUBLIC_` prefix 환경 변수만 브라우저에 노출합니다.
- 사용자 음성·답변·세션 데이터는 별도 보안 정책에 따라 관리합니다.
- AI 서버는 Internal API Key와 Worker Token으로 외부 직접 호출을 차단합니다.

---

## 🧯 Troubleshooting

### 개발 서버가 실행되지 않는 경우 (Frontend)

```bash
rm -rf node_modules .next
npm install
npm run dev
```

### PostgreSQL 연결 실패

```bash
docker compose up -d
# 컨테이너 상태 확인
docker compose ps
```

---

## 📚 문서

| 항목 | 위치 |
| --- | --- |
| ERD | [dbdiagram >](https://dbdiagram.io/d/Copy-of-AX-AI-Tutor_ver2-69f8425cddb9320fdcc5c77f) |
| Cloud Run 배포 가이드 | `backend/avatar-coach/docs/cloud-run-deploy-checklist.md` |
| AI API 계약 | `ai-server/docs/plans/` |
| GCS 업로드 가이드 | `backend/avatar-coach/docs/gcs-signed-url-upload-guide.md` |
| Jira Board | [AXIA-AI-Tutor Jira 워크스페이스 >](https://axia-ai-tutor.atlassian.net/jira/software/projects/KAN/boards/2) |

---
