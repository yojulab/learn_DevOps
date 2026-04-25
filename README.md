# DevOps 개요

> **DevOps**는 소프트웨어 개발(Development)과 IT 운영(Operations)을 통합하여, 더 빠르고 안정적인 소프트웨어 배포를 실현하는 문화·철학·방법론이다.

---

## 1. DevOps란 무엇인가?

DevOps는 단순한 도구나 기술이 아니라 **조직 문화의 변화**다. 개발팀과 운영팀 사이의 장벽을 허물고, 협업·자동화·지속적 개선을 통해 고품질 소프트웨어를 빠르게 제공하는 것을 목표로 한다.

### 핵심 가치 (CALMS)

| 항목 | 설명 |
|------|------|
| **C**ulture | 협업과 공유의 조직 문화 |
| **A**utomation | 반복 작업의 자동화 |
| **L**ean | 낭비 제거, 지속적 흐름 |
| **M**easurement | 측정과 데이터 기반 의사결정 |
| **S**haring | 지식·도구·책임의 공유 |

---

## 2. DevOps 무한 루프 (주요 흐름)

DevOps는 끝이 없는 **무한 루프(∞)** 구조로 이루어진다. 계획부터 모니터링까지의 사이클이 반복되며 지속적으로 개선된다.

```mermaid
flowchart LR
    A([🗂️ Plan\n계획]) --> B([💻 Code\n개발])
    B --> C([🔨 Build\n빌드])
    C --> D([🧪 Test\n테스트])
    D --> E([🚀 Release\n릴리즈])
    E --> F([📦 Deploy\n배포])
    F --> G([⚙️ Operate\n운영])
    G --> H([📊 Monitor\n모니터링])
    H --> A

    style A fill:#6366f1,color:#fff,stroke:none
    style B fill:#8b5cf6,color:#fff,stroke:none
    style C fill:#ec4899,color:#fff,stroke:none
    style D fill:#f43f5e,color:#fff,stroke:none
    style E fill:#f97316,color:#fff,stroke:none
    style F fill:#eab308,color:#fff,stroke:none
    style G fill:#22c55e,color:#fff,stroke:none
    style H fill:#06b6d4,color:#fff,stroke:none
```

---

## 3. CI/CD 파이프라인

DevOps의 핵심 실천법은 **CI/CD (지속적 통합 / 지속적 배포)** 파이프라인이다.

```mermaid
flowchart TD
    DEV[👨‍💻 개발자\nCode Push] --> GIT[📁 Git Repository]
    GIT --> CI

    subgraph CI["🔄 CI - 지속적 통합 (Continuous Integration)"]
        direction LR
        C1[코드 체크아웃] --> C2[의존성 설치]
        C2 --> C3[단위 테스트]
        C3 --> C4[코드 품질 분석]
        C4 --> C5[빌드 & 아티팩트 생성]
    end

    CI --> CD

    subgraph CD["🚀 CD - 지속적 배포 (Continuous Deployment)"]
        direction LR
        D1[스테이징 배포] --> D2[통합 테스트]
        D2 --> D3{승인\nGate}
        D3 -->|통과| D4[프로덕션 배포]
        D3 -->|실패| D5[🔔 알림 & 롤백]
    end

    CD --> MON[📊 모니터링 & 피드백]
    MON --> DEV
```

---

## 4. DevOps 주요 구성 요소

```mermaid
mindmap
  root((DevOps))
    소스 관리
      Git
      GitHub / GitLab
      Bitbucket
    CI/CD 도구
      Jenkins
      GitHub Actions
      GitLab CI
      CircleCI
    컨테이너 & 오케스트레이션
      Docker
      Kubernetes
      Helm
    인프라 자동화
      Terraform
      Ansible
      Pulumi
    모니터링 & 로깅
      Prometheus
      Grafana
      ELK Stack
      Datadog
    클라우드 플랫폼
      AWS
      GCP
      Azure
```

---

## 5. DevOps vs 전통적 개발 방식

