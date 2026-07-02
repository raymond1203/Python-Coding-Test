# Problem Solving Patterns

> 목적: 자료구조와 알고리즘을 실제 문제에 적용하는 **문제 인식 → 도구 선택 → 풀이 설계** 과정을 정리합니다.  
> 범위: 실제 문제 풀이는 [04. Problems](../04.%20Problems/README.md)에서 관리하고, 이 폴더는 풀이 기법과 사고 패턴만 다룹니다.  
> 참고 커버리지: Tech Interview Handbook의 topic coverage와 study plan을 참고하되, 패턴 체계와 설명은 이 저장소의 Python 중심 구조에 맞게 독립 구성합니다.

## Positioning

Data Structures는 “무엇을 저장할 것인가”를 묻고, Algorithms는 “탐색/계산을 어떻게 줄일 것인가”를 묻습니다. Problem Solving Patterns는 그 중간에서 다음 질문을 다룹니다.

- 문제 문장에서 어떤 신호를 읽어야 하는가?
- 어떤 자료구조/알고리즘 후보를 떠올려야 하는가?
- 첫 번째 brute force에서 어떤 병목을 제거해야 하는가?
- 풀이 중 어떤 불변식을 유지해야 하는가?
- 구현할 때 Python에서 어떤 실수를 조심해야 하는가?

## Pattern Map

| Order | Pattern | Main Trigger | Note |
|---:|---|---|---|
| 01 | Two Pointers | 양끝/두 위치를 움직이며 조건을 좁힘 | [Two Pointers](01.%20Two%20Pointers.md) |
| 02 | Sliding Window | 연속 구간의 조건을 유지 | [Sliding Window](02.%20Sliding%20Window.md) |
| 03 | Prefix Sum / Difference Array | 구간 합/구간 업데이트 반복 | [Prefix Sum and Difference Array](03.%20Prefix%20Sum%20and%20Difference%20Array.md) |
| 04 | Hashing and Counting | 존재 여부/빈도/역매핑 | [Hashing and Counting](04.%20Hashing%20and%20Counting.md) |
| 05 | Fast and Slow Pointers | cycle, middle, linked list pointer | [Fast and Slow Pointers](05.%20Fast%20and%20Slow%20Pointers.md) |
| 06 | Monotonic Stack | 다음 큰/작은 값, span | [Monotonic Stack](06.%20Monotonic%20Stack.md) |
| 07 | Monotonic Queue | window max/min 최적화 | [Monotonic Queue](07.%20Monotonic%20Queue.md) |
| 08 | Graph Traversal Patterns | reachability, shortest unweighted path | [Graph Traversal Patterns](08.%20Graph%20Traversal%20Patterns.md) |
| 09 | Backtracking Search Patterns | permutation/combination/subset/path | [Backtracking Search Patterns](09.%20Backtracking%20Search%20Patterns.md) |
| 10 | Trie Prefix Search | prefix 기반 문자열 집합 검색 | [Trie Prefix Search](10.%20Trie%20Prefix%20Search.md) |
| 11 | Design with Multiple Structures | 한 자료구조로 모든 연산이 안 됨 | [Design with Multiple Structures](11.%20Design%20with%20Multiple%20Structures.md) |
| 12 | Matrix Traversal | grid 좌표/방향/방문 | [Matrix Traversal](12.%20Matrix%20Traversal.md) |
| 13 | In-place Reversal | linked list/array 구간 뒤집기 | [In-place Reversal](13.%20In-place%20Reversal.md) |
| 14 | Tree Traversal Patterns | subtree, depth, ancestor | [Tree Traversal Patterns](14.%20Tree%20Traversal%20Patterns.md) |
| 15 | Recursive Divide and Conquer | 같은 구조의 작은 문제 | [Recursive Divide and Conquer](15.%20Recursive%20Divide%20and%20Conquer.md) |
| 16 | Union Find Connectivity | component merge/query | [Union Find Connectivity](16.%20Union%20Find%20Connectivity.md) |
| 17 | Topological Ordering | dependency/prerequisite | [Topological Ordering](17.%20Topological%20Ordering.md) |
| 18 | Top K with Heap | k개 선택/streaming priority | [Top K with Heap](18.%20Top%20K%20with%20Heap.md) |
| 19 | Sweep Line and Intervals | 구간 시작/끝 event 처리 | [Sweep Line and Intervals](19.%20Sweep%20Line%20and%20Intervals.md) |
| 20 | Sort Then Scan | 정렬 후 한 번 훑기 | [Sort Then Scan](20.%20Sort%20Then%20Scan.md) |
| 21 | Binary Search on Answer | 정답 후보가 단조적 | [Binary Search on Answer](21.%20Binary%20Search%20on%20Answer.md) |
| 22 | DP State Design | 상태 정의가 풀이의 핵심 | [DP State Design](22.%20DP%20State%20Design.md) |
| 23 | Knapsack Style DP | capacity/choice 기반 최적화 | [Knapsack Style DP](23.%20Knapsack%20Style%20DP.md) |
| 24 | Sequence DP | index/order 기반 최적화 | [Sequence DP](24.%20Sequence%20DP.md) |
| 25 | Bitmask State Compression | subset/state를 bit로 압축 | [Bitmask State Compression](25.%20Bitmask%20State%20Compression.md) |
| 26 | Modular Arithmetic and Counting | 나머지/조합/개수 세기 | [Modular Arithmetic and Counting](26.%20Modular%20Arithmetic%20and%20Counting.md) |
| 27 | Coordinate Geometry | 좌표/거리/방향/교차 | [Coordinate Geometry](27.%20Coordinate%20Geometry.md) |

## 루프엔지니어링 작성 순서

1. **Loop 0 - Structure**: 패턴 taxonomy와 note template 정리
2. **Loop 1 - Linear Scan Patterns**: Two Pointers, Sliding Window, Prefix Sum, Hashing
3. **Loop 2 - Stack/Queue/Heap Patterns**: Monotonic Stack, Monotonic Queue, Top-K Heap
4. **Loop 3 - Graph/Tree/Search Patterns**: Graph Traversal, Tree Traversal, Backtracking, Trie
5. **Loop 4 - Optimization Patterns**: Binary Search on Answer, DP State Design, Greedy/Sweep Line
6. **Loop 5 - Specialized Patterns**: Bitmask, Math, Geometry, Multi-structure Design
7. **Problem Links - Ongoing Practice Layer**: 각 패턴에서 Problems 폴더의 실제 풀이로 연결

## Pattern Note Quality Bar

- [ ] 문제 신호가 문장 단위로 정리되어 있다.
- [ ] brute force 병목과 패턴이 제거하는 비용을 설명한다.
- [ ] 핵심 불변식을 명시한다.
- [ ] Python 구현 template 또는 pseudo-code가 있다.
- [ ] 연결되는 Data Structures / Algorithms를 링크한다.
- [ ] 실제 문제는 제목만 언급하거나 Problems로 링크하고, 풀이 본문은 중복하지 않는다.

## References

- [Tech Interview Handbook - Algorithms study cheatsheets](https://www.techinterviewhandbook.org/algorithms/study-cheatsheet/)
- [Tech Interview Handbook - Coding interview study plan](https://www.techinterviewhandbook.org/coding-interview-study-plan/)
- [Python 3.14.6 Documentation](https://docs.python.org/3/)
