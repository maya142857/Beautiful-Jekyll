---
layout: post
title: Pwn Tutorial
subtitle: Buffer Overflow
gh-repo: maya142857/maya142857.github.io
gh-badge:[star, fork, follow]
tags: [test]
comments: true
---

## 出題概念：
瞭解 C/C++ 在使用 gets() 讀取資料時，不會對存取空間邊界做檢查，攻擊者可透過輸入較長資料造成程式執行錯誤或改變流程。


## 解題思路：
* 先分析原始碼，存在 可執行任意指令的qwer()，但程式正常執行的話不會被執行到。
* main() function使用gets() 去存取固定大小的buffer。
* 嘗試透過 buffer overflow 改變程式流程以執行任意指令。

![](https://i.imgur.com/pNWLOXA.png)

## 解題步驟

透過 gdb 觀察輸入一長串A之後的執行過程。

![](https://i.imgur.com/xgBqaWh.png)



程式在 return 時發生錯誤。原因是stack上堆滿了A，原本的 return address 被覆蓋成了不合法的地址。

![](https://i.imgur.com/2xXBw1i.png)



算出會從輸入的第幾個byte開始蓋到return address。

![](https://i.imgur.com/W80Ez5k.png)



gets() 輸入 48 bytes (0x30)後會抵達 rbp 指的位址，rbp 指的位址還有8 bytes 的 save rbp，接下來就是return address。

![](https://i.imgur.com/XmkCqpL.png)



找到qwer()的記憶體位址0x400687。構造一個 48 bytes (0x38) + qwer()記憶體位址的輸入。

![](https://i.imgur.com/UBnoXYH.png)




## 參考答案
![](https://i.imgur.com/zvz8DD1.png)

![](https://i.imgur.com/AGXlUZF.png)


