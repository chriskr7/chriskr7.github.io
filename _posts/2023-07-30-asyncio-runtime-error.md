---
title:  "RuntimeError: Event loop is closed 처리"
categories: programming
tags:
  - SQLAlchemy
  - asyncio
  - aiomysql
  - RuntimeError
  - "Event loop is closed"
comments: true
---

SQLAlchemy Async를 사용하거나 Windows 환경에서 asyncio.run(main())을 실행시킬 때
RuntimeError: Event loop is closed 에러를 마주칠 때가 있다. 아래와 같은
방법으로 해결할 수 있는 지 체크해보자.

### SQLAlchemy Async (aiomysql) 사용시 에러 처리

engine을 engine과 관련된 처리가 끝났을 때나 프로그램이 Exit할 때
dispose 시켜줘야 한다.

```python
# engine 선언 - 다른 파일에 있을 수 있다.
engine = create_async_engine(
    DB_URL,
    future=True,
    echo=True,
    pool_size=mysql_cfg.pool_size,
    pool_recycle=mysql_cfg.pool_recycle
)

# 아래를 프로그램 Exit할 때나 engine 관련 처리를 다 마친 후  실행
await engine.dispose()
```

### Windows 환경

Windows에서는 EventLoopPolicy와 문제가 있는 것 같다. 아래의 코드를 삽입함으로
해결할 수 있다.

```python
# 아래 코드 삽입
syncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
asyncio.run(main())
```
