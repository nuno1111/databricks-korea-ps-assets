# LangChain Agent

Databricks 환경에서 LangChain Agent를 활용하는 예제입니다.

## 개요

LangChain Agent는 LLM이 도구(Tools)를 활용하여 복잡한 작업을 수행할 수 있도록 해주는 프레임워크입니다.
이 예제에서는 Databricks Foundation Model APIs와 LangChain을 연동하여 Agent를 구성하는 방법을 다룹니다.

## 노트북 목록

| 노트북 | 설명 | Tool 방식 | 실행 방식 |
|--------|------|-----------|-----------|
| [vector_store.ipynb](./vector_store.ipynb) | llms.txt → Vector Search 구축 + Agent | Vector Search | Sync |
| [driver_async.ipynb](./driver_async.ipynb) | MCP 기반 Agent (비동기 버전) | MCP Server | Async |
| [driver_sync.ipynb](./driver_sync.ipynb) | llms.txt 기반 Agent (동기 버전) | llms.txt | Sync |

### vector_store.ipynb (Vector Search 구축 + Agent)

**llms.txt** 파일을 읽어 **Databricks Vector Search** 인덱스를 구축하고, 이를 활용하는 Agent 예제입니다.

- **데이터 소스**: [LangChain 공식 문서 llms.txt](https://docs.langchain.com/llms.txt) (Docs by LangChain)
- **주요 흐름**:
  1. llms.txt에서 마크다운 링크 URL을 파싱하여 각 문서 콘텐츠를 수집
  2. Unity Catalog Delta 테이블에 저장
  3. Vector Search Endpoint & Index 생성 (Delta Sync + 자동 임베딩)
  4. LangChain v1 `create_agent`로 문서 검색 Agent 구성
- **주요 컴포넌트**: `WorkspaceClient`, `DeltaSyncVectorIndexSpecRequest`, `ChatDatabricks`, `create_agent`
- **임베딩 모델**: `databricks-qwen3-embedding-0-6b` (Databricks 제공)
- **사용 사례**: llms.txt 기반 문서를 벡터화하여 유사도 검색 및 RAG Agent 구축

### driver_async.ipynb (비동기 버전)

**MCP(Model Context Protocol)** 서버와 연동하는 Agent 예제입니다.

- **특징**: LangChain의 MCP Tool은 **비동기(Async) 방식만 지원**하므로 비동기 패턴 사용
- **주요 컴포넌트**: `DatabricksMCPServer`, `DatabricksMultiServerMCPClient`
- **사용 사례**: MCP 서버에 연결하여 다양한 도구를 동적으로 활용

### driver_sync.ipynb (동기 버전)

**llms.txt** 파일을 활용하는 경량 Agent 예제입니다.

- **특징**: LangChain의 `stream_mode="messages"`를 사용한 **동기 방식** 표준 예제
- **주요 컴포넌트**: `LLMSTxtLoader`, `DocumentFetcher`, 커스텀 Tools
- **사용 사례**: MCP 없이 llms.txt 인덱스 기반으로 문서를 검색하고 가져오는 경량 구현

## 공통 아키텍처

```
┌─────────────────────────────────────────────────────────────┐
│                    MLflow ResponsesAgent                     │
│  - predict(): 단일 응답                                      │
│  - predict_stream(): 스트리밍 응답                           │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                     LangChain Agent                          │
│  - ChatDatabricks (Foundation Model API)                     │
│  - Tools (MCP / Custom / Vector Search)                      │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                  Databricks Platform                         │
│  - Model Serving Endpoint                                    │
│  - Unity Catalog (Delta Tables)                              │
│  - Vector Search (Endpoint + Index)                          │
│  - MLflow                                                    │
└─────────────────────────────────────────────────────────────┘
```

## 사전 요구사항

- Databricks Workspace
- Unity Catalog 활성화
- Foundation Model APIs 접근 권한
- Serverless 또는 DBR 17 이상 클러스터
- Vector Search 사용 권한 (vector_store.ipynb 실행 시)

## 주요 파일 (노트북 실행 시 생성)

### Vector Search 버전 (vector_store)
- Delta 테이블: `ml.default.langchain_llms_txt_documents`
- Vector Search Endpoint: `langchain_docs_vs_endpoint`
- Vector Search Index: `ml.default.langchain_docs_vs_index`

### 비동기 버전 (driver_async)
- `mcp_langchain_agent.py` - MCP 기반 Agent 코어 로직
- `mcp_langchain_wrapper_agent.py` - MLflow ResponsesAgent 래퍼

### 동기 버전 (driver_sync)
- `langchain_sync_agent.py` - llms.txt 기반 Agent 코어 로직
- `langchain_sync_wrapper_agent.py` - MLflow ResponsesAgent 래퍼

## 참고 자료

- [LangChain 공식 문서](https://docs.langchain.com/)
- [LangChain llms.txt](https://docs.langchain.com/llms.txt)
- [Databricks Foundation Model APIs](https://docs.databricks.com/en/machine-learning/foundation-models/index.html)
- [Databricks Vector Search](https://docs.databricks.com/en/generative-ai/vector-search.html)
- [Databricks Agent Framework](https://docs.databricks.com/generative-ai/agent-framework/author-agent.html)
- [MLflow ResponsesAgent](https://mlflow.org/docs/latest/api_reference/python_api/mlflow.pyfunc.html#mlflow.pyfunc.ResponsesAgent)
