# 用 BBC micro:bit 學 Python：免費、免安裝的完全入門指南

作：Alan Wang, Feb 2021

本指南採用 [GNU 3.0 通用公共授權](https://www.chinasona.org/gnu/gnulgpl-v3-tc.html)，注意這意味著衍生作品必須同樣採用 GNU 授權。

![01](https://user-images.githubusercontent.com/44191076/107118869-ab2f2280-68be-11eb-9a6d-87b02939a7e2.png)

## 關於本指南

這份指南不是替兒童而寫，而是有興趣使用 BBC micro:bit 開發板為媒介來學習 Python 語言的成人或青少年。

現有的書籍或網路教材，在談及 micro:bit 的 Python 教學時，多半著重在 micro:bit 的功能和相關的範例，卻鮮少深入 Python 的語言特色。這份指南的目的是借用 micro:bit 的硬體特色來介紹 Python 的重要概念，好讓學習者將來能將之應用在正式的 Python 程式開發。

至於 micro:bit 是什麼、為什麼現在要學 Python，這些在網路上已經有非常多討論，這裡就不再贅述。

## 閱讀本指南所需的準備

本指南會使用 2020 年 10 月後上市的新版 [micro:bit V2](https://microbit.org/zh-tw/new-microbit/)，但大部分程式碼也適用於 V1。筆者推薦使用 V2，因為價格相近但規格更佳，有更多記憶體執行 Python 直譯器。

你也需要 micro USB 線以及一台有 Google Chrome 瀏覽器的電腦，就這樣。不須安裝其他東西。

### Python 編輯器

打開 https://python.microbit.org/v/2 ：

![02](https://user-images.githubusercontent.com/44191076/107118877-baae6b80-68be-11eb-8bda-aa8013b3698f.png)

這是 micro:bit 官方提供的線上版瀏覽器，雖然簡單，但就本指南的目的來說夠用了。後面有機會的話，筆者會在談如何使用其他 Python 編輯器。

畫面上方有五個大按鈕：

* Download/Flash (下載/燒錄)
* Connect/Disconnect (連線/解除連線)
* Load/Save (上載檔案/存檔)
* Open Serial/Close Serial (打開 REPL/關閉 REPL)
* Help (線上說明)

按鈕的內容會因與 micro:bit 連線與否而有所不同。

### 與 micro:bit 連線和燒錄第一支程式／韌體

Python 是直譯式語言，這表示程式運作的地方必須裝有直譯器。幸好，官方編輯器簡化了這個流程：你從編輯器下載的 .hex 程式檔中，有一部分會包著 micro:bit 用的 Python 韌體，剩下的則是你寫的程式（又稱草稿碼）。即使你的 micro:bit 之前並沒有安裝 Python 韌體，只要燒錄一次程式就會自動完成韌體安裝。這也使得第一次燒錄──或者在使用過其他語言後重新燒錄──Python 會花較久的時間。

> micro:bit 使用的 Python 實際上是 Micropython──設計給記憶體有限的開發版的 Python 版本。Micropython 本身就有好幾種版本，針對不同類型的開發板而作，不過它們都是是以 Python 3.4 為基礎。所以，Python 3.4 擁有的核心語言功能，在 micro:bit 都是可以使用的。

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
MicroPython v1.13 on 2020-12-21; micro:bit v2.0.0-beta.3 with nRF52833
Type "help()" for more information.
>>> 
```

就是 REPL 的提示，類似 Windows 命令提示字元或 Unix 的終端機。它告訴我們 Python 直譯器的版本，並等待我們輸入指令。

現在於 >>> 後面輸入 ```1 + 2```，並按 Enter：

```
MicroPython v1.13 on 2020-12-21; micro:bit v2.0.0-beta.3 with nRF52833
Type "help()" for more information.
>>> 1 + 2
3
```

可以發現 Python 直譯器解讀了你輸入的程式，並自動算出答案。

## Python 基礎：運算式

在 Python 中，程式碼可分為兩類：**陳述（statement）**與**運算式（expression）**。這兩者都會執行某個功能，但運算式本身也代表某個值，比如前面的 ```1 + 2``` 會得到 ```3```。

運算式可以放在其他運算式中，而 Python 有很多會傳回值的東西都可以當成運算式的一部分。最簡單的運算式由值和**運算子（operator）**構成，例如上面的算式有數字 1 和 2，中間是加號運算子（+）。

我們先來看 Python 常用的運算子：

功能 | Python 算術運算子
--- | ---
加 | +
減 | -
乘 | *
除 | /
整數除 | //
餘數除 | %
次方 | **
小括號 | ()

在 REPL 中試試看：

```
>>> 2 + 3
5
>>> 2 - 3
-1
>>> 2 * 3
6
>>> 10 / 3
3.333333
>>> 10 // 3
3
>>> 10 % 3
1
>>> 2 ** 3
8
>>> 10 / (2 + 3)
2.0
```

和算數學一樣，乘除的優先度比加減高，所以得在適當的時候用小括號提高加減的優先度。

> 數字與運算子之間一定要有空格嗎？不用，甚至打一堆空格都無所謂，但留一個空格是最美觀的。

## Python 基礎：資料型別

前面的運算式，其實只使用到數字而已。而在 Python 中，不同的資料有不同的**型別（type）**，而型別決定了資料（以及你對資料）能做哪些事。

Python 最基本的幾種型別為：

Python 型別 | 範例
int (整數) | 42
float (浮點數) | 3.14159
str (字串) | "test"
bool (布林值) | True/False
NoneType (無值) | None

### 整數與浮點數

以上面的運算子來說，整數（無小數點）和浮點數（有小數點）是可以放在一起運算的，而取決於運算子，結果會是整數或浮點數。例如，整數用 / 相除會得到浮點數, 用 // 相除卻會得到整數。

如果用 // 時算式裡有浮點數呢？這樣就還是會得到浮點數：

```
>>> 10 // 3.0
3.0
```

### 字串

字串前後必須用雙引號 (") 或單引號 (') 括起來：

```
>>> "This is a string"
'This is a string'
```

Python 預設是用單引號，不過大多時候沒有差別，取決於個人習慣。單引號看起來比較簡潔，但雙引號在多數語言是表示字串的通用用法。其實 Python 還有幾種其他代表字串的變形形式，不過一開始不需要知道。

當然，字串內容可以打任何語言。不過 micro:bit 的網頁編輯器打中文有點容易亂跳，所以還是打英文吧。

字串和整數、浮點數最大的差別，在於它們不能放在一起運算：

```
>>> 1 + 1
2
>>> "1" + "1"
'11'
>>> 1 + "1"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported types for __add__: 'int', 'str'
>>> "1" + 1
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can't convert 'int' object to str implicitly
```

當你把數字寫在用 " 或 ' 括起來的字串中，那些數字就是字串而不是數字了。當你對兩個字串使用 + 號，這些字串只會直接連在一起。

這顯示了 + 運算子對數值和字串的意義是不一樣的，所以你嘗試執行 ```1 + "1"``` 或 ```"1" + 1``` 就產生錯誤了。之後我們會再談到怎麼藉由轉換資料型別的方式，好讓它們可以一起運算。

> 執行遇到問題時，Python 直譯器會顯示錯誤訊息，告訴你錯誤發生的行數（由於我們在使用 REPL 介面，所以位置是第 1 行）以及原因（TypeError，型別錯誤）。這時我們就可以根據這些訊息來嘗試排解錯誤（除錯）。

### 布林值

布林值（Boolean）就是「是／否」或「真／假」的二元結果，在 Python 中這兩個值分別是 True 和 False（第一個字一定大寫），主要用在邏輯判斷用途，這些後面會再探討。

Python 的布林值同時也是一種特殊的數值，0 和 None 就等於 False，除此以外的其他任何數字則等於 True。

### None

None 是個特殊的值，代表沒有值的意思。你用到它的機會非常少，但有些 Python 功能會把它當成沒有值時的預設值。

## Python 基礎概念：變數

### 宣告變數

**變數（variable）**是用來在程式中記住資料的方法，就像是解數學方程式時的 X 或 Y。變數會有一個值，而這個值是可以隨意改變的。

在 Python 中，若要宣告（define）一個新變數，辦法是用 =（指派運算子）號指定或賦予一個值給某個名稱：

```
>>> a = 1
```

此舉建立了名為 a 的變數，值為整數 1。

```
>>> a
1
>>> a + 3
4
```

如果用 = 指定新的值給 a，它的值會隨之改變：

```
>>> a = 2
>>> a
2
>>> a * 3
6
```

你甚至可以這樣寫：

```
>>> a = 1
>>> a
1
>>> a = a + 4
>>> a
5
```

請記住 = 號不是數學上的等於，而是**將其右邊的值指定給左邊**。一開始 a 的值為 1，然後我們指定新的值 a + 4（也就是 5） 給 a。因此 = 完成運算後，a 就變成 5 了。

最後，來按編輯器的 **Close serial** 關閉 REPL 介面，然後重開（記得按 Ctrl+C），並再次輸入 ```a``` 查詢其值：

```
>>> a
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'a' isn't defined
```

現在產生了錯誤，說名稱 a 未定義。這是因為重開 REPL 後，變數 a 就在記憶體裡消失了。

後面我們在執行完整的程式時，每次重新執行的效果就等同於重新啟動 REPL，所以你得在程式裡適當的地方宣告變數，後面的程式使用它時才不會遇到問題。

### Python 變數是個「路牌」

現在要來講一個比較難懂、但仍然非常重要的觀念，因為這能解釋 Python 變數在賦值時的獨特特性。

在許多程式語言中，變數是個容器，實際上就是記憶體裡的一個空間，然後把值存在那裡面。既然是事先指定的空間，你只能用它來儲存特定類型的資料。但 Python 的做法卻剛好相反：值本身已經存在於記憶體中某處了，你所做的其實是做一個「路牌」來指向它。

以 ```a = 1``` 為例，整數 1 其實已經存在於記憶體內，我們只是新增一個名稱 a 來代表它：

```
a ---> 1
```

而當你執行 ```a = 2``` 時，路牌 a 會指向記憶體中的整數 2。這時整數 1 仍然存在，只是除了直接寫出 1 以外，我們沒有別的方式能引用 1。

```
       1
a ---> 2
```

> 整數 1 和 2 等等是特例，它們是 Python 一開始就擁有的資料。有些資料則是由使用者在執行程式期間建立的。

換言之，變數可以任意指向任何資料，就算途中改變資料的類型也無謂：

```
>>> a = 1
>>> a
1
>>> a = "test"
>>> a
'test'
```

上面 a 一開始是整數，後來變成字串了。要注意的是，在同一支程式裡太任意更改變數指向的資料類型，只會把程式搞得更複雜而已。最好還是把不同功能的變數分開宣告，讀起來也比較容易懂。

### Python 變數的命名規則

Python 變數的命名非常自由，除了不能用數字開頭以外，可以隨意混搭字母、數字和底線：

```
>>> _Number_01 = 42
>>> _Number_01
42
```

Python 變數名稱也支援 Unicode，因此用中文、日文或各國語言建立變數名稱都是可行的。使用非 Unicode 特殊字元會產生錯誤。不過，一般習慣上還是會以英數為主，而且最好清楚反映變數本身的用途。

當然還有一件你不該做的事，就是拿 Python 內建功能的名稱來當變數。這會導致那個名稱指向了你給予的資料，結果等於是把原本的功能蓋掉了，說不定還會導致程式錯誤。

## Python 基礎：內建函式

函式（function）是一個功能，就像數學函數一樣可以輸入資料給它，而函式會拿資料來做某件事。有的函式也會傳回處理完的結果。後面我們會再看到 Python 函式是怎麼定義的，但 Python 本身也提供了一系列內建函式，你不需要做任何事就能直接使用它們。

最常用的內建函式之一是 print()，可以用來把資料輸出到 REPL 介面：

```
>>> print(42)
42
>>> print("Test")
'Test'
>>> print(2 ** 10)
1024
```

要是 REPL 可以直接印出資料、變數和運算式，幹嘛多此一舉呢？這是因為在正式寫程式的時候，你不會寫在 REPL 裡面，而是直接讓 REPL 執行一個，而這時你若想輸出一些資料的話，就必須用 print() 函式。

函式跟變數一樣有個名稱，這個名稱（路牌）會指向真正的函式：

```
>>> print
<function>
```

REPL 告訴我們 print 是個函式。若要使用（也就是呼叫）函式，得在這個名稱後面加上小括號，和寫數學函數很像。

如前面所提，如果你用 = 指派一個新的值給 print 這個名稱，原本的函式就找不到了（直到下次重新執行程式、或重開直譯器為止），所以這種行為是能免則免。

相對的，也有些內建函式主要是讓你在 REPL 介面使用的，用來探索 Python 的功能。下面我們會一一講解。 

## 印出文字 - 模組與函式

好了，我們花了點時間講解一些 Python 基礎。現在先來看這能怎麼應用在 micro:bit 的功能吧。

在 REPL 照順序輸入以下兩行指令：

```
>>> import microbit
>>> microbit.display.show(42)
```

執行第二行時，。micro:bit 的 5x5 LED 顯示幕會依序顯示 4 和 2。

接著：

```
>>> s = "Test"
>>> microbit.display.show(s)
```

這回 micro:bit 顯示了變數 s 的內容，也就是 Test。

你也可以直接在 ...show() 裡面寫運算式：

```
>>> microbit.display.show(2 ** 10)
```

但是，這個功能是個函數嗎？第一行的 import... 是在做什麼呢？

### 探索 microbit 模組

模組（module）代表 Python 中一系列特定的功能，歸類在某個名稱底下。在其他語言中，我們會稱模組為函式庫。例如，math 模組含有跟數學計算有關的功能，而 time 模組有些可用於計時和處理時間的功能。

如前面提過的，micro:bit 的 Python（MicroPython）是經過精簡的版本，所以它的模組不會像正規 Python 那麼多和完整，也有些模組是特別針對開發板的硬體功能而設計。不過，使用方式是完全一樣的。

在 micro:bit 上，**microbit** 模組囊括了 micro:bit 絕大部分的硬體功能。現在我們來看看它裡面有什麼東西。

首先要「匯入」microbit 模組：

```
>>> import microbit
```

你可以想像這是在把模組載入到記憶體中，雖然實際上這更像是在建立一個變數名稱 microbit，它會指向一系列功能。當我們使用這個名字時，Python 會去系統中尋找這模組的檔案，並執行當中的程式功能。

匯入模組以後，就可以用 help() 函式來調查它的內容：

```
>>> help(microbit)
object <module 'microbit'> is of type module
  __name__ -- microbit
  Image -- <class 'MicroBitImage'>
  Sound -- <class 'MicroBitSound'>
  SoundEvent -- <class 'MicroBitSoundEvent'>
  display -- <MicroBitDisplay>
  button_a -- <MicroBitButton>
  button_b -- <MicroBitButton>
  accelerometer -- <MicroBitAccelerometer>
  compass -- <MicroBitCompass>
  speaker -- <MicroBitSpeakerPin>
  microphone -- <MicroBitMicrophone>
  audio -- <module 'audio'>
  i2c -- <MicroBitI2C>
  uart -- <MicroBitUART>
  spi -- <MicroBitSPI>
  reset -- <function>
  sleep -- <function>
  running_time -- <function>
  panic -- <function>
  temperature -- <function>
  set_volume -- <function>
  ws2812_write -- <function>
  pin0 -- <MicroBitTouchPin>
  pin1 -- <MicroBitTouchPin>
  pin2 -- <MicroBitTouchPin>
  pin3 -- <MicroBitAnalogDigitalPin>
  pin4 -- <MicroBitAnalogDigitalPin>
  pin5 -- <MicroBitDigitalPin>
  pin6 -- <MicroBitDigitalPin>
  pin7 -- <MicroBitDigitalPin>
  pin8 -- <MicroBitDigitalPin>
  pin9 -- <MicroBitDigitalPin>
  pin10 -- <MicroBitAnalogDigitalPin>
  pin11 -- <MicroBitDigitalPin>
  pin12 -- <MicroBitDigitalPin>
  pin13 -- <MicroBitDigitalPin>
  pin14 -- <MicroBitDigitalPin>
  pin15 -- <MicroBitDigitalPin>
  pin16 -- <MicroBitDigitalPin>
  pin19 -- <MicroBitDigitalPin>
  pin20 -- <MicroBitDigitalPin>
  pin_logo -- <MicroBitTouchOnlyPin>
  pin_speaker -- <MicroBitDigitalPin>
```

另一個類似的方式是使用 dir() 函式：

```
>>> dir(microbit)
['__class__', '__name__', 'Image', 'Sound', 'SoundEvent', 'accelerometer', 'audio', 'button_a', 'button_b', 'compass', 'display', 'i2c', 'microphone', 'panic', 'pin0', 'pin1', 'pin10', 'pin11', 'pin12', 'pin13', 'pin14', 'pin15', 'pin16', 'pin19', 'pin2', 'pin20', 'pin3', 'pin4', 'pin5', 'pin6', 'pin7', 'pin8', 'pin9', 'pin_logo', 'pin_speaker', 'reset', 'running_time', 'set_volume', 'sleep', 'speaker', 'spi', 'temperature', 'uart', 'ws2812_write']
```

如果你想知道一個模組（或者模組內的功能）底下有什麼東西，又覺得花時間查閱線上文件麻煩的話，可以很快用 help() 或 dir() 查一下。

> micro:bit 的[MicroPython 線上文件](https://microbit-micropython.readthedocs.io/en/latest/index.html)是另一個好用的工具，雖然它對程式入門者來說會比較難讀。

現在，micro:bit LED 顯示幕的相關功能都放在 microbit 模組的 display 項目下。我們可以查查看它是什麼：

```
>>> help(microbit.display)
object <MicroBitDisplay> is of type MicroBitDisplay
  get_pixel -- <function>
  set_pixel -- <function>
  show -- <function>
  scroll -- <function>
  clear -- <function>
  on -- <function>
  off -- <function>
  is_on -- <function>
  read_light_level -- <function>
```

microbit.display（用句號連接）是個物件，它本身也有一些功能，包括 show() 和其他幾個函式。之後我們會再談物件是什麼。

因此，若你想使用上面的 show() 函式來顯示資料，你得連同模組跟物件的名稱來呼叫它：

```
>>> microbit.display.show("Hello World!")
```

> micro:bit 的 LED 顯示幕只能顯示英文、數字和一些標點符號，不支援中文或全形符號等等。

你甚至可試著呼叫其他函式：

```
>>> microbit.display.scroll("Hello World!")
```

你應該會發現，scroll() 正如其名，能在 LED 幕捲動顯示文字，使之變成跑馬燈。

### 用草稿碼的方式執行程式

現在，我們要把上面的幾行程式合併起來，讓 micro:bit 能一口氣全部執行。

關閉 REPL 介面、切回寫程式的介面，清掉原本的程式和換成以下內容：

```python
import microbit

microbit.display.scroll("Hello World!")
```

![2](https://user-images.githubusercontent.com/44191076/107321984-89ac8180-6ade-11eb-9981-e40dcf2937bb.png)

確定編輯器已與 micro:bit 連線，然後按 **Flash** 上傳程式。

跑馬燈跑完後，你可以按 micro:bit 背面的 reset（重置）鈕來重新啟動板子。你會發現同一支程式又跑了一次，不再需要我們手動輸入。這是因為現在這幾行程式已經以檔案形式存在 micro:bit 上，它每次開機時 MicroPython 直譯器就會自動執行之。

### 匯入模組的其他方式

你可能會覺得，為了使用模組的某個功能，就得寫一長串名稱，未免太麻煩了。

這時，你可以選擇只「匯入」模組底下的某樣東西：

```python
from microbit import display

display.scroll("Hello World!")
```

從字面上，就是「從 microbit 模組匯入 display」的意思。這麼一來，你就能在程式中直接使用 display 的功能，而不需要再在前面加上 microbit 了。只是要注意，現在程式中就不會匯入 microbit 這個名稱了。

那麼，是不是可以進一步寫成  ```from microbit.display import scroll``` 或其他寫法呢？可惜沒辦法，因為 display 是個物件而不是模組，它不支援這種匯入語法。但在正規 Python 的許多第三方套件中，常會有分割成很多子套件，就可以這樣使用。

還有一種方式，是把 microbit 模組底下的所有東西都一概匯入，這也是網路上 micro:bit 教材常見的寫法：模組下的所有功能：

```python
from microbit import *
```

這樣在 micro:bit 上幾乎不太可能遇到問題，但在正規的 Python 程式設計中，反而不見得是好主意，因為你有可能無意間加入了太多功能的名稱（Python 並不是真的把模組讀進記憶體，畢竟它們已經存在了；實際上更像是建立一個路標指向它），而有兩個模組的子功能剛好撞名，因此有一邊會蓋掉另一邊。所以在本教材中，我們還是會很明確的寫出我們要匯入什麼。

### 結合模組的不同功能：顯示圖案

現在我們來結合 microbit 模組下的多個功能，好在 LED 顯示幕上秀一些事先定義好的圖案。

回到 REPL 模式，來調查 microbit.Image 下的內容：

```
>>> import microbit
>>> dir(microbit.Image)
['__class__', '__name__', 'copy', '__bases__', '__dict__', 'ALL_ARROWS', 'ALL_CLOCKS', 'ANGRY', 'ARROW_E', 'ARROW_N', 'ARROW_NE', 'ARROW_NW', 'ARROW_S', 'ARROW_SE', 'ARROW_SW', 'ARROW_W', 'ASLEEP', 'BUTTERFLY', 'CHESSBOARD', 'CLOCK1', 'CLOCK10', 'CLOCK11', 'CLOCK12', 'CLOCK2', 'CLOCK3', 'CLOCK4', 'CLOCK5', 'CLOCK6', 'CLOCK7', 'CLOCK8', 'CLOCK9', 'CONFUSED', 'COW', 'DIAMOND', 'DIAMOND_SMALL', 'DUCK', 'FABULOUS', 'GHOST', 'GIRAFFE', 'HAPPY', 'HEART', 'HEART_SMALL', 'HOUSE', 'MEH', 'MUSIC_CROTCHET', 'MUSIC_QUAVER', 'MUSIC_QUAVERS', 'NO', 'PACMAN', 'PITCHFORK', 'RABBIT', 'ROLLERSKATE', 'SAD', 'SILLY', 'SKULL', 'SMILE', 'SNAKE', 'SQUARE', 'SQUARE_SMALL', 'STICKFIGURE', 'SURPRISED', 'SWORD', 'TARGET', 'TORTOISE', 'TRIANGLE', 'TRIANGLE_LEFT', 'TSHIRT', 'UMBRELLA', 'XMAS', 'YES', 'blit', 'crop', 'fill', 'get_pixel', 'height', 'invert', 'set_pixel', 'shift_down', 'shift_left', 'shift_right', 'shift_up', 'width']
>>> 
```

Image 包含一系列圖形，你可以把這些東西傳給 display.show()。

現在回到程式撰寫畫面：

```python
from microbit import display, Image

display.show(Image.HAPPY)
```

這裡從 microbit 模組匯入了 display 和 Image 名稱（兩者用逗號分隔），然後在 LED 顯示幕顯示笑臉。我們之後會再講怎麼「畫出」你自己的圖案。

![04](https://user-images.githubusercontent.com/44191076/107517822-3541f880-6be9-11eb-9022-b64821444012.png)

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

microbit 模組的 sleep() 函式可以用來製造延遲：

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

前面只有在條件判斷式成立時才會印出訊息。如果我們也想在 a 不大於 4 時印出另一個訊息呢？

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

如果 if 的條件判斷式不成立，那麼就會看下一個 elif 的條件判斷式是否成立。elif 可以寫無限多個，只要它放在 if 後面就好。至於 else 則可寫可不寫，但一定會位於整個邏輯判斷的尾端，代表 if 與所有 elif 之條件全都不成立時的狀況。

if...elif...else 是由上往下依序判斷，所以務必注意前面的判斷對後面的影響。以上面的 ```elif a > 2:``` 為例，既然前面已經判斷過 a 是否大於 4 ，不成立就表示 a 一定小於或等於 4。也就是說，第二個判斷式其實就等於 ```elif 2 < a <= 4:```。

## 第一個能對使用者操作做出回應的程式

前面講了這麼多，現在終於可以來看 if 要如何應用在 micro:bit 上了。

microbit 模組下的 button_a 和 button_b 分別代表板子上的 A 與 B 鈕。這兩個物件都擁有函式 is_pressed()，當你按住按鈕時就會傳回 True，反之傳回 False：

```python
from microbit import button_a, sleep

while True:
    print(button_a.is_pressed())
    sleep(100)
```

上面的程式會在 REPL 介面輸出 A 鈕 is_pressed() 的傳回值，有按下時就變成 True。留意這裡也加了 100 毫秒的延遲時間，否則輸出的訊息太過密集，REPL 介面會難以應付。

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

(持續寫作中...)
