# Dev Log

> 작업하면서 겪은 이슈, 해결 방법, 메모를 날짜별로 기록합니다.

---

## 2026-07-01

### 프로젝트 시작

- 개인 노트북에 VM 2대 구성 계획
  - VM1: Podman 호스트 (Ollama + 컨테이너 서비스)
  - VM2: 배포 서버
- OS: Rocky Linux 8.10
- AI 엔진: Gemma (Ollama로 서빙)
- 네트워크: 폐쇄망
- GitHub repo 생성: `podman-gemma`

### 환경 구성

- 머신 구성 확정
  - macOS (MacBook Pro 2018, Intel i7, RAM 16GB): VM1 호스트
  - Windows: VM2 호스트
- VM 소프트웨어 선택: VMware Fusion 13 (Intel Mac, 개인용 무료)
- Rocky Linux 8.10 x86_64 DVD ISO 다운로드 완료 (14.21GB)

### VM 생성 및 Rocky Linux 설치

- VMware Fusion 13으로 ISO 인식 시도 → 1시간 이상 스피너 지속, 실패
- **UTM** 으로 전환 (mac.getutm.app 무료 다운로드)
- UTM으로 VM 생성 성공
  - 이름: `rocky-vm1`
  - CPU: 4코어, RAM: 8GB, 디스크: 64GB, 아키텍처: x86_64
- Rocky Linux 8.10 설치 완료
- 재시작 후 ISO가 다시 마운트되는 문제 → UTM CD/DVD 초기화로 해결
- GNOME 데스크탑 정상 부팅 확인

### Podman 설치

- 네트워크 연결 안 됨 → `sudo nmcli con up enp0s1` 으로 해결
- `sudo dnf install -y podman` 설치 완료
- 버전 확인: `podman version 4.9.4-rhel`

### Ollama + Gemma 설치

- `zstd` 미설치로 Ollama 설치 실패 → `sudo dnf install -y zstd` 후 재시도
- Ollama 0.31.1 설치 완료 (`/usr/local`, systemd 서비스 자동 등록)
- API 주소: `127.0.0.1:11434`
- GPU 없음 → CPU-only 모드 (정상)
- Gemma 3 4B 모델 다운로드 완료 (`ollama pull gemma3:4b`)
- 동작 확인: `ollama run gemma3:4b "안녕하세요"` → 정상 응답

### 이슈

- **문제**: VMware Fusion 13에서 ISO 인식 1시간 이상 소요
- **원인**: 14GB DVD ISO 파일이 너무 커서 VMware가 처리 못함
- **해결**: UTM으로 VM 소프트웨어 교체

- **문제**: Rocky Linux 재시작 후 설치 메뉴가 다시 뜸
- **원인**: ISO가 CD/DVD에 계속 마운트된 상태
- **해결**: UTM VM 설정 → CD/DVD → 초기화

- **문제**: `sudo dnf install` 시 네트워크 오류
- **원인**: VM 네트워크 인터페이스(enp0s1)가 비활성 상태
- **해결**: `sudo nmcli con up enp0s1` 으로 활성화

- **문제**: Ollama 설치 시 `ERROR: This version requires zstd`
- **원인**: Rocky Linux에 zstd 미설치
- **해결**: `sudo dnf install -y zstd` 후 재설치

### SSH 원격 접속 설정

- VM1 IP: `192.168.64.2`
- macOS 터미널에서 `ssh admin@192.168.64.2` 접속 성공
- 이후 모든 작업 맥북 터미널(SSH)에서 진행

### Open WebUI 설치

- `podman run` 으로 Open WebUI 컨테이너 실행
- 초기 실행 시 `ERR_CONNECTION_REFUSED` → firewalld 3000 포트 오픈으로 해결
- Ollama 연결 문제 (`host.containers.internal` 미작동) → `--network=host` 옵션으로 변경
- Ollama 기본 설정이 `127.0.0.1`만 허용 → `OLLAMA_HOST=0.0.0.0` 설정 후 해결
- HuggingFace에서 sentence-transformers 모델 다운로드 중 (초기 구동 시 필요)

### 이슈

- **문제**: Open WebUI에서 Ollama 모델 연결 안 됨
- **원인**: Ollama가 `127.0.0.1:11434`만 리슨, 외부 접근 불가
- **해결**: `sudo systemctl edit ollama` → `Environment="OLLAMA_HOST=0.0.0.0"` 추가 후 재시작

- **문제**: `podman run -p 3000:8080 --network=host` 포트 매핑 무시됨
- **원인**: `--network=host` 사용 시 포트 매핑 옵션 무효화
- **해결**: `--network=host` 사용 시 8080 포트로 직접 접속

### Open WebUI 버전 이슈 해결

- `open-webui:main` (v0.10.1) → Gemma3:4b와 tools 호환 문제로 채팅 불가
- 에러: `registry.ollama.ai/library/gemma3:4b does not support tools`
- **해결**: `open-webui:v0.6.5` 로 다운그레이드 → 채팅 정상 동작 확인

### 다음 작업 계획

- DB(PostgreSQL) 컨테이너 추가
- Backend 컨테이너 추가
- Podman Compose로 전체 서비스 통합 관리

### 이슈

- **문제**: GitHub push 시 403 Permission denied
- **원인**: 로컬 git이 `seo-dongchan` 계정 HTTPS 인증 상태, repo는 `seo337dc` 소유
- **해결**: `gh auth login` 으로 `seo337dc` 재인증 후 remote URL을 토큰 포함 HTTPS로 변경

```bash
git remote set-url origin https://seo337dc:$(gh auth token)@github.com/seo337dc/podman-gemma.git
```

---

<!-- 아래 형식으로 계속 추가 -->

<!-- 
## YYYY-MM-DD

### 작업 내용

- 

### 이슈

- **문제**: 
- **원인**: 
- **해결**: 

### 메모

- 
-->
