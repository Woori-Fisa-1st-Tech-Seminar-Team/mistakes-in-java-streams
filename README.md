# 📘 Mistakes in Java Streams

Java Stream API를 사용하면서 자주 발생하는 **실수(Mistakes)** 를 정리하고,
각 실수가 실제로 어떤 문제를 유발하는지 **실험을 통해 검증**한 스터디 프로젝트입니다.

---

## 🎯 프로젝트 목표

- Java Stream의 동작 방식을 정확히 이해한다
- 자주 발생하는 Stream 사용 실수를 재현한다
- 실험을 통해 각 실수의 문제점을 검증한다
- 올바른 Stream 사용 방법을 코드와 결과로 정리한다

---

## 👨‍👩‍👧 팀원 소개

| | | |
|:--:|:--:|:--:|
| <img src="https://avatars.githubusercontent.com/u/60088307?v=4?s=200"/> | <img src="https://avatars.githubusercontent.com/u/122732599?v=4"/> | <img src="https://avatars.githubusercontent.com/u/72748734?v=4?s=200"/> |
| [**손정원**](https://github.com/handgarden) | [**이동욱**](https://github.com/cuterrabbit) | [**이승준**](https://github.com/HiLeeS) |

---

## 🧪 테스트 케이스

본 프로젝트는 **테스트 케이스 단위로 실험을 진행**하며,
각 테스트 케이스의 상세 내용과 실행 결과는 **팀원이 개별적으로 작성**합니다.

### Case 1 — 터미널 연산 없는 Stream 실행 여부 검증

- 중간 연산만 정의된 Stream은 실제로 실행되지 않음을 코드로 검증한다.
- **결과**: 터미널 연산이 없으면 `map()` 내부 연산은 **0회 실행**되고, 터미널 연산(`toList()`) 호출 시점에 파이프라인 전체가 실행된다. (Lazy Evaluation)
- 📄 상세 문서: [case1-no-terminal.md](./docs/case1-no-terminal.md)
- 👤 담당자: 이승준 ([HiLeeS](https://github.com/HiLeeS))

![Case 1 실행 결과](./docs/case1-no-terminal.png)

---

### Case 2 — For-loop vs Sequential Stream vs Parallel Stream

- 같은 양의 데이터를 처리하는 일반 For문, Sequential Stream, Parallel Stream의 실행 시간을 비교한다.
- **결과**: 데이터 규모가 작고 연산이 단순할 때는 스레드 분할·병합 오버헤드로 인해 Parallel Stream이 오히려 느릴 수 있다.

<div align="center">

| 측정 항목 | 실행 시간 (ms) |
|:---|---:|
| **For-loop** | 1493.4539 |
| **Sequential Stream** | 1322.5975 |
| **Parallel Stream** | 2260.0708 |

</div>

- 📄 상세 문서: [Stream.md](./docs/Stream.md)
- 👤 담당자: 이동욱 ([cuterrabbit](https://github.com/cuterrabbit))

---

### Case 3 — Stream API 구현 및 동작 확인

- Stream API를 직접 구현하고 실제 Stream의 동작과 비교한다.
- **결과**: 직접 구현한 `CustomStream` 역시 터미널 연산을 호출할 때만 이터레이터를 순회하며 중간 연산을 적용한다. Stream의 지연 실행 원리를 코드로 재현했다.
- 📄 상세 문서: [StreamAPITest.md](./docs/StreamAPITest.md)
- 👤 담당자: 손정원 ([handgarden](https://github.com/handgarden))

![Stream API 실행 결과](./docs/StreamAPITest.png)

---

## 📂 프로젝트 구조

```text
.
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   └── custom.md
│   └── pull_request_template.md
├── docs/
│   ├── Stream.md                 # Case 2 상세 문서
│   ├── StreamAPITest.md          # Case 3 상세 문서
│   ├── case1-no-terminal.md      # Case 1 상세 문서
│   ├── case1-no-terminal.png
│   ├── StreamAPITest.png
│   ├── Stream.png
│   ├── BaseStream.png
│   ├── Collection.png
│   ├── SequencedCollection.png
│   └── List.png
├── src/
│   ├── LazyCase1.java            # Case 1 실험 코드
│   ├── Stream.java               # Case 2 실험 코드
│   └── StreamAPITest.java        # Case 3 실험 코드
└── .gitignore
```
