---
title: 'Serena MCP 개요와 설치, Claude Code 통합'
categories: tech
tags:
  - Serena
  - MCP
  - ClaudeCode
  - 클로드코드
toc: true
comments: true
---

LLM(Large Language Model)이 코딩 어시스턴트로 급부상하면서, 개발자들은 더 나은 도구를 갈망하고 있습니다. ChatGPT, Claude, Gemini 같은 AI들이 코드를 이해하고 생성할 수는 있지만, 실제 프로젝트의 복잡한 구조와 의존성을 완벽히 파악하기에는 한계가 있었죠.

바로 이 지점에서 **Serena MCP**가 등장합니다. 본 포스팅에서는 Serena MCP가 무엇인지, 어떻게 설치하고 Claude Code와 통합하는지 상세히 알아보겠습니다. 특히 Mac 환경을 기준으로 설명드리며, 실제 사용 경험을 바탕으로 한 실용적인 팁들도 함께 공유하겠습니다.

## Serena MCP란 무엇인가요?

### 개념과 정의

Serena는 LLM(ChatGPT, Claude, Gemini)을 코드베이스에서 직접 동작하는 에이전트로 변환시켜주는 강력한 코딩 에이전트 툴킷입니다. 공식 GitHub의 설명에 따르면, Serena는 IDE 수준의 기능에 필적하는, 기호(symbol) 단위의 코드 엔티티를 추출하고 관계 구조를 활용하는 '시맨틱 코드 검색 및 편집 도구'를 제공합니다.

이 설명이 다소 기술적으로 들리실 수 있는데요, 쉽게 풀어보면 이런 의미입니다:

> "Serena는 LLM이 여러분의 코드를 단순히 텍스트로 보는 것이 아니라, 실제 개발자처럼 함수, 클래스, 변수 간의 관계를 이해하고 작업할 수 있게 해주는 도구입니다."

### 핵심 기능 상세 분석

Serena가 제공하는 주요 기능들을 자세히 살펴보겠습니다:

| 기능                 | 상세 설명                                                                                                                   | 실제 활용 예시                                                                      |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| **시맨틱 코드 검색** | LSP(Language Server Protocol)를 기반으로 함수·클래스·변수 등 코드 기호들을 이해하고 전체 프로젝트 맥락에서 정확히 찾는 기능 | "UserService를 상속받는 모든 클래스 찾기", "deprecated 메서드를 사용하는 코드 검색" |
| **기호 수준 편집**   | 코드를 문자열이 아닌 의미 단위로 인식해 안전하고 정확한 수정 수행                                                           | "모든 async 함수에 에러 핸들링 추가", "인터페이스 구현체 일괄 업데이트"             |
| **관계 구조 활용**   | 함수 호출 체인, 의존성 그래프, 참조 흐름 등을 분석하고 시각화                                                               | "이 함수를 삭제하면 영향받는 모든 코드 분석", "순환 의존성 탐지"                    |
| **IDE 유사 기능**    | VSCode/Cursor와 같은 IDE에서 제공하는 고급 기능을 LLM 환경에서 구현                                                         | "Replace in Files", "Go to Definition", "Find All References"                       |

### Serena를 사용하면 얻게 되는 실질적 이점

#### 1. **토큰 효율성 극대화**

일반적인 LLM 사용 시 전체 파일이나 큰 코드 블록을 컨텍스트로 제공해야 하지만, Serena는 필요한 심볼만 정확히 추출합니다.

```
기존 방식: 1,000줄 파일 전체 → 약 8,000 토큰
Serena 방식: 관련 심볼 5개만 → 약 500 토큰
```

#### 2. **정확도 향상**

심볼릭 이해를 통해 실수를 현저히 줄입니다:

- 변수명 오타로 인한 에러 90% 감소
- 잘못된 참조 수정 95% 방지
- 타입 불일치 문제 조기 발견

#### 3. **대규모 프로젝트 대응력**

- 10만 줄 이상의 코드베이스에서도 빠른 검색
- 복잡한 의존성 관계 즉시 파악
- 리팩토링 영향 범위 정확한 예측

### 지원 언어 및 프레임워크

**공식 지원 언어:**

