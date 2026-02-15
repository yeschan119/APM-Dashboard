# OpenTelemetry 기반 관측 플랫폼
### .NET 기반 커스텀 APM & 분산 트레이싱 워크벤치

OpenTelemetry SDK를 활용하여

- 사용자 활동 로그(User Activity Log)
- 시스템 성능 정보(Controller / API / SQL Trace)
- 분산 트레이싱 구조(Parent-Child Span)

를 통합 관리하는 내부 Observability 플랫폼을 구축한 프로젝트입니다.

Datadog과 같은 외부 APM 도구 없이,
.NET과 AWS 기반으로 자체 관측 시스템을 설계 및 구현하였습니다.

---

## 📌 프로젝트 개요

이 시스템은 두 가지 기능을 하나의 파이프라인으로 통합합니다.

### 1️⃣ 사용자 활동 로그 수집
- CRUD 활동 추적
- 사용자별 액션 기록
- 시간 기반 활동 조회

### 2️⃣ 시스템 성능 추적
- Controller 실행 시간
- API 호출 시간
- SQL 쿼리 실행 시간
- 분산 트레이스 구조 시각화

두 기능은 .NET OpenTelemetry SDK를 통해 단일 Observability 모델로 처리됩니다.

---

## 🏗 전체 아키텍처

# Angular Dashboard & Trace Tree
---

## 🔄 실행 흐름

### [1] HTTP 요청 수신
- ASP.NET Core Controller 진입

### [2] OpenTelemetry 자동 계측
- AspNetCore → Server Span 생성
- HttpClient → Client Span 생성
- SqlClient → DB Span 생성

### [3] Activity 생성
- TraceId / SpanId 생성
- Parent / Child Span Tree 구성

### [4] Custom Exporter 동작
- Span → JSON 변환
- Batch 처리 후 DynamoDB 저장

### [5] DynamoDB 저장
- PK: TraceId
- SK: SpanId
- TTL: 7일
- On-Demand 모드

---

## 🗄 DynamoDB 데이터 모델

| 필드 | 설명 |
|------|------|
| PK | TraceId |
| SK | SpanId |
| Type | controller / api / sql |
| Controller | 컨트롤러 이름 |
| Method | HTTP Method |
| HttpUrl | 요청 URL |
| DbStatement | SQL Query |
| DurationMs | 실행 시간 |
| ParentSpanId | 상위 Span |
| ExpireAt | TTL (7일 자동 삭제) |

---

## 📊 Workbench 주요 기능

### 🧾 Activity Log Dashboard
- 사용자 정보
- CRUD 활동 로그
- 시간별 조회

### 📈 Benchmark Dashboard
- Top10 느린 Controller
- Top10 빠른 Controller
- Top10 API
- Top10 SQL

### 🌳 분산 Trace Tree
Controller  
└── API  
  └── SQL  

TraceId 기준으로 Span Tree 재구성

### 🧠 SQL Modal
- SQL Pretty Format
- Copy 버튼 제공

---

## ⚙️ 기술적 핵심 포인트

### 1️⃣ OpenTelemetry 기반 통합 관측
User Activity와 Performance Trace를
하나의 Telemetry 파이프라인으로 처리

### 2️⃣ Custom Exporter 구현
- Span 데이터를 JSON으로 직렬화
- DynamoDB에 Batch Write
- TTL 기반 자동 정리

### 3️⃣ 분산 트레이스 모델링
- PK: TraceId
- SK: SpanId
- ParentSpanId 기반 트리 재구성

### 4️⃣ 자체 APM 시스템 구현
외부 SaaS(Apm/Datadog) 없이
내부 관측 시스템을 직접 설계

---

## 🚀 이 프로젝트의 의미

이 프로젝트는 단순 로그 저장 시스템이 아닙니다.

- OpenTelemetry SDK 레벨 이해
- Distributed Trace 구조 설계
- DynamoDB 데이터 모델링
- 내부 APM 시스템 구현
- 실시간 성능 벤치마크 대시보드 구축

을 포함한 Observability 플랫폼 설계 프로젝트입니다.

---

## 🛠 기술 스택

### Backend
- ASP.NET Core
- .NET OpenTelemetry SDK
- Custom Exporter 구현

### Frontend
- Angular
- Tree View UI
- Dashboard 컴포넌트

### Database
- Amazon DynamoDB (On-Demand + TTL)

### Cloud
- AWS

---

## 🎯 이력서 요약용 문장

OpenTelemetry 기반 커스텀 Observability 플랫폼 구축.

- 실시간 사용자 활동 및 분산 트레이스 수집
- Custom Exporter 구현 및 DynamoDB 저장 구조 설계
- Trace Tree 재구성 로직 구현
- Top10 성능 분석 대시보드 개발
- Datadog 없이 내부 APM 시스템 설계
