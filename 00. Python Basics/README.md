# Python Basics

> 기준 버전: **Python 3.14.6**  
> 목적: 코딩 테스트용 문법 암기가 아니라, Python이라는 언어를 정확히 이해하기 위한 기반 문서입니다.  
> 구성 참고: `Effective Python, 3rd Edition`의 공식 공개 목차를 장/아이템 체크리스트로만 참고합니다.

## Positioning

Python Basics는 “코테에서 자주 쓰는 문법 모음”이 아닙니다. 이 폴더의 목표는 다음 질문에 답할 수 있게 만드는 것입니다.

- Python은 값을 어떻게 표현하고 전달하는가?
- Python다운 코드는 왜 읽기 쉬운가?
- 함수, iterator, generator, class, exception, context manager는 어떤 사고방식을 요구하는가?
- 표준 라이브러리는 어떤 문제를 어떤 추상화로 해결하는가?
- Python 3.14.6 기준으로 과거의 습관 중 무엇을 유지하고 무엇을 바꿔야 하는가?

## Public Repo Rule

- 책의 문장, 문단, 그림, 예제 코드를 복사하지 않습니다.
- 공개 목차의 제목은 학습 로드맵으로만 사용합니다.
- 각 item은 직접 이해한 설명과 직접 작성한 예제로 채웁니다.
- Python 3.14.6에서 바뀐 문법/API는 공식 문서로 재확인합니다.

## Python 3.14.6 Baseline

- [Python 3.14.6 Baseline](00.%20Python%203.14%20Baseline.md)
- 공식 문서: <https://docs.python.org/3/>
- Python 3.14 What's New: <https://docs.python.org/3/whatsnew/3.14.html>
- 버전별 문서: <https://www.python.org/doc/versions/>

## Chapter Index

| Chapter | Topic | Note | Items |
|---:|---|---|---:|
| 01 | Pythonic Thinking | [Pythonic Thinking](01.%20Pythonic%20Thinking.md) | 9 |
| 02 | Strings and Slicing | [Strings and Slicing](02.%20Strings%20and%20Slicing.md) | 7 |
| 03 | Loops and Iterators | [Loops and Iterators](03.%20Loops%20and%20Iterators.md) | 8 |
| 04 | Dictionaries | [Dictionaries](04.%20Dictionaries.md) | 5 |
| 05 | Functions | [Functions](05.%20Functions.md) | 10 |
| 06 | Comprehensions and Generators | [Comprehensions and Generators](06.%20Comprehensions%20and%20Generators.md) | 8 |
| 07 | Classes and Interfaces | [Classes and Interfaces](07.%20Classes%20and%20Interfaces.md) | 10 |
| 08 | Metaclasses and Attributes | [Metaclasses and Attributes](08.%20Metaclasses%20and%20Attributes.md) | 9 |
| 09 | Concurrency and Parallelism | [Concurrency and Parallelism](09.%20Concurrency%20and%20Parallelism.md) | 13 |
| 10 | Robustness | [Robustness](10.%20Robustness.md) | 12 |
| 11 | Performance | [Performance](11.%20Performance.md) | 8 |
| 12 | Data Structures and Algorithms | [Data Structures and Algorithms](12.%20Data%20Structures%20and%20Algorithms.md) | 8 |
| 13 | Testing and Debugging | [Testing and Debugging](13.%20Testing%20and%20Debugging.md) | 8 |
| 14 | Collaboration | [Collaboration](14.%20Collaboration.md) | 10 |

Total items: **125**

## Chapter-by-Chapter Workflow

각 챕터는 한 번에 대량으로 채우기보다 다음 순서로 작성합니다.

1. 챕터의 중심 질문을 정의합니다.
2. item 제목을 읽고, 각 item이 Python 이해에서 어떤 위치를 갖는지 정의합니다.
3. Python 3.14.6 공식 문서로 현재 동작을 확인합니다.
4. 책 본문은 참고하지 않고, 직접 이해한 설명과 직접 작성한 예제를 남깁니다.
5. 좋은 사용 예시와 위험한 사용 예시를 비교합니다.
6. 다음 챕터와 연결되는 개념을 기록합니다.

## Quality Bar

각 item 노트는 아래 기준을 만족해야 public repo에 올릴 수 있습니다.

- [ ] 원문 복사 없이 직접 설명했습니다.
- [ ] Python 3.14.6 기준으로 검증했습니다.
- [ ] 직접 작성한 Python 예제가 있습니다.
- [ ] 사용 방식이 왜 Python다운지 설명했습니다.
- [ ] 실수하기 쉬운 지점을 짚었습니다.
- [ ] 관련 언어 개념 또는 표준 라이브러리와 연결했습니다.

## Attribution

자세한 출처와 public repo 작성 기준은 [References and Attribution](References%20and%20Attribution.md)을 참고합니다.