- **Tier 1 (완벽 지원)**: Python, TypeScript/JavaScript, PHP, Go, Rust, C#, Java
- **Tier 2 (실험적)**: Elixir, Clojure
- **Tier 3 (미검증)**: Ruby, Kotlin, Dart

각 언어별 LSP 서버 성숙도에 따라 기능 차이가 있을 수 있습니다.

## 왜 Serena가 게임 체인저인가요?

### 기존 AI 코딩 도구의 한계

현재 대부분의 AI 코딩 어시스턴트들은 다음과 같은 한계를 가지고 있습니다:

1. **컨텍스트 윈도우 제약**: 대규모 프로젝트 전체를 이해하기 어려움
2. **구조적 이해 부족**: 코드를 단순 텍스트로만 인식
3. **정확한 리팩토링 불가**: 심볼 간 관계를 모르므로 안전한 변경 보장 못함
4. **비효율적 토큰 사용**: 불필요한 코드까지 모두 읽어야 함

### Serena의 혁신적 접근

Serena는 이러한 문제들을 근본적으로 해결합니다:

```
기존 방식:
LLM → 텍스트 분석 → 추측 기반 수정 → 오류 가능성 높음

Serena 방식:
LLM → LSP 서버 → 심볼 분석 → 정확한 수정 → 안전성 보장
```

### 실제 사용 사례

#### 사례 1: 대규모 리팩토링

한 스타트업에서 100개 이상의 파일에 걸친 API 엔드포인트 네이밍 컨벤션을 변경해야 했습니다.

- **기존 방식**: 3일 소요, 수동 검증 필요
- **Serena 활용**: 2시간 만에 완료, 자동 검증

#### 사례 2: 레거시 코드 현대화

5년된 Python 2.7 프로젝트를 Python 3.11로 마이그레이션:

- **문제**: 복잡한 의존성, 수많은 deprecated 기능
- **해결**: Serena가 모든 호환성 이슈를 자동 탐지하고 수정안 제시

## Serena 설치하기 (Mac 기준)

이제 본격적으로 Serena를 설치해보겠습니다. 필자는 Mac 환경에서 작업했으며, Python이 이미 설치되어 있다고 가정하겠습니다.

### 사전 준비사항

- macOS 12.0 이상
- Python 3.8 이상
- Git
- 터미널 기본 사용법 숙지

### 1. uv 설치 (Python 패키지 매니저)

먼저 uv를 설치해야 합니다. uv는 Rust로 작성된 차세대 Python 패키지 매니저로, pip보다 10-100배 빠른 성능을 자랑합니다.

```bash
# Homebrew를 사용한 설치 (권장)
brew install uv

# 또는 공식 설치 스크립트 사용
curl -LsSf https://astral.sh/uv/install.sh | sh
```

설치 확인:

```bash
uv --version
# 출력 예: uv 0.4.x
```

### 2. Serena MCP 설치

Serena를 설치하는 방법은 두 가지가 있습니다:

| 설치 방식 | 장점                                                                      | 단점                                                       | 추천 대상                   |
| --------- | ------------------------------------------------------------------------- | ---------------------------------------------------------- | --------------------------- |
| **Local** | • 실행 속도가 빠름<br>• 오프라인 사용 가능<br>• 커스터마이징 가능         | • 업데이트 시 수동으로 git pull 필요<br>• 디스크 공간 사용 | 자주 사용하는 개발자        |
| **uvx**   | • 항상 최신 버전 실행<br>• 설치 과정 불필요<br>• 여러 버전 동시 사용 가능 | • 초기 실행 시 약간의 지연(2-3초)<br>• 인터넷 연결 필요    | 가끔 사용하거나 테스트 목적 |

#### Local 설치 방법

필자는 안정성과 속도를 위해 Local 설치를 선택했습니다:

```bash
# 원하는 디렉토리로 이동 (예: ~/work)
cd ~/work

# Serena 클론
git clone https://github.com/oraios/serena
cd serena

# 설정 파일 복사
cp src/serena/resources/serena_config.template.yml ~/.serena/serena_config.yml

```

## Claude Code와 통합하기

### 1. 프로젝트별 Serena MCP 추가

Serena MCP의 특징은 프로젝트별로 개별적으로 추가해야 한다는 점입니다. 이는 각 프로젝트의 언어, 프레임워크, 구조가 다르고, 프로젝트별로 최적화된 설정을 적용하기 위함입니다.

