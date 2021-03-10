# Part 06：資料集合與迴圈迭代

[回到目錄](https://github.com/alankrantas/learn-python-on-microbit-guide/blob/main/README.md)

## 資料集和

在程式中能用變數記錄資料很方便，但當資料量變多時，管理上就會更麻煩。Python 提供了幾種形式的集合（collection）讓我們整理資料，而且可以用迴圈去批次處理它們。在 Python 中，這通常會用 for 迴圈來做。

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

### 建立和使用串列

在 Python 的集合當中，最基本也最常用的集合叫做**串列（list）**。下面宣告了個變數 lst，其內容是個含有 5 筆資料的串列：

```
>>> lst = [1, 2, 3, 4, 5]
>>> lst
[1, 2, 3, 4, 5]
```

串列的資料用中括號 ```[]``` 包住，每一筆資料之間用半形逗號隔開。每一筆資料稱為串列的元素（element），而串列也可被稱為是個容器（container）。

串列就是 list 型別：

```
>>> type(lst)
<class 'list'>
```

串列當中的元素, 可以用 ```串列名稱[索引]``` 的方式讀取或修改之：

```
>>> lst[0]
1
>>> lst[2]
3
>>> lst[4] = 10
>>> lst[4]
10
>>> lst
[1, 2, 3, 4, 10]
```

索引一定是從 0 開始，這是電腦系統中一個長久以來就存在的習慣。以這個串列為例, 它有 5 個元素, 所以最後一個元素的索引就會是元素的總數（或串列長度）減 1。

### 串列與其元素的關係

注意到串列元素會有自己的型別：

```
>>> type(lst[0])
<class 'int'>
```

也就是說，串列雖然有自己的型別，但它裡面的元素卻是各自不同的變數，指向整數型別的數字。

記得在 Part 2 時，我們提過 Python 變數是個路標，它指向資料而不是儲存資料。這裡我們就要來看一下這種特質對於串列的奇特影響。下面我們建立一個串列，然後建立另一個串列來「複製」它。接著我們修改其中一個串列的元素值，猜猜看會發生什麼事？

```
>>> lst1 = [1, 2, 3]
>>> lst2 = lst1
>>> lst2[0] *= 10
>>> lst1
[10, 2, 3]
```

兩個串列的索引 0 元素一起改變了。這自然是因為兩個串列變數指向了同一個 list 型別物件，所以它們的元素等於就是共用的。

雖然你目前會這樣「複製」串列的場合還不多，但你事先不曉得 Python 變數是這種性質的話，就會覺得很奇怪了。之後我們也會提到如何真正複製出一個串列，使兩個串列變成分離的物件。

### 串列可以儲存任何型別的元素

Python 串列的元素不見得只能儲存同樣型別的元素：

```
>>> lst = [1, 3.14159, 'Hello World', True, None]
>>> lst
[1, 3.14159, 'Hello World', True, None]
```

元素的型別取決於它的值。例如，```lst[2]``` 是字串型別。

> 在其他程式語言中會有陣列（array），但陣列是長度固定、也只能儲存同一種資料型別的容器。Python 不是沒有陣列，但它最主要和最常用的容器就是串列。

## 改寫音符清單

現在，我們回頭來用串列改寫一開始的音符播放程式：

```python
from microbit import sleep
import music

notes = [587, 659, 523, 262, 392]
delay = [500, 500, 500, 500, 1500]

music.pitch(notes[0])
sleep(delay[0])

music.pitch(notes[1])
sleep(delay[1])

music.pitch(notes[2])
sleep(delay[2])

music.pitch(notes[3])
sleep(delay[3])

music.pitch(notes[4])
sleep(delay[4])

music.stop()
```

看起來似乎沒有變得更簡短，頂多只是把數值收集到了同一個位置而已。能不能把程式變得更精簡呢？

## Python 基礎：for 迴圈
