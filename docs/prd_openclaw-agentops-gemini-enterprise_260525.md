---
title: "OpenClaw AgentOps for Gemini Enterprise PRD"
date: "2026-05-25 00:51:50 KST"
type: "idea2planning-prd"
status: "draft-v0.1"
version: "v0.1"
tags: [idea2planning, prd, gemini-enterprise, openclaw, agentops, enterprise-ai-governance]
---

# [26.05.25] OpenClaw AgentOps for Gemini Enterprise PRD

> 작성일: 2026.05.25  
> 작성자: Hermes Agent for Hans Kim  
> 관련 조직: Enterprise AI CoE, IT/Cloud Platform, DevOps/Platform Engineering, Security/Governance, 현업 부서 Owner, Executive Sponsor  
> 문서 상태: Draft  
> 문서 버전: v0.1  
> 기준 문서: `1pager_openclaw-agentops-gemini-enterprise_260524.md`

---

## 0. 문서 개요

### 0-1. 프로젝트 요약

**OpenClaw AgentOps for Gemini Enterprise**는 기업이 여러 OpenClaw 기반 AI Agent를 **Gemini Enterprise Agent Platform** 위에서 안전하게 등록, 승인, 배포, 실행, 모니터링, 평가, 통제할 수 있도록 하는 엔터프라이즈 Agent 운영 플랫폼입니다.

이 제품은 “Agent를 만드는 도구”가 아니라, 각 부서에서 빠르게 증가하는 Agent PoC를 **Production 운영체계**로 전환하기 위한 표준 운영·거버넌스·성과관리 계층입니다.

핵심 구현 방향은 다음과 같습니다.

- **Agent Catalog**: 전사 Agent 등록, Owner, 목적, 상태, 재사용성 관리
- **Deployment & Approval Workflow**: PoC → Review → Approved → Production 전환 기준과 승인 이력 관리
- **Tool Access Control**: Agent별 도구, 데이터, 시스템 접근 권한 통제
- **Observability & Audit**: 실행 로그, Tool Call, 실패율, 비용, 감사 이력 수집
- **Quality & Human Review**: 품질 평가, Hallucination 의심 플래그, 개선 백로그 운영
- **Executive Dashboard**: 도입 현황, 운영 리스크, 비용, ROI, Production 전환율 보고

### 0-2. 배경

#### 현재 상황

생성형 AI 도입은 Chatbot/Assistant에서 Tool-using Agent와 Multi-Agent Workflow 단계로 이동하고 있습니다. 기업 내부에서는 영업, 마케팅, 개발, 고객센터, 재무, PMO 등 여러 부서가 개별 Agent PoC를 만들고 있으나, 운영 기준은 아직 성숙하지 않습니다.

주요 현상은 다음과 같습니다.

- 부서별 Agent PoC 수가 3~6개월 내 10~30개 이상으로 증가할 수 있음
- 유사한 Agent가 부서별로 중복 개발될 가능성이 높음
- Agent별 권한, 비용, 품질, 책임자, 로그 기준이 표준화되어 있지 않음
- PoC 성공 후 Production 전환 시 보안·운영·책임체계 검토에서 병목 발생
- 경영진은 AI 투자 효과를 알고 싶지만 Agent 단위 성과와 리스크를 통합적으로 보기 어려움

#### 문제 정의

| 문제 영역 | 현재 Pain Point | 비즈니스 영향 |
|---|---|---|
| 운영 표준 부재 | 누가 어떤 Agent를 만들고 운영하는지 추적 어려움 | Shadow Agent 증가, 중복 투자 발생 |
| Production 전환 병목 | PoC 이후 승인·보안·운영 기준이 불명확 | AI 투자가 실험 단계에 머무름 |
| 권한/보안 리스크 | Agent가 어떤 도구와 데이터에 접근하는지 중앙 통제 미흡 | 데이터 유출, 과도 권한, 감사 리스크 |
| 품질/신뢰성 부족 | 실패율, 환각, 사용자 피드백 관리 체계 부족 | 업무 적용 신뢰도 저하 |
| 비용 가시성 부족 | Agent/부서/업무 단위 비용 산정 어려움 | 예산 통제와 ROI 설명 어려움 |