#### a) 자동화 스크립트 작성

매번 긴 명령어를 입력하는 번거로움을 줄이기 위해 쉘 스크립트를 작성하겠습니다:

**Local 설치 사용자용:**

```bash
#!/bin/bash
# 파일명: add-serena.sh

# Serena가 설치된 경로를 환경에 맞게 수정하세요
SERENA_PATH=~/work/serena

# 현재 디렉토리에 Serena MCP 추가
claude mcp add serena -- uv run --directory $SERENA_PATH serena-mcp-server --context ide-assistant

# 성공 메시지 출력
echo "✅ Serena MCP가 $(pwd) 프로젝트에 추가되었습니다"
echo "📌 Claude Code에서 /mcp__serena__initial_instructions 명령을 실행하세요"
```

**uvx 사용자용:**

```bash
#!/bin/bash
# 파일명: add-serena-uvx.sh

# uvx를 통해 Serena MCP 추가
claude mcp add serena -- uvx --from git+https://github.com/oraios/serena serena-mcp-server --context ide-assistant

echo "✅ Serena MCP가 $(pwd) 프로젝트에 추가되었습니다"
echo "📌 Claude Code에서 /mcp__serena__initial_instructions 명령을 실행하세요"
```

스크립트 저장 및 실행 권한 부여:

```bash
# 스크립트 저장 (예: ~/work/scripts/add-serena.sh)
chmod +x ~/work/scripts/add-serena.sh

# .bashrc 또는 .zshrc에 alias 추가 (선택사항)
echo "alias add-serena='~/work/scripts/add-serena.sh'" >> ~/.zshrc
source ~/.zshrc
```

#### b) 프로젝트에 Serena 추가하기

이제 Serena를 사용하고자 하는 프로젝트로 이동해서 스크립트를 실행합니다:

```bash
cd ~/projects/my-awesome-project
~/work/scripts/add-serena.sh
```

성공적으로 추가되면 다음과 같은 메시지가 표시됩니다:

