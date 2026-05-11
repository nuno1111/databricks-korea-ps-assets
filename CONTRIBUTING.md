# Contributing Guide

데이터브릭스 코리아 PS 팀의 자산 저장소에 기여하는 방법을 안내합니다. 모든 자산은 **PR 기반**으로 추가/수정합니다.

## 카테고리 구조

자산은 성격에 따라 아래 카테고리 중 하나에 추가합니다.

| 카테고리 | 설명 | 예시 |
|----------|------|------|
| `libraries/` | 재사용 가능한 Python 라이브러리(wheel) | dmp_module |
| `pipelines/` | DAB 기반 프로덕션 ETL 파이프라인 | database-pipeline |
| `demos/` | 고객 데모/PoC 노트북 | salesforce-lakeflow |
| `architectures/` | 아키텍처 다이어그램/마이그레이션 플랜 | salesforce-to-databricks |
| `genai/` | GenAI 관련 자산 | langchain-agent |

## PR 워크플로우

```bash
# 1. clone & 브랜치 생성
git clone https://github.com/nuno1111/databricks-korea-ps-assets
cd databricks-korea-ps-assets
git checkout -b add/<asset-name>

# 2. 자산 추가
mkdir -p <category>/<asset-name>
# ... 코드/문서 작성 ...

# 3. 익명화 체크리스트 검토 (아래 참조)

# 4. 커밋 & PR
git add <category>/<asset-name>
git commit -m "feat: add <asset-name> to <category>"
git push -u origin add/<asset-name>
gh pr create
```

## 자산별 README 템플릿

자산 디렉토리에 반드시 `README.md`를 포함하세요.

```markdown
# <Asset Name>

## Overview
- 한 줄 설명
- 주요 사용 사례 (Use Case)

## Prerequisites
- Databricks 환경 (Unity Catalog, Serverless 등)
- 외부 의존성 (다른 자산, 라이브러리)

## Quick Start
1. 설치/배포 단계
2. 실행 방법
3. 주요 파라미터

## Architecture
- 아키텍처 다이어그램 (Mermaid/ASCII/이미지)
- 데이터 흐름 설명

## Originally Authored
- @username (유지보수 담당자, 필수)
```

## 익명화 체크리스트 (필수)

⚠️ **이 저장소는 public입니다. 모든 PR은 머지 전 아래 항목을 점검하세요.**

### 식별자
- [ ] AWS 계정 ID, ARN 제거 또는 placeholder(`123456789012`) 처리
- [ ] Databricks Workspace URL 일반화 (`https://my-workspace.cloud.databricks.com`)
- [ ] Service Principal UUID, Webhook ID, Policy ID 마스킹
- [ ] AWS Secret 이름/경로에서 고객사명 제거

### 고객/조직 식별 정보
- [ ] 고객사명 (예: 삼성, KE, 대한항공) 직접 언급 제거 또는 `EXAMPLE_CO`로 치환
- [ ] 부서명/팀명/billing 태그 익명화
- [ ] 내부 시스템/프로젝트 코드명 (예: CSS, KEKAL) 일반화

### 개인 정보 (PII)
- [ ] 이메일 주소 제거 (`@example.com` 또는 익명)
- [ ] 한국 이름, 사번, 전화번호 제거
- [ ] 실제 데이터 샘플 제거 (반드시 가상 데이터 사용)

### 외부 링크
- [ ] 내부 Confluence/Jira/Drive 링크 제거
- [ ] 내부 문서 ID 마스킹
- [ ] Slack 채널/메시지 링크 제거

### 코드 내 하드코딩
- [ ] 비밀번호, API 키, 토큰 제거 (`dbutils.secrets.get(...)` 사용)
- [ ] DB 연결 문자열에서 호스트/계정 정보 제거
- [ ] S3 버킷 이름 일반화

## PR 본문 템플릿

```markdown
## Summary
- 추가하는 자산 설명 (1~2문장)

## Category
- `<libraries|pipelines|demos|architectures|genai>/<asset-name>/`

## Anonymization Checklist
- [ ] CONTRIBUTING.md의 익명화 체크리스트 모두 검토 완료

## Test Plan
- [ ] 코드 실행 검증
- [ ] README 따라 처음 사용자가 실행 가능한지 확인
```

## 코드 리뷰 기준

리뷰어는 다음을 확인합니다.
1. **익명화** — 위 체크리스트 모두 통과
2. **README 품질** — 처음 보는 사람이 따라할 수 있는가
3. **카테고리 적절성** — 올바른 카테고리에 위치하는가
4. **재사용성** — 다른 고객/프로젝트에 적용 가능한 수준인가

## 질문/제안

자산 추가가 애매하거나 카테고리가 불분명한 경우, 먼저 GitHub Issue를 열어 팀과 논의하세요.
