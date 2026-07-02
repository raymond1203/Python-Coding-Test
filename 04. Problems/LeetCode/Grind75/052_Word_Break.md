# Grind 75 052. Word Break

> 문자열을 dictionary 단어들의 연결로 만들 수 있는지 판단하는 DP 문제다. `dp[i]`를 `s[:i]`가 분해 가능한지로 두면 마지막 단어의 시작 위치를 시도할 수 있다.

## Link

- [LeetCode - Word Break](https://leetcode.com/problems/word-break)

## Problem Model

`s`를 하나 이상의 dictionary word로 쪼갤 수 있는지 확인한다. 단어는 여러 번 사용할 수 있다.

## Pattern

- [Dynamic Programming](../../../02.%20Algorithms/06.%20Dynamic%20Programming.md)
- [DP State Design](../../../03.%20Problem%20Solving%20Patterns/22.%20DP%20State%20Design.md)
- [Trie Prefix Search](../../../03.%20Problem%20Solving%20Patterns/10.%20Trie%20Prefix%20Search.md)
- [String](../../../01.%20Data%20Structures/02.%20String.md)

## Approach

1. `word_set`으로 단어 조회를 `O(1)`에 가깝게 만든다.
2. `dp[0] = True`로 둔다. 빈 문자열은 분해 가능하다.
3. `i`까지의 prefix `s[:i]`에 대해 마지막 단어의 시작점 `j`를 시도한다.
4. `dp[j]`가 true이고 `s[j:i]`가 단어면 `dp[i] = True`다.

## Python 3.14 Solution

```python
class Solution:
    def wordBreak(self, s: str, wordDict: list[str]) -> bool:
        word_set = set(wordDict)
        max_len = max((len(word) for word in word_set), default=0)
        dp = [False] * (len(s) + 1)
        dp[0] = True

        for i in range(1, len(s) + 1):
            start = max(0, i - max_len)
            for j in range(start, i):
                if dp[j] and s[j:i] in word_set:
                    dp[i] = True
                    break

        return dp[-1]
```

## Correctness Notes

- `dp[i]`는 prefix `s[:i]`가 dictionary 단어들로 분해 가능한지를 의미한다.
- 마지막 단어가 `s[j:i]`라면 그 앞 prefix `s[:j]`도 분해 가능해야 한다.
- 모든 가능한 `j`를 확인하므로 가능한 마지막 단어 선택을 놓치지 않는다.
- `max_len`보다 긴 dictionary word는 없으므로 더 먼 `j`는 볼 필요가 없다.

## Complexity

- Time: `O(n * L * slice_cost)`, where `L` is max word length.
- Space: `O(n + total dictionary size)`

## Mistake Notes

- `dp[0] = True`가 없으면 첫 단어를 시작할 수 없다.
- 단어는 재사용 가능하므로 사용 여부를 따로 표시하지 않는다.
- topic은 trie로 분류되기도 하지만, 기본 풀이로는 DP + set이 가장 간결하다.