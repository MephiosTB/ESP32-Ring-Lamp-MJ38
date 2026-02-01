English

This is my first post on GitHub. I'm from Russia, but I'll be writing mostly in English (using my own knowledge and a translator).<br>

After using this Chinese lamp (model MJ36) for a long time, it stopped turning on.<br>
The lamp itself consists of a ring strip containing lines of warm LEDs, cold LEDs, and RGB LEDs, and a control panel with a microcontroller.
<br>
<img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/3f29dcd1-56c2-4f9c-a8ea-782caf9c4ee7" />
<img width="77" height="200" alt="image" src="https://github.com/user-attachments/assets/706a16b9-c41d-4ab9-9426-250065e24c2c" />
<br>
After troubleshooting and inspecting the components, I discovered a faulty RGB strip and missing markings on the microcontroller.<br>

I didn't bother to investigate the cause of the RGB strip's failure (it had never been used) or find out what microcontroller it uses. <br>
I simply desoldered the microcontroller and tested the contact lines, matching their numbers with the markings on the board:

| # | Name | # | Name |
|---|---|---|---|
| 1 | RGB | 20 | A |
| 2 | LED | 19 | SW4 "Down" |
| 3 | SW1 "Mode" | 18 |  |
| 4 | +5V | 17 | GND |
| 5 |  | 16 |  |
| 6 |  | 15 |  |
| 7 |  | 14 |  |
| 8 | SW5 "Up" | 13 |  |
| 9 |  | 12 | B |
| 10 | SW2 "RGB" | 11 | SW3 "Power" |

I won't draw the board diagram - it's extremely simple:
1. The "+5V" power supply has a capacitor for filtering and a 5 ohm resistor for limiting the supply voltage;
2. The "LED" indicator is connected to "GND" through a 1 kOhm resistor;
3. All the buttons are connected to "GND";
4. Lines "A" (warm light) and "B" (cold light) are controlled via transistors, also routed to GND, and there are 1 kOhm resistors between the signal line and GND.

