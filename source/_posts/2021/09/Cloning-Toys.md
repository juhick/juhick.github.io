---
title: Cloning Toys
date: 2021-09-23 19:48:24
tags:
 - Codeforces
 - 简单数学
categories:
 - Codeforces
Math: true
---

[原题链接](https://codeforces.com/contest/922/problem/A)

# 题目大意

Imp初始有个**原型**玩具，并且有一个能够克隆玩具的机器。该机器有两种工作模式：

1. 放入一个**原型**玩具将获得一个额外的**原型**玩具以及一个玩具**副本**
2. 放入一个玩具**副本**将获得的两个额外的玩具**副本**

现在给定一个$x$和一个$y$，$x$代表玩具**副本**的个数，$y$代表**原型**玩具的个数，试问进行以上的两次操作是否能够得到指定个数的**副本**及**原型**。

<!-- more -->

# 解题思路

1. 初始时一定有1个**原型**，也就是说$y = 0$时直接输出No
2. $x$最多比$y$小1，而且是只进行操作一的结果，但凡$x$太少则可以直接输出No
3. 由于操作一产生的两种玩具是$1:1$的关系，所以$x - y + 1$就是全由操作二产出的**副本**，并且易知该数应该是一个偶数

# 代码示例

## Java

```java
import java.util.Scanner;

public class Main {
    private static Scanner in = new Scanner(System.in);

    public static void main(String[] args) {
        int x = in.nextInt();
        int y = in.nextInt();

        int left = y - 1;
        if(x - y < -1 || y == 0 || y == 1 && x > 0){
            System.out.println("No");
        }else if((x - left) % 2 == 0){
            System.out.println("Yes");
        }else {
            System.out.println("No");
        }
    }
}
```

## C++

```cpp
#include <iostream>
using namespace std;

int main() {
    int x, y;
    cin >> x >> y;
    if(x - y < -1 || y == 0 || y == 1 && x != 0){
        cout << "No" << endl;
    }else if((x - y + 1) % 2 == 0){
        cout << "Yes" << endl;
    }else cout << "No" << endl;
    return 0;
}
```
