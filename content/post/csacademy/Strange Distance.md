---
title: "CSAcademy Strange Distance"
date: 2021-06-28
draft: false
tags: ["CSAcademy", "Brute Force"]
categories: ["CSAcademy"]
markup: mmark
---
<!--more-->

# 題目
有一堆點，點距離定義為 $$\min(|x_1-x_2|,|y_1-y_2|)$$，尋找第 $$K$$ 小點。

# 解法
如果直接暴搜 $$O(n^2)$$ 肯定超時，可以試試二分搜。

`ranking` 函數為某個距離最小排名，`1 1 2 2` 中 2 最小排名為 3。
計算比距離 $$d$$ 小的距離數量，分兩種情況:
- $$x_i-x_j<d$$
- $$x_i-x_j\ge d\ and\ y_i-y_j<d$$

第一種將點排序即可，第二種在 $$x_i-x_j\ge d$$ 時將 $$y$$ 座標放進 BIT。

```c++
using ll = long long;

ll ranking(int dist) {
    fill_n(tree, 100001, 0);
    ll result = 0;
    for(int i = 0, j = 0; i < n; i++) {
        while(j < n && points[i].x - points[j].x > dist) {
            add(points[j].y);
            j++;
        }
        result += (i - j) + prefixSum(points[i].y + dist) - prefixSum(points[i].y - dist - 1);
    }
    return result;
}
```

**算 $$A \cap B$$ 時可分為 $$A \cup (!A \cap B)$$**