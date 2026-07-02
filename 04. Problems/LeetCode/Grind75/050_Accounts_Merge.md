# Grind 75 050. Accounts Merge

> 같은 이메일을 공유하는 account들을 하나로 합치는 문제다. 이메일을 정점으로 보고 같은 account 안의 이메일들을 연결하면 connected component 문제가 된다.

## Link

- [LeetCode - Accounts Merge](https://leetcode.com/problems/accounts-merge)

## Problem Model

각 account는 `[name, email1, email2, ...]` 형태다. 이메일이 하나라도 겹치면 같은 사람의 account로 병합해야 한다. 같은 component의 이메일들을 정렬해 반환한다.

## Pattern

- [Graph](../../../01.%20Data%20Structures/09.%20Graph.md)
- [Union Find](../../../02.%20Algorithms/09.%20Union%20Find.md)
- [Union Find Connectivity](../../../03.%20Problem%20Solving%20Patterns/16.%20Union%20Find%20Connectivity.md)
- [Hash Table](../../../01.%20Data%20Structures/03.%20Hash%20Table.md)

## Approach

Union-Find를 사용한다.

1. 각 이메일의 parent를 자기 자신으로 초기화한다.
2. 같은 account 안의 모든 이메일을 첫 이메일과 union한다.
3. 이메일별 name을 저장한다.
4. root별로 이메일을 모은다.
5. 각 component를 정렬해 `[name, *emails]`로 반환한다.

## Python 3.14 Solution

```python
from collections import defaultdict


class Solution:
    def accountsMerge(self, accounts: list[list[str]]) -> list[list[str]]:
        parent: dict[str, str] = {}
        email_to_name: dict[str, str] = {}

        def find(email: str) -> str:
            if parent[email] != email:
                parent[email] = find(parent[email])
            return parent[email]

        def union(a: str, b: str) -> None:
            root_a = find(a)
            root_b = find(b)
            if root_a != root_b:
                parent[root_b] = root_a

        for account in accounts:
            name = account[0]
            emails = account[1:]
            for email in emails:
                if email not in parent:
                    parent[email] = email
                email_to_name[email] = name

            first_email = emails[0]
            for email in emails[1:]:
                union(first_email, email)

        groups: dict[str, list[str]] = defaultdict(list)
        for email in parent:
            groups[find(email)].append(email)

        result: list[list[str]] = []
        for root, emails in groups.items():
            result.append([email_to_name[root], *sorted(emails)])

        return result
```

## Correctness Notes

- 같은 account에 있는 이메일들은 같은 사람의 이메일이므로 union해도 안전하다.
- 이메일이 여러 account에 걸쳐 등장하면 union chain을 통해 같은 component로 연결된다.
- find 결과가 같은 이메일들은 같은 사람에게 속한다.
- component별 이메일을 정렬하고 해당 name을 붙이면 요구 형식이 된다.

## Complexity

- Time: `O(E α(E) + E log E)` mainly from union-find and sorting grouped emails.
- Space: `O(E)`, where `E` is total distinct emails.

## Mistake Notes

- account 이름이 같다고 같은 사람이라고 단정하면 안 된다. 연결 기준은 이메일이다.
- 이메일이 하나뿐인 account도 parent 초기화와 group 생성이 되어야 한다.
- root 이메일의 name을 사용할 수 있도록 `email_to_name`을 모든 이메일에 저장한다.