#### 참고 데이터 및 초기 가정

| 항목 | 초기 가정 | PRD 기준 목표 |
|---|---:|---:|
| 대기업 1개 고객 내 Agent PoC 수 | 10~30개 / 3~6개월 | 주요 Agent 80% 이상 Catalog 등록 |
| PoC → Production 전환율 | 20~30% 이하 | 3개월 내 30% 이상 |
| 유사 Agent 중복률 | 20~40% | 재사용 추천 기반 30% 이상 감소 |
| 이상 비용/실패 탐지 시간 | 수동 확인, 수일 가능 | 1시간 이내 탐지 |
| 승인 리드타임 | 케이스별 상이 | 표준 체크리스트 기준 3~5영업일 |

#### 사용자 Voice / 예상 발화

> “Agent PoC는 많은데, 실제 운영으로 올릴 때 누가 승인해야 하는지 모르겠습니다.” — AI CoE Lead

> “Agent가 어떤 내부 시스템을 호출했는지 감사 로그가 필요합니다.” — Security/Governance 담당자

> “경영진에게 Agent별 사용량, 비용, 업무 절감 효과를 한 화면에서 보고하고 싶습니다.” — CIO/CDO 조직

> “부서마다 비슷한 Agent를 또 만들고 있습니다. 재사용할 수 있는 Catalog가 필요합니다.” — Platform Engineering Lead

---

## 1. 목표 (Goal)

### 1-1. 프로젝트 목표

#### 사용자 가치 측면

1. 현업 부서가 승인된 Agent를 안전하게 발견하고 사용할 수 있게 합니다.
2. Agent Owner가 Agent의 상태, 권한, 비용, 품질을 한 곳에서 관리할 수 있게 합니다.
3. 보안/운영 담당자가 Agent 실행과 Tool Call에 대한 추적·감사 기준을 확보하게 합니다.

#### 비즈니스 측면

1. Agent PoC의 Production 전환율을 높여 AI 투자 성과를 가시화합니다.
2. 유사 Agent 중복 개발을 줄이고 재사용 가능한 Agent 자산을 축적합니다.
3. Gemini Enterprise 기반 Agent Platform 도입 고객에게 차별화된 AgentOps 상품 패키지를 제공합니다.

#### 운영 측면

1. Agent 등록, 검토, 승인, 배포, 운영, 폐기까지의 Lifecycle을 표준화합니다.
2. 권한·비용·품질·감사 로그를 Agent 단위로 통합 모니터링합니다.
3. 운영 리스크를 조기에 탐지하고 Human Review 및 개선 백로그로 연결합니다.

### 1-2. 성공 기준

#### 정량 목표

| 지표 | 현재 | v0.1 목표 |
|---|---:|---:|
| Agent Catalog 등록률 | 기준 없음 | 주요 Agent 80% 이상 |
| PoC → Production 전환율 | 20~30% 이하 가정 | 3개월 내 30% 이상 |
| 승인 리드타임 | 비표준 | 3~5영업일 |
| 이상 실패율/비용 탐지 시간 | 수동 확인 | 1시간 이내 |
| Agent 단위 비용 추적률 | 낮음 | 90% 이상 |
| 유사 Agent 재사용률 | 기준 없음 | 30% 이상 |

#### 정성 목표

- AI CoE, IT, 보안, 현업이 동일한 Agent 운영 기준을 공유합니다.
- 경영진 보고 시 “Agent 수, 운영 상태, 비용, 리스크, 성과”를 단일 언어로 설명할 수 있습니다.
- OpenClaw 기반 Agent를 기업 표준 운영모델 안으로 편입하는 참조 아키텍처를 확보합니다.

---

## 2. KPI 및 측정지표

### 2-1. 핵심 KPI

