---
title: "Atcoder Beginner Contest 197F"
date: 2021-06-28
draft: false
tags: ["Atcoder", "BFS"]
categories: ["Atcoder"]
markup: mmark
---
<!--more-->

# 題目
有一張圖，每個邊上有一字元，求 1 到 N 的最短路徑，邊組成的字串必須是回文。

# 解法
放兩個人 $$p_1,p_2$$ 在 $$1,N$$，每次讓他們走一步，走的邊字元相等，做 BFS。<br>
假設他們在 $$u,v$$，當 $$u=v$$ 或 $$u$$ 連接 $$v$$，路徑就建出來了。<br>
記得存狀態是否訪問過。

```c++
while(!q.empty()) {
    state = q.front(), q.pop();
    if(state.u == state.v) {
        return state.step;
    }
    if(is_connected[state.u][state.v]) {
        return state.step + 1;
    }
    for(int c = 0; c < 26; c++) {
        for(int a : graph[state.u][c]) {
            for(int b : graph[state.v][c]) {
                if(!visited[a][b]) {
                    q.push({a, b, state.step + 2});
                    visited[a][b] = 1;
                }
            }
        }
    }
}
```

這樣還是會漏一種情況:
```
  2 ─ 3
 /     \
1 ─ 4 ─ 5
```
狀態 [2,3] 在 queue 的位置較狀態 [4,4] 前面。<br>
因此拿到路徑後，要把剩下東西 pop 掉才能拿到最佳路徑。

**有時理所當然的事並不理所當然**，但想到這種 case 也太強了吧。