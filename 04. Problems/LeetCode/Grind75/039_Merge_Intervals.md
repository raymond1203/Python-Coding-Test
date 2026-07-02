# Grind 75 039. Merge Intervals

> 겹치는 interval들을 합치는 문제다. 시작점 기준으로 정렬한 뒤, 현재 병합 구간의 끝점과 다음 구간의 시작점을 비교하면 된다.

## Link

- [LeetCode - Merge Intervals](https://leetcode.com/problems/merge-intervals)

## Problem Model

Interval `[start, end]`들이 주어졌을 때, 서로 겹치는 구간들을 하나로 합쳐 non-overlapping interval 목록을 만든다.

## Pattern

- [Interval](../../../01.%20Data%20Structures/12.%20Interval.md)
- [Sweep Line and Intervals](../../../03.%20Problem%20Solving%20Patterns/19.%20Sweep%20Line%20and%20Intervals.md)
- [Sort Then Scan](../../../03.%20Problem%20Solving%20Patterns/20.%20Sort%20Then%20Scan.md)

## Approach

1. 시작점 기준으로 interval을 정렬한다.
2. 결과 list의 마지막 interval과 현재 interval을 비교한다.
3. 현재 시작점이 마지막 끝점 이하이면 겹치므로 끝점을 확장한다.
4. 겹치지 않으면 새 interval로 추가한다.

## Python 3.14 Solution

```python
class Solution:
    def merge(self, intervals: list[list[int]]) -> list[list[int]]:
        intervals.sort(key=lambda interval: interval[0])
        merged: list[list[int]] = []

        for start, end in intervals:
            if not merged or merged[-1][1] < start:
                merged.append([start, end])
            else:
                merged[-1][1] = max(merged[-1][1], end)

        return merged
```

## Correctness Notes

- 정렬 후에는 현재 interval과 겹칠 수 있는 이전 interval은 결과의 마지막 interval뿐이다.
- `merged[-1][1] < start`이면 완전히 분리되어 있으므로 새 구간을 추가한다.
- 그렇지 않으면 두 구간은 겹치므로 끝점을 최댓값으로 확장하면 합집합이 된다.
- 모든 interval을 순서대로 처리하므로 최종 결과는 정렬되고 서로 겹치지 않는다.

## Complexity

- Time: `O(n log n)` due to sorting.
- Space: `O(n)` for output.

## Mistake Notes

- 끝점이 시작점과 같은 경우도 겹치는 것으로 본다. 따라서 분리 조건은 `< start`다.
- 정렬 없이 병합하면 이전에 놓친 겹침이 생길 수 있다.
- 입력 interval을 직접 수정해도 되는 상황인지 확인하고, 필요하면 copy해서 사용한다.