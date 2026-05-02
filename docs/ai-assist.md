# AI 어시스트 사용 가이드

우측 하단 ⚡ 오렌지 FAB 버튼을 누르면 AI 어시스트 패널이 열립니다.

> **로컬 LLM 기반** — 모든 추론·임베딩이 사용자 서버 내부에서 실행됩니다. 외부 API 호출 0회.

## 스택

| 컴포넌트 | 모델/도구 |
|---|---|
| 추론 | **Gemma 27B Q4_K_M** via Ollama (~16GB VRAM) |
| 임베딩 | **BAAI/bge-m3** (다국어 — 한국어 강함) |
| 벡터 DB | **ChromaDB** (cosine, persistent) |
| (선택) 파인튜닝 | **Unsloth QLoRA** — 도메인 특화 어댑터 |

## RAG 코퍼스

기본 인덱싱된 8개 MSA 기술 문서:

| # | 파일 | 내용 |
|---|------|------|
| 01 | event-storming.md | 이벤트 스토밍 표준 (Brandolini), 6단계 워크플로우, 명명 규칙 |
| 02 | hexagonal.md | Ports & Adapters, Driving/Driven, 4 I/F 박스, DDD 매핑 |
| 03 | api-gateway.md | 라우팅·JWT·rate-limit·CORS·BFF 트레이드오프 |
| 04 | saga.md | Choreography vs Orchestration, 보상 트랜잭션, Outbox 결합 |
| 05 | cqrs.md | 분리 수준, Read Model 갱신, 이벤트 스토밍 매핑 |
| 06 | outbox.md | dual-write 문제, Relay 구현 (폴링/CDC), 멱등성 |
| 07 | ddd-tactical.md | 애그리거트·VO·Repository·도메인 이벤트·ApplicationService |
| 08 | bounded-context.md | BC 식별 신호, Context Mapping (ACL·OHS·Conformist) |

→ 추가 문서를 `data/corpus/` 에 넣고 인덱서 재실행하면 RAG 지식 확장.

---

## 3 모드

### 💬 채팅 모드

자유 질문/도메인 컨설팅. RAG 가 자동으로 관련 청크 검색해 답변에 출처 인용.

**예시 질문:**
- "결제 BC 를 헥사고날로 설계할 때 핵심 포트는?"
- "주문 흐름의 보상 트랜잭션 시퀀스를 설계해줘"
- "API Gateway 에서 JWT 클레임을 BC 로 어떻게 전파하지?"
- "Outbox 와 CDC 의 트레이드오프?"

**응답 특징:**
- SSE 스트리밍 (토큰 단위 흐름)
- 출처 인용 칩: `📄 04-saga.md`
- 마크다운 렌더 (코드블록·강조·리스트)
- 현재 state(이벤트 50건·도메인) 자동 컨텍스트 주입 → "이 BC 에 누락된 외부시스템이 뭐 있을까?" 같은 질문 가능

**단축키:** Enter 전송, Shift+Enter 줄바꿈

---

### ⚡ 이벤트 추출 모드

자연어 도메인 묘사 → 17 필드 JSON 자동 채우기 → 보드 즉시 반영.

**입력:**
1. **도메인 흐름 묘사** (자유 텍스트)
   - 예: "고객이 카드결제로 주문하고 PG 콜백 후 주문확정·재고차감·배송요청이 일어난다"
2. **시스템** (선택) — 비어있으면 LLM 추정
3. **도메인** (선택) — 비어있으면 LLM 추정

**출력 (예시):**
```json
{
  "events": [
    { "system":"온라인쇼핑몰", "domain":"주문", "subdomain":"주문생성",
      "event":"주문생성됨", "command":"주문생성",
      "actor":["고객"], "aggregate":["Order","OrderLine"],
      "readPolicy":"재고/가격 검증 + Saga 시작",
      "external":[], "hotspot":"결제대기",
      "dataStore":["OrderDB"], "redis":"",
      "messaging":"order.created", "cqrs":"Command",
      "useCase":"PlaceOrder", "inboundAdapter":"REST API"
    },
    ...
  ],
  "actors":   [...],
  "externals":[...],
  "dataStores":[...]
}
```

**원클릭 머지:**
- "+ 보드에 추가" 버튼 → state.events 에 합쳐짐
- 마스터(액터·외부·저장소) 중복 제거 후 자동 추가
- 시스템·도메인이 새로 추가되면 BC 자동 생성
- 추가 후 새 BC 로 사이드바 자동 이동