| KPI명 | 정의 | 계산식 | 목표 | 측정 주기 | 측정 도구 |
|---|---|---|---:|---|---|
| Catalog Coverage | 운영 대상 Agent 중 Catalog 등록 비율 | 등록 Agent 수 / 전체 식별 Agent 수 | 80%+ | 주간 | AgentOps DB, Discovery 로그 |
| Production Conversion | PoC Agent 중 운영 전환 비율 | Production Agent 수 / PoC Agent 수 | 30%+ | 월간 | Agent 상태 로그 |
| Approval Lead Time | Review 요청부터 Approved까지 소요시간 | Approved At - Review Requested At | 3~5영업일 | 주간 | Workflow 이벤트 |
| Policy Violation Rate | 권한/정책 위반 이벤트 비율 | 정책 위반 이벤트 / 전체 실행 | 1% 이하 | 일간 | Policy Engine 로그 |
| Incident Detection Time | 이상 비용·실패율 탐지까지 시간 | Alerted At - Anomaly Started At | 1시간 이내 | 일간 | Observability 로그 |
| Cost Attribution Coverage | Agent 단위 비용 추적 가능 비율 | 비용 매핑 실행 수 / 전체 실행 수 | 90%+ | 월간 | Billing/Usage 로그 |

### 2-2. 보조 지표

- Agent별 월간 실행 수
- Agent별 성공률 / 실패율 / Timeout 비율
- 평균 응답 시간 및 P95 응답 시간
- Tool Call 수, 고위험 Tool Call 비율
- 사용자 피드백 점수(Thumbs up/down, 5점 척도)
- Human Review Queue 적체 건수
- 개선 백로그 처리 리드타임
- 부서별 Agent 사용량 및 비용

### 2-3. 모니터링 지표

#### 유입/등록

- 신규 등록 Agent 수
- Draft → Review 전환율
- Review 반려 사유 TOP 5

#### 탐색/재사용

- Catalog 검색 수
- 유사 Agent 추천 클릭률
- 기존 Agent Fork/Reuse 수

#### 실행/운영

- Agent Run Count
- Success / Fail / Timeout / Blocked by Policy
- Tool Call Count
- Cost per Run

#### 리텐션/확산

- 월간 활성 Agent 수
- 부서별 활성 사용자 수
- Production Agent 유지율
- Deprecated Agent 비율

---

## 3. 사용자 및 이해관계자

### 3-1. 대상 사용자

| 사용자 세그먼트 | 설명 | 주요 니즈 |
|---|---|---|
| AI CoE Lead | 전사 AI Agent 운영 체계를 설계·관리 | Agent 표준화, 성과관리, 확산 전략 |
| IT/Cloud Platform Admin | GE, Google Cloud, IAM, 로그, 비용 관리 | 안정적 실행 환경, 권한 통제, 비용 가시성 |
| Security/Governance Reviewer | 보안 검토, 감사, 정책 승인 | 최소 권한, 감사 로그, 위반 탐지 |
| Agent Owner | 특정 Agent의 기획/운영 책임자 | 등록, 배포, 품질개선, 사용자 피드백 관리 |
| 현업 사용자 | 승인된 Agent를 업무에 활용 | 안전한 Agent 발견, 사용 가이드, 신뢰성 |
| Executive Sponsor | AI 투자 성과와 리스크를 판단 | 통합 Dashboard, ROI, 리스크 현황 |

#### 제외 대상

- 개인이 비공식적으로 실험하는 미등록 Shadow Agent의 직접 운영 지원
- 고객사 보안 정책상 외부 Agent Runtime 연결이 금지된 환경
- 규제상 생성형 AI Agent의 자동 실행이 허용되지 않는 고위험 업무의 완전 자동화
- OpenClaw 외 모든 Agent Framework를 v0.1에서 동일 수준으로 지원하는 범위

### 3-2. 이해관계자

| 조직 | 역할 |
|---|---|
| Product/PM | 요구사항, 우선순위, 로드맵 관리 |
| Engineering | AgentOps 플랫폼, Adapter, API, DB, UI 개발 |
| Security | 권한 정책, 감사 로그, 위험 Tool Call 기준 정의 |
| Cloud Platform | Gemini Enterprise, IAM, Billing, Observability 연계 |
| AI CoE | Agent 운영 표준, 승인체계, 교육/확산 |
| 현업 부서 | Agent 수요 제안, 사용자 검증, 성과 피드백 |
| Executive | 투자 승인, 운영 성과 리뷰, 확산 의사결정 |

---

## 4. 정책 정의

