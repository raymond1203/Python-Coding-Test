# Grind 75 020. Longest Substring Without Repeating Characters

> 중복 문자가 없는 가장 긴 연속 substring을 찾는 문제다. 오른쪽 포인터를 늘리면서, 중복이 생기는 순간 왼쪽 경계를 필요한 만큼만 점프시킨다.

## Link

- [LeetCode - Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters)

## Problem Model

현재 window `[left, right]`가 중복 없는 substring이 되도록 유지한다. 문자가 다시 나타나면 이전 위치 다음 칸으로 `left`를 옮긴다.

## Pattern

- [Sliding Window](../../../03.%20Problem%20Solving%20Patterns/02.%20Sliding%20Window.md)
- [Hash Table](../../../01.%20Data%20Structures/03.%20Hash%20Table.md)
- [String](../../../01.%20Data%20Structures/02.%20String.md)

## Approach

1. `last_seen[char] = index`를 저장한다.
2. 오른쪽 포인터 `right`로 문자열을 순회한다.
3. 현재 문자가 window 안에서 이미 등장했다면 `left`를 `last_seen[char] + 1`로 이동한다.
4. 현재 window 길이로 `best`를 갱신한다.
5. 현재 문자의 마지막 위치를 갱신한다.

## Python 3.14 Solution

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        last_seen: dict[str, int] = {}
        left = 0
        best = 0

        for right, char in enumerate(s):
            if char in last_seen and last_seen[char] >= left:
                left = last_seen[char] + 1

            best = max(best, right - left + 1)
            last_seen[char] = right

        return best
```

## Correctness Notes

- 매 반복 후 window `[left, right]`는 중복 문자가 없다.
- 중복 문자가 window 밖에 있는 경우에는 `left`를 되돌리면 안 되므로 `last_seen[char] >= left`를 확인한다.
- 모든 가능한 오른쪽 끝점에 대해 가장 왼쪽으로 확장된 유효 window 길이를 계산하므로 최댓값을 놓치지 않는다.

## Complexity

- Time: `O(n)`
- Space: `O(k)`, distinct characters in the current scan.

## Mistake Notes

- `left = last_seen[char] + 1`을 무조건 적용하면 left가 뒤로 움직일 수 있다.
- substring은 연속이고 subsequence가 아니다.
- set 기반 window도 가능하지만 last index 방식이 경계 점프가 명확하다.