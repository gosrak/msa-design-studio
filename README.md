<h1 align="center">MSA Design Studio</h1>

<p align="center">
  <strong>이벤트 스토밍 + DDD + 헥사고날 아키텍처 + AI 어시스트로 MSA 설계를 가이드하는 웹 저작도구</strong><br>
  <em>Event Storming · Domain-Driven Design · Hexagonal Architecture · AI-assisted Microservice Design Studio</em>
</p>

<p align="center">
  <a href="https://msa-dev.fact-mine.com">
    <img alt="Live demo" src="https://img.shields.io/badge/▶_Live_Demo-msa--dev.fact--mine.com-F5A623?style=for-the-badge&labelColor=0F1729">
  </a>
</p>

<p align="center">
  <a href="#"><img alt="Stack" src="https://img.shields.io/badge/Stack-Express_·_FastAPI_·_Ollama-2E868C?labelColor=222D45"></a>
  <a href="#"><img alt="LLM" src="https://img.shields.io/badge/LLM-Gemma_27B_+_RAG-F5A623?labelColor=222D45"></a>
  <a href="#"><img alt="Docs" src="https://img.shields.io/badge/Docs-Korean_·_English-0EA5E9?labelColor=222D45"></a>
  <a href="LICENSE"><img alt="License" src="https://img.shields.io/badge/License-Proprietary_·_Demo_Free-EC4899?labelColor=222D45"></a>
</p>

---

## ✨ What is this?

**MSA Design Studio** 는 알베르토 브란돌리니의 **이벤트 스토밍** 워크숍 기법을 디지털 캔버스로 옮긴 도구입니다.
도메인 이벤트(과거형)를 출발점으로 액터·커맨드·애그리거트·정책을 6단계로 채우면서 자연스럽게 **바운디드 컨텍스트를 식별**하고, 최종적으로 **헥사고날 아키텍처 다이어그램**과 **마이크로서비스 명세**를 자동으로 도출합니다.

> 한 마디로: **엑셀 한 장 → 스토밍 보드 → 7종 산출물(XLSX·PPTX) → MSA 청사진**

