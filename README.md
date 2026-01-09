# Coding-Assistant
# [기획안] 폐쇄망 엔터프라이즈 AI 코딩 어시스턴트 구축

본 문서는 사내 보안 가이드라인을 준수하며 개발 생산성을 극대화하기 위한 **폐쇄망(Air-gapped) 환경 전용 AI 코딩 어시스턴트** 구축 기획안입니다.

---

## 1. 프로젝트 개요
* **목적:** 외부망 연결 없이 사내 인프라 내에서 동작하는 LLM 기반 코딩 지원 도구 구축
* **배경:** * ChatGPT 등 외부 AI 서비스 이용 시 소스코드 유출 위험 상존
    * 사내 고유 프레임워크 및 레거시 코드에 대한 AI의 이해도 부족 해결 필요
* **핵심 가치:** 데이터 보안(Security), 지식 자산화(Knowledge Management), 개발 효율화(Efficiency)

---

## 2. 시스템 아키텍처 (Architecture)

전체 시스템은 외부 통신이 차단된 내부 GPU 서버 계층에서 운영됩니다.

1.  **Client Layer:** IDE Extension (VS Code, IntelliJ 등)
2.  **API Gateway:** 부하 분산 및 사용자 인증
3.  **Inference Layer:** LLM 서빙 엔진 (vLLM, TGI 등)
4.  **Knowledge Layer:** RAG(Retrieval-Augmented Generation) 시스템 및 Vector DB
5.  **Data Layer:** 사내 소스코드 저장소(GitLab/Bitbucket), 사내 위키(Confluence)

---

## 3. 주요 기능 (Core Features)

### 3.1. 지능형 코드 완성 (Code Completion)
* **Context-aware Filling:** 현재 작성 중인 코드의 상하 문맥을 파악하여 실시간 코드 추천
* **Line/Function Completion:** 함수 단위의 로직 자동 생성

### 3.2. 사내 지식 기반 질의응답 (Internal RAG Q&A)
* **Internal Doc Search:** 사내 표준 라이브러리, API 명세서 기반 답변 생성
* **Legacy Code Analysis:** 기존 프로젝트 코드를 참고하여 사내 코딩 컨벤션에 맞는 가이드 제공

### 3.3. 코드 품질 및 보안 관리
* **Security Scanning:** 폐쇄망 내 보안 취약점 패턴 매칭 및 개선 제안
* **Code Refactoring:** 복잡도가 높은 코드의 구조 개선 및 가독성 향상 제안
* **Unit Test Generation:** 주요 로직에 대한 테스트 케이스 자동 생성

---

## 4. 기술 스택 (Technical Stack)

| 구분 | 추천 기술 | 비고 |
| :--- | :--- | :--- |
| **Model** | DeepSeek-Coder-33B, CodeLlama, StarCoder2 | 코딩 특화 오픈소스 모델 |
| **Inference** | vLLM, NVIDIA Triton Inference Server | 고성능 추론 및 양자화 지원 |
| **Vector DB** | Milvus, FAISS, Qdrant (On-premise) | 사내 코드/문서 임베딩 저장 |
| **Embedding** | BGE-M3, HuggingFace Local Models | 다국어 및 코드 이해도가 높은 모델 |
| **Backend** | Python (FastAPI), LangChain/LlamaIndex | RAG 파이프라인 및 API 구현 |
| **Frontend** | VS Code Extension SDK, JetBrains Plugin SDK | 사용자 인터페이스 |

---

## 5. 단계별 구축 로드맵

1.  **1단계: 환경 구축 (PoC)**
    * GPU 서버 확보 및 OS/드라이버 설치
    * 오픈소스 모델 선정 및 추론 성능 테스트
2.  **2단계: 데이터 연동 및 RAG 최적화**
    * 사내 소스코드 및 문서 임베딩(Embedding) 프로세스 자동화
    * 검색 정확도 향상을 위한 하이브리드 검색(Hybrid Search) 적용
3.  **3단계: IDE 플러그인 개발 및 배포**
    * 사내 개발자 대상 베타 테스트 진행
    * 사설 저장소(Artifact)를 통한 플러그인 배포 및 업데이트 체계 마련
4.  **4단계: 고도화**
    * 사용자 피드백 기반 미세 조정(Fine-tuning)
    * GPU 자원 모니터링 및 오토스케일링 적용

---

## 6. 기대 효과
* **완벽한 보안:** 사내 소스코드가 외부망으로 절대 유출되지 않음
* **생산성 증가:** 반복 작업 감소를 통해 비즈니스 로직 설계에 집중할 수 있는 환경 조성
* **기술 상향 평준화:** 신입 개발자도 사내 표준에 맞는 코드를 빠르게 작성 가능