### 4-1. 기본 정책

| 항목 | 정책 |
|---|---|
| Agent 등록 대상 | 고객사에서 업무 목적으로 개발·운영하려는 OpenClaw 기반 Agent |
| Agent Owner | 모든 Agent는 1명 이상의 Business Owner와 1명 이상의 Technical Owner를 가져야 함 |
| Production 전환 | 보안/권한/로그/비용/운영 체크리스트 통과 후 가능 |
| 권한 원칙 | Least Privilege. Agent별 Tool Permission Scope를 명시해야 함 |
| 비용 한도 | Agent별 월간 Budget과 Alert Threshold 설정 가능 |
| 데이터 접근 | Agent별 접근 데이터 Source와 민감도 등급을 등록해야 함 |
| 감사 로그 | Production Agent는 실행 요청, Tool Call, 정책 판단, 결과 상태를 저장해야 함 |
| Human Approval | 고위험 Tool Call 또는 민감 데이터 접근은 승인 정책 적용 가능 |
| 폐기 정책 | 90일 이상 미사용 또는 Owner 부재 Agent는 Review 후 Deprecated 처리 가능 |

### 4-2. 상태값 정의

| 상태 | 설명 | 진입 조건 | 다음 상태 |
|---|---|---|---|
| DRAFT | Agent 등록 초안 | Owner가 기본 정보 저장 | REVIEW_REQUESTED |
| REVIEW_REQUESTED | 운영 승인 검토 요청 | 필수 메타데이터와 체크리스트 제출 | APPROVED / REJECTED |
| REJECTED | 검토 반려 | 보안/운영/품질 기준 미달 | DRAFT |
| APPROVED | 운영 승인 완료 | Reviewer 승인 | STAGING / PRODUCTION |
| STAGING | 제한된 사용자 대상 검증 | 테스트 배포 | PRODUCTION / ROLLBACK |
| PRODUCTION | 공식 운영 | 운영 기준 충족 | SUSPENDED / DEPRECATED |
| SUSPENDED | 일시 중지 | 정책 위반, 장애, 비용 초과 | PRODUCTION / DEPRECATED |
| DEPRECATED | 폐기 예정/폐기 | 대체 Agent 존재, 미사용, Owner 요청 | ARCHIVED |
| ARCHIVED | 보관 | 감사/이력 보존 목적 | 없음 |

### 4-3. 예외 정책

| 상황 | 처리 |
|---|---|
| 필수 메타데이터 누락 | Review 요청 불가. 누락 항목을 UI에서 표시 |
| Owner 미지정 | Production 전환 불가 |
| 보안 검토 반려 | Agent 상태를 REJECTED로 변경하고 반려 사유 기록 |
| 권한 정책 위반 | 실행 차단, 이벤트 저장, Owner/운영자 알림 |
| 고위험 Tool Call 발생 | 정책에 따라 Human Approval Queue로 전환 |
| Gemini/GE API Timeout | 최대 1회 재시도 후 FAILED_TIMEOUT 기록, 사용자에게 재시도 안내 |
| OpenClaw Runtime 오류 | FAILED_RUNTIME 기록, Agent Owner에게 알림 |
| 비용 한도 80% 도달 | Warning Alert 발송 |
| 비용 한도 100% 초과 | 정책에 따라 실행 제한 또는 승인 필요 상태로 전환 |
| 로그 수집 실패 | Agent 실행은 정책에 따라 허용하되 LOGGING_DEGRADED 이벤트 기록. Production Agent는 반복 발생 시 SUSPENDED 검토 |
| 사용자 피드백 악화 | N회 이상 부정 피드백 또는 품질 기준 미달 시 Human Review Queue 생성 |

---

## 5. 서비스 구조

### 5-1. 서비스 흐름

```text
Agent 아이디어/PoC 발생
  → Agent Catalog 등록(DRAFT)
  → 메타데이터/권한/데이터/비용/운영 체크리스트 작성
  → Review 요청(REVIEW_REQUESTED)
  → 보안·운영·품질 검토
  → 승인(APPROVED)
  → Staging 배포 및 제한 사용자 검증
  → Production 전환
  → 실행 로그/비용/품질/피드백 모니터링
  → 개선 백로그 또는 Deprecated/Archive
```

