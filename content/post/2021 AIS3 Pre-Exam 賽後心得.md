---
title: 2021 AIS3 Pre-Exam 賽後心得
date: 2021-05-24
draft: false
tags: ["CTF", "AIS3"]
categories: ["CTF"]
---

## Cat Slayer ᶠᵃᵏᵉ | Nekogoroshi
打密碼，一旦打錯就會 Lock 掉。

我是直接手打，最多也就試 120 次，好像可以用 `pwntools`，只是不知道怎麼處理不是純文字的情況。

## Microchip
題目是每四個字元為一組做加密，flag 格式是 `AIS3{...}` 可反推 key。

## Blind
題目會先給一個 syscall 再把 STDOUT_FILENO 關掉，最後呼叫 sh。

我們沒辦法阻止 `close(1)` 運行，那要怎麼辦呢?

應該很多人知道 bash 中 `>` 代表把資料寫到檔案，而 `2>` 則是把錯誤訊息寫到檔案。

這種技巧可以套用到這題，在 [syscall](https://w3challs.com/syscalls/?arch=x86_64) 表可以找到 `dup2` 函數，它的作用是將對 `new_fd` 操作導到 `old_fd`，也就是說，`write(new_fd, buf, sizeof(buf));` 等同於 `write(old_fd, buf, sizeof(buf));`。

在syscall 參數填入 33 2 1 0 (0x21 STDERR_FILENO STDOUT_FILENO any) 就可以拿到 flag。

## Microcheese
**遊戲規則:**
有很多堆石子，每回合玩家可以選某一堆拿走一些石子，當玩家沒有任何石子可以拿的時候就輸了。

只要打敗 AI 就可以拿到 flag，但此遊戲被設計為 AI 一定能贏(具體算法我也不清楚)。

```python
def play(game: Game):
    ai_player = AIPlayer()
    win = False

    while not game.ended():
        game.show()
        print_game_menu()
        choice = input('it\'s your turn to move! what do you choose? ').strip()

        if choice == '0':
            pile = int(input('which pile do you choose? '))
            count = int(input('how many stones do you remove? '))
            if not game.make_move(pile, count):
                print_error('that is not a valid move!')
                continue

        elif choice == '1':
            game_str = game.save()
            digest = hash.hexdigest(game_str.encode())
            print('you game has been saved! here is your saved game:')
            print(game_str + ':' + digest)
            return

        elif choice == '2':
            break

        # no move -> player wins!
        if game.ended():
            win = True
            break
        else:
            print_move('you', count, pile)
            game.show()

        # the AI plays a move
        pile, count = ai_player.get_move(game)
        assert game.make_move(pile, count)
        print_move('i', count, pile)

    if win:
        print_flag(flag)
        exit(0)
    else:
        print_lose()
```
`server.py` 中 `play()` 沒有處理 `choice` 等於其他數值的情況，後面的程式還是會被執行。

因此如果一直按 `3` 就變成 AI 在跟自己玩，我們可以按到剩一堆石子再拿它。

第一次操作的時候必須先隨便拿一顆石子，不然 `print_move('you', count, pile)` 的時候 `count` 和 `pile` 還沒被定義。

# 心得
開始之前我原本想說也許可以寫三題 Pwn 兩題 Web，Misc 就盡量 google，Reverse 和 Crypto 做到簡單題就好了。
結果在賽中完全被打敗，跟去年題目不是同一個等級。

我打 Write Me 時有買 hint，它只有寫"懶惰綁定"四個字，執行檔叫 gotplt 就算不懂的人也會去查吧。不能改 plt 而且改 got 會被蓋到我也沒有解出來。

晚上睡覺還會夢見自己在看 Wireshark。

還是有許多很厲害或我之前忽略的題目，例如 Yet Another Login Page 沒有想到利用 json 做 injection，明明有想過 None 等於什麼是 True，就差一點點...

官網說共錄取 150 人備取 30 人，但過去好幾年都超過著個數字，不知道能不能上，臨時抱佛腳沒上也不意外就是了。

很棒的比賽，不管看考古或今年都能發現很多有趣的新事物。

明年再加油吧!