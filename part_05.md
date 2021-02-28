# Part 05：資料輸入與型別轉換

[回到目錄](https://github.com/alankrantas/learn-python-on-microbit-guide/blob/main/README.md)

##  用 input() 接收使用者輸入的資料

micro:bit 本身有多種感測器，可以傳回不同的資料或狀態等等，不過這些會留到後面再講。現在，我們要來看怎麼從使用者接收更複雜的資料，辦法是用 input() 函式：

```python
from microbit import display

data = input('Enter your data: ')
display.scroll(data)
```

上傳程式後切到 REPL 模式, 你會看到程式顯示 ```Enter your data:``` 這句提示並停下來，直到你輸入一些東西和按 Enter。接著，你剛才輸入的文字會顯示在 micro:bit 的 LED 幕上。

## 字串與數值：資料型別的轉換

input() 函式傳回的資料一定是字串。那麼，如果我們想輸入數字，比如連續輸入兩個數字和顯示相加結果呢？但前面也看到了，如果把字串「相加」，它們只會連在一起而已。

這時有些 Python 函式就派得上用場，它們可以把其他類型的資料轉換成特定類型的資料：

Python 函式 | 功能
--- | ---
int() | 把資料轉換成整數
float() | 把資料轉換成浮點數
str() | 把資料轉換成字串

以 ```int()``` 來說，如果字串本身完全是由整數組成，你就可以用它將字串變成整數：

```python
from microbit import display

num1 = input('Enter number 1: ')
num2 = input('Enter number 2: ')
result = int(num1) + int(num2)  # 把字串轉成整數後相加

print('Result:', result)
display.scroll(result)
```

> print() 函式可以連續輸出多個值，而且可以混和不同型別的值，各個值得用逗號分開；而它們在輸出時，彼此之間會空一格。相對的 input() 的提示文字後面不會留空格，所以要自己來。

執行結果如下（同時結果也會顯示在 LED 幕上）：

```
Enter number 1: 42
Enter number 2: 101
Result: 143
```

### 型別轉換錯誤

現在問題來了：如果使用者在程式要求你輸入數字時，故意輸入非數字，會發生什麼事呢？

```
Enter number 1: 1
Enter number 2: a
Traceback (most recent call last):
  File "<stdin>", line 5, in <module>
ValueError: invalid syntax for integer with base 10
```

這裡發生了個錯誤叫 ValueError（值錯誤），發生在程式的第五行，說你不能（對 ```int()```）使用非十進位數字（比如這裡我們輸入的字母 a）。簡單地說，Python 嘗試把 a 轉成整數，當然就失敗了。

> 在 Python 中，錯誤的類型稱為例外（exception），而程式遇到錯誤時會「引發」（raise）一個例外。在更進階的 Python 程式設計中，你甚至可以定義自己的例外類型。

之後我們會再談到，怎麼預防或是事後攔截這種有可能在執行期間時發生的錯誤（雖然說解決錯誤的手法，通常取決於錯誤本身是什麼）。這裡只是先讓你知道，型別轉換不一定會成功。

### 更多型別轉換的效果

```str()``` 能把整數和浮點數轉成字串，這是不會產生問題的。

```float()``` 能把字串轉成浮點數（前提當然是可以轉換），整數也會轉成浮點數。

如果用 ```int()``` 把浮點數轉成整數呢？在 REPL 模式下試試看：

```
>>> int(3.14)
3
```

可以發現浮點數的小數點被截掉了。乍看之下這似乎也是一種錯誤，但有時我們反而可以特意用這個方法去掉小數位，比如若做四捨五入（之後會提）有可能會讓整數超過我們所需的範圍的時候。

## 蜂鳴器與整數頻率

我們來看一些例子，示範為什麼你需要留意資料型別的轉換。

先來看以下程式：

```python
from microbit import sleep
import music, time

music.pitch(440)  # 播放頻率 440 Hz (中音 A)
sleep(500)
music.stop()  # 停止播放
```

這裡我們用了 ```music``` 模組，顧名思義是用來放音樂的。這會使 micro:bit 用板子背面的蜂鳴器播放中音 A，持續 500 毫秒，然後停止。（如果你使用 micro:bit 第一代，你可以買一個無源蜂鳴器，並用鱷魚夾接到 micro:bit 底下的金手指──正極接 P0，負極接 GND。）

> 你可以在[維基百科](https://zh.wikipedia.org/wiki/%E9%8B%BC%E7%90%B4%E9%8D%B5%E9%A0%BB%E7%8E%87)找到鋼琴 88 個鍵的音和對應的頻率。不過得注意，music.pitch() 只能接收整數，所以像是中音 C（261.626 Hz）得四捨五入為 262。

現在我們希望讓使用者自己來輸入頻率，然後讓蜂鳴器放出來：

```python
from microbit import sleep
import music, time

while True:
    note = int(input('Enter frequency: '))
    music.pitch(note)
    sleep(500)
    music.stop()
```

既然 input() 傳回的是字串，得先用 int() 轉成整數才行。當然，這樣是在假設使用者不會（故意或不慎）輸入無法轉換的資料。你可以試試看輸入一些整數頻率。

或者我們可以先輸入浮點數，然後讓 Python 把它轉成整數：

```python
from microbit import sleep
import music, time

while True:
    note = float(input('Enter frequency: '))
    music.pitch(int(note))
    sleep(500)
    music.stop()
```

現在問題來了，你要怎麼避免使用者輸入錯誤的資料（比如一般的文字）而害程式當掉呢？

### try...except 錯誤攔截

如果我們知道某段程式碼有可能在執行時出錯，我們可以把它放在 **try** 敘述的區塊內：

```python
from microbit import sleep
import music, time

while True:
    try:
        note = float(input('Enter frequency: '))
        music.pitch(int(note))
        sleep(500)
        music.stop()
    except:
        print('Error: invalid frequency')
```

如果 try 區塊內的程式碼產生錯誤，它就會被「攔截」下來。而這也是為何 try 後面需要寫第二個區塊，代表出錯後你要怎麼處置。

**except** 敘述代表「出錯後要執行的區塊」，通常是用來警告使用者發生了什麼事。如果你不打算做任何事，except 還是得寫，我們可以在裡面只寫一行 ```pass``` 代表不做事情：

```python
from microbit import sleep
import music, time

while True:
    try:
        note = float(input('Enter frequency: '))
        music.pitch(int(note))
        sleep(500)
        music.stop()
    except:
        pass
```

try 可以搭配的另一個敘述是 **finally**。不管 try 區塊有沒有發生錯誤，而 finally 敘述有寫的話，最後一定會執行 finally：

```python
from microbit import sleep
import music, time

while True:
    try:
        note = float(input('Enter frequency: '))
        music.pitch(int(note))
        sleep(500)
    except:
        print('Error: invalid frequency')
    finally:
        music.stop()  # 不管有沒有錯誤，都確保蜂鳴器關閉
```

只寫 try...finally 也是可以的，雖然大部分時候我們只會用到 try...except。

### 用 round() 四捨五入

下面來看一個稍微變化的版本。在鋼琴上有 88 個鍵，其中中音 A（440 Hz）是第 49 個音。很有趣的是，有一條公式可以用音符的位置編號換算出它們的對應頻率：

```
頻率 = 440 * 2 ** ((音符編號 - 49) / 12)
```

這時換算出來的頻率，就要用四捨五入轉成整數了，這樣才能盡可能接近原始的頻率（即使人耳可能聽不出差異），而不是用 int() 直接丟掉小數點：

```python
from microbit import sleep
import music, time

while True:
    try:
        n = int(input('Enter note number: '))
        freq = 440 * 2 ** ((n - 49) / 12)  # 換算頻率
        print('Playing frequency:', freq)
        
        music.pitch(round(freq))
        sleep(500)
        music.stop()
    except Exception as e:
        print('Error :', type(e), e)
```

頻率換算出來是浮點數, 但程式也會印出四捨五入成整數後的結果，讓我們確認轉換效果：

```
Enter note number: 40
Playing frequency: 261.6256 -> 262
Enter note number: 41
Playing frequency: 277.1827 -> 277
Enter note number: 42
Playing frequency: 293.6648 -> 294
Enter note number: 43
Playing frequency: 311.127 -> 311
Enter note number: 44
Playing frequency: 329.6276 -> 330
Enter note number: 45
Playing frequency: 349.2282 -> 349
```

其實 round() 不只是能四捨五入到整數而已。它可以接收第二個參數，代表到捨入到小數哪一位（不寫就是 0，因此等於整數）：

```
>>> round(3.141592654)
3
>>> round(3.141592654, 2)
3.14
>>> round(3.141592654, 4)
3.1416
```
