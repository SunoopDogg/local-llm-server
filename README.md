# local-llm-server

Docker Compose로 로컬 LLM을 서빙합니다. vLLM, Ollama, llama.cpp를 지원합니다.

## 빠른 시작

```bash
cp .env.example .env
vi .env
docker compose --profile <vllm|ollama|llama-cpp> up -d
```

## 엔진별 설정

### vLLM

| 변수 | 설명 |
|------|------|
| `VLLM_MODEL_NAME` | HuggingFace 모델 ID |
| `VLLM_SERVED_MODEL_NAME` | API 모델 별칭 |
| `VLLM_TENSOR_PARALLEL_SIZE` | GPU 분산 수 |
| `VLLM_GPU_MEMORY_UTILIZATION` | GPU 메모리 사용률 (0.0~1.0) |
| `VLLM_TOOL_CALL_PARSER` | Tool calling 파서 |
| `VLLM_REASONING_PARSER` | Reasoning 파서 |

```env
# 예시: Gemma 4
VLLM_MODEL_NAME=cyankiwi/gemma-4-26B-A4B-it-AWQ-4bit
VLLM_SERVED_MODEL_NAME=gemma-4
VLLM_TENSOR_PARALLEL_SIZE=2
VLLM_GPU_MEMORY_UTILIZATION=0.9
VLLM_TOOL_CALL_PARSER=gemma4
VLLM_REASONING_PARSER=gemma4
```

### Ollama

| 변수 | 설명 |
|------|------|
| `OLLAMA_MODEL_NAME` | Ollama 모델 태그 |

서비스 시작 후 모델을 pull해야 합니다:

```bash
docker exec ollama ollama pull <model>
```

### llama.cpp

| 변수 | 설명 |
|------|------|
| `LLAMACPP_MODEL_PATH` | 컨테이너 내 GGUF 모델 경로 |
| `LLAMACPP_GPU_LAYERS` | GPU에 올릴 레이어 수 (99=전부) |
| `LLAMACPP_CONTEXT_SIZE` | 컨텍스트 윈도우 크기 |

GGUF 모델 파일을 `models/` 디렉토리에 미리 배치해야 합니다.

## 공통 설정

| 변수 | 설명 |
|------|------|
| `PORT` | API 서버 포트 (기본값: `8000`) |

## 검증

```bash
curl http://localhost:8000/v1/models
```

## 사전 요구사항

- Docker, Docker Compose
- NVIDIA GPU + [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
