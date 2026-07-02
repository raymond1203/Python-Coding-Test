# Grind 75 012. Insert Interval

> 이미 정렬되어 있고 겹치지 않는 interval 목록에 새 interval 하나를 끼워 넣는 문제다. 왼쪽의 무관한 구간, 겹치는 구간, 오른쪽의 무관한 구간으로 나누면 깔끔하다.

## Link

- [LeetCode - Insert Interval](https://leetcode.com/problems/insert-interval)

## Problem Model

`intervals`는 시작점 기준으로 정렬되어 있고 서로 겹치지 않는다. `newInterval`을 추가한 뒤에도 같은 조건을 만족하도록 병합된 interval 목록을 반환한다.

## Pattern

- [Interval](../../../01.%20Data%20Structures/12.%20Interval.md)
- [Sweep Line and Intervals](../../../03.%20Problem%20Solving%20Patterns/19.%20Sweep%20Line%20and%20Intervals.md)
- [Sort Then Scan](../../../03.%20Problem%20Solving%20Patterns/20.%20Sort%20Then%20Scan.md)

## Approach

세 구간으로 스캔한다.

1. `newInterval`보다 완전히 왼쪽에 있는 interval은 그대로 답에 넣는다.
2. `newInterval`과 겹치는 interval은 시작점 최솟값, 끝점 최댓값으로 병합한다.
3. 병합된 interval을 답에 넣는다.
4. 나머지 오른쪽 interval을 그대로 붙인다.

## Python 3.14 Solution

```python
class Solution:
    def insert(
        self,
        intervals: list[list[int]],
        newInterval: list[int],
    ) -> list[list[int]]:
        result: list[list[int]] = []
        start, end = newInterval
        i = 0
        n = len(intervals)

        while i < n and intervals[i][1] < start:
            result.append(intervals[i])
            i += 1

        while i < n and intervals[i][0] <= end:
            start = min(start, intervals[i][0])
            end = max(end, intervals[i][1])
            i += 1

        result.append([start, end])
        result.extend(intervals[i:])
        return result
```

## Correctness Notes

- 첫 while에서 추가하는 interval은 `newInterval`과 절대 겹치지 않고 순서도 앞이므로 그대로 둔다.
- 두 번째 while에서 만나는 interval은 시작점이 현재 `end` 이하이므로 겹친다. 병합 후에도 `[start, end]`는 지금까지 겹친 모든 구간의 합집합이다.
- 두 번째 while이 끝나면 남은 interval은 병합된 구간보다 오른쪽에 있으므로 그대로 붙여도 정렬성과 비겹침이 유지된다.

## Complexity

- Time: `O(n)`
- Space: `O(n)` for the output list.

## Mistake Notes

- `intervals[i][1] < start`는 완전히 왼쪽인 경우다. `<=`를 쓰면 끝점과 시작점이 붙는 interval을 병합하지 못할 수 있다.
- 겹침 조건은 `intervals[i][0] <= end`다.
- 이미 정렬되어 있으므로 다시 sort할 필요가 없다.

## Related Notes

- [Interval](../../../01.%20Data%20Structures/12.%20Interval.md)
- [Sweep Line and Intervals](../../../03.%20Problem%20Solving%20Patterns/19.%20Sweep%20Line%20and%20Intervals.md)
