# Pipelines

Databricks Asset Bundles(DAB) 기반의 프로덕션 ETL 파이프라인 자산 모음입니다. 실제 고객 환경에서 검증된 데이터 파이프라인 패턴을 정리합니다.

## 자료 목록

| 자료명 | 설명 | 상태 |
|--------|------|------|
| database-pipeline | RDBMS → Delta Lake ETL 프레임워크 (PostgreSQL/Oracle/MSSQL/HANA) | 🚧 TBD |
| sap-bw-migration | SAP BW → Lakehouse 마이그레이션 파이프라인 | 🚧 TBD |
| zerobus-cdp | ZeroBus 기반 실시간 이벤트 수집 파이프라인 | 🚧 TBD |

## 자산 추가 가이드

파이프라인 자산은 다음을 포함합니다.
- `databricks.yml` (DAB 번들 정의)
- `configuration/variables.yml` (환경별 변수)
- `jobs/` (Job YAML 정의)
- `src/` (노트북, 변환 로직)
- `README.md` (아키텍처, 배포 절차, 파라미터 설명)

## 관련 Databricks 제품
- Databricks Asset Bundles (DAB)
- Lakeflow Connect / Lakeflow Declarative Pipelines
- Delta Live Tables (DLT)
- Unity Catalog
- Databricks Workflows