**시간:** 27B 모델로 1-2분 (17 필드 × 여러 이벤트 JSON 생성)

---

### 🔍 검토 모드

현재 보드 전체 분석 → 마크다운 리포트.

**검토 항목 5종:**

1. **명명 일관성**
   - 이벤트 과거형 종결어미 통일 (~됨)
   - 커맨드 동사형 통일
   - 도메인 용어(ubiquitous language) 동일 개념 동일 표현

2. **누락 필드**
   - 외부시스템과 연동되는 이벤트인데 `external` 비어있음
   - 결제·인증 같은 핵심 흐름에 `readPolicy` 없음
   - 데이터 변경 이벤트에 `dataStore` 또는 `aggregate` 누락
   - inboundAdapter 미지정

3. **Saga / 핫스팟 식별**
   - 분산 트랜잭션이 추정되는데 보상 흐름이 없는 이벤트
   - 외부 의존도 높은데 hotspot 비어있는 이벤트

4. **BC 결합도**
   - 동일 BC 내 강한 결합 (서브도메인 분리 가능?)
   - BC 간 약결합 가이드

5. **CQRS 분포 점검**
   - Command/Event/Read 비율 비정상

**출력:**

```markdown
## 📋 이벤트 스토밍 검토 리포트 — 주문 BC

### ① 명명 일관성  (3건 발견)
- ⚠️ `결제승인` (event 필드) → 과거형 권장: `결제승인됨`
- ⚠️ `주문하기` vs `주문생성` 혼용 — 통일 권장
- ✓ 액터명 일관성 우수

### ② 누락 필드  (5건)
- 🔴 `재고차감됨` 이벤트 → external 비어있음. **InventoryDB**·**Redis 재고락** 추가 권장
...

### ③ Saga / 핫스팟
- 🔴 결제 흐름에 보상 트랜잭션 부재. `결제실패` 핫스팟 + `주문취소` 보상 추가 권장

### ④ 권장 사항
1. ...

**총평**: 명명은 양호. Saga 흐름과 외부시스템 매핑 보강 시 BFD 출력 품질 크게 향상.
```

이모지로 심각도 구분: 🔴 critical · 🟡 warning · ✓ ok · ⚠️ minor

---

## 도메인 특화 파인튜닝 (선택)

기본 LLM 응답이 너무 일반적이라면 **QLoRA** 어댑터로 도메인 특화:

1. **시드 생성**: `seed_generator.py` — MSA 4 카테고리 × 8 도메인 = 160 건
2. **학습**: `train.py` (Unsloth 기반) — 5090 32GB · 3 epoch · 30~60 분
3. **Ollama 통합**: Modelfile 작성 → `ollama create eventstorming-gemma`
4. **`.env` 업데이트** → `LLM_MODEL=eventstorming-gemma`

학습 데이터셋 카테고리:
- A. 이벤트 스토밍 (자연어 → 이벤트 추출)
- B. 헥사고날 (포트·어댑터 설계)
- C. API Gateway (라우팅·인증·BFF)
- D. Saga (분산 트랜잭션 + 보상)

8 도메인:
- 온라인쇼핑몰 (주문, 결제) · 물류 (배송) · 은행코어 (계좌이체)
- 헬스케어 (예약) · 공공안전 (신고접수) · 차량공유 (라이드매칭) · 구독서비스 (구독결제)

---

## 보안 / 프라이버시

- **모든 추론·임베딩이 로컬 GPU** 에서 실행
- **localStorage 만 사용** — 작성 데이터 외부 전송 없음
- **LLM 호출 = 사용자 → 자체 서버 → 자체 LLM** — 어떤 외부 API 도 호출하지 않음

→ **금융·의료·공공·국방** 등 민감 도메인 도입 적합.

---

## 트러블슈팅

| 증상 | 원인 | 해결 |
|------|------|------|
| ⚡ 패널이 안 보임 | `file://` 로 열고 있음 | `http://localhost:17898/` 로 접속 |
| `● AI 서버 연결 실패` | Python 서버 미기동 | `python ai/server.py` 실행 |
| 첫 응답 30초+ | Ollama 콜드 스타트 | 첫 호출만 느림, 두 번째부터 빠름 |
| 추출 1-2분 소요 | 27B JSON 큰 출력 정상 | 정상 동작 — 화면 닫지 마세요 |
| RAG OFF 표시 | 인덱스 미빌드 | `python ai/ingest.py` 실행 |
| GPU 메모리 부족 | 27B 가 24GB+ 필요 | Gemma 12B 또는 Q3 양자화 권장 |