### 5-2. 사용자 플로우

#### Agent Owner 플로우

1. AgentOps 포털 접속
2. 신규 Agent 등록
3. Agent 목적, 업무 범위, Owner, OpenClaw Runtime 정보 입력
4. 사용 Tool, 데이터 접근 범위, 사용자 권한, 비용 한도 설정
5. 운영 체크리스트 완료
6. Review 요청
7. 승인 결과 확인
8. Staging/Production 배포
9. Dashboard에서 사용량, 실패율, 비용, 피드백 확인
10. 개선 백로그 처리 또는 버전 업데이트

#### Security Reviewer 플로우

1. Review Queue 접속
2. Agent 메타데이터, Tool Permission, 데이터 접근 범위 확인
3. 위험 도구 호출 여부와 Human Approval 필요 여부 판단
4. 승인 / 반려 / 조건부 승인 처리
5. 감사 로그와 정책 변경 이력 확인

#### Executive 플로우

1. Executive Dashboard 접속
2. 전체 Agent 수, Production 전환율, 부서별 도입 현황 확인
3. 비용, 업무 절감 효과, 위험 Agent 목록 확인
4. 확산 대상 Agent와 개선 필요 영역 판단

### 5-3. 주요 화면

| 화면 | 목적 |
|---|---|
| Executive Dashboard | 전사 Agent 현황, 비용, 리스크, ROI 보고 |
| Agent Catalog | Agent 검색, 등록, 상태, Owner, 재사용 관리 |
| Agent Detail | Agent 메타데이터, 권한, 버전, 로그, 품질, 비용 상세 |
| Approval Queue | Review 요청, 승인/반려, 조건부 승인 관리 |
| Policy & Tool Access | Tool 권한, 데이터 접근, Human Approval 정책 관리 |
| Observability | 실행 로그, 실패율, 응답시간, 비용, 알림 확인 |
| Human Review Queue | 품질 이슈, 환각 의심, 고위험 실행 결과 검토 |
| Settings/Admin | 조직, 역할, 권한, 알림, 감사 보존 정책 설정 |

---

## 6. 상세 요구사항

### 6-1. 기능 요구사항

| ID | 기능 | 설명 | 우선순위 |
|---|---|---|---|
| FR-001 | Agent Catalog 등록 | Agent 이름, 설명, 업무 목적, Owner, Runtime, 상태 저장 | High |
| FR-002 | Agent 상태 관리 | DRAFT~ARCHIVED 상태 전환 및 변경 이력 기록 | High |
| FR-003 | 메타데이터 필수 검증 | Review 요청 전 필수 입력값 누락 검증 | High |
| FR-004 | Approval Workflow | Reviewer 지정, 승인/반려/조건부 승인, 코멘트 기록 | High |
| FR-005 | Tool Permission Scope | Agent별 허용 Tool, 위험도, 승인 필요 여부 정의 | High |
| FR-006 | Data Access Scope | Agent별 데이터 Source, 민감도, 접근 목적 등록 | High |
| FR-007 | Runtime Adapter | OpenClaw Agent Runtime과 GE 실행/Tool Calling 구조 연결 | High |
| FR-008 | Execution Log 수집 | 요청, 응답, 상태, latency, tool call, error code 저장 | High |
| FR-009 | Cost Attribution | Agent/부서/사용자 단위 비용 추정 및 집계 | High |
| FR-010 | Alert Rule | 실패율, 비용, latency, 정책 위반 기준 알림 | High |
| FR-011 | Executive Dashboard | 전사 Agent 현황, 비용, 전환율, 위험 Agent 시각화 | High |
| FR-012 | Agent Detail Dashboard | Agent별 사용량, 성공률, 비용, 피드백, 버전 확인 | High |
| FR-013 | Human Review Queue | 품질 이슈와 고위험 실행 결과 검토·처리 | Medium |
| FR-014 | 유사 Agent 검색 | 이름/설명/업무 목적 기반 유사 Agent 추천 | Medium |
| FR-015 | Agent Template | 승인된 Agent를 Template/Fork로 재사용 | Medium |
| FR-016 | Audit Export | 감사 로그 CSV/JSON Export 및 보존 정책 적용 | Medium |
| FR-017 | ROI 추정 | 실행 수, 절감 시간, 비용 기반 업무 효과 추정 | Medium |
| FR-018 | Multi-Agent Workflow 표시 | Phase 3에서 Agent 간 연결과 의존성 시각화 | Low |

