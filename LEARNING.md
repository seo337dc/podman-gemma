# 학습 노트

> 프론트엔드 개발자가 AI 인프라를 구축하며 배운 것들을 정리합니다.

---

## VM (Virtual Machine)

- 물리 서버 1대 위에 독립된 가상 컴퓨터를 여러 대 올리는 기술
- 각 VM은 별도의 OS를 가짐
- 이 프로젝트에서는 Rocky Linux 8.10 사용
- **Rocky Linux**: CentOS 후속 격인 RHEL 계열 무료 리눅스 (기업용으로 안정적)

---

## Podman

- Docker와 거의 동일한 컨테이너 런타임
- **차이점**: 데몬(백그라운드 서비스)이 없음, rootless(root 권한 없이 실행 가능)
- Docker CLI 명령어 대부분 그대로 사용 가능 (`podman run`, `podman ps` 등)
- Rocky Linux에서 `dnf install podman` 으로 설치

### 컨테이너란?
- 앱 + 실행 환경을 하나로 패키징한 것
- VM보다 가볍고 빠름
- `docker-compose` → Podman에서는 `podman-compose` 또는 `podman play kube`

---

## Ollama

- 로컬에서 AI 모델을 실행하는 도구
- 설치하면 `localhost:11434` 에 API 서버가 뜸
- OpenAI API 형식과 호환 → 기존 코드 거의 그대로 사용 가능
- 모델 파일은 `~/.ollama/models/` 에 저장됨

```bash
ollama run gemma3:4b   # 모델 실행
ollama list            # 설치된 모델 목록
ollama pull gemma3:4b  # 모델 다운로드
```

---

## Gemma

- Google이 만든 오픈소스 LLM (Large Language Model)
- **Gemini와 다름**: Gemini는 클라우드 API, Gemma는 로컬 실행 가능
- 모델 크기별 종류:
  | 모델 | RAM 필요량 | 특징 |
  |------|-----------|------|
  | gemma3:1b | ~2GB | 가장 빠름, 품질 낮음 |
  | gemma3:4b | ~4GB | 균형잡힌 선택 |
  | gemma3:12b | ~8GB | 고품질, 느림 |
  | gemma3:27b | ~16GB+ | 최고품질, 매우 느림 |

---

## 폐쇄망 (Air-gapped Network)

- 인터넷과 완전히 격리된 네트워크
- 보안이 중요한 환경에서 사용
- 모든 파일(모델, 이미지 등)을 미리 다운받아서 내부로 옮겨야 함

---

## SELinux

- Rocky Linux 기본 보안 모듈
- 파일/프로세스 접근을 정책으로 제어
- Podman 볼륨 마운트 시 `:Z` 옵션 필요한 이유
  ```bash
  podman run -v /host/path:/container/path:Z ...
  ```

---

## systemd

- Linux 서비스 관리자
- Ollama를 백그라운드 서비스로 등록할 때 사용
- 서버 재시작 시 자동으로 Ollama가 뜨게 설정 가능

```bash
systemctl start ollama    # 서비스 시작
systemctl enable ollama   # 부팅 시 자동 시작 등록
systemctl status ollama   # 상태 확인
```

---

## VMware Fusion

- macOS에서 VM을 실행하는 소프트웨어
- **Fusion 13**: Intel Mac 지원, 개인용 무료
- **Fusion 26H1/25H2**: 최신 버전, Apple Silicon 최적화
- Intel Mac에는 13 버전이 더 안정적

### ISO란?
- CD/DVD 디스크 이미지 파일
- OS 설치 시 이 파일을 VM에 마운트해서 설치
- **DVD ISO**: 오프라인 설치 가능한 전체 패키지 (약 10GB)
- **Minimal ISO**: 최소 패키지만 포함, 나머지는 인터넷으로 설치 (약 1GB)
- 폐쇄망 환경이므로 반드시 **DVD ISO** 사용

### VM 만들 때 주요 설정
- **CPU**: 물리 코어의 절반 이하 할당 권장 (호스트 성능 유지)
- **RAM**: Gemma 4B 기준 최소 6GB, 권장 8GB
- **디스크**: 최소 50GB (OS + 모델 파일 포함)
- **네트워크**: NAT (기본) or Host-only (폐쇄망 구성 시)

---

## 네트워크 모드 (VM)

| 모드 | 설명 | 용도 |
|------|------|------|
| NAT | VM이 호스트 네트워크를 통해 인터넷 접속 | 기본값, 외부 접속 가능 |
| Bridged | VM이 호스트와 동일한 네트워크에 연결 | VM을 독립 PC처럼 사용 |
| Host-only | VM↔호스트 간 통신만 가능, 인터넷 차단 | 폐쇄망 시뮬레이션 |

이 프로젝트에서는 **Host-only** 또는 **Custom 내부망** 사용 예정

---

## Git 인증 방식

### HTTPS vs SSH
- **HTTPS**: ID/PW 또는 토큰으로 인증, 설정 쉬움
- **SSH**: 키 쌍으로 인증, 한번 설정하면 편리

### GitHub CLI (gh)
- `gh auth login`: GitHub 계정 인증
- `gh auth token`: 현재 인증된 토큰 출력
- 인증 후 `git push`가 자동으로 해당 계정으로 동작

### 여러 GitHub 계정 사용 시 주의
- 로컬에 여러 계정이 있으면 push 시 엉뚱한 계정으로 시도할 수 있음
- remote URL에 계정 명시하면 해결:
  ```bash
  git remote set-url origin https://계정명:토큰@github.com/계정명/repo.git
  ```