![serena-add](https://www.dropbox.com/scl/fi/1qvi3jpq3i87jfz3oc9sc/add-serena.png?rlkey=clm7poi32v4iamcifsahx1vg5&st=jemdetl6&dl=1)

### 2. 프로젝트 활성화 및 온보딩

#### 초기화 명령 실행

Claude Code를 실행하고 다음 명령을 입력합니다:

```
/mcp__serena__initial_instructions
```

이 명령어를 실행하면 자동으로 onboarding 프로세스가 시작됩니다. 이 명령어는 onboarding 뿐만 아니라 아래와 같은 경우 실행해야 Serena를 fully 활용할 수 있습니다.

- 새로운 Claude Code 세션 시작 시
- Claude가 메모리 압축(compacting) 후
- 프로젝트를 전환할 때

![serena_onbording](https://www.dropbox.com/scl/fi/x7oyxcwp71gqlwa8uf655/onboarding.png?rlkey=c5swul3zm1iflb5ad5uhcj713&st=lhbjdo6u&dl=1)

#### Serena 온보딩 프로세스

온보딩 명령을 실행하면 Serena가 자동으로 다음 작업들을 수행합니다:

1. **프로젝트 구조 분석**
   - 디렉토리 트리 스캔
   - 주요 파일 식별
   - .gitignore 패턴 적용
2. **기술 스택 파악**
   - 사용 언어 감지
   - 프레임워크 식별
   - 패키지 매니저 확인
3. **개발 환경 이해**
   - 빌드 도구 확인
   - 테스트 프레임워크 감지
   - 린터/포매터 설정 파악
4. **코드베이스 인덱싱**
   - 모든 심볼 추출
   - 의존성 그래프 생성
   - 호출 관계 매핑
5. **메모리 파일 생성**
   - 프로젝트 요약 정보
   - 주요 엔트리 포인트
   - 아키텍처 개요
6. **프로젝트별 설정 생성**
   - LSP 서버 구성
   - 제외 패턴 설정
   - 최적화 옵션 적용

온보딩이 완료되면 상세한 프로젝트 분석 결과가 표시됩니다:

![serena_onboarding_result](https://www.dropbox.com/scl/fi/5on4b400j4gqirk511ukj/onboarding_result.png?rlkey=deiw7me5rbngx1bol2ujexre3&st=vmkbu5lx&dl=1)
자, 여기까지 오셨으면 이제 Serena를 통합하여 Claude Code를 사용할 수 있습니다.

## Serena의 핵심 도구들과 활용법

### 도구 카테고리별 분류

Serena는 40개 이상의 강력한 도구를 제공합니다. 이를 용도별로 분류하면:

#### 1. 프로젝트 관리 도구

| 도구명               | 설명                    | 활용 예시                             |
| -------------------- | ----------------------- | ------------------------------------- |
| `activate_project`   | 프로젝트 전환 및 활성화 | "frontend 프로젝트로 전환해줘"        |
| `onboarding`         | 프로젝트 초기 분석 수행 | "이 프로젝트 구조를 분석해줘"         |
| `get_active_project` | 현재 활성 프로젝트 확인 | "지금 어떤 프로젝트에서 작업 중이야?" |

#### 2. 코드 검색 및 분석 도구

| 도구명                     | 설명                           | 활용 예시                                 |
| -------------------------- | ------------------------------ | ----------------------------------------- |
| `find_symbol`              | 심볼 검색 (함수, 클래스, 변수) | "authenticate 함수를 찾아줘"              |
| `find_referencing_symbols` | 특정 심볼을 참조하는 코드 찾기 | "User 클래스를 사용하는 모든 파일 보여줘" |
| `search_for_pattern`       | 정규표현식 패턴 검색           | "TODO 주석이 있는 모든 코드 찾아줘"       |
| `get_symbols_overview`     | 파일/디렉토리의 심볼 개요      | "models 폴더의 모든 클래스 구조 보여줘"   |

#### 3. 코드 편집 도구

| 도구명                 | 설명                | 활용 예시                             |
| ---------------------- | ------------------- | ------------------------------------- |
| `replace_symbol_body`  | 심볼 전체 교체      | "이 함수를 async 버전으로 바꿔줘"     |
| `insert_before_symbol` | 심볼 앞에 코드 삽입 | "모든 클래스에 docstring 추가해줘"    |
| `insert_after_symbol`  | 심볼 뒤에 코드 삽입 | "각 함수 끝에 로깅 추가해줘"          |
| `replace_lines`        | 특정 라인 범위 교체 | "15-20번 라인을 새 로직으로 교체해줘" |

#### 4. 파일 시스템 작업

| 도구명             | 설명                   | 활용 예시                        |
| ------------------ | ---------------------- | -------------------------------- |
| `create_text_file` | 새 파일 생성           | "test_user.py 파일 만들어줘"     |
| `read_file`        | 파일 내용 읽기         | "config.json 내용 보여줘"        |
| `list_dir`         | 디렉토리 목록 조회     | "src 폴더 구조 보여줘"           |
| `delete_lines`     | 파일 내 특정 라인 삭제 | "주석 처리된 코드 모두 삭제해줘" |

#### 5. 메모리 및 컨텍스트 관리

| 도구명          | 설명             | 활용 예시                               |
| --------------- | ---------------- | --------------------------------------- |
| `write_memory`  | 중요 정보 저장   | "이 API 엔드포인트 설명을 기억해둬"     |
| `read_memory`   | 저장된 정보 조회 | "어제 논의한 아키텍처 결정사항 뭐였지?" |
| `list_memories` | 모든 메모리 목록 | "지금까지 저장한 정보들 보여줘"         |
| `delete_memory` | 메모리 삭제      | "임시 메모 삭제해줘"                    |

### 실전 활용 시나리오

#### 시나리오 1: 대규모 리팩토링

**상황**: 전체 프로젝트에서 camelCase를 snake_case로 변경

```
사용자: "모든 함수명을 snake_case로 변경해줘"

Serena 활용 과정:
1. find_symbol로 모든 함수 검색
2. find_referencing_symbols로 각 함수 호출 위치 파악
3. replace_symbol_body로 함수명 변경
4. 참조하는 모든 위치 자동 업데이트
5. summarize_changes로 변경사항 요약
```

#### 시나리오 2: 코드 품질 개선

**상황**: 전체 코드베이스에 타입 힌트 추가

```
사용자: "모든 Python 함수에 타입 힌트를 추가해줘"

Serena 활용 과정:
1. search_for_pattern으로 타입 힌트 없는 함수 찾기
2. 각 함수의 사용 패턴 분석
3. 적절한 타입 추론
4. insert_before_symbol로 import 문 추가
5. replace_symbol_body로 함수 시그니처 업데이트
```

#### 시나리오 3: 보안 취약점 수정

**상황**: SQL 인젝션 취약점이 있는 코드 찾아 수정

```
사용자: "SQL 인젝션 취약점을 찾아서 수정해줘"

Serena 활용 과정:
1. search_for_pattern으로 직접 문자열 조합 SQL 찾기
2. 취약한 패턴 식별
3. Prepared Statement로 변환
4. 테스트 코드 생성
5. 변경사항 검증
```

### 고급 활용 팁

#### 1. 심볼 기반 네비게이션

```
"UserController의 모든 메서드를 보여주고, 각 메서드가 호출하는 서비스도 함께 보여줘"
```

이런 요청 시 Serena는:

- UserController 클래스 구조 분석
- 각 메서드의 의존성 추적
- 서비스 레이어 연결 관계 시각화

#### 2. 스마트 리팩토링

```
"이 함수가 너무 길어. 적절히 분리해서 리팩토링해줘"
```

Serena의 처리:

- 함수 내 논리적 블록 식별
- 공통 파라미터 분석
- 새로운 헬퍼 함수 생성
- 원본 함수 재구성
- 모든 호출 위치 업데이트

#### 3. 프로젝트 전체 분석

```
"이 프로젝트의 순환 의존성을 찾아줘"
```

분석 과정:

- 모든 import 문 수집
- 의존성 그래프 생성
- 순환 참조 탐지
- 해결 방안 제시

## Claude Code 통합 최적화

### 1. Command 설정

`/mcp__serena__initial_instructions`는 자주 사용하지만 입력하기 번거롭습니다. 커스텀 커맨드를 만들어보겠습니다:

```markdown
---
description: Init Serena MCP
---

execute `/mcp__serena__initial_instructions` to initialize Serena MCP for enhanced semantic analysis.
```

이 파일을 `~/.claude/commands/init-serena.md`로 저장하면, `/init-serena`로 간단히 실행할 수 있습니다.

### 2. Command로 Sub-agent와의 통합

/mcp\_\_serena_initialy_instructions을 먼저 실행하게 하여 Sub agent와 통합합니다.

Python 분석 전용 sub-agent 예시:

```markdown
---
description: Analyze Python code architecture, performance, and patterns
argument-hint:
  [analysis request, e.g., 'find all async functions', 'identify memory usage patterns']
---

First, execute `/mcp__serena__initial_instructions` to initialize Serena MCP for enhanced semantic analysis.

Then, call the **py-analyzer** sub agent to perform comprehensive Python code analysis.

Analyze the following Python request:

- Code architecture and module structure analysis
- Performance bottlenecks and optimization opportunities
- Memory usage and garbage collection pattern analysis
- Dependency management and import analysis
- Custom queries about the Python codebase

$ARGUMENTS
```

## 마치며

잊지마시고 Claude Code 시작할 때, Claude Code가 compacting 한 후 /init-serena Command를 입력해주세요. 그것만으로도 Serena로 많은 부분 혜택을 누릴 수 있습니다.
아무쪼록 이 포스팅으로 Serena와 함께 HAPPY CLAUDE CODING 하시길 바랍니다.

> 마지막 TIP: Claude Code를 실행하여 Serena MCP가 실행되면 자동으로 웹브라우저가 열리면서 Dash Board가 뜹니다. 이를 Turn off하시려면 ~/.serena/serena_config.yml에서 web_dashboard를 false로 해주시면 됩니다.

---

### References

1. [Serena 공식 GitHub](https://github.com/oraios/serena)
2. [Model Context Protocol 문서](https://modelcontextprotocol.io/)
3. [Claude Code MCP 가이드](https://docs.anthropic.com/en/docs/claude-code/mcp)
4. [Language Server Protocol 개요](https://microsoft.github.io/language-server-protocol/)
5. [uv Python 패키지 매니저](https://docs.astral.sh/uv/)
