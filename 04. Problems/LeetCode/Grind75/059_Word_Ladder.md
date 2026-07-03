# Grind 75 059. Word Ladder

> 한 글자씩 바꾸어 beginWord에서 endWord까지 가는 최단 변환 길이를 구하는 문제다. 단어를 정점, 한 글자 차이를 간선으로 보면 unweighted shortest path이므로 BFS가 정답이다.

## Link

- [LeetCode - Word Ladder](https://leetcode.com/problems/word-ladder)

## Problem Model

한 번에 한 글자만 바꿀 수 있고, 중간 단어는 wordList에 있어야 한다. 최단 변환 횟수를 묻기 때문에 graph BFS로 모델링한다.

## Pattern

- [Graph](../../../01.%20Data%20Structures/09.%20Graph.md)
- [DFS and BFS](../../../02.%20Algorithms/04.%20DFS%20and%20BFS.md)
- [Graph Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/08.%20Graph%20Traversal%20Patterns.md)

## Approach

1. 각 단어에서 한 글자만 wildcard `*`로 바꾼 pattern을 만든다.
2. 같은 pattern을 공유하는 단어들은 한 글자 차이다.
3. beginWord에서 BFS를 시작한다.
4. endWord를 처음 만나는 level이 최단 변환 길이다.
5. 방문한 단어는 다시 방문하지 않는다.

## Python 3.14 Solution

```python
from collections import defaultdict, deque


class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: list[str]) -> int:
        if endWord not in wordList:
            return 0

        word_length = len(beginWord)
        patterns: dict[str, list[str]] = defaultdict(list)

        for word in wordList:
            for i in range(word_length):
                pattern = word[:i] + "*" + word[i + 1 :]
                patterns[pattern].append(word)

        queue: deque[tuple[str, int]] = deque([(beginWord, 1)])
        visited = {beginWord}

        while queue:
            word, distance = queue.popleft()
            if word == endWord:
                return distance

            for i in range(word_length):
                pattern = word[:i] + "*" + word[i + 1 :]
                for next_word in patterns[pattern]:
                    if next_word in visited:
                        continue
                    visited.add(next_word)
                    queue.append((next_word, distance + 1))

                patterns[pattern] = []

        return 0
```

## Correctness Notes

- 같은 wildcard pattern을 공유하는 단어들은 정확히 한 글자만 다르므로 한 번의 변환으로 이동 가능하다.
- BFS는 unweighted graph에서 시작점으로부터의 최단 거리를 보장한다.
- endWord를 처음 pop하거나 발견한 level이 최단 변환 길이다.
- visited는 cycle과 중복 방문을 막는다.

## Complexity

- Time: `O(n * L^2)` 정도로 볼 수 있다. `n`은 단어 수, `L`은 단어 길이다.
- Space: `O(n * L)` for wildcard pattern buckets.

## Mistake Notes

- endWord가 wordList에 없으면 변환할 수 없다.
- 모든 단어 쌍을 비교해 graph를 만들면 `O(n^2 * L)`로 커질 수 있다.
- BFS level의 값은 edge 수가 아니라 문제에서 요구하는 변환 sequence 길이이므로 beginWord를 1로 시작한다.