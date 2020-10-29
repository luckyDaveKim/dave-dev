---
template: post
draft: true
image: img/cover.jpg
date: 2020-10-28 03:09:18 +09:00
title: "[LeetCode] #94. Binary Tree Inorder Traversal"
excerpt: 주어진 이진 트리의 중위 순회 값을 구해봅니다.
tags:
  - leet_code
  - algorithm
  - medium_level
  - java
  - hash_table
  - stack
  - tree
---

# 문제
## [#94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal)
Given the root of a binary tree, return the inorder traversal of its nodes' values.  
*Follow up: Recursive solution is trivial, could you do it iteratively?*  

## Example 1.
```
Input: root = [1,null,2,3]
Output: [1,3,2]
```

## Example 2.
```
Input: root = []
Output: []
```

## Example 3.
```
Input: root = [1]
Output: [1]
```

## Example 4.
```
Input: root = [1,2]
Output: [2,1]
```

## Example 5.
```
Input: root = [1,null,2]
Output: [1,2]
```

## Constraints.
```
The number of nodes in the tree is in the range [0, 100].
-100 <= Node.val <= 100
```

---

# 해법
트리의 순회 방법에는 `전위 순회`, `중위 순회`, `후위 순회` 3가지 방법이 있습니다.  
이 중 중위 순회를 통한 트리의 결과를 반환하면 됩니다.  

## 나의 해법
### 성능
- 시간 복잡도 : `O(n)`
- 공간 복잡도 : `O(n)`

### 해법
재귀 함수를 통한 해법으로, 최초에 재귀 함수로 root 노드를 전달 합니다.  
재귀 함수 내에서는 전달받은 노드를 기준으로 왼쪽 자식 노드를 재귀 함수로 호출하고, 현재 노드의 값을 결과에 추가 후, 오른쪽 자식 노드를 재귀 함수로 호출합니다.  
이 재귀 함수는 전달받은 노드가 `null`일 때 까지 진행합니다.  
1. 결과를 담을 `result` 리스트를 선언합니다.  
2. 재귀 함수에 `result`와 root 노드를 전달합니다.  
2-1. 전달 받은 노드가 `null`일 때 까지 진행합니다.  
2-2. 노드의 왼쪽 자식 노드를 재귀 함수로 호출합니다.  
2-3. 노드의 값을 `result`에 추가합니다.  
2-4. 노드의 오른쪽 자식 노드를 재귀 함수로 호출합니다.  

### Java 코드
```java
public class BinaryTreeInorderTraversal {

	public List<Integer> inorderTraversal(TreeNode root) {
		List<Integer> result = new LinkedList<>();

		inorderTraversal(result, root);

		return result;
	}

	public void inorderTraversal(List<Integer> result, TreeNode root) {
		if (root == null) {
			return;
		}

		inorderTraversal(result, root.left);
		result.add(root.val);
		inorderTraversal(result, root.right);
	}

	public class TreeNode {
		int val;
		TreeNode left;
		TreeNode right;

		TreeNode() {
		}

		TreeNode(int val) {
			this.val = val;
		}

		TreeNode(int val, TreeNode left, TreeNode right) {
			this.val = val;
			this.left = left;
			this.right = right;
		}
	}

}
```

## 다른 해법
### 성능
- 시간 복잡도 : `O(n)`
- 공간 복잡도 : `O(n)`

### 해법
재귀 함수 대신 반복문을 통한 해법으로, 


DP를 이용한 해법으로 `i`번째 자리수 까지의 디코딩 가능한 모든 경우의 수는 다음과 같이 구할 수 있습니다.  
1. `i`번째 수가 0이 아닌경우. 즉, 한자리 수를 통한 디코딩이 가능한 경우로써 `i - 1`번째 까지의 디코딩 방식을 더합니다.  
2. `i`번째 수와 `i - 1`번째 수가 10(J) ~ 26(Z)사이의 숫자인 경우. 즉, 두자리 수로 문자를 만들 수 있는 경우. 
이 경우에는 한자리 수를 통한 디코딩과 두자리 수를 통한 디코딩이 가능하므로, "`i - 1`번째 까지의 디코딩 방식" + "`i - 1`번째 까지의 한자리 수를 통한 디코딩 방식 (그래야 `i - 1`번째 수와 `i`번째를 합친 두자리 수로 디코딩이 가능하다)" 로 나타낼 수 있습니다.

### Java 코드
```java
public class DecodeWays {

	public int numDecodings(String s) {
		int[] dp = new int[s.length()];
		dp[0] = s.charAt(0) == '0' ? 0 : 1;

		for (int i = 1; i < s.length(); i++) {
			int curOnesNum = s.charAt(i) - '0';
			int curTensNum = (s.charAt(i - 1) - '0') * 10 + curOnesNum;

			if (curOnesNum != 0) {
				dp[i] += dp[i - 1];
			}

			if (curTensNum >= 10 && curTensNum <= 26) {
				dp[i] += (i >= 2) ? dp[i - 2] : 1;
			}
		}

		return dp[s.length() - 1];
	}

}
```

# 코멘트
이 문제의 핵심은,
- 백트래킹이 아닌 DP를 통한 메모이제이션으로 중복된 계산을 피하는 것.
- `char`를 `int`로 변환하여 로직을 단순화 하는 것.
- 디코딩이 불가능한 경우가 존재한다는 것.
