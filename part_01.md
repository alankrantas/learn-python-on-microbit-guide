# Part 01：事前準備與 REPL

[回到目錄](https://github.com/alankrantas/learn-python-on-microbit-guide/blob/main/README.md)

## 閱讀本指南所需的準備

本指南會使用 2020 年 10 月後上市的新版 [micro:bit V2](https://microbit.org/zh-tw/new-microbit/)，但大部分程式碼也適用於 V1。筆者推薦使用 V2，因為價格相近但規格更佳，有更多記憶體執行 Python 直譯器。

你也需要 micro USB 線以及一台有 Google Chrome 瀏覽器的電腦，就這樣。不須安裝其他東西。

### Python 編輯器

打開 https://python.microbit.org/v/2 ：

![02](https://user-images.githubusercontent.com/44191076/107118877-baae6b80-68be-11eb-8bda-aa8013b3698f.png)

這是 micro:bit 官方提供的線上版瀏覽器，雖然簡單，但就本指南的目的來說夠用了。後面有機會的話，筆者會在談如何使用其他 Python 編輯器。你也可以試試[Beta 版](https://python.microbit.org/v/beta)，它可能會包含一些新功能以及更新的韌體。

畫面上方有五個大按鈕：

* Download/Flash (下載/燒錄)
* Connect/Disconnect (連線/解除連線)
* Load/Save (上載檔案/存檔)
* Open Serial/Close Serial (打開 REPL/關閉 REPL)
* Help (線上說明)

按鈕的內容會因與 micro:bit 連線與否而有所不同。

### 與 micro:bit 連線和燒錄第一支程式／韌體

Python 是直譯式語言，這表示程式運作的地方必須裝有直譯器。幸好，官方編輯器簡化了這個流程：你從編輯器下載的 .hex 程式檔中，有一部分會包著 micro:bit 用的 Python 韌體，剩下的則是你寫的程式（又稱script，草稿碼）。即使你的 micro:bit 之前並沒有安裝 Python 韌體，只要燒錄一次程式就會自動完成韌體安裝。這也使得第一次燒錄──或者在使用過其他語言後重新燒錄──Python 會花較久的時間。

> micro:bit 使用的 Python 實際上是 MicroPython──設計給記憶體有限的開發版的 Python 版本。MicroPython 本身就有好幾種版本，針對不同類型的開發板而作，不過它們都是是以 Python 3.4 為基礎。因此，只要是 Python 3.4 擁有的核心語言功能（這些在接下來的版本中也幾乎沒有變化），在 micro:bit 都是可以使用的。

那麼，我們就來燒錄一支程式，直接用官方編輯器一打開就出現的程式即可：

```python
# Add your Python code here. E.g.
from microbit import *


while True:
    display.scroll('Hello, World!')
    display.show(Image.HEART)
    sleep(2000)
```

將 micro:bit 接上電腦，然後按畫面中的 **Connect**，讓 Chrome 瀏覽器跟你的裝置連線：

![05](https://user-images.githubusercontent.com/44191076/107119477-c4d26900-68c2-11eb-94fd-6ad62ec2e567.png)

連線後按 **Flash**。等待程式燒錄作業結束。

> 較近期的 micro:bit 的韌體支援 WebUSB，讓瀏覽器能跟 USB 裝置連線，這麼一來編輯器就能直接下載程式到 micro:bit 上。現在市售的 micro:bit 應該都有新韌體了；但若你沒辦法連線，請用 Download 下載 .hex 檔後複製／貼上到 micro:bit 的 USB 磁碟區。

## REPL 互動介面

想學 Python，就不可不知 **REPL**（Read-Eval-Print Loop，讀取-求值-輸出循環）。簡單說，這是 Python 直譯器的互動介面，能讓開發者用來做簡單的測試、探索 Python 的功能、讀取程式的回應等等，不管在電腦或開發板上，對程式開發者來說都是很實用的工具。因此後面在講解 Python 入門時，我們會先在 REPL 下來示範。

在 micro:bit 連線的情況下，點編輯器的 **Open Serial**，然後點按鈕下方右側的「Send CTRL-C for REPL」或直接在鍵盤上按 Ctrl+C。你應該會看到以下畫面：

![04](https://user-images.githubusercontent.com/44191076/107119544-44603800-68c3-11eb-9f0a-6305b40195ea.png)

這幾行文字

```
MicroPython v1.13 on 2021-02-19; micro:bit v2.0.0-beta.4 with nRF52833
Type "help()" for more information.
>>> 
```

就是 REPL 的提示，類似 Windows 命令提示字元或 Unix 的終端機。它告訴我們 Python 直譯器的版本（隨著未來編輯器的更新，你可能會看到更新的版本），並等待我們輸入指令。

如果你使用的是 micro:bit 一代，你會看到它用的是不同的版本：

```
MicroPython v1.9.2-34-gd64154c73 on 2017-09-01; micro:bit v1.0.1 with nRF51822
Type "help()" for more information.
>>> 
```

現在於 >>> 後面輸入 ```1 + 2```，並按 Enter：

```
>>> 1 + 2
3
```

可以發現 Python 直譯器解讀了你輸入的程式，並自動算出答案。

### REPL 彩蛋：this、antigravity 與 love

> 如果使用 micro:bit 二代，這兩個功能只有 v2.0.0-beta.4 之後的版本才有。但一代也支援這些功能。

你也可以試試看在 REPL 輸入以下指令：

```
>>> import this
```

你會看到

```
The Zen of MicroPython, by Nicholas H. Tollervey

Code,
Hack it,
Less is more,
Keep it simple,
Small is beautiful,

Be brave! Break things! Learn and have fun!
Express yourself with MicroPython.

Happy hacking! :-)
```

這段文字的翻譯如下：

```
MicroPython 之禪，作者：Nicholas H. Tollervey

寫程式，
搞變化，
越簡潔越好，
越單純越佳。

大膽一點，把程式搞壞也無所謂！享受學習樂趣！
盡情用 MicroPython 表達自我。

好好當個小駭客吧 :)
```

這個彩蛋「MicroPython 之禪」源自正規 Python 版本的「[Python 之禪](https://zh.wikipedia.org/zh-tw/Python%E4%B9%8B%E7%A6%85)」（The Zen of Python）。這兩個文件的核心意旨都是：在學習寫 Python 程式時，雖然沒有規定你能怎麼寫，但程式碼能寫得越精簡越乾淨越好，並用最簡單漂亮的方式解決問題。有些人會說這種寫程式的方式叫做「符合 Python 風格」（Pythonic）。

再來是第二個彩蛋：

```
>>> import antigravity
```

會出現

```
+-xkcd.com/353---------------------------------------------------+
|                                                                |
|                                                    \0/         |
|                                                  /   \         |
|        You're flying!                  MicroPython!  /|        |
|            How?                                      \ \       |
|            /                                                   |
|          0                                                     |
|         /|\                                                    |
|          |                                                     |
|-----____/_\______________________________----------------------|
|                                                                |
+----------------------------------------------------------------+
```

這同樣是源自正規 Python 的[同名彩蛋](https://xkcd.com/353/)。這漫畫的意思是，Python（以及 MicroPython）是如此無所不能，甚至還能讓人飛起來（比喻性的）。

最後是 micro:bit 的 MicroPython 獨有的彩蛋。輸入下面這行字，然後看你的 micro:bit，有沒有發現到什麼現象？

```
>>> import love
```
