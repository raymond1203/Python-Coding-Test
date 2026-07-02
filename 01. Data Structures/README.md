# Data Structures

> 목적: 자료구조를 “암기 목록”이 아니라 **상태를 저장하고 접근하는 방식의 선택지**로 이해합니다.  
> 참고 커버리지: Tech Interview Handbook의 Algorithms study cheatsheet topic list를 참고하되, 설명/예제/그림은 독립적으로 작성합니다.  
> 작성 방식: `개념 모델 → 불변식 → 연산 복잡도 → Python 도구 → 문제 신호 → 풀이 패턴 연결`

## Positioning

Data Structures 섹션은 실제 문제 풀이를 담는 공간이 아닙니다. 문제 풀이는 [04. Problems](../04.%20Problems/README.md)에서 관리하고, 이 폴더에서는 각 자료구조가 다음 질문에 어떻게 답하는지 정리합니다.

- 어떤 상태를 저장하는가?
- 어떤 연산을 빠르게 만들고, 어떤 연산을 포기하는가?
- Python에서는 어떤 built-in 또는 표준 라이브러리로 표현하는가?
- 문제에서 어떤 신호가 보이면 이 자료구조를 떠올려야 하는가?
- 어떤 풀이 패턴과 자주 결합되는가?

## Topic Map

| Order | Topic | Priority | Core Question | Note |
|---:|---|---|---|---|
| 01 | Array / List | High | 순서와 index를 어떻게 활용할까? | [Array and List](01.%20Array%20and%20List.md) |
| 02 | String | High | immutable sequence를 어떻게 탐색/변환할까? | [String](02.%20String.md) |
| 03 | Hash Table | High | 존재 여부와 빈도를 어떻게 O(1)에 가깝게 다룰까? | [Hash Table](03.%20Hash%20Table.md) |
| 04 | Matrix | High | 2차원 좌표와 방향 이동을 어떻게 모델링할까? | [Matrix](04.%20Matrix.md) |
| 05 | Linked List | Medium | 포인터 재연결 문제를 어떻게 안전하게 다룰까? | [Linked List](05.%20Linked%20List.md) |
| 06 | Stack | Medium | 최근 상태와 되돌아가기를 어떻게 표현할까? | [Stack](06.%20Stack.md) |
| 07 | Queue / Deque | Medium | 처리 순서와 BFS frontier를 어떻게 관리할까? | [Queue and Deque](07.%20Queue%20and%20Deque.md) |
| 08 | Tree | High | 계층 구조와 재귀적 subproblem을 어떻게 본다? | [Tree](08.%20Tree.md) |
| 09 | Graph | High | 관계와 상태 전이를 어떻게 탐색한다? | [Graph](09.%20Graph.md) |
| 10 | Heap | Medium | 매번 최소/최대/Top-K를 어떻게 뽑는다? | [Heap](10.%20Heap.md) |
| 11 | Trie | Medium | prefix 기반 검색을 어떻게 구조화한다? | [Trie](11.%20Trie.md) |
| 12 | Interval | Medium | 구간의 겹침/병합/정렬을 어떻게 처리한다? | [Interval](12.%20Interval.md) |

## 루프엔지니어링 작성 순서

1. **Loop 0 - Structure**: 목차, 우선순위, note template, 출처 정책 정리
2. **Loop 1 - Linear Structures**: Array/List, String, Hash Table, Matrix
3. **Loop 2 - Pointer & Order Structures**: Linked List, Stack, Queue/Deque, Heap
4. **Loop 3 - Hierarchical & Relational Structures**: Tree, Graph, Trie, Interval
5. **Loop 4 - Diagram & Pattern Linking**: Mermaid/그림 추가, Problem Solving Patterns와 역링크 연결

## Quality Bar

각 자료구조 노트는 최소한 아래를 만족해야 합니다.

- [ ] 저장하는 상태와 핵심 불변식을 설명한다.
- [ ] 주요 연산의 시간/공간 복잡도를 표로 정리한다.
- [ ] Python 3.14.6에서 사용할 built-in/표준 라이브러리를 명시한다.
- [ ] 문제에서 보이는 선택 신호를 정리한다.
- [ ] 자주 결합되는 풀이 패턴을 연결한다.
- [ ] 실수하기 쉬운 edge case를 따로 적는다.
- [ ] 실제 문제 풀이는 Problems로 넘기고, 여기서는 이론과 기법에 집중한다.

## References

- [Tech Interview Handbook - Algorithms study cheatsheets](https://www.techinterviewhandbook.org/algorithms/study-cheatsheet/)
- [Tech Interview Handbook - Coding interview study plan](https://www.techinterviewhandbook.org/coding-interview-study-plan/)
- [Python 3.14.6 Documentation](https://docs.python.org/3/)