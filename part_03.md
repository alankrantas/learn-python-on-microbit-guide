# Part 3：初探函式、模組與 micro:bit 控制

[回到目錄](https://github.com/alankrantas/learn-python-on-microbit-guide/blob/main/README.md)

## Python 基礎：內建函式

函式（function）是一個功能，就像數學函數一樣可以輸入資料給它，而函式會拿資料來做某件事。有的函式也會傳回處理完的結果。後面我們會再看到 Python 函式是怎麼定義的，但 Python 本身也提供了一系列內建函式，你不需要做任何事就能直接使用它們。

最常用的內建函式之一是 ```print()```，可以用來把資料輸出到 REPL 介面：

```
>>> print(42)
42
>>> print("Test")
'Test'
>>> print(2 ** 10)
1024
```

要是 REPL 可以直接印出資料、變數和運算式，幹嘛多此一舉呢？這是因為在正式寫程式的時候，你不會寫在 REPL 裡面，而是直接讓 REPL 執行一個完整的程式或草稿碼，而這時你若想輸出一些資料的話，就必須用 print() 函式。

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
