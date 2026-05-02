# GitHub 저장소 설정 가이드

이 디렉토리(`github-public/`) 를 그대로 GitHub 공개 저장소로 push 하기 위한 가이드.

## 추천 저장소 이름 (택 1)

| 후보 | 의미 | 비고 |
|------|------|------|
| **`msa-design-studio`** ⭐ | MSA 설계 스튜디오 | 가장 직관적·SEO 강함 |
| `eventstorming-studio` | 이벤트 스토밍 스튜디오 | 도메인 명시 |
| `bfd-canvas` | Business Function Design Canvas | 한국 기업 친화 |
| `domain-storming` | 도메인 스토밍 | 짧고 모던 |
| `hexagonal-storming` | 헥사고날 + 스토밍 | 결과물 강조 |

권장: **`msa-design-studio`** (메인 README 도 이 이름 기준)

## 추천 GitHub 토픽 (Topics) — 최대 20개 등록 가능

복사해서 GitHub 저장소 설정에 그대로 붙여넣으세요:

```
event-storming
domain-driven-design
ddd
hexagonal-architecture
microservices
msa
bounded-context
saga-pattern
api-gateway
cqrs
outbox-pattern
ports-and-adapters
event-driven-architecture
ai-assistant
local-llm
gemma
rag
korean
mlops
software-architecture
```

각 토픽 사용 효과:
- `event-storming` · `ddd` · `hexagonal-architecture` — 핵심 도메인 검색
- `microservices` · `msa` — 가장 큰 검색 흐름
- `local-llm` · `gemma` · `rag` — AI 트렌드 검색
- `korean` — 한국어 자료 찾는 사용자
- `software-architecture` — 일반 아키텍처 학습자

## 저장소 설명 (Description)

GitHub 페이지 상단 한 줄 설명에 추가:

> Event Storming + DDD + Hexagonal Architecture + AI Assistant — Microservice Design Studio (한국어 친화) · Live demo: msa-dev.fact-mine.com

## Website (URL)

```
https://msa-dev.fact-mine.com
```

## Social Preview Image

`docs/images/hero.png` 를 1200×630 으로 만들어 등록하면 트위터/링크드인 공유 시 미리보기 강력해짐.

---

## 저장소 만들고 push 하기

### 1) GitHub 저장소 생성
1. GitHub.com 로그인
2. 우상단 ＋ → **New repository**
3. Repository name: `msa-design-studio`
4. Description: 위 한 줄 설명 붙여넣기
5. **Public** 선택
6. **Do NOT initialize** with README/license/.gitignore (이미 있음)
7. **Create repository**

### 2) 로컬에서 push

```bash
cd /home/gosrak/compile/makeEvt/github-public

git init
git add .
git status                                  # 의도한 파일들만 들어가는지 확인
git commit -m "Initial release — docs · samples · screenshots"

git branch -M main
git remote add origin https://github.com/<your-account>/msa-design-studio.git
git push -u origin main
```

### 3) 토픽 등록
GitHub 저장소 페이지 → 우측 ⚙ (About) → Topics 에 위 20개 토픽 붙여넣기

### 4) 추가 설정
- **Description**: 위 한 줄
- **Website**: `https://msa-dev.fact-mine.com`
- **Topics**: 위 20개
- **Pages**: 사용 안 함 (데모 사이트가 있으므로)
- **Discussions**: ✅ 활성화 (커뮤니티 Q&A)
- **Issues**: ✅ 활성화 (템플릿 자동 인식)
- **Wiki**: ✗ (`docs/` 사용)
- **Projects**: 선택

---

## 비공개 소스 저장소 (별도)

실제 소스 코드는 별도 저장소로:

```bash
# 별도 디렉토리에서
cd /home/gosrak/compile/makeEvt
git init
# .gitignore 에 다음 추가:
#   github-public/   ← 이 공개 저장소 폴더는 제외
#   ai/.venv/
#   ai/vectorstore/
#   ai/models/
#   web/dist/
#   web/node_modules/

git add event_storming.html web/ ai/server.py ai/lib/ ai/prompts/ ai/data/corpus/
git commit -m "Initial private repo"

# GitHub 에서 Private 저장소 생성 → push
git remote add origin git@github.com:<your-account>/msa-design-studio-source.git
git push -u origin main
```

> 비공개 저장소에는 `event_storming.html`, `web/`, `ai/` 의 실제 코드가 들어갑니다.
> 공개 저장소에는 그 결과물(데모 사이트)을 소개하는 문서·샘플만 들어갑니다.

---

## 운영 권장 사항

### Release 태깅
주요 기능 추가 시:

```bash
git tag -a v1.0.0 -m "Initial release with 8 BCs sample"
git push origin v1.0.0
```

GitHub Releases 에 변경 사항 작성:
- 🎉 **새 기능**
- 🐛 **버그 수정**
- 📚 **문서**
- 🔧 **내부**

### README 배지 동기화
사이트가 다운되면 `https://img.shields.io/website?url=...` 배지로 상태 표시.

### 보안 · 라이선스 추가 검토
- `SECURITY.md` — 취약점 신고 가이드
- `CODE_OF_CONDUCT.md` — Contributor Covenant

---

## 체크리스트

GitHub push 전 확인:

- [ ] `README.md` 의 데모 URL 정확
- [ ] `LICENSE` 의 저작권 연도·소유자
- [ ] `samples/` 안의 실제 데이터 — 민감 정보 없는지
- [ ] `docs/images/` 스크린샷 추가 (이번 단계에는 미포함 — 별도 추가 권장)
- [ ] `.gitignore` 가 `node_modules/`, `.venv/`, `__pycache__/` 등 모두 제외
- [ ] `.github/ISSUE_TEMPLATE/` 3종 템플릿 동작 (생성 후 GitHub UI 에서 확인)
- [ ] 비공개 저장소가 별도로 분리됨

준비 완료 시 push 후 토픽 등록!
