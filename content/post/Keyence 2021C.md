---
title: "Keyence 2021C"
date: 2021-06-28
draft: false
tags: ["Atcoder", "DP"]
categories: ["Atcoder"]
---
<!--more-->

# 題目
給定 $$H\times W$$ 表格，每個格子可放字元 `R`,`D`,`X`。<br>
有 $$K$$ 個格子已指定好字元不可更改，因此表格總共有 $$3^{HW-K}$$ 種可能。

對於每種可能，計算機器人從左上走到右下的方法數，每到一個格子執行以下操作:
- 當前字元 `R` 則向右
- 當前字元 `D` 則向下
- 當前字元 `X` 向右向下皆可

將答案加總 mod 998244353。

# 錯誤解法
一開始我只有考慮當前格子上面和左邊是不是空格，考慮上面下來的路徑，轉移式如下:
```c++
if(board[i - 1][j] != 'R') {
    tmp = dp[i - 1][j];
    if(board[i][j - 1] == '\0') {
        tmp *= 3;
    }
    if(board[i - 1][j] == '\0') {
        tmp <<= 1;
    }
    dp[i][j] += tmp % MOD;
    dp[i][j] %= MOD;
}
```

這完全是錯誤的方法，別的格子也可以對路徑出現次數造成影響，像下面情況:
```
XXX
0XX
```

# 正確解法

設 $$dp_{i,j,n}$$ 表示有幾個走到位置 $$i,j$$ 且經過 $$n$$ 個空格的路徑。<br>
要保持某個路徑，經過的空格只能變成 (`R`or`D`),`X`，不經過的空格 `R`,`D`,`X` 皆可

答案 $$\textstyle\sum_{1\le k\le H+W} dp_{H,W,k}2^k3^{HW−K-k}$$<br>
時間複雜度 $$O(HW(H+W))$$ 肯定超時。

式子可以轉化成 $$3^{HW−K}\textstyle\sum_{1\le k\le H+W}dp_{H,W,k}(\frac{2}{3})^k$$<br>
想像對於每個路徑原本權重 $$w=1$$，經過 $$n$$ 個空格則 $$w=(\frac{2}{3})^n$$ ，將所有路徑權重加起來再乘上 $$3^{HW−K}$$

```c++
dp[0][0] = 1;
number_2div3 = 2 * pow_mod(3, mod - 2) % mod;
for(int i = 0; i < n; i++) {
    for(int j = 0; j < m; j++) {
        if(board[i][j] == 'R') {
            dp[i][j + 1] += dp[i][j];
        } else if(board[i][j] == 'D') {
            dp[i + 1][j] += dp[i][j];
        } else if(board[i][j] == 'X') {
            dp[i][j + 1] += dp[i][j];
            dp[i + 1][j] += dp[i][j];
        } else {
            dp[i][j + 1] += number_2div3 * dp[i][j] % mod;
            dp[i + 1][j] += number_2div3 * dp[i][j] % mod;
        }
        dp[i + 1][j] %= mod;
        dp[i][j + 1] %= mod;
    }
}
cout << pow_mod(3, n * m - k) * dp[n - 1][m - 1] % mod;
```

# 重點
以前都沒想過可以做這種看起來肯定超時的 dp，只要**找到方法優化**就好了。