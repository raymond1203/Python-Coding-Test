# Grind 75 041. Time Based Key-Value Store

> key마다 시간순 value history를 저장하고, 특정 timestamp 이하의 가장 최근 값을 찾는 설계 문제다. 저장은 append, 조회는 binary search로 처리한다.

## Link

- [LeetCode - Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store)

## Problem Model

`set(key, value, timestamp)`는 key의 시간 기록을 추가한다. `get(key, timestamp)`는 해당 timestamp 이하에서 가장 큰 timestamp의 value를 반환해야 한다.

## Pattern

- [Hash Table](../../../01.%20Data%20Structures/03.%20Hash%20Table.md)
- [Binary Search](../../../02.%20Algorithms/02.%20Binary%20Search.md)
- [Design with Multiple Structures](../../../03.%20Problem%20Solving%20Patterns/11.%20Design%20with%20Multiple%20Structures.md)

## Approach

1. `store[key]`에 `(timestamp, value)` list를 저장한다.
2. 문제 조건상 같은 key에 대한 timestamp는 증가 순서로 들어오므로 append만 하면 정렬이 유지된다.
3. get에서는 timestamp 이하의 마지막 기록을 binary search로 찾는다.
4. 기록이 없으면 빈 문자열을 반환한다.

## Python 3.14 Solution

```python
from bisect import bisect_right
from collections import defaultdict


class TimeMap:
    def __init__(self):
        self.store: dict[str, list[tuple[int, str]]] = defaultdict(list)

    def set(self, key: str, value: str, timestamp: int) -> None:
        self.store[key].append((timestamp, value))

    def get(self, key: str, timestamp: int) -> str:
        history = self.store.get(key, [])
        index = bisect_right(history, (timestamp, chr(0x10FFFF))) - 1

        if index < 0:
            return ""
        return history[index][1]
```

## Correctness Notes

- 각 key의 history는 timestamp 오름차순으로 저장된다.
- `bisect_right(history, (timestamp, high_char))`는 timestamp 이하인 모든 기록의 오른쪽 경계를 찾는다.
- 그 직전 index가 timestamp 이하 중 가장 큰 timestamp의 기록이다.
- index가 음수이면 조건을 만족하는 기록이 없다.

## Complexity

- `set`: `O(1)` amortized
- `get`: `O(log n)` for that key's history length
- Space: `O(total set calls)`

## Mistake Notes

- 전체 key를 하나의 list에 섞으면 조회가 복잡해진다. key별 history가 필요하다.
- timestamp가 증가 순서로 들어온다는 조건을 활용하면 set에서 정렬할 필요가 없다.
- tuple binary search에서 같은 timestamp의 value 비교 문제가 생기지 않도록 충분히 큰 sentinel 문자열을 사용한다.