I had an ESP32-S3 Super Mini microcontroller (https://www.espboards.dev/esp32/esp32-s3-super-mini/) as a backup.<br>
It's overkill for this project, but I didn't have anything else on hand.<br>
Furthermore, the lamp can be integrated into a smart home system.<br>

I chose pins that didn't interfere with internal components and had holes.<br>
The only exception is pin 9; it's used to connect external flash memory, which I don't have, so it's safe to use.<br>
I also configured the microcontroller for a minimum processor speed of 80 MHz (instead of the standard 240 MHz).<br>

<img width="500" height="331" alt="image" src="https://github.com/user-attachments/assets/3c5f1764-418b-416c-b83b-7b7913bd6b30" />

The resulting connection table is as follows:

| # | Name | Pin | # | Name | Pin |
|---|---|---|---|---|---|
| 1 | RGB | GPIO15 | 20 | A | GPIO01 |
| 2 | LED | GPIO09 | 19 | SW4 "Down" | GPIO07 |
| 3 | SW1 "Mode" | GPIO04 | 18 |  |  |
| 4 | +5V | +5V | 17 | GND | GND |
| 5 |  |  | 16 |  |  |
| 6 |  |  | 15 |  |  |
| 7 |  |  | 14 |  |  |
| 8 | SW5 "Up" | GPIO08 | 13 |  |  |
| 9 |  |  | 12 | B | GPIO02 |
| 10 | SW2 "RGB" | GPIO05 | 11 | SW3 "Power" | GPIO06 |

I didn't use the "RGB" pin (but intended to connect it to pin 15), since I don't need RGB LEDs.<br>
I programmed the SW2 button, which was responsible for switching RGB modes, to cycle between warm and cold with additional transition steps.<br>

I didn't take any final photos – they look terrible. I installed the esp32 in an existing case with a pushbutton, modifying it with improvised means (in honor of blue electrical tape).<br>

I programmed the microcontroller itself using ESPHome in Home Assistant.<br>
The program code is described in the file "ring_light.yaml".<br>
I'm not very good with yaml, so the code is assembled from "ready-made" elements + neural networks, and may not be optimal.<br>

<hr>
<hr>

Русский

Это моя первая публиция на GitHub. Я из России, но в основном буду писать на английском (используя собственные знания и переводчик).<br>

После долгого времени использования этой китайской лампы (модель MJ36) - она перестала включаться.<br>
Сама лампа состоит из кольцевой ленты, на которой расположены линии теплых свотодиодов, холодных светодиодов и RGB свотодиоды, и пульт управления с мк.<br>
<br>
<img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/3f29dcd1-56c2-4f9c-a8ea-782caf9c4ee7" />
<img width="77" height="200" alt="image" src="https://github.com/user-attachments/assets/706a16b9-c41d-4ab9-9426-250065e24c2c" />
<br>
После диагностики и осмотра компонентов была выявлена неисправность RGB ленты и отсутствующая маркировка микроконтроллера.<br>

Я не стал разбираться в причинах неработоспособности RGB ленты (она никогда не использовалась), и выяснять что за мк используется.<br>
Я просто выпаял мк и прозвонил линии контактов, сопоставив их номера с маркировкой на плате:

| # | Name | # | Name |
|---|---|---|---|
| 1 | RGB | 20 | A |
| 2 | LED | 19 | SW4 "Down" |
| 3 | SW1 "Mode" | 18 |  |
| 4 | +5V | 17 | GND |
| 5 |  | 16 |  |
| 6 |  | 15 |  |
| 7 |  | 14 |  |
| 8 | SW5 "Up" | 13 |  |
| 9 |  | 12 | B |
| 10 | SW2 "RGB" | 11 | SW3 "Power" |

Схему платы отрисовывать не буду - она крайне простая:
1. Питание "+5V" имеет конденсатов для фильтрации, и резистор в 5 Ом для ограничения птания;
2. Индикатор "LED" поброшен на "GND" через резистор на 1 кОм;
3. Все кнопки просто идут на "GND";
4. Линии "A" (теплые свет) и "B" (холодный свет) управляются через транзиторы, также проброшенные на "GND", и стоят резисторы на 1 кОм между сигнальной линией и "GND".<br>

В резерве у меня был мк ESP32-S3 Super Mini (https://www.espboards.dev/esp32/esp32-s3-super-mini/).<br>
Он избыточен для данного проекта, но другого под рукой не было.<br>
Заодно, лампу можно будет интегрировать в систему умного дома.<br>

Пины выбирал без пересечения с внутренними компонентами,  и имеющие отверстие.<br>
Исключение только в 9 пине, он использется для подключения внешней flash памяти, которой нет, по этому можно спокойно его использовать.<br>
Так же я настроил мк на минимальную скорость процессора - 80 МГЦ (вместо стандартных 240МГц).<br>

<img width="500" height="331" alt="image" src="https://github.com/user-attachments/assets/3c5f1764-418b-416c-b83b-7b7913bd6b30" />

Получилась следующая таблица подключения:

| # | Name | Pin | # | Name | Pin |
|---|---|---|---|---|---|
| 1 | RGB | GPIO15 | 20 | A | GPIO01 |
| 2 | LED | GPIO09 | 19 | SW4 "Down" | GPIO07 |
| 3 | SW1 "Mode" | GPIO04 | 18 |  |  |
| 4 | +5V | +5V | 17 | GND | GND |
| 5 |  |  | 16 |  |  |
| 6 |  |  | 15 |  |  |
| 7 |  |  | 14 |  |  |
| 8 | SW5 "Up" | GPIO08 | 13 |  |  |
| 9 |  |  | 12 | B | GPIO02 |
| 10 | SW2 "RGB" | GPIO05 | 11 | SW3 "Power" | GPIO06 |

Контакт "RGB" я не использовал (но предполагал подключить на 15 пин), поскольку RGB лента мне не нужна.<br>
Кнопку SW2, которая отвечала за переключени режимов RGB, за запрограммировал на циклично переключение "теплый-холодный" с пополнительными шагами перехода.<br>

Я не делал финальных фотографий - смотрится ужасно. esp32 я разместил в имеющемся корпусе кнопок с доработкой подручными средствами (во славу синей изоленты).<br>


Саму мк я программировал через ESPHome в Home Assistant.
Код программы описан в файле "ring_light.yaml".
С yaml я не очень, по этому код собран из "готовых" элементов + нейросети, и может быть не оптимальным.
