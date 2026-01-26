# Langchain Agent

Databricks 환경에서 Langchain Agent를 활용하는 예제입니다.

## 개요

Langchain Agent는 LLM이 도구(Tools)를 활용하여 복잡한 작업을 수행할 수 있도록 해주는 프레임워크입니다.
이 예제에서는 Databricks Foundation Model APIs와 Langchain을 연동하여 Agent를 구성하는 방법을 다룹니다.

## 노트북 목록

| 노트북 | 설명 | Tool 방식 | 실행 방식 |
|--------|------|-----------|-----------|
| [driver_async.ipynb](./driver_async.ipynb) | MCP 기반 Agent (비동기 버전) | MCP Server | Async |
| [driver_sync.ipynb](./driver_sync.ipynb) | llms.txt 기반 Agent (동기 버전) | llms.txt | Sync |

### driver_async.ipynb (비동기 버전)

**MCP(Model Context Protocol)** 서버와 연동하는 Agent 예제입니다.

- **특징**: Langchain의 MCP Tool은 **비동기(Async) 방식만 지원**하므로 비동기 패턴 사용
- **주요 컴포넌트**: `DatabricksMCPServer`, `DatabricksMultiServerMCPClient`
- **사용 사례**: MCP 서버에 연결하여 다양한 도구를 동적으로 활용

### driver_sync.ipynb (동기 버전)

**llms.txt** 파일을 활용하는 경량 Agent 예제입니다.

- **특징**: Langchain의 `stream_mode="messages"`를 사용한 **동기 방식** 표준 예제
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
│                     Langchain Agent                          │
│  - ChatDatabricks (Foundation Model API)                     │
│  - Tools (MCP 또는 Custom)                                   │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                  Databricks Platform                         │
│  - Model Serving Endpoint                                    │
│  - Unity Catalog                                             │
│  - MLflow                                                    │
└─────────────────────────────────────────────────────────────┘
```

## 사전 요구사항

- Databricks Workspace
- Unity Catalog 활성화
- Foundation Model APIs 접근 권한
- Serverless 또는 DBR 17 이상 클러스터

## 주요 파일 (노트북 실행 시 생성)

### 비동기 버전 (driver_async)
- `mcp_langchain_agent.py` - MCP 기반 Agent 코어 로직
- `mcp_langchain_wrapper_agent.py` - MLflow ResponsesAgent 래퍼

### 동기 버전 (driver_sync)
- `langchain_sync_agent.py` - llms.txt 기반 Agent 코어 로직
- `langchain_sync_wrapper_agent.py` - MLflow ResponsesAgent 래퍼

## 참고 자료

- [Langchain 공식 문서](https://python.langchain.com/)
- [Databricks Foundation Model APIs](https://docs.databricks.com/en/machine-learning/foundation-models/index.html)
- [Databricks Agent Framework](https://docs.databricks.com/generative-ai/agent-framework/author-agent.html)
- [MLflow ResponsesAgent](https://mlflow.org/docs/latest/api_reference/python_api/mlflow.pyfunc.html#mlflow.pyfunc.ResponsesAgent)
