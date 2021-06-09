---
title: "Codeforces 1385E"
date: 2021-06-09
draft: false
tags: ["C++", "Codeforces", "Topical Sort"]
categories: ["C++"]
---

給定一個有向和無向邊的圖，將無向邊定向並避免產生環，最後輸出圖。

拓樸排序，不考慮無向邊，排出點的順序後就能將邊定向了。

```c++
vector<int> graph[200001];
int n, m, result[200001], in_degree[200001];

void bfs() {
    queue<int> q;
    for(int i = 1; i <= n; i++) {
        if(!in_degree[i]) {
            q.push(i);
        }
    }
    fill_n(result + 1, n, 0);
    int now, k = 1;
    while(!q.empty()) {
        now = q.front(), q.pop();
        result[now] = k++;
        for(int child : graph[now]) {
            in_degree[child]--;
            if(!in_degree[child]) {
                q.push(child);
            }
        }
    }
}

struct Edge {
    int u, v, type;
} edges[400000];

void solve() {
    cin >> n >> m;
    fill_n(in_degree + 1, n, 0);
    for(int i = 1; i <= n; i++) {
        graph[i].clear();
    }
    int k = 0;
    for(int i = 0; i < m; i++, k++) {
        cin >> edges[k].type >> edges[k].u >> edges[k].v;
        if(edges[k].type) {
            graph[edges[k].u].push_back(edges[k].v);
            in_degree[edges[k].v]++;
        } else {
            k++;
            edges[k] = {edges[k - 1].v, edges[k - 1].u, 0};
        }
    }
    bfs();
    if(count(result + 1, result + n + 1, 0)) {
        cout << "NO\n";
        return;
    }
    cout << "YES\n";
    for(int i = 0; i < k; i++) {
        if(edges[i].type || result[edges[i].u] < result[edges[i].v]) {
            cout << edges[i].u << ' ' << edges[i].v << '\n';
        }
    }
}
```