🔗 **데모 사이트**: [https://msa-dev.fact-mine.com](https://msa-dev.fact-mine.com)

---

## 📸 Screenshots

### 1) 인트로 스플래시 — 첫 방문 안내

이벤트 스토밍 6단계 워크플로우와 7가지 산출물을 한눈에 보여주는 시작 화면.

![Intro Splash](docs/images/01-intro-splash.png)

### 2) 메인 스토밍 보드 + AI 검토 패널

좌측 BC 트리 + 중앙 이벤트 카드 (이벤트·커맨드·액터·정책·핫스팟·외부) + 우측 ⚡ AI 검토 패널 — 명명 일관성·누락 필드·Saga 흐름·BC 결합도 자동 분석 리포트.

![Board with AI Review](docs/images/02-board-ai-review.png)

### 3) 헥사고날 아키텍처 화면 (BC 단위 자동 생성)

DDD 3계층(Domain · Application · Infrastructure)에 매핑된 3중 nested 헥사곤 + Driving/Driven 색상 띠 + 4 I/F 박스 + Command/Event 슬롯 + 16종 화살표 + OR Mapper + DB. **PPT 출력 시 BC 1개 = 3 슬라이드** (BFD-A · BFD-B · 헥사곤).

![Hexagonal Architecture](docs/images/03-hexagonal.png)

### 4) AI 채팅 + RAG (출처 인용)

로컬 Gemma 27B + BGE-M3 + ChromaDB 8개 MSA 기술문서 인덱스로 구성된 RAG. 답변에 자동 `[출처: 04-saga.md]` 형태의 청크 인용 포함.

![Board with AI Chat](docs/images/04-board-ai-chat.png)

---

## 🎯 Why this exists

엔터프라이즈 환경에서 **모놀리식 시스템을 MSA 로 분해**할 때 가장 큰 장벽은:

1. 도메인 전문가와 개발자 간 **공통 언어 부재** (DDD ubiquitous language)
2. **이벤트·커맨드·애그리거트 식별** 의 비체계성
3. 결국 BC 도출이 직관에 의존 → 마이크로 모놀리식·God BC·순환 의존
4. PPT/Excel 다이어그램 작성에 **기획자 시간 80% 낭비**

이 도구는 그 모든 단계를 한 화면에서 처리하고, 결과물을 **PPTX 30 슬라이드 + XLSX 8 시트**로 자동 생성합니다.

---

## 🧩 Core Features

### 1. 6단계 이벤트 스토밍 워크플로우

| 단계 | 결과물 | 색상 | 핵심 질문 |
|------|--------|------|-----------|
| ① **이벤트 도출** | 오렌지 포스트잇 (과거형) | `#F5A623` | "시스템에서 어떤 일이 일어나는가?" |
| ② **＋ 액터 식별** | 노랑 포스트잇 | `#FFE066` | "누가/무엇이 이벤트를 발생시키는가?" |
| ③ **＋ 커맨드 도출** | 민트 포스트잇 | `#6BCFE5` | "어떤 명령이 이벤트를 트리거하는가?" |
| ④ **＋ 애그리거트** | 연두 포스트잇 | `#A0E050` | "어떤 도메인 객체가 일관성을 유지하는가?" |
| ⑤ **＋ 외부/정책/핫스팟** | 핑크/보라/핫핑크 | — | "외부 의존, 정책, 미해결 의문" |
| ⑥ **＋ 바운디드 컨텍스트** | 점선 박스 | — | "서브도메인 → 마이크로서비스 후보" |

### 2. 7가지 산출물 자동 출력 (XLSX · PPTX)

| # | 산출물 | XLSX | PPTX | 슬라이드 수 |
|---|--------|:----:|:----:|:----:|
| ① | 이벤트 도출 | ✅ | ✅ | 도메인별 그리드 |
| ② | 이벤트 요소 도출 | ✅ | ✅ | 한 페이지 한 클러스터 (7요소 포스트잇) |
| ③ | 액터 도출 | ✅ | ✅ | 액터 카드 |
| ④ | 외부시스템 정의 | ✅ | ✅ | 메타 표 (유형·프로토콜·인증·SLA) |
| ⑤ | 데이터저장소 정의 | ✅ | ✅ | 카테고리별 색상 카드 |
| ⑥ | 바운디드 컨텍스트 | ✅ | ✅ | MS 후보 카드 |
| ⑦ | **헥사고날 아키텍처** | ✅ | ✅ | **BC 1개 = 3 슬라이드 (BFD·BFD·헥사 다이어그램)** |
| ★ | 통합 XLSX | ✅ | — | 전체 7시트 |

### 3. 헥사고날 다이어그램 (16:9 PPT 출력)

DDD 3계층(Domain · Application · Infrastructure)에 매핑된 3중 nested 헥사곤 + Driving/Driven 측면 색상 띠 + 4개 I/F 박스(Service Inbound · Proxy/Event/Repository Outbound) + 16종 화살표 + Command/Event 6슬롯 + ApplicationService 유스케이스 스트립.

### 4. AI 어시스트 (로컬 LLM + RAG)

우측 floating ⚡ 패널에 3 모드:

- **💬 채팅** — 자유 질문, RAG로 8개 MSA 기술문서 자동 인용 (`[출처: 04-saga.md]`)
- **⚡ 이벤트 추출** — 자연어 도메인 묘사 → 이벤트/액터/외부/저장소 JSON → 클릭 한 번에 보드 추가
- **🔍 검토** — 현재 state 분석 → 명명 일관성·누락 필드·Saga·핫스팟 마크다운 리포트

> 백엔드: **Gemma 27B + BGE-M3 임베딩 + ChromaDB** — 모든 추론·임베딩이 로컬에서 실행, 외부 API 호출 0회.

### 5. 엑셀 워크플로우

- 📋 **양식지 다운로드** — 5 시트 빈 양식 (전체이벤트 + 마스터 3종 + 작성가이드)
- 📂 **엑셀 불러오기** — 17 컬럼 자동 매핑, 다중값 필드 자동 분할
- ⬇ **산출물 출력** — 7종 XLSX + 7종 PPTX + 통합 XLSX 일괄 출력

---

## 🚀 Quick Start

데모 사이트에서 바로 체험:

➡️ [https://msa-dev.fact-mine.com](https://msa-dev.fact-mine.com)

1. 사이트 접속 → 인트로 스플래시 → **시작하기** 클릭
2. 「온라인쇼핑몰」 샘플 (10 도메인 / 83 이벤트) 자동 로드
3. 우측 ⚡ 버튼 → AI 어시스트 활성화 (로컬 백엔드 환경)
4. ⬇ 산출물 출력 → PPTX/XLSX 다운로드

---

## 📑 Sample Outputs

### Live HTML — 「온라인쇼핑몰」 이벤트 스토밍 결과

샘플 결과물을 그대로 웹에서 확인할 수 있습니다.

📂 [`samples/01-ecommerce-event-storming.html`](samples/01-ecommerce-event-storming.html) — 이벤트 스토밍 보드를 그대로 webpage 로 export 한 결과 (인쇄 가능)

📂 [`samples/01-ecommerce-events.xlsx`](samples/01-ecommerce-events.xlsx) — 입력 데이터 (이 도구의 표준 입력 형식)

`samples/01-ecommerce-event-storming.html` 을 다운로드 후 브라우저로 열면 다음 도메인의 풀 스토밍 결과를 볼 수 있습니다:

| BC | 서브도메인 | 이벤트 | 핵심 흐름 |
|----|-----------|-------:|-----------|
| 회원 | 가입·인증·프로필·등급 | 15 | 본인인증 + 회원가입 + JWT + 등급 산정 |
| 상품 | 카테고리·상품·재고·검색 | 12 | 등록·색인 + 재고 분산락 + 추천 |
| 장바구니 | 장바구니·위시리스트 | 5 | Redis 카트 (TTL 7일) |
| 주문 | 주문·취소·조회 | 12 | Saga 시작 → 결제 → 확정 → 보상 |
| 결제 | 처리·환불·결제수단 | 10 | PG 연동 + Idempotency-Key + 빌링키 |
| 배송 | 요청·추적·반품교환 | 8 | 택배사 Webhook + D+7 자동 구매확정 |
| 리뷰 | 후기·평점 | 5 | 구매확정자 한정 + 평점 평균 갱신 |
| 프로모션 | 쿠폰·적립금 | 6 | 선착순 락 + 1% 적립 |
| 알림 | 트랜잭션·마케팅 | 5 | order.confirmed → SMS·SES·FCM |
| 정산 | 매출분석·정산 | 5 | 일별 정산 + ERP 연동 |

---

## 📐 Architecture

```
┌──────────────────┐        ┌──────────────────┐        ┌──────────────────┐
│  브라우저         │───────▶│ Express (web/)   │───────▶│ Python AI 서버   │
│ event_storming   │  /api  │  proxy + auth    │ HTTP   │ FastAPI (ai/)    │
└──────────────────┘ /assist└──────────────────┘        └────────┬─────────┘
                                                                  │
                              ┌──────────────────────────┬───────┼───────┐
                              ▼                          ▼       ▼       ▼
                    ┌─────────────────┐  ┌────────────────┐ ┌─────────┐ ┌──────┐
                    │ Ollama          │  │ ChromaDB       │ │ BGE-M3  │ │ Local│
                    │ Gemma 27B + LoRA│  │ RAG 인덱스     │ │ Embedder│ │ Files│
                    └─────────────────┘  └────────────────┘ └─────────┘ └──────┘
```

- **클라이언트**: HTML/CSS/JS 단일 파일, Pretendard 폰트, 다크 네이비 + 오렌지/틸 톤
- **Express (Node.js)**: 정적 호스팅 + `/api/*` 프록시 + 보안 헤더(helmet)
- **Python (FastAPI)**: PPTX/XLSX 생성, RAG 검색, LLM 호출
- **Ollama**: Gemma 27B Q4_K_M (~16GB VRAM)
- **ChromaDB + BGE-M3**: 한국어/영어 다국어 임베딩 + cosine 검색
- **모든 데이터 처리는 로컬** — 외부 API 호출 0회 (보안 민감 도메인 적합)

---

## 📚 Documentation

- 📖 [기능 상세](docs/features.md) — 7가지 산출물·헥사고날·AI 어시스트
- 🔄 [6단계 워크플로우](docs/workflow.md) — 이벤트 스토밍 절차
- 📊 [산출물 가이드](docs/outputs.md) — XLSX/PPTX 시트별 의미
- 🤖 [AI 어시스트 사용법](docs/ai-assist.md) — 채팅/추출/검토 모드

---

## 🎨 Design

다크 네이비 + 오렌지 액센트 + Pretendard 폰트의 모던 미니멀 디자인.

- 배경: `#0F1729` → `#1A2542` → `#222D45` (gradient + grid overlay)
- 액센트: `#F5A623` (오렌지), `#2E868C` (틸), `#E64C3C` (레드)
- 보드: `#F5F1E8` (크림 — 포스트잇 가독성 보존)
- ES 표준 스티키: 오렌지·민트·노랑·연두·보라·핑크·핫핑크

---

## 🔒 Source Code

이 저장소는 **소개·문서·샘플 전용** 입니다. 실제 소스 코드는 비공개 저장소에서 관리되며, 데모 사이트에서 모든 기능을 사용할 수 있습니다.

- 🌐 라이브 데모 — [msa-dev.fact-mine.com](https://msa-dev.fact-mine.com) (무료, 로그인 없음)
- 💼 엔터프라이즈/온프레미스 도입 문의 — Issue 또는 데모 사이트 연락처

---

## 📜 License

이 저장소(문서·샘플)는 자유롭게 참고·인용 가능합니다.

데모 사이트는 무료로 사용 가능하며, 작성한 데이터는 사용자 브라우저(localStorage)에만 저장됩니다.

자세한 내용은 [LICENSE](LICENSE) 참고.

---

## 🙏 Credits

- **Event Storming** — Alberto Brandolini (2013)
- **Hexagonal Architecture** — Alistair Cockburn (2005)
- **Domain-Driven Design** — Eric Evans (2003)
- **Saga Pattern** — Hector Garcia-Molina, Kenneth Salem (1987)
- **Pretendard 폰트** — orioncactus

---

<p align="center">
  <strong>🌐 <a href="https://msa-dev.fact-mine.com">msa-dev.fact-mine.com</a> 에서 직접 사용해보세요</strong>
</p>
