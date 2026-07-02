# Grind 75 025. Course Schedule

> prerequisite 관계가 cycle 없이 모두 수행 가능한지 묻는 문제다. Course를 정점, prerequisite을 방향 간선으로 보면 topological ordering 가능 여부와 같다.

## Link

- [LeetCode - Course Schedule](https://leetcode.com/problems/course-schedule)

## Problem Model

`[course, prerequisite]`는 `prerequisite -> course` 방향 간선이다. 모든 course를 들을 수 있으려면 directed graph에 cycle이 없어야 한다.

## Pattern

- [Graph](../../../01.%20Data%20Structures/09.%20Graph.md)
- [Topological Sort](../../../02.%20Algorithms/10.%20Topological%20Sort.md)
- [Topological Ordering](../../../03.%20Problem%20Solving%20Patterns/17.%20Topological%20Ordering.md)
- [Queue and Deque](../../../01.%20Data%20Structures/07.%20Queue%20and%20Deque.md)

## Approach

Kahn's Algorithm을 사용한다.

1. 각 course의 indegree를 계산한다.
2. indegree가 0인 course를 queue에 넣는다.
3. queue에서 course를 꺼내 수강 처리한다.
4. 그 course가 unlock하는 다음 course들의 indegree를 줄인다.
5. 처리한 course 수가 `numCourses`와 같으면 가능하다.

## Python 3.14 Solution

```python
from collections import deque


class Solution:
    def canFinish(self, numCourses: int, prerequisites: list[list[int]]) -> bool:
        graph: list[list[int]] = [[] for _ in range(numCourses)]
        indegree = [0] * numCourses

        for course, prerequisite in prerequisites:
            graph[prerequisite].append(course)
            indegree[course] += 1

        queue: deque[int] = deque(
            course for course in range(numCourses) if indegree[course] == 0
        )
        completed = 0

        while queue:
            course = queue.popleft()
            completed += 1

            for next_course in graph[course]:
                indegree[next_course] -= 1
                if indegree[next_course] == 0:
                    queue.append(next_course)

        return completed == numCourses
```

## Correctness Notes

- indegree 0인 course는 아직 남은 prerequisite이 없으므로 지금 들을 수 있다.
- course를 처리하면 그 course를 prerequisite으로 삼는 간선들이 제거된 것과 같다.
- cycle 안의 노드들은 indegree가 0이 될 수 없으므로 처리되지 않는다.
- 모든 노드를 처리했다면 cycle이 없고 모든 course를 수강할 수 있다.

## Complexity

- Time: `O(V + E)`
- Space: `O(V + E)`

## Mistake Notes

- 간선 방향을 반대로 잡으면 indegree 의미가 깨진다. `[course, prerequisite]`는 `prerequisite -> course`다.
- DFS cycle detection으로도 풀 수 있지만, prerequisite 문제는 topological sort로 설명하기 좋다.
- 처리한 개수를 세어 cycle 여부를 판단하면 ordering list를 따로 저장하지 않아도 된다.