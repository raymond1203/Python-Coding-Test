# Grind 75 073. Task Scheduler

> 같은 작업 사이에 cooldown `n`이 필요할 때 전체 실행 시간을 최소화하는 문제다. 가장 빈도가 높은 작업들이 idle slot 구조를 결정한다.

## Link

- [LeetCode - Task Scheduler](https://leetcode.com/problems/task-scheduler)

## Problem Model

각 task는 한 unit time이 걸리고, 같은 task 사이에는 최소 `n`칸의 간격이 필요하다. 다른 task나 idle로 그 간격을 채울 수 있다.

## Pattern

- [Heap](../../../01.%20Data%20Structures/10.%20Heap.md)
- [Greedy](../../../02.%20Algorithms/07.%20Greedy.md)
- [Hashing and Counting](../../../03.%20Problem%20Solving%20Patterns/04.%20Hashing%20and%20Counting.md)

## Approach

가장 많이 등장한 task를 기준으로 frame을 만든다.

- 최대 빈도를 `max_freq`라고 하자.
- 최대 빈도 task를 배치하면 `max_freq - 1`개의 full frame이 생긴다.
- 각 frame 길이는 `n + 1`이다.
- 최대 빈도 task가 여러 개면 마지막 frame에 함께 들어간다.
- 필요한 최소 시간은 frame 계산값과 실제 task 수 중 큰 값이다.

## Python 3.14 Solution

```python
from collections import Counter


class Solution:
    def leastInterval(self, tasks: list[str], n: int) -> int:
        counts = Counter(tasks)
        max_freq = max(counts.values())
        max_freq_tasks = sum(1 for count in counts.values() if count == max_freq)

        frame_time = (max_freq - 1) * (n + 1) + max_freq_tasks
        return max(len(tasks), frame_time)
```

## Correctness Notes

- 가장 빈도가 높은 task들은 서로 최소 `n` 간격을 가져야 하므로 전체 schedule의 골격을 만든다.
- `max_freq - 1`개의 간격 구간은 각각 길이 `n + 1` frame으로 볼 수 있다.
- 최대 빈도 task가 여러 개이면 마지막 위치에 나란히 추가되어 `max_freq_tasks`만큼 끝부분을 차지한다.
- 다른 task들이 frame의 빈칸을 모두 채우면 idle이 없어질 수 있으므로 실제 task 수와 frame 계산값 중 큰 값이 정답이다.

## Complexity

- Time: `O(t)`, where `t = len(tasks)`.
- Space: `O(k)`, distinct task types.

## Mistake Notes

- heap simulation도 가능하지만 수식 풀이가 더 간결하다.
- cooldown이 0이면 정답은 task 수다. 위 공식도 이를 처리한다.
- 최대 빈도 task가 여러 개인 경우를 빼먹으면 off-by-one이 생긴다.