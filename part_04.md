# Part 04：迴圈與邏輯判斷

[回到目錄](https://github.com/alankrantas/learn-python-on-microbit-guide/blob/main/README.md)

## 迴圈與延遲

### while 迴圈

現在我們讓 micro:bit 在 LED 顯示幕上顯示了文字，但就只會執行這麼一次而已。但是，你自然會希望程式能在開發板上重複執行程式吧，就像編輯器剛打開時出現的範例程式那樣。這時我們需要兩個東西：**迴圈（loop）**和**延遲（delay）**。

迴圈顧名思義，能重複執行程式碼。最簡單的迴圈形式是無窮迴圈，也就是會不停重複的迴圈：

```python
while True:
    # 程式碼
```

這裡定義了一個 **while** 迴圈，稍後我們會講到後面的布林值 True 的用途。

在 Python 語言中，有些程式碼會隸屬於某個功能，稱為一個程式區塊。其他程式語言多半會用大括號 {} 將程式區塊包起來，但 Python 是用**縮排**來識別之，而該功能的結尾會寫冒號（:），代表下面的程式是它的區塊。

> 井字號 # 用來寫程式註解──在 # 後面的任何文字都不會被 Python 直譯器執行。如果你的程式比較複雜，或是要分享給別人看，適當且清楚的註解能讓人更快看懂程式。

現在，我們在 while 迴圈中放兩行程式碼，好讓 micro:bit 上的圖案會閃動：

```python
from microbit import display, Image

while True:
    display.show(Image.HAPPY)  # 顯示圖案
    display.clear()            # 清除畫面
```

> 註解也不一定要對齊，但有時對齊比較好看。

### sleep 延遲

不過，燒錄了上面的程式後，你會發現圖案以非常快的速度閃動，效果反而看不清楚。

有時候，我們並不需要程式用最快的速度執行，而是得配合人類的操作速度。換言之，我們得在顯示和清除圖案之間加入一些時間延遲才行。

microbit 模組的 ```sleep()``` 函式可以用來製造延遲：

```python
from microbit import display, Image, sleep

while True:
    display.show(Image.HAPPY)
    sleep(500)
    display.clear()
    sleep(500)
```

sleep() 接收一個數字，代表要等待的毫秒（millisecond，千分之一秒），所以 500 代表 0.5 秒。執行 sleep() 時，程式不會做其他事情，直到指定的時間結束為止。

這麼一來，顯示跟清除圖案的動作都有半秒的延遲，圖案閃動的效果就更明顯了。

> 如果你用過 MakeCode 的積木程式介面，它的圖形顯示積木本身其實包含有 0.4 秒的延遲，但 MicroPython 的 show() 就沒有。

## Python 基礎：邏輯判斷

在前面的 while 迴圈，後面的 True 其實是它的「判斷條件」，用來決定「要不要（繼續）執行迴圈」。如果你把後面的 True 改成 False 然後重新上傳程式，你會發現程式完全不會執行。事實上，這個判斷條件可以用更複雜的形式來寫，使我們決定 while 迴圈重複執行到什麼時候就該停止。

邏輯判斷是控制程式流程的核心，能讓程式針對不同情況做出不同反應，或者進入不同的執行階段。正因為有邏輯判斷，我們可以讓程式做出更豐富的行為，並配合我們的任何需求。

### 比較運算子

在 Python 中，有幾個敘述會用到判斷條件，而判斷條件是個**條件運算式（conditional expression）**，它和一般的運算式一樣，但傳回的值得是布林值（True/False）。我們先來看條件運算式可以怎麼寫。

比較運算子用來判斷某個變數是否等於（或不等於）某個值，或者比它大或小：

功能 | Python 比較運算子
--- | ---
等於 | ==
不等於 | !=
小於 | <
小於等於 | <=
大於 | >
大於等於 | >=

注意到程式中的等於是雙等號（==），因為單等號是用來指定值給變數。

在 REPL 模式裡試試看：

```
>>> a = 5
>>> b = 3
>>> a == b
False
>>> a != b
True
>>> a < b
False
>>> a > b
True
```

和其他語言相比，Python 的比較運算子還能在同一行串聯使用。只要所有運算子的條件都成立，整個運算式就會得到 True：

```
>>> a, b, c = 5, 3, 4
>>> a > b < c
True
>>> a != b == c - 1
True
```

> 注意到這裡用 = 一次指定三個值給三個變數，相當於你分開寫 ```a = 5```，```b = 3``` 和 ```c = 4```。

### 邏輯運算子

也有的時候，我們不想單純串聯變數，而是根據幾個條件運算式的結果，來決定最終條件是否成立。這讓我們可以在多重條件同時滿足、或者有任一條件滿足的時候，才讓程式做某件事。

功能 | Python 邏輯運算子 | 意義
--- | --- | ---
且 | and | 兩個條件皆為 True 時傳回 True
或 | or | 任一條件為 True 時傳回 True
非 | not | 反轉條件（如 True 變成 False）

```
>>> a, b = 5, 3
>>> a > 4 and b > 2
True
>>> a > 4 and b > 4
False
>>> a > 4 or b > 4
True
>>> a > 4 and not b > 4
True
>>> not (a > 4 and b > 4)
True
```

> 比較運算子和邏輯運算子的優先度比算術運算子更低，所以正常情況下你不太需要用到小括號把運算式括起來。不過，有時我們會為了讓程式更容易閱讀而加入小括號。

### if 敘述

有了條件運算式，要怎麼根據它（們）的值決定做什麼事呢？這時就要用到 if...else 敘述。

if 敘述可以在 REPL 模式輸入，但有點沒那麼方便，所以下面還是切回正常程式畫面來寫：

```python
a = 5

if a > 4:
    print('a is bigger than 4')
```

上面的程式會判斷 a 是否大於 4，是的話就在 REPL 印出一行訊息。（燒錄程式之後，切到 REPL 模式觀看。如果沒看到，按 Ctrl+D 重跑一次程式。）

和 while 迴圈一樣，if 敘述有它自己的程式區塊，所以 if 這行的結尾得加上冒號（這是初學 Python 時非常容易犯的錯之一）。程式區塊必須往內縮排，每一行的縮排得一致。

前面只有在條件運算式成立時才會印出訊息。如果我們也想在 a 不大於 4 時印出另一個訊息呢？

```python
a = 5

if a > 4:
    print('a is bigger than 4')

if not a > 4:
    print('a is not bigger than 4')
```

但你實際上可以用 if...else 來寫，這樣只要做一次判斷就行：

```python
a = 5

if a > 4:
    print('a is bigger than 4')
else:
    print('a is not bigger than 4')
```

如果你想判斷的條件有不只一種，那麼可以在 if 和 else 之間加入 elif（即 else if 的簡寫）：

```python
a = 3

if a > 4:
    print('a is bigger than 4')
elif a > 2:
    print('a is 3 or 4')
else:
    print('a is smaller than 3')
```

如果 if 的條件運算式不成立，那麼就會看下一個 elif 的條件運算式是否成立。elif 可以寫無限多個，只要它放在 if 後面就好。至於 else 則可寫可不寫，但一定會位於整個邏輯判斷的尾端，代表 if 與所有 elif 之條件全都不成立時的狀況。

if...elif...else 是由上往下依序判斷，所以務必注意前面的判斷對後面的影響。以上面的 ```elif a > 2:``` 為例，既然前面已經判斷過 a 是否大於 4 ，不成立就表示 a 一定小於或等於 4。也就是說，第二個判斷式其實就等於 ```elif 2 < a <= 4:```。

## 第一個能對使用者操作做出回應的程式

前面講了這麼多，現在終於可以來看 if 要如何應用在 micro:bit 上了。

microbit 模組下的 button_a 和 button_b 分別代表板子上的 A 與 B 鈕。這兩個物件都擁有函式 ```is_pressed()```，當你按住按鈕時就會傳回 True，反之傳回 False：

```python
from microbit import button_a, sleep

while True:
    print(button_a.is_pressed())
    sleep(100)
```

上面的程式會在 REPL 介面輸出 A 鈕 is_pressed() 的傳回值，有按下時就變成 True。留意這裡也加了 100 毫秒的延遲時間，否則輸出的訊息太過密集，REPL 介面會難以應付。

> 小提示：試著親自練習輸入程式，而不是複製貼上。就算你記不住所有語法，這樣也能讓你習慣寫程式的過程，並找出你比較容易犯的錯誤。

接著，我們希望 micro:bit 在使用者按住 A 時短暫顯示一個圖案：

```python
from microbit import display, Image, button_a, sleep

while True:
    if button_a.is_pressed():
        display.show(Image.YES)
        sleep(500)
        display.clear()
```

這支程式會用無窮迴圈來檢查使用者有沒有按下 A 鈕，如果有就顯示一個圖案、等待 500 毫秒（好讓圖案能被看見），然後清空螢幕。

下面是另一種寫法，直接用使用者按鈕與否兩種狀態來決定要顯示還是清空圖案：

```python
from microbit import display, Image, button_a, sleep

while True:
    if button_a.is_pressed():
        display.show(Image.YES)
    else:
        display.clear()
    sleep(100)
```

如果你想判斷更多種操作，例如按 A+B、以及單獨按 A 或 B，可以用 elif 敘述來組合：

```python
from microbit import display, Image, button_a, button_b, sleep

while True:
    if button_a.is_pressed() and button_b.is_pressed():  # 如果按下 A+B
        display.show(Image.HEART)
    elif button_a.is_pressed():  # 如果按下 A
        display.show(Image.YES)
    elif button_b.is_pressed():  # 如果按下 B
        display.show(Image.NO)
    else:
        display.clear()
    sleep(100)
```

注意 A+B 的按下放在最前面判斷，因為就算沒有按 A+B，你也有可能單獨按了其中一種。要是先判斷按下 A 或 B，誤判的機會就很大了，畢竟你不可能那麼剛好在同一時間按下 A+B。

## 再探 while：有條件停止迴圈

### 在某個條件不再成立時停止迴圈

if 並不是唯一會用到條件運算式的敘述，其實 while 迴圈也會。在前面的幾個範例中，```while True``` 的 True 就是它的執行條件；while 每次重複迴圈之前，會檢查條件是否成立，成立時才執行迴圈下的區塊。若條件傳回 False，while 就會停止。

下面來看個例子，我們想讓等使用者按 10 下 A 鈕，然後重複顯示一個結束訊息：

```python
from microbit import display, button_a, sleep

count = 0

while count < 10:
    display.show(count)
    if button_a.is_pressed():
        count = count + 1
        sleep(250)

display.scroll('Done!')
```

這個 while 迴圈的條件是 ```count < 10```，而 count 變數一開始是 0。迴圈裡也會把 count 顯示在 LED 顯示幕上。如果使用者按下 A 鈕，count 的值會遞增 1。所以當 count 的值變成 10，while 迴圈就會停了，並執行它後面的 display.scroll()。

> 這裡的 250 毫秒延遲是用來防止使用者按鈕之後，手還沒有離開按鈕，就被迴圈重複偵測到按鈕行為，導致 count 一瞬間就加到 10 了。

就意義上來說，我們是要讓 while 迴圈「在 count >= 10 時停止」。但既然 while 必須在其條件運算式為 True 時才會繼續執行，所以你得讓 while 判斷「停止條件的相反」。不然寫錯的話，迴圈還沒開始就馬上結束了。

> 另外， ```count = count + 1``` 也可以簡寫成 ```count += 1```。這個縮寫法適用於所有的算術算符。

### 暫停程式，等到條件成立時才繼續

while 迴圈的另一個用處是可以拿來等待某件事發生：

```python
from microbit import display, button_a, sleep

count = 0

while count < 10:
    display.show(count)
    while not button_a.is_pressed():
        pass
    
    count += 1
    sleep(250)

display.scroll('Done!')
```

在 while 迴圈內的第二個 while，會判斷「使用者是否沒有按 A 鈕」。它底下的 pass 代表不做任何事，寫這個只是因為程式區塊內一定要寫點東西。

也就是說：程式在這裡會進入第二個迴圈，並一直卡在那裡，直到使用者按了 A 鈕為止。這麼做會讓外側的 while 迴圈停止不動，但反過來說，你可以強迫程式在某個時候只等待的事發生，而不是得一直檢查迴圈裡有哪些程式是應該要執行的。

### 用 break 跳出迴圈

另一個結束 while 迴圈的方式是使用 break 敘述。在下面的程式，使用者想按幾下 A 鈕都行，程式不會檢查什麼時候該停止。但若使用者按下 B 鈕，迴圈就會結束：

```python
from microbit import display, button_a, button_b, sleep

count = 0

while True:
    display.show(count)
    
    if button_a.is_pressed():
        count += 1
        sleep(250)
    
    if button_b.is_pressed():
        break

display.scroll('Done!')
```

什麼時候該用 break 見仁見智，不過通常是用 while 本身的條件不太方便的時候，比如迴圈只有極少數情況需要中止，或者你不想讓迴圈繼續執行一些程式、回到開頭才判斷是否繼續。

此外，break 只會跳出它所在的 while 迴圈。如果這迴圈外面還有一層迴圈，那麼外層迴圈是不會中斷的。

## 補充：更多使用邏輯判斷和迴圈的小應用

### 搖骰子

```python
from microbit import display, accelerometer, sleep
import random

while True:
    if accelerometer.current_gesture() == 'shake':
        display.show(random.randint(1, 6))
        sleep(500)
```

這裡我們使用了 microbit 模組下的 accelerometer 子功能，它掌管板子背面的加速計的相關功能。```current_gesture()``` 會傳回一些字串，代表板子現在的角度或移動方式是否符合某個事先定義好的動作。在此例我們要判別的是 'shake'（晃動）。後面有機會的話會再講到其他的動作。

如果使用者搖晃 micro:bit，它會用 random 模組隨機產生一個介於 1 到 6 的數字，並顯示在 LED 顯示幕上。random 模組的 ```randint(a, b)``` 會傳回 a 到 b 之間的一個整數。

> 要注意的是，電腦亂數其實是用演算法產生的，還是有某種模式可循，所以稱為偽隨機數（pseudorandom number）。

最後，為了避免 while 迴圈太快就再次判讀 micro:bit 的當前動作，我們加入 500 毫秒的延遲。

### 哪邊朝上？

```python
from microbit import display, Image, accelerometer
import random

while True:
    gesture = accelerometer.current_gesture()
    if gesture == 'face up':
        display.show(Image.YES)
    elif gesture == 'face down':
        display.show(Image.NO)
    elif gesture == 'up':
        display.show(Image.ARROW_N)
    elif gesture == 'down':
        display.show(Image.ARROW_S)
    elif gesture == 'left':
        display.show(Image.ARROW_E)
    elif gesture == 'right':
        display.show(Image.ARROW_W)
```

這回我們使用了 if 和多重 elif 來判斷 micro:bit 的多種動作。動作 'up' 的意思是 micro:bit 的上緣高於下緣，相對的 'face up' 是指 LED 幕水平面向上方。所以如果 micro:bit 歪向某一邊，它會顯示對應的箭頭指向「上方」，而真正朝上時會顯示打勾。

這次我們不需要加入時間延遲，因為板子轉動到某個位置後，短時間內是不太會有什麼改變的。事實上你反而可能會希望程式能及時判讀 micro:bit 的當前動作。

### 音量警告計

這個範例只適用於 micro:bit 二代，因為會用到麥克風和蜂鳴器。麥克風啟用時，正面的麥克風小圖案會點亮。

```python
from microbit import display, Image, microphone, sleep
import music

while True:
    if microphone.sound_level() >= 16:
        music.pitch(440)
        sleep(100)
        music.stop()
```

```microphone.sound_level()``` 會傳回麥克風的相對音量（整數 0~255）。這裡我們設了個門檻，音量大於門檻時會用蜂鳴器播放一個聲音，持續半秒鐘。如果覺得麥克風太靈敏或太不靈敏，調整那個數字即可。

```music.pitch()``` 會播放一個特定頻率的聲音，```music.stop()``` 則是停止聲音。後面我們會再看到更詳細的使用方式。
