---
title: Magic Forest
date: 2021-09-23 20:30:53
tags:
 - Codeforces
 - 简单数学
categories:
 - Codeforces
math: true
---

[原题链接](https://codeforces.com/contest/922/problem/B)

# 题目大意

给定一个$n$，在$1-n$的范围内寻找三个数，这三个数需要满足三个条件：

1. $1 \leq a \leq b \leq c \leq n$
2. $a \oplus b \oplus c = 0$，这里的$x \oplus y$就是按位异或
3. $(a, b, c)$这三个数需要能够组成三角形

<!-- more -->

# 解题思路

第一个条件很容易满足，我们可以通过循环来控制。

第二个条件比较关键，因为异或有一条性质，即自己与自己的异或结果为0——$x \oplus x = 0$，所以我们找$c$时可以直接从所有的$a \oplus b$中寻找，这样可以节省大量时间。找出来的$c$首先要满足的条件就是$b \leq c \leq n$，不满足条件的可以直接舍弃。

看三条边能否构成三角形就很简单了，满足任意两条边的和大于第三边即可。

# 代码示例

## Java

```java
import java.util.Scanner;

public class Main {
    private static Scanner in = new Scanner(System.in);

    public static void main(String[] args) {
        int n = in.nextInt();
        int res = 0;
        for(int i = 1; i <= n; i++){
            for(int j = i; j <= n; j++){
                int m = i ^ j;
                if(m >= j && m <= n && i + j > m && i + m > j && j + m > i){
                    res++;
                }
            }
        }
        System.out.println(res);
    }
}
```

## C++

```c++
#include <iostream>
using namespace std;

int main() {
    int n;
    int res = 0;
    cin >> n;

    for(int i = 1; i <= n; i++){
        for(int j = i; j <= n; j++){
            int c = i ^ j;
            if(c >= j && c <= n && i + j > c && i + c > j && j + c > i){
                res++;
            }
        }
    }
    cout << res << endl;

    return 0;
}
```