### 6-2. 운영 요구사항

- 모든 Production Agent는 Business Owner와 Technical Owner가 지정되어야 합니다.
- Review Queue에는 SLA가 적용되어야 하며, 5영업일 이상 미처리 건은 Escalation 알림을 보냅니다.
- Production Agent의 정책 위반 이벤트는 Owner, Security Reviewer, Platform Admin에게 알립니다.
- 장애/비용 초과/품질 이슈는 Incident 수준을 Low/Medium/High/Critical로 분류합니다.
- 운영자는 Agent별 Runbook URL 또는 운영 가이드를 등록할 수 있어야 합니다.
- 월간 운영 리뷰에서 Deprecated 후보 Agent, 고비용 저성과 Agent, 재사용 후보 Agent를 자동 리포팅합니다.

### 6-3. 데이터 요구사항

| 데이터 항목 | 설명 |
|---|---|
| Agent | ID, 이름, 설명, 목적, Owner, 조직, 상태, Runtime, 생성/수정일 |
| Agent Version | 버전, 변경 내용, 배포 상태, 배포자, Rollback 가능 여부 |
| Permission Scope | 허용 Tool, 위험도, 데이터 접근 범위, 승인 필요 여부 |
| Review Request | 요청자, Reviewer, 상태, 코멘트, 승인/반려 사유, 타임스탬프 |
| Execution Event | Run ID, Agent ID, User ID, Status, Latency, Token/Cost, Error Code |
| Tool Call Event | Tool Name, Input/Output Metadata, Policy Decision, Approval Status |
| Feedback Event | 사용자 평가, 코멘트, 품질 이슈 유형, Review 연결 여부 |
| Cost Event | 모델/런타임/Tool 비용, 부서/Agent 매핑, Budget 사용률 |
| Alert Event | 알림 유형, 심각도, 감지 시각, 담당자, 처리 상태 |
| Audit Event | Actor, Action, Before/After, IP/Session, Timestamp, Retention |

---

## 7. UX/UI 고려사항

### UX 원칙

- **운영자는 위험을 먼저 본다**: Dashboard 첫 화면은 전체 수보다 위험 Agent, 비용 초과, 승인 대기, 실패율 이상을 우선 노출합니다.
- **현업은 쉽게 등록한다**: Agent 등록은 복잡한 보안 용어보다 업무 목적, 대상 사용자, 사용할 도구를 자연어 중심으로 입력하게 합니다.
- **보안 검토자는 증거를 본다**: Reviewer 화면은 데이터 접근 범위, Tool 위험도, 로그 수집 여부, 비용 한도 같은 판단 근거를 요약합니다.
- **경영진은 성과와 리스크를 함께 본다**: ROI, Production 전환율, 부서 확산 현황과 함께 위험 Agent 수를 동시에 보여줍니다.

### 상태별 UI

| 상태 | UI 처리 |
|---|---|
| Loading | Skeleton Card와 “Agent 운영 데이터를 불러오는 중” 표시 |
| Empty | “등록된 Agent가 없습니다. 첫 Agent를 등록하세요” CTA 제공 |
| Error | 오류 원인, 재시도, 관리자 문의, Request ID 표시 |
| Permission Denied | 접근 권한 없음과 필요한 Role 안내 |
| Policy Blocked | 차단 사유, 관련 정책, 승인 요청 가능 여부 표시 |
| Approval Pending | 현재 Reviewer, SLA, 남은 예상 시간 표시 |

### 주요 CTA

- `+ Register Agent`
- `Request Review`
- `Approve / Reject / Conditional Approve`
- `Promote to Production`
- `Suspend Agent`
- `Open Human Review`
- `Export Audit Log`

---

## 8. 기술 고려사항

### 8-1. 기술 구조

