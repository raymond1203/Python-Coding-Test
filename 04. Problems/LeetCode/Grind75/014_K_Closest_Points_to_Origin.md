# Grind 75 014. K Closest Points to Origin

> 모든 점을 정렬하지 않고도 가장 가까운 K개만 유지할 수 있다. Top-K 문제는 heap 크기를 K로 제한하면 불필요한 후보를 계속 버릴 수 있다.

## Link

- [LeetCode - K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin)

## Problem Model

각 점 `(x, y)`의 원점 거리 기준으로 가장 가까운 K개를 반환한다. 실제 거리 `sqrt(x^2 + y^2)` 대신 제곱 거리 `x^2 + y^2`를 비교해도 순서는 같다.

## Pattern

- [Heap](../../../01.%20Data%20Structures/10.%20Heap.md)
- [Top K with Heap](../../../03.%20Problem%20Solving%20Patterns/18.%20Top%20K%20with%20Heap.md)
- [Geometry](../../../02.%20Algorithms/13.%20Geometry.md)

## Approach

Python의 `heapq`는 min-heap이므로, 제곱 거리에 음수를 붙여 max-heap처럼 사용한다.

1. 각 점의 `dist = x*x + y*y`를 계산한다.
2. `(-dist, x, y)`를 heap에 넣는다.
3. heap 크기가 K를 넘으면 가장 먼 점, 즉 가장 작은 음수 거리를 pop한다.
4. 끝까지 처리하면 heap에는 가장 가까운 K개만 남는다.

## Python 3.14 Solution

```python
import heapq


class Solution:
    def kClosest(self, points: list[list[int]], k: int) -> list[list[int]]:
        heap: list[tuple[int, int, int]] = []

        for x, y in points:
            dist = x * x + y * y
            heapq.heappush(heap, (-dist, x, y))

            if len(heap) > k:
                heapq.heappop(heap)

        return [[x, y] for _, x, y in heap]
```

## Correctness Notes

- heap 크기를 K로 제한하면, 지금까지 본 점 중 가장 가까운 K개 후보만 유지된다.
- 새 점을 넣어 K + 1개가 되었을 때 가장 먼 점을 제거하므로, 제거된 점은 이후 답이 될 수 없다.
- 모든 점을 처리한 뒤 heap에 남은 점들은 전체 점 중 거리 기준 상위 K개다.

## Complexity

- Time: `O(n log k)`
- Space: `O(k)`

## Mistake Notes

- `sqrt`는 필요 없다. 제곱 거리 비교로 충분하다.
- 반환 순서는 중요하지 않은 문제다. 정렬된 순서를 만들려고 추가 비용을 쓸 필요가 없다.
- `k`가 매우 크거나 전체 정렬이 허용되는 상황이면 `sorted(points, key=...)[:k]`도 가능하지만, Top-K 패턴 학습 목적이라면 heap 풀이를 우선 익히자.

## Related Notes

- [Heap](../../../01.%20Data%20Structures/10.%20Heap.md)
- [Top K with Heap](../../../03.%20Problem%20Solving%20Patterns/18.%20Top%20K%20with%20Heap.md)
