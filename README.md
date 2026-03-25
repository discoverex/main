# Discoverex

Discoverex는 AI 기반 게임 경험을 제공하기 위해 여러 저장소를 역할별로 분리해 운영하는 프로젝트다. 이 저장소는 전체 생태계를 한눈에 소개하는 메타 레포이며, 각 서비스와 실행 계층이 어떻게 연결되는지 빠르게 파악할 수 있도록 돕는다.

## 프로젝트 소개

Discoverex는 다음 흐름을 중심으로 구성된다.

- 웹 애플리케이션이 사용자 경험과 게임 진입점을 제공한다.
- 백엔드가 인증, 게임 API, 스토리지 연동을 담당한다.
- 엔진이 게임용 장면 생성과 검증을 수행한다.
- 오케스트레이터가 실험 실행, worker 운영, artifact 추적을 관리한다.
- 별도 생성 파이프라인이 매직아이 데이터셋과 모델을 준비한다.

즉, 프론트엔드, API, 생성 엔진, orchestration, 데이터/모델 파이프라인이 분리된 멀티레포 구조로 운영되는 AI 게임 플랫폼이다.

## 레포 구성

| Repository | Link | Role |
| --- | --- | --- |
| `backend` | <https://github.com/discoverex/backend> | FastAPI 기반 게임 백엔드. 인증, 공통 응답 구조, Magic Eye API, GCS/DB/Redis 연동을 담당한다. |
| `engine` | <https://github.com/discoverex/engine> | 숨은 오브젝트 장면 생성과 검증을 담당하는 코어 엔진. CLI, Prefect flow, worker 운영 도구를 함께 제공한다. |
| `orchestrator` | <https://github.com/discoverex/orchestrator> | 실험을 job 단위로 분해하고 실행을 제어하는 orchestration 레이어. Prefect, storage, MLflow, worker runtime을 관리한다. |
| `web` | <https://github.com/discoverex/web> | Next.js/Turborepo 기반 프론트엔드 모노레포. 게임 허브와 개별 게임 앱, 공용 UI 패키지를 포함한다. |
| `magic-eye-generator` | <https://github.com/discoverex/magic-eye-generator> | 매직아이 데이터셋 생성, 모델 학습, ONNX 변환, GCS 업로드를 담당하는 데이터/모델 파이프라인 저장소다. |

## 전체 구조

```text
Discoverex
├── web                   # 사용자-facing 앱과 게임 허브
├── backend               # 인증, 게임 API, 서비스 로직
├── engine                # 장면 생성/검증 엔진과 실행 CLI
├── orchestrator          # 실험 orchestration, worker, artifact 추적
├── magic-eye-generator   # 매직아이 데이터/모델 생성 파이프라인
├── data                  # 로컬 데이터 보조 디렉토리
└── runtime               # 로컬 런타임 보조 디렉토리
```

구조적으로는 `web -> backend -> engine/orchestrator -> storage/model assets` 흐름으로 이해하면 된다. `magic-eye-generator`는 주로 매직아이용 학습 데이터와 모델 자산을 준비하는 독립 파이프라인 역할을 맡는다.

## 기술 스택

### Frontend

- Next.js
- React
- Turborepo
- Tailwind CSS

### API and Services

- FastAPI
- Uvicorn
- Firebase 기반 인증 연동

### Orchestration and Runtime

- Prefect
- Python 3.11
- `uv`
- Typer

### ML and Generation

- PyTorch
- Diffusers
- Transformers
- ONNX / ONNX Runtime
- Ultralytics / MobileSAM

### Storage and Metadata

- Google Cloud Storage
- MinIO
- MLflow
- PostgreSQL
- Redis

### Infra and Delivery

- Docker
- GitHub Actions
- Google Cloud Run

## 어떻게 읽으면 되는가

- 사용자 경험과 앱 구성이 궁금하면 `web`부터 본다.
- 서비스 API와 인증 흐름이 궁금하면 `backend`를 본다.
- 생성 파이프라인과 실행 계약이 궁금하면 `engine`을 본다.
- 원격 실행, worker, artifact 추적 구조가 궁금하면 `orchestrator`를 본다.
- 매직아이 데이터셋과 모델 생성 과정을 보려면 `magic-eye-generator`를 본다.

이 저장소는 구현 코드를 담기보다 Discoverex 전체 구조를 빠르게 소개하는 진입점 역할을 한다.
