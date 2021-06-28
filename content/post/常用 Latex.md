---
title: "常用 Latex"
date: 2020-03-25
lastmod: 2020-03-25
draft: false
tags: ["Latex", "Markdown"]
categories: ["Markdown"]
---
<!--more-->

這裡列出了一些常用 Latex 技巧

放上一個範例

$$
v=v_0+at
$$

$$
x=x_0+v_0t+\dfrac{1}{2}at^2
$$

$$
v^2=v_0^2+2a(x-x_0)
$$

## 附加符號

| 代碼           | 顯示結果         | 代碼           | 顯示結果         |
| -------------- | ---------------- | -------------- | ---------------- |
| `x_0`          | $$x_0$$          | `x^2`          | $$x^2$$          |
| `\dfrac{1}{x}` | $$\dfrac{1}{x}$$ | `\tfrac{1}{x}` | $$\tfrac{1}{x}$$ |
| `\sqrt{2}`     | $$\sqrt{x}$$     | `\sqrt[y]{x}`  | $$\sqrt[y]{x}$$  |
| `\vec{x}`      | $$\vec{x}$$      | `\hat{x}`      | $$\hat{x}$$      |
| `\ddot{x}`     | $$\ddot{x}$$     | `\dot{x}`      | $$\dot{x}$$      |
| `60^\omicron`  | $$60^\omicron$$  |

## 特殊符號

| 代碼     | 顯示結果   | 代碼     | 顯示結果   |
| -------- | ---------- | -------- | ---------- |
| `\Delta` | $$\Delta$$ | `\delta` | $$\delta$$ |
| `\theta` | $$\theta$$ | `\omega` | $$\omega$$ |
| `\pi`    | $$\pi$$    | `\alpha` | $$\alpha$$ |
| `\beta`  | $$\beta$$  | `\gamma` | $$\gamma$$ |
| `\ `     | $$\\ $$    | `\%`     | $$\\%$$    |
| `\infty` | $$\infty$$ | `\ldots` | $$\ldots$$ |

## 運算

| 代碼               | 顯示結果             | 代碼                  | 顯示結果                |
| ------------------ | -------------------- | --------------------- | ----------------------- |
| `\sin x`           | $$\sin x$$           | `\lim_{x\to 0}`       | $$\lim_{x\to 0}$$       |
| `\log_y x`         | $$log_y x$$          | `\times`              | $$\times$$              |
| `\div`             | $$\div$$             | `\bullet`             | $$\bullet$$             |
| `\sum_{i=1}^n a_i` | $$\sum_{i=1}^n a_i$$ | `\int_{a}^{b} 2x\ dx` | $$\int_{a}^{b} 2x\ dx$$ |

## 關係

| 代碼              | 顯示結果            | 代碼               | 顯示結果             |
| ----------------- | ------------------- | ------------------ | -------------------- |
| `=`               | $$=$$               | `\ne`              | $$\ne$$              |
| `>`               | $$>$$               | `<`                | $$<$$                |
| `\ge`             | $$\ge$$             | `\leq`             | $$\leq$$             |
| `\approx`         | $$\approx$$         | `\equiv`           | $$\equiv$$           |
| `\pmod{m}`        | $$\pmod{m}$$        | `a \bmod b`        | $$a \bmod b$$        |
| `\in`             | $$\in$$             | `\subset`          | $$\subset$$          |
| `\because`        | $$\because$$        | `\therefore`       | $$\therefore$$       |
| `\rightharpoonup` | $$\rightharpoonup$$ | `\leftharpoondown` | $$\leftharpoondown$$ |
| `\Rightarrow`     | $$ \Rightarrow$$    | `\Leftrightarrow`  | $$\Leftrightarrow$$  |
| `\propto`         | $$\propto$$         | `\perp`            | $$\perp$$            |

| 代碼                                  | 顯示結果                                   |
| ------------------------------------- | ------------------------------------------ |
| `\begin{cases}x=2y\\x+y=6\end{cases}` | $$\begin{cases}x=2y \\\ x+y=6\end{cases}$$ |
| `\underbrace{a_1+\ldots+a_n} \\ n`    | $$\underbrace{a_1+\ldots+a_n} \\\ n$$      |
