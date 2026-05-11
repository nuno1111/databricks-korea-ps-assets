# Libraries

재사용 가능한 Python 라이브러리(wheel) 자산 모음입니다. Databricks 클러스터에 설치해 다양한 프로젝트에서 공통 모듈로 활용할 수 있는 패키지를 정리합니다.

## 자료 목록

| 자료명 | 설명 | 상태 |
|--------|------|------|
| dmp_module | RDBMS 커넥터 + RTW 패턴 + Job Logger 공용 라이브러리 | 🚧 TBD |
| connector-pack | (예시) JDBC 커넥터 모음 | 🚧 TBD |

## 자산 추가 가이드

라이브러리 자산은 다음을 포함합니다.
- `pyproject.toml` 또는 `setup.py` (패키지 정의)
- `src/` 패키지 코드
- `tests/` 단위 테스트
- `README.md` (설치/사용 가이드, 의존성, 버전 정보)

## 관련 Databricks 제품
- Databricks Asset Bundles (DAB)
- Unity Catalog Volumes (wheel 배포)
- Databricks Cluster Libraries