```mermaid
flowchart LR
    subgraph OLD["❌ 전통적 방식 (Waterfall)"]
        direction TB
        O1[요구사항 분석] --> O2[설계]
        O2 --> O3[개발 - 수개월]
        O3 --> O4[QA 테스트]
        O4 --> O5[운영팀에 전달]
        O5 --> O6[배포 - 불안정]
    end

    subgraph NEW["✅ DevOps 방식"]
        direction TB
        N1[계획] --> N2[개발 - 스프린트]
        N2 --> N3[자동 테스트]
        N3 --> N4[자동 배포]
        N4 --> N5[모니터링]
        N5 --> N1
    end

    OLD -->|전환| NEW
```

---

## 6. DevOps 성숙도 모델

```mermaid
graph LR
    L1["📌 Level 1\n초기\n수동 배포\n사일로 구조"]
    L2["📋 Level 2\n관리됨\n일부 자동화\nCI 도입"]
    L3["⚙️ Level 3\n정의됨\nCI/CD 완성\n협업 문화"]
    L4["📊 Level 4\n측정됨\n메트릭 기반\n지속 개선"]
    L5["🚀 Level 5\n최적화\nAI/ML 활용\n완전 자동화"]

    L1 --> L2 --> L3 --> L4 --> L5

    style L1 fill:#94a3b8,color:#fff,stroke:none
    style L2 fill:#60a5fa,color:#fff,stroke:none
    style L3 fill:#34d399,color:#fff,stroke:none
    style L4 fill:#f59e0b,color:#fff,stroke:none
    style L5 fill:#ef4444,color:#fff,stroke:none
```

---

## 7. 핵심 메트릭 (DORA Metrics)

Google의 DevOps Research & Assessment(DORA) 팀이 정의한 4가지 핵심 지표:

| 메트릭 | 설명 | 엘리트 수준 |
|--------|------|------------|
| **배포 빈도** (Deployment Frequency) | 얼마나 자주 배포하는가 | 하루 여러 번 |
| **변경 리드타임** (Lead Time for Changes) | 코드 커밋 → 프로덕션 배포까지 시간 | 1시간 미만 |
| **변경 실패율** (Change Failure Rate) | 배포 후 장애 발생 비율 | 0~15% |
| **복구 시간** (Time to Restore) | 장애 발생 후 복구까지 시간 | 1시간 미만 |

---

## 8. SRE (Site Reliability Engineering)

SRE는 DevOps의 실천적 구현체로, Google이 제안한 운영 방법론이다.

```mermaid
flowchart TD
    SRE["⚙️ SRE\nSite Reliability Engineering"]
    SRE --> SLI["📏 SLI\n서비스 수준 지표\nex. 가용성 99.9%"]
    SRE --> SLO["🎯 SLO\n서비스 수준 목표\nex. 월간 다운타임 < 43분"]
    SRE --> SLA["📄 SLA\n서비스 수준 협약\n고객과의 계약"]
    SRE --> EB["🧮 Error Budget\n허용 오류 예산\n위반 시 기능 개발 중단"]

    style SRE fill:#6366f1,color:#fff,stroke:none
    style SLI fill:#8b5cf6,color:#fff,stroke:none
    style SLO fill:#06b6d4,color:#fff,stroke:none
    style SLA fill:#22c55e,color:#fff,stroke:none
    style EB fill:#f97316,color:#fff,stroke:none
```

---

## 9. DevOps 도입 시 고려사항

1. **문화부터 바꿔라** — 도구보다 조직 문화의 변화가 우선이다.
2. **작게 시작하라** — 한 팀, 한 파이프라인부터 시범 적용한다.
3. **측정하라** — DORA 메트릭을 기준으로 현재 수준을 파악한다.
4. **자동화하라** — 반복되는 수동 작업을 모두 자동화 대상으로 삼는다.
5. **피드백 루프를 짧게 유지하라** — 문제를 빠르게 발견하고 빠르게 수정한다.
6. **보안을 내재화하라 (DevSecOps)** — 보안을 파이프라인 처음부터 포함시킨다.

---
