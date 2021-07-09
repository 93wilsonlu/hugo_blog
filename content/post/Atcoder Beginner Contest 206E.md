---
title: "Atcoder Beginner Contest 206E"
date: 2021-07-02T23:25:22+08:00
draft: false
markup: mmark
---
<!--more-->

# 題目
計算區間內滿足以下條件數對 $$(x,y)$$ 數量:

- $$g=\gcd(x,y)\ne 1$$
- $$\frac{x}{g}\ne 1$$
- $$\frac{y}{g}\ne 1$$

# 解法
用排容原理枚舉數對的 gcd，算區間內不互質數對數量。
```c++
for(ll i = 2; i <= r; i++) {
    if(!n_factors[i]) {
        for(ll j = i; j <= r; j += i) {
            n_factors[j]++;
        }
    }
}
ll result = 0;
for(ll i = 2; i <= r; i++) {
    if(n_factors[i] & 1) {
        result += f(r / i - (l - 1) / i);
    } else {
        result -= f(r / i - (l - 1) / i);
    }
}
```
注意對於所有 $$a$$ 滿足 $$a|b^2$$ 不要算。<br>
因為像 $$2,3$$ 會對 $$6$$ 覆蓋兩次，用排容原理處理。<br>
而 $$2$$ 對 $$8$$ 不會，$$8$$ 的倍數集合在 $$2$$ 的倍數集合裡，因此不用管它。
```c++
for(ll i = 2; i <= r; i++) {
    if(!n_factors[i]) {
        ...
        for(ll j = i * i; j <= r; j += i * i) {
            n_factors[j] = -1e18;
        }
    }
}
for(ll i = 2; i <= r; i++) {
    if(n_factors[i] > 0) {
        ...
    }
}
```
最後還要處理數對 $$x|y$$ 情況
```c++
for(ll i = max(2, l); i <= r; i++) {
    result -= r / i - 1;
}
```