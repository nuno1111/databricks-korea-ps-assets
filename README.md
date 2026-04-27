# databricks-korea-ps-assets

데이터브릭스 코리아 PS Team의 자산 모음입니다. 고객 프로젝트에서 검증된 코드, 데모, 아키텍처를 한곳에 모아 팀 내에서 재사용할 수 있도록 정리합니다.

## 카테고리

| 카테고리 | 설명 |
|----------|------|
| [libraries/](./libraries/) | 재사용 가능한 Python 라이브러리(wheel) — 공용 커넥터, 유틸리티 |
| [pipelines/](./pipelines/) | DAB 기반 프로덕션 ETL 파이프라인 |
| [demos/](./demos/) | 고객 데모/PoC 노트북 |
| [architectures/](./architectures/) | 아키텍처 다이어그램, 마이그레이션 플랜 |
| [genai/](./genai/) | Generative AI 관련 자산 |

## 기여 방법

모든 자산은 **PR 기반**으로 추가합니다. 자세한 절차는 [CONTRIBUTING.md](./CONTRIBUTING.md)를 참고하세요.

```bash
git clone https://github.com/nuno1111/databricks-korea-ps-assets
cd databricks-korea-ps-assets
git checkout -b add/<asset-name>
# ... 자산 추가 ...
gh pr create
```

⚠️ **이 저장소는 public입니다.** 모든 PR은 머지 전 [CONTRIBUTING.md의 익명화 체크리스트](./CONTRIBUTING.md#익명화-체크리스트-필수)를 반드시 검토하세요.

## 관련 Databricks 제품
- Databricks Asset Bundles (DAB)
- Unity Catalog
- Lakeflow Connect
- Mosaic AI / MLflow
- Databricks SQL / AI/BI Dashboards
