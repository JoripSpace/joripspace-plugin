# JoripSpace Plugin

Codex와 Claude Code에서 JoripSpace 프로젝트를 연결하고 제작·배포·관리하는 플러그인입니다.

프로젝트 시작, DB, 스토리지, 배포·롤백, 저장본, 템플릿, Secret 생성, 크론, 도메인, 메일, 사용량·로그와 Realtime v2 작업을 지원합니다. 소스 파일 편집은 에이전트의 기본 파일 도구를 사용합니다.

## 준비

- Codex 또는 Claude Code의 로컬 실행 환경
- Node.js 18 이상과 npm
- 작업할 폴더와 셸에 대한 접근 권한
- JoripSpace 계정과 기존 프로젝트

JoripSpace CLI를 미리 설치할 필요는 없습니다. CLI가 없으면 플러그인이 `npx -y @joripspace/cli@latest`로 실행합니다. 최초 다운로드에는 인터넷 연결이 필요합니다. MCP 서버를 등록하거나 OAuth MCP 연결을 승인할 필요는 없습니다.

## Codex 설치

터미널에서 실행하세요.

```powershell
codex plugin marketplace add JoripSpace/joripspace-plugin
codex plugin add joripspace@joripspace
```

설치 후 새 Codex 대화 또는 CLI 세션을 시작하세요.

## Claude Code 설치

터미널에서 실행하세요.

```powershell
claude plugin marketplace add JoripSpace/joripspace-plugin
claude plugin install joripspace@joripspace
```

설치 후 Claude Code를 다시 시작하거나 실행 중인 대화에서 `/reload-plugins`를 입력하세요.

## 사용

작업할 프로젝트 폴더에서 새 대화를 열고 다음처럼 요청하세요.

> JoripSpace `내-프로젝트-이름` 시작해줘.

에이전트가 CLI를 확인하고 프로젝트 연결과 온보딩을 진행합니다. 최초 연결에는 각 사용자가 브라우저에서 승인하고 표시된 5분 일회용 연결 코드를 전달해야 할 수 있습니다. 비밀번호나 API 토큰을 채팅에 넣지 마세요.

이후 자연어로 작업을 요청할 수 있습니다.

- 데이터베이스 테이블을 보여줘.
- 현재 폴더를 검증한 다음 배포해줘.
- 지금 작업한 내용을 저장본으로 만들어줘.
- 최근 배포와 오류 로그를 확인해줘.

프로젝트 생성·삭제, 결제, API 토큰, 멤버, 웹훅 관리와 외부 Secret 값 입력은 JoripSpace 웹에서 처리합니다. 로컬 셸과 파일 접근이 없는 웹 채팅에서는 이 플러그인의 CLI 작업을 실행할 수 없습니다.

## 문의

[JoripSpace](https://joripspace.com/) · [문서](https://joripspace.com/docs/) · [문제 신고](https://github.com/JoripSpace/joripspace-plugin/issues)
