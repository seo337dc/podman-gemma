# 작업 리스트

> 진행 상황을 트래킹합니다. `[ ]` 미완료 / `[x]` 완료

---

## 환경 구성

- [x] 개인 노트북에 VM 소프트웨어 설치 (UTM)
- [x] VM1 생성 — Rocky Linux 8.10 설치
- [ ] VM2 생성 — Rocky Linux 8.10 설치
- [ ] VM간 내부 네트워크 설정

---

## VM1 — Podman 호스트 설정

- [ ] Rocky 8.10 기본 설정 (hostname, timezone, locale)
- [x] Podman 설치 (`dnf install podman`)
- [x] firewalld 포트 오픈 (3000, 8080, 11434)
- [ ] SELinux 설정 확인

---

## SSH 원격 접속 설정

- [x] VM1 IP 확인 (192.168.64.2)
- [x] SSH 서비스 활성화 (sshd 기본 활성)
- [x] macOS 터미널에서 SSH 접속 확인
- [x] firewalld SSH 포트 오픈 확인 (22번)

---

## Ollama + Gemma 설치

- [x] Ollama 설치 (v0.31.1, systemd 자동 등록)
- [x] Gemma 3 4B 모델 다운로드
- [x] systemd 서비스 등록 (부팅 시 자동 시작)
- [x] Ollama 외부 접근 설정 (OLLAMA_HOST=0.0.0.0)
- [x] API 동작 확인 (`curl http://192.168.64.2:11434/api/tags`)

---

## Podman 서비스

- [x] Open WebUI 컨테이너 실행
- [ ] Open WebUI ↔ Ollama 연결 확인 (진행 중 — HuggingFace 파일 다운로드 중)
- [ ] 웹 브라우저에서 채팅 정상 동작 확인
- [ ] Podman Compose 파일 작성 (서비스 관리용)

---

## 다음 단계 (친구 엔지니어 제안)

- [ ] DB 컨테이너 설정 (PostgreSQL)
- [ ] Backend 컨테이너 설정
- [ ] 전체 서비스 Podman Compose로 통합

---

## VM2 — 배포 서버 설정

- [ ] Rocky 8.10 기본 설정
- [ ] VM1 서비스 연결 확인
- [ ] 리버스 프록시 설정 (Nginx or Caddy)

---

## 문서화

- [x] LEARNING.md 작성 (기본 개념 정리)
- [x] DEVLOG.md 생성
- [x] TODO.md 생성
- [x] README.md 작성
- [x] INSTALL.md 작성 (설치 가이드)
- [ ] 설치 스크립트 작성 (`scripts/setup.sh`)
