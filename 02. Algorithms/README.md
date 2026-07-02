# Algorithms

> 목적: 알고리즘을 “외우는 풀이 공식”이 아니라 **문제의 상태 공간을 줄이는 전략**으로 이해합니다.  
> 참고 커버리지: Tech Interview Handbook의 topic priority와 study plan을 참고하되, 설명/코드/그림은 Python 3.14.6 기준으로 독립 작성합니다.  
> 작성 방식: `문제 조건 → 단조성/중복/그래프성 확인 → 알고리즘 선택 → 불변식/정당성 → Python 구현`

## Positioning

Algorithms 섹션은 실제 문제 풀이 모음이 아닙니다. 이 폴더에서는 다음 질문에 답합니다.

- 이 알고리즘은 어떤 탐색 공간을 줄이는가?
- 어떤 조건이 있어야 적용 가능한가?
- 정당성은 어떤 불변식이나 증명 아이디어로 설명되는가?
- Python에서는 어떤 구현 패턴이 안전한가?
- 어떤 자료구조 및 문제 해결 패턴과 결합되는가?

실제 문제별 코드는 [04. Problems](../04.%20Problems/README.md)에 기록합니다.

## Topic Map

| Order | Algorithm | Priority | Core Question | Note |
|---:|---|---|---|---|
| 01 | Sorting | High | 순서를 부여하면 문제가 단순해지는가? | [Sorting](01.%20Sorting.md) |
| 02 | Binary Search | High | 정렬/단조성을 이용해 후보를 절반씩 버릴 수 있는가? | [Binary Search](02.%20Binary%20Search.md) |
| 03 | Recursion | Medium | 문제를 같은 형태의 작은 문제로 나눌 수 있는가? | [Recursion](03.%20Recursion.md) |
| 04 | DFS / BFS | High | 상태 공간을 깊이/거리 기준으로 탐색해야 하는가? | [DFS and BFS](04.%20DFS%20and%20BFS.md) |
| 05 | Backtracking | Medium | 후보를 만들며 불가능한 가지를 잘라낼 수 있는가? | [Backtracking](05.%20Backtracking.md) |
| 06 | Dynamic Programming | High | 중복 부분 문제와 최적 부분 구조가 있는가? | [Dynamic Programming](06.%20Dynamic%20Programming.md) |
| 07 | Greedy | Medium | 현재 선택이 미래 선택을 망치지 않는가? | [Greedy](07.%20Greedy.md) |
| 08 | Shortest Path | Medium | 가중 그래프에서 최소 비용을 찾아야 하는가? | [Shortest Path](08.%20Shortest%20Path.md) |
| 09 | Union Find | Medium | 연결 여부를 동적으로 합치고 물어보는가? | [Union Find](09.%20Union%20Find.md) |
| 10 | Topological Sort | Medium | 선행 관계가 있는 작업 순서를 구해야 하는가? | [Topological Sort](10.%20Topological%20Sort.md) |
| 11 | Bit Manipulation | Low | bit mask로 상태/집합/연산을 압축할 수 있는가? | [Bit Manipulation](11.%20Bit%20Manipulation.md) |
| 12 | Math | Low | 수학적 성질로 탐색을 줄일 수 있는가? | [Math](12.%20Math.md) |
| 13 | Geometry | Low | 좌표, 거리, 방향, 교차를 다루는가? | [Geometry](13.%20Geometry.md) |

## 루프엔지니어링 작성 순서

1. **Loop 0 - Structure**: 목차, 우선순위, note template, 출처 정책 정리
2. **Loop 1 - Search Foundations**: Sorting, Binary Search, Recursion, DFS/BFS
3. **Loop 2 - Exhaustive to Optimized Search**: Backtracking, Dynamic Programming, Greedy
4. **Loop 3 - Graph Algorithms**: Shortest Path, Union Find, Topological Sort
5. **Loop 4 - Specialized Tools**: Bit Manipulation, Math, Geometry
6. **Loop 5 - Diagram & Pattern Linking**: 그림/불변식/Problem Solving Patterns 역링크 연결

## Quality Bar

각 알고리즘 노트는 최소한 아래를 만족해야 합니다.

- [ ] 적용 가능한 조건과 불가능한 조건을 구분한다.
- [ ] 핵심 불변식 또는 정당성 아이디어를 설명한다.
- [ ] Python 구현 template를 제공한다.
- [ ] 시간/공간 복잡도를 입력 크기 변수와 함께 설명한다.
- [ ] 대표 edge case를 정리한다.
- [ ] 연결되는 자료구조와 문제 해결 패턴을 링크한다.

## References

- [Tech Interview Handbook - Algorithms study cheatsheets](https://www.techinterviewhandbook.org/algorithms/study-cheatsheet/)
- [Tech Interview Handbook - Coding interview study plan](https://www.techinterviewhandbook.org/coding-interview-study-plan/)
- [Python 3.14.6 Documentation](https://docs.python.org/3/)