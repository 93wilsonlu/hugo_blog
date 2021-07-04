---
title: "Codeforces 1542C"
date: 2021-07-04T00:03:20+08:00
draft: false
---
<!--more-->

# 題目
設 $$f(i)$$ 為不是 $$i$$ 的因數中最小的數，求 $$\textstyle\sum_{i=1}^{n}f(i)$$

# 解法
直接枚舉 $$f(i)$$<br>
如果 $$f(i)=k$$，$$i$$ 一定被 $$lcm(1...k-1)$$ 整除，總共有 $$\frac{n}{lcm(1...k-1)}$$ 數字符合<br>
還要減掉 $$k$$ 更大的情況，答案為 $$\textstyle\sum_{k=1}^{m}(\frac{n}{lcm(1...k-1)} - \frac{n}{lcm(1...k)})k$$

```c++
int base[100] = {};

void solve() {
    ll n;
    cin >> n;
    ll lcm = 1, last = 1, result = 0;
    for(int i = 2; i < 100 && lcm <= n; i++) {
        if(base[i] > 0) {
            lcm *= base[i];
        }
        result += (n / last - n / lcm) % mod * i % mod;
        result %= mod;
        last = lcm;
    }
    cout << result << '\n';
}

int main() {
    cin.sync_with_stdio(0);
    cin.tie(0);
    for(int i = 2; i < 100; i++) {
        if(!base[i]) {
            base[i] = i;
            for(int j = i + i; j < 100; j += i) {
                base[j] = -1;
            }
            for(int j = i * i; j < 100; j *= i) {
                base[j] = i;
            }
        }
    }
    int t;
    cin >> t;
    while(t--)
        solve();
    return 0;
}
```

比完賽後懶得簡化程式 (被打)