# Part 06：資料集和與迴圈迭代

[回到目錄](https://github.com/alankrantas/learn-python-on-microbit-guide/blob/main/README.md)

## 資料集和

在程式中能用變數記錄資料很方便，但當資料量變多時，管理上就會更麻煩。Python 提供了幾種形式的**集合（collection）**讓我們整理資料，而且可以用迴圈去批次處理它們。在 Python 中，這通常會用 for 迴圈來做。

舉個例子，若我們想讓 micro:bit 播放一系列預定的音符：

```python
from microbit import sleep
import music

music.pitch(587)
sleep(500)

music.pitch(659)
sleep(500)

music.pitch(523)
sleep(500)

music.pitch(262)
sleep(500)

music.pitch(392)
sleep(1500)

music.stop()
```

這樣看來很好，但你會發現每個動作其實大同小異，只有音的頻率不同而已。能不能把這些動作簡化成同一個動作呢？

## Python 基礎：串列（list）和元祖（tuple）集合

在 Python 的集合當中，最基本也最常用的集合叫做**串列（list）**。下面宣告了個變數 lst，其內容是個含有 5 筆資料的串列：

```
>>> lst = [1, 2, 3, 4, 5]
>>> lst
[1, 2, 3, 4, 5]
```

串列的資料用中括號 ```[]``` 包住，每一筆資料之間用半形逗號隔開。每一筆資料稱為串列的元素（element），而串列也可被稱為是個容器（container）。




