# podman-gemma

> 인터넷 없이 내 컴퓨터에서 AI를 직접 돌리는 프로젝트

---

## 한 줄 요약

**폐쇄망 환경에서 Gemma AI를 로컬 서버에 올려 ChatGPT처럼 사용하기**

---

## 왜 만드냐면

일반적으로 AI를 쓰려면 이렇게 해야 해요:

```
내 앱 → ChatGPT API (인터넷) → 응답
```

이 프로젝트는 이렇게 바꿉니다:

```
내 앱 → 내 서버 AI (인터넷 없음) → 응답
```

| 일반 AI API | 이 프로젝트 |
|------------|------------|
| 인터넷 필요 | 인터넷 없어도 됨 |
| 사용할수록 돈 나감 | 무료 |
| 데이터 외부 전송 | 내부에서만 처리 |
| 회사 보안망에서 못씀 | 폐쇄망에서 사용 가능 |

---

## 최종 결과물

완성되면 브라우저에서 아래처럼 동작해요.

```
맥북
└── VM (Rocky Linux)
    ├── Ollama        ← AI 실행 엔진 (localhost:11434)
    │   └── Gemma     ← Google 오픈소스 AI 모델
    └── Podman
        └── Open WebUI  ← 브라우저에서 ChatGPT처럼 사용
```

---

## 기술 스택

| 역할 | 기술 |
|------|------|
| 호스트 OS | macOS (MacBook Pro 2018, Intel) |
| 가상화 | VMware Fusion 13 |
| VM OS | Rocky Linux 8.10 (x86_64) |
| 컨테이너 | Podman |
| AI 엔진 | Ollama |
| AI 모델 | Gemma 3 (Google 오픈소스) |
| 채팅 UI | Open WebUI |
| 네트워크 | 폐쇄망 (Host-only) |

---

## 진행 상황

- [x] GitHub repo 생성
- [x] VMware Fusion 설치
- [x] Rocky Linux 8.10 ISO 다운로드
- [ ] VM 생성 및 Rocky Linux 설치
- [ ] Podman 설치
- [ ] Ollama + Gemma 설치
- [ ] Open WebUI 컨테이너 실행
- [ ] 브라우저에서 AI 채팅 확인

---

## 문서

| 파일 | 내용 |
|------|------|
| [TODO.md](./TODO.md) | 전체 작업 리스트 |
| [DEVLOG.md](./DEVLOG.md) | 날짜별 작업 기록 및 이슈 해결 |
| [LEARNING.md](./LEARNING.md) | VM, Podman, Ollama 등 개념 정리 |