```text
[User / Admin UI]
  → [AgentOps API]
  → [Catalog Service]
  → [Workflow Service]
  → [Policy Engine]
  → [OpenClaw Adapter]
  → [Gemini Enterprise Agent Platform]
  → [OpenClaw Agent Runtime / Tools]

[Execution Events]
  → [Event Collector]
  → [Observability Store]
  → [Cost & Quality Processor]
  → [Dashboard / Alert / Audit Export]
```

### 8-2. 인증/권한

- 기업 SSO 및 IAM 연계를 전제로 합니다.
- Role 예시: `Agent Viewer`, `Agent Owner`, `Reviewer`, `Security Admin`, `Platform Admin`, `Executive Viewer`.
- API는 Role-Based Access Control과 Agent별 Resource-Level Permission을 모두 고려합니다.
- 고위험 Tool Call은 정책에 따라 Human Approval Token 또는 Workflow 승인 이벤트가 필요합니다.

### 8-3. 성능 목표

| 항목 | 목표 |
|---|---:|
| Catalog 검색 응답 | P95 < 500ms |
| Dashboard 초기 로딩 | P95 < 2.5s |
| Policy Evaluation | P95 < 100ms |
| Execution Log 수집 지연 | 60초 이내 Dashboard 반영 |
| Alert 감지 지연 | 5분 이내, Critical은 1분 이내 목표 |
| Audit Export | 10만 이벤트 기준 60초 이내 비동기 생성 |

### 8-4. 보안/감사

- 민감 입력/출력 본문 저장 여부는 고객 정책에 따라 Masking 또는 Metadata-only 모드 선택 가능해야 합니다.
- Tool Call 입력/출력 전문 저장은 기본 비활성화하고, 감사 요구가 있는 경우 제한적으로 활성화합니다.
- 감사 로그는 변경 불가능성, 보존 기간, Export 권한을 별도 정책으로 관리합니다.
- Public Internet 노출이 아닌 고객사 Tenant/조직 경계 내 운영을 기본 가정합니다.

---

## 9. 실험 및 검증

### 9-1. MVP 검증 시나리오

| 실험 | 목적 | 성공 기준 |
|---|---|---|
| Agent 등록/승인 Pilot | Catalog와 Workflow가 실제 운영 기준을 담을 수 있는지 확인 | 5개 이상 대표 Agent 등록, 80% 이상 승인 플로우 완료 |
| Tool Permission Pilot | Agent별 Tool Scope와 정책 차단이 유효한지 확인 | 고위험 Tool Call 100% 정책 판단 기록 |
| Observability Pilot | 실행 로그, 실패율, 비용 추적 가능성 확인 | 실행 이벤트 90% 이상 Agent ID와 비용 매핑 |
| Executive Report Pilot | 경영진 보고 가능성 확인 | 전환율, 비용, 위험 Agent, ROI 카드 포함한 월간 리포트 생성 |

### 9-2. 검증 방법

- AI CoE/보안/현업 Owner 대상 워크숍
- 1~2개 고객 또는 내부 Demo Tenant에서 대표 Agent 3~5개 온보딩
- 승인 리드타임, 등록 누락, 정책 반려 사유 분석
- Dashboard 사용성 인터뷰
- 운영 회의체에서 월간 리포트 리뷰

---

## 10. 리스크 및 대응

| 리스크 | 영향도 | 대응 |
|---|---|---|
| GE와 OpenClaw Runtime 호환성 부족 | High | Adapter Layer를 분리하고 1~2개 대표 Agent로 호환성 PoC 선행 |
| 로그/비용 매핑 정확도 부족 | High | 공통 Agent Event Schema와 Cost Attribution Key를 우선 정의 |
| 권한 정책이 복잡해 현업 속도 저하 | Medium | 기본 정책 Template 제공, 고위험 Tool만 강화 승인 적용 |
| Dashboard가 보고용에 그치고 운영 액션이 부족 | Medium | Alert, Review Queue, Suspend, Backlog 생성까지 연결 |
| 보안팀 승인 병목 | Medium | 조건부 승인, SLA, 자동 검증 체크리스트 도입 |
| 제품 범위 과대 | High | Phase 1은 Catalog+Workflow+기본 로그+Dashboard에 집중 |
| OpenClaw 가치 인식 부족 | Medium | OpenClaw를 단독 도구가 아니라 GE 위 Agent Runtime/업무 Agent 패턴으로 설명 |

