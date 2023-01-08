# NM-Tools
Универсальный сборник ваших самописных инструментов, с возможностью разложить скрипты по папкам.

###Использование
Для начала, нам нужно понять, какие библиотеки нам нужны, как их добавить и как их использовать.

Все достаточно просто! Для начала обратимся к коду.

```python
import random, os, winsound, argparse, inspect
winsound.Beep(500, 300)
os.system("color 0a")
```

Как мы видим, с самого начала импортируются самые важные библиотеки, они нужны для работы скрипта, они встроенные и потому их устанавливать не нужно.

Далее мы сразу используем <b>winsound</b>, чтобы встроенный динамик компьютера запищал, тем самым сигнализируя о начале работы. Ну и соответственно меняем цветовую схему терминала, куда же без этого!

><i>Как вы можете заметить, программа не кроссплатформенная, вы не можете ее запустить на <b>Linux</b> или <b>termux</b> без дополнительных манипуляций с кодом</i>.

Продолжим!
Далле вы можете увидеть массивы массивов. Начнем с первого:
```
moduleNames = [
            ["random   ",     "sys",          True   ],
            ["argparse ",     "sys",          True   ],
            ...
            ["pystyle  ",     "additional",   True   ]
            ]
```
Разберемся что мы здесь видим.
Каждый модуль состоит из:

`[<lib>, <sys/additional>, <True/false>, <OPTION>]`

А теперь по порядку. 
- Первый пункт это название библиотеки, что мы импортируем.
- Второй пункт это определение, системная она или нет.
>Надо бы автоматизировать это, в будующих обновлениях обязательно поправлю
- Третий пункт это определение, всю ли мы импортируем библиотеку или нет.
- Четвертый пункт используется только тогда, когда в предыдущем пункте было значение `False`. Здесь мы перечисляем функции, которые мы импортируем из библиотеки.

**Обратите внимание при написании скриптов:** 
<i>Когда мы импортируем библиотеку, но не уточняем, какие функции нам нужны, то ее использование выглядит так:</i>
```python
import lib

lib.function1

#>>> function1 output
```
<i>Но в случае, когда мы уточняем, что мы  импортируем, получается такой код:</i>
```python
from lib import function1

function1

#>>> function1 output
```
Надеюсь, с этим разобрались.
Идем дальше.

Следующий массив массивов выглядит так:
```
menuNames = [
            ["VK tools",      "loadMenuVK",  "VK", True  ],
            ["1 tools",       "loadMenu2",   "VK", False ],
            ["2 tools",       "loadMenu3",   "VK", False ],
            ["3 tools",       "loadMenu4",   "VK", False ],
            ["Other Tools",   "loadMenuOT",  "OT", True  ]
            ]
```
Здесь уже немного сложнее.
`[<Folder name>, <localFunction name>, <execNames suffix>, <True/False>]`
- Первый пункт понятен: он означает имя папки и текст на баннере (об этом позже).
- Второй пункт. В классе localFunction мы создаем функцию с этим названием, там генерируется ваше меню:
```python
global Method
functionName = 'localFunction.'+inspect.currentframe().f_code.co_name
try:  
    buildMenu.buildChosenMenu({Цифра по порядку, на котором стоит ваше меню}, {Массив массивов с функциями для этого меню})
except:
    exit(f"\n{prefix}An unexpected error has occurred, exiting...")
``` 
Проанализируем код.
Переменная `Method` глобальная, чтобы не было внутренних конфликтов.
Затем идет конструкция `try-except`, для отлова ошибок
Дальше "процедурно" генерируется  ваше меню с нужным именем и нужными функциями.
- Третий пункт означает суффикс из 2-3 заглавных латинских букв, означающих сокращение имени вашей папки
>Простой совет: Допустим у вас есть несколько скриптов для высшей математики, численных методов к примеру. Ваша папка должна называться NM tools (Сокращение от англ. Numeric Methods tools). Функция генерирующая ваше меню должна называться loadMenuNM. Соответственно суффикс будет NM.
- Четвертый пункт определяет, показывать папку в главном меню, или нет.

Последние массивы массивов представляют из себя конструкции вида:
```
execNames{Суффикс соответствующей папки} = [
            ['{Функция 1}',   '{Как мы объявили ее}',    {True/False}    ], 
            ...
            ['{Функция n}',   '{Как мы объявили ее}',    {True/False}    ] 
            ]
```
Думаю, дополнительных разъяснений касательно этих массивов не требуется.

><i>Сразу отвечу на предполагаемый вопрос:"Nakkamurro, почему же ты используешь массивы массивов, а не словари, это же удобнее и нагляднее в коде!"
Отвечаю, массивы хоть и являются динамическими списками, они не увеличивают со временем свой размер в памяти. Банальная оптимизация, благодаря которой этот код можно скомпилировать и запустить хоть на чайнике. Но возможно я переделаю логику массива массивов и заменю его на словари. Да и вообще было бы неплохо разбить скрипт на несколько файлов.</i>

Далее указывается авторская информация:
```
version = "0.5.2b"
author = "Nakkamurro"
programName = "nero tools"
userFont = "slant"
``` 
Тут все понятно, в последнем пункте мы указываем шрифт для `pyfiglet`
><i>Нет, я не против, чтобы вы пользовались моими решениями и идеями, но пожалуйста, делайте это через форк проекта, гитхаб отлично продумал эту идею, все в плюсе, Я смогу улучшить свой код, вы сможете использовать удобную систему запуска ваших скриптов.</i>

Идем далее.
```
class buildMenu(object):...
class localFunction(object):...
class execFunctionVK(object):...
class execFunctionOT(object):...

buildMenu = buildMenu()
localFunction = localFunction()
execFunctionVK = execFunctionVK()
execFunctionOT = execFunctionOT()
```
Я использую Visual Studio Code, поэтому у меня все эти классы свернуты.
Все просто:
- Класс buildMenu не трогаем.
- В класс localFunction мы добавляем наше меню
- Классы execFunctionVK и execFunctionVK мы не трогаем
Ниже мы прописываем такие конструкции:
`{Имя класса} = {Имя класса}()`
Это сделано для удобства записи и инициализации класса.

Ниже мы спускаться не будем, так как менять там уже ничего не надо
