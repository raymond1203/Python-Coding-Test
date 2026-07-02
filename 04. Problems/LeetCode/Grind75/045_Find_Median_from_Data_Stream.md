# Grind 75 045. Find Median from Data Stream

> 계속 들어오는 숫자들의 중앙값을 빠르게 구하는 설계 문제다. 작은 절반은 max-heap, 큰 절반은 min-heap으로 유지하면 중앙값이 heap top에 놓인다.

## Link

- [LeetCode - Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream)

## Problem Model

숫자가 하나씩 추가될 때마다 지금까지의 모든 숫자의 median을 반환해야 한다. 전체를 매번 정렬하면 느리므로, median 주변의 값만 빠르게 접근할 수 있어야 한다.

## Pattern

- [Heap](../../../01.%20Data%20Structures/10.%20Heap.md)
- [Top K with Heap](../../../03.%20Problem%20Solving%20Patterns/18.%20Top%20K%20with%20Heap.md)
- [Design with Multiple Structures](../../../03.%20Problem%20Solving%20Patterns/11.%20Design%20with%20Multiple%20Structures.md)

## Approach

- `small`: 작은 절반을 담는 max-heap. Python에서는 음수로 저장한다.
- `large`: 큰 절반을 담는 min-heap.
- 두 heap의 크기 차이는 최대 1로 유지한다.
- 홀수 개라면 더 큰 heap의 top이 median이다.
- 짝수 개라면 두 heap top의 평균이다.

## Python 3.14 Solution

```python
import heapq


class MedianFinder:
    def __init__(self):
        self.small: list[int] = []
        self.large: list[int] = []

    def addNum(self, num: int) -> None:
        heapq.heappush(self.small, -num)
        heapq.heappush(self.large, -heapq.heappop(self.small))

        if len(self.large) > len(self.small):
            heapq.heappush(self.small, -heapq.heappop(self.large))

    def findMedian(self) -> float:
        if len(self.small) > len(self.large):
            return float(-self.small[0])
        return (-self.small[0] + self.large[0]) / 2
```

## Correctness Notes

- `small`의 모든 값은 `large`의 모든 값 이하가 되도록 add 과정에서 값을 한 번 넘긴다.
- 크기 균형을 맞춰 `small`이 같거나 하나 더 많게 유지한다.
- 홀수 개일 때는 작은 절반이 하나 더 많으므로 `small` top이 median이다.
- 짝수 개일 때는 작은 절반 최댓값과 큰 절반 최솟값의 평균이 median이다.

## Complexity

- `addNum`: `O(log n)`
- `findMedian`: `O(1)`
- Space: `O(n)`

## Mistake Notes

- Python `heapq`는 min-heap만 제공하므로 max-heap은 음수로 저장한다.
- 두 heap의 크기 차이를 1 이하로 유지해야 median 계산이 단순해진다.
- 값의 순서 invariant와 크기 invariant를 둘 다 유지해야 한다.