---

## 11. 일정 및 운영계획

### 11-1. 단계별 출시 계획

| 단계 | 기간 | 핵심 범위 | 산출물 |
|---|---|---|---|
| Phase 0. Discovery | 2주 | OpenClaw Runtime/GE 연계 검토, 고객 운영 요구 수집 | 기술 검토서, 운영 요구 워크숍 결과 |
| Phase 1. AgentOps Foundation | 4~6주 | Catalog, 상태관리, 수동 승인, 기본 실행 로그, 기본 Dashboard | MVP Demo, Agent 등록 템플릿, 전환 체크리스트 |
| Phase 2. Governance & Observability | 6~8주 | Tool Access Control, 비용/품질/감사, Alert, Human Review | 운영 KPI Dashboard, 보안 정책, 비용 리포트 |
| Phase 3. Enterprise Scale | 8~12주 | Template Marketplace, 유사 Agent 추천, ROI, Multi-Agent Workflow | 전사 확산 패키지, Executive Report, CoE 운영모델 |

### 11-2. 운영 회의체

| 회의 | 주기 | 참석자 | 목적 |
|---|---|---|---|
| Product Sync | 주 1회 | PM, Engineering, Design | 요구사항/개발 진행 점검 |
| Governance Review | 주 1회 | Security, AI CoE, Platform | 승인 정책과 예외 검토 |
| Pilot Review | 격주 | 현업 Owner, AI CoE, PM | Pilot Agent 성과와 개선점 확인 |
| Executive Review | 월 1회 | Executive Sponsor, AI CoE Lead | 성과, 비용, 리스크, 확산 의사결정 |

### 11-3. 오픈 전략

- 초기에는 내부 Demo Tenant 또는 대표 고객 1곳에서 제한된 Agent 3~5개로 Soft Launch합니다.
- Phase 1에서는 “수동 승인 + 기본 로그 + Dashboard” 중심으로 운영 기준을 검증합니다.
- Phase 2부터 보안/비용/품질 자동화 수준을 높입니다.
- Phase 3에서 Template Marketplace와 전사 확산 모델로 확장합니다.

---

## 12. Appendix

### 12-1. 용어 정의

| 용어 | 정의 |
|---|---|
| AgentOps | AI Agent의 등록, 배포, 관측, 품질, 비용, 보안, 운영을 관리하는 체계 |
| Gemini Enterprise Agent Platform | 기업용 Agent 실행과 통합을 위한 Google Gemini Enterprise 기반 플랫폼 계층 |
| OpenClaw | 업무 실행/개발 자동화 Agent Runtime 또는 Agent Framework로 가정한 대상 |
| Agent Catalog | 전사 Agent 목록, 상태, Owner, 목적, 권한, 사용 현황을 관리하는 저장소 |
| Tool Access Control | Agent가 호출할 수 있는 도구와 데이터 접근 범위를 통제하는 정책 계층 |
| Human Review | 고위험 실행, 품질 이슈, 정책 위반 후보를 사람이 검토하는 절차 |
| Production Conversion | PoC Agent가 공식 운영 Agent로 전환되는 과정 또는 비율 |

### 12-2. 변경 이력

| 버전 | 날짜 | 내용 |
|---|---|---|
| v0.1 | 2026.05.25 | 1-pager 기반 PRD 최초 작성 |

### 12-3. 추가 확인 필요 사항

1. OpenClaw의 정확한 Runtime 구조와 GE 연동 방식
2. Gemini Enterprise Agent Platform의 외부 Runtime/Tool 연동 제약
3. 고객사 IAM/SSO/Billing/Logging 연동 범위
4. Tool Call 전문 저장 가능 여부와 보안 정책
5. 비용 산정 단위: 모델 호출, Tool 호출, Runtime 자원, 인프라 비용 포함 범위
6. 초기 Demo Agent 후보: 개발 Agent, 영업 제안 Agent, PMO Agent, 고객센터 Agent 중 우선순위
