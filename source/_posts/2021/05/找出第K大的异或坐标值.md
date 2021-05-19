---
title: 找出第K大的异或坐标值
date: 2021-05-19 09:27:38
tags:
 - leetcode
 - 位运算
categories:
 - leetcode
mathjax: true
---

[原题链接](https://leetcode-cn.com/problems/find-kth-largest-xor-coordinate-value/)

# 题目描述

给你一个二维矩阵 `matrix` 和一个整数 `k` ，矩阵大小为 `m x n` 由非负整数组成。

矩阵中坐标 `(a, b)` 的 **值** 可由对所有满足 `0 <= i <= a < m` 且 `0 <= j <= b < n` 的元素 `matrix[i][j]`（**下标从 0 开始计数**）执行异或运算得到。

请你找出 `matrix` 的所有坐标中第 `k` 大的值（**`k` 的值从 1 开始计数**）。

<!-- more -->

**示例 1：**

```
输入：matrix = [[5,2],[1,6]], k = 1
输出：7
解释：坐标 (0,1) 的值是 5 XOR 2 = 7 ，为最大的值。
```

**示例 2：**

```
输入：matrix = [[5,2],[1,6]], k = 2
输出：5
解释：坐标 (0,0) 的值是 5 = 5 ，为第 2 大的值。
```

**示例 3：**

```
输入：matrix = [[5,2],[1,6]], k = 3
输出：4
解释：坐标 (1,0) 的值是 5 XOR 1 = 4 ，为第 3 大的值。
```

**示例 4：**

```
输入：matrix = [[5,2],[1,6]], k = 4
输出：0
解释：坐标 (1,1) 的值是 5 XOR 2 XOR 1 XOR 6 = 0 ，为第 4 大的值。
```

**提示：**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 1000`
- `0 <= matrix[i][j] <= 106`
- `1 <= k <= m * n`

# 思路分析

因为需要快速获取到每个异或坐标值，需要计算二维数组的二维异或前缀和。

设二维前缀和是矩阵$matrix$中满足中所有满足$0 \leq x < i$且$0 \leq y < j$的元素执行按位异或运算的结果，则
$$
pre(i, j) = pre(i - 1, j) \oplus pre(i, j - 1) \oplus pre(i - 1, j - 1) \oplus matrix(i, j)
$$
下面是二位前缀和的可视化展示。

![fig1](https://raw.githubusercontent.com/juhick/picJuhick/master/20210519093919.png)

在得到前缀和之后，我们只需要找去其中第K大的元素即可，可以采用快速选择算法进行实现。

## 细节

在二维前缀和的计算过程中，如果我们正在计算首行或者首列，即$i=0$或$j=0$，此时例如$\textit{pre}(i-1,j-1)$是一个超出下标范围的结果。因此我们可以使用一个$(m+1) \times (n+1)$的二维矩阵，将首行和首列空出来赋予默认值0，并使用接下来的$m$行和$n$列存储二维前缀和，这样就不必进行下标范围的判断了。

## 代码实现

```java
class Solution {
    public int kthLargestValue(int[][] matrix, int k) {
        int m = matrix.length;
        int n = matrix[0].length;
        int[][] pre = new int[m + 1][n + 1];
        List<Integer> results = new ArrayList<>();
        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                pre[i][j] = pre[i - 1][j] ^ pre[i][j - 1] ^ pre[i - 1][j - 1] ^ matrix[i - 1][j - 1];
                results.add(pre[i][j]);
            }
        }
        quickSelect(results, 0, results.size() - 1, k - 1);
        return results.get(k - 1);
    }

    public void quickSelect(List<Integer> results, int left, int right, int k){
        if(left == right) return;
        int pivot = (int)(left + Math.random()*(right - left + 1));
        swap(results, right, pivot);
        int sepl = left - 1, sepr = left - 1;
        for(int i = left; i <= right; i++){
            if(results.get(i) > results.get(right)){
                swap(results, i, ++sepr);
                swap(results, sepr, ++sepl);
            }else if(results.get(i) == results.get(right)){
                swap(results, i, ++sepr);
            }
        }
        if(left + k > sepl && left + k <= sepr){
            return;
        }else if(left + k <= sepl){
            quickSelect(results, left, sepl, k);
        }else{
            quickSelect(results, sepr + 1, right, k - (sepr - left + 1));
        }

    }

    public void swap(List<Integer> results, int index1, int index2){
        int temp = results.get(index1);
        results.set(index1, results.get(index2));
        results.set(index2, temp);
    }
}
```

