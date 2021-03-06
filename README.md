
# 3. Учебный проект «Холст»

[![1.png](https://i.postimg.cc/RZdc81p4/1.png)](https://postimg.cc/mPcz19G5)

### Координаты обозначенные черными стрелками, это основные константы. Их 11.
### Координаты обозначенные красными стрелками, это производные значения, обычно вычисляемые через базовые константы. Их 7

**1.** Верхняя граница облака. `cloudTop`. Значение - 100

**2.** Левая граница облака. `cloudLeft`. Значение - 10

**3.** Ширина облака = ширина тени облака. `cloudWidth`. Значение - 420

**4.** Высота облака = высота тени облака. `cloudHeight`. Значение - 270

**5.** Верхняя граница тени облака. `cloudShadowTop`. Значение - `cloudTop` + 10

**6.** Левая граница тени облака. `cloudShadowLeft`. Значение - `cloudLeft` + 10

**7.** Левая граница текста поздравления. `congratTextLeft`. Вычисляется от `cloudLeft`. К примеру `cloudLeft` + 20

**8.** Базовая линия первой строки текста поздравления. `congratTextBaseline`. Вычисляется от `cloudTop`. К примеру `cloudTop` + 30.

**9.** Базовая линия второй строки текста поздравления. Не нужно сохранять в переменную, но можно создать константу `lineHeight` - высота строки. Вычисляется от `congratTextBaseline`. К примеру `congratTextBaseline` + 20.

**10.** Левая граница гистограммы. `histogramLeft`. Задается от `cloudLeft`. К примеру `cloudLeft` + 40

**11.** Нижняя граница гистограммы. `histogramBase`. Вычисляется от `cloudTop`. К примеру `cloudTop` + 230

**12.** Базовая линия подписи игрока. Вычисляется от `histogramBase`. К примеру `histogramBase` + 20

**13.** Максимальная высота столбца гистограммы. `columnMaxHeight`. Значение - 150

**14.** Высота произвольного столбца. Вычисляется динамически пропорционально исходя из `columnMaxHeight`

**15.** Базовая линия подписи времени. Вычисляется динамически от результата вычитания высоты текущего столбца из `histogramBase`.

**16.** Ширина столбца гистограммы. `columnWidth`. Значение - 40

**17.** Расстояние между столбцами гистограммы. `columnGap`. Значение - 50

**18.** Интервал между столбцами гистограммы. `columnWidth` + `columnGap`


### Все названия значений условные
---

### Пример реализации

**1.** Все начинается с функции `window.renderStatistics`.

**2.** В функции `window.renderStatistics` объявляются константы `CLOUD_X` (левая граница облака.), `CLOUD_Y` (верхняя граница облака.) и `COLUMN_WIDTH` (ширина столбца гистограммы). Функция `window.renderStatistics` вызывает функцию `init`

**3.** Функция `init` вызывает 3 функции:

_а)_ `renderCloudWithShadow` (функция, которая отрисовывает облако с тенью), 
	
_б)_ `renderTopText` (функция, которая отрисовывает тект поздравления), 
	
_в)_ `renderHistogram` (функция, которая отрисовывает гистограмму)
	
**4.** Функция `renderCloudWithShadow` объявляет 3 константы. `GAP` для отступа тени облака, `CLOUD_COLOR` - цвет облака, `SHADOW_COLOR` - цвет тени облака.

**5.** Функция `renderCloudWithShadow` два раза вызывает функцию `renderCloud`, для отрисовки тени, а затем облака (чтоб облако было над тенью)

**6.** Функция `renderCloud` объявляет две константы `CLOUD_WIDTH` и `CLOUD_HEIGHT` - ширина и высота облака соответственно и отрисовывает прямоугольник с заданными параметрами с помощью метода канваса `fillRect`

**7.** Функция `renderTopText` объявляет константу - текст поздравления. Это массив из двух строк.

**8.** Функция `renderTopText` вызывает функцию `renderText` и передает в нее текст поздравления

**9.** В функции `renderText` объявляются константы `FONT_PROPERTIES` (свойства шрифта), `LINE_HEIGHT` (высота строки), `BASIC_FONT_COLOR` (черный цвет), `TOP_TEXT_X_POS` (левая граница текста поздравления), `TOP_TEXT_Y_POS` (базовая линия первой строки текста поздравления). Если координаты текста не пришли в параметрах используется `TOP_TEXT_X_POS` и `TOP_TEXT_Y_POS`. Если цвет не пришел в параметрах, используется `BASIC_FONT_COLOR`.

**10.** В функции `renderText` отрисовывается массив строк в методе `forEach`, вызовом метода канваса `fillText`. Значение вертикальной координаты должно меняться динамически путем прибавления к базовой координате высоты строки умноженной на текущий индекс в итерации

**11.** В функции `renderHistogram` объявляются константы `HISTOGRAM_LEFT` (Левая граница гистограммы), `COLUMN_HEIGHT` (Максимальная высота столбца гистограммы), `COLUMN_GAP` (Расстояние между столбцами гистограммы). Вычисляется максимальное время (максимальное значение массива times) и записывается в переменную `maxTime`

**12.** В функции `renderHistogram` запускается итерация по массиву names методом `forEach`. В каждой итерации: 

_а)_ Вычисляется высота текущего столбца исходя из текущего значения времени по формуле `times[i] / maxTime * COLUMN_HEIGHT` и записывается в переменную `currentColumnHeight`. 
	
_б)_ Вычисляется левая координата текущего столбца по формуле `HISTOGRAM_LEFT + (i * (COLUMN_WIDTH + COLUMN_GAP))` и записывается в переменную `currentColumnX`.
	
_в)_ Вызывается функция `renderHist`, которая получает высоту текущего столбца, левую координату текущего столбца, текущие имя и время
	
**13.** Максимальный элемент массива нужно вычислять отдельной функцией. В ней использовать либо метод `reduce` (первый аргумент - функция-коллбэк, второй - `-Infinity`) либо `Math.max.apply`

**14.** В  функции `renderHist` объявляются переменные `HISTOGRAM_BASE` (Нижняя граница гистограммы), вычисляются `userTimeTextY` и `userNameTextY` (базовые линии подписи времени и подписи игрока соответственно), округляется время до целого значения, условно задается цвет текущего столбца (тернарником). Нахождение синего цвета со случайной насыщенностью можно оформить отдельной функцией. В ней использовать еще одну функцию для нахождения случайного числа.

**15.** Функция `renderHist` вызывает два раза `renderText` (для отрисовки подписи времени и подписи игрока) и функцию `renderColumn` для отрисовки столбца, в которую передает координаты, высоту и цвет столбца

**16.** Функция `renderColumn` отрисовывает столбец с помощью метода канваса `fillRect`. Вторым значением в этот метод нужно передавать результат вычитания высоты текущего столбца от координаты основания гистограммы

Готовый пример можно посмотреть [здесь](https://gist.github.com/Muhammadabumansur/f7a3b27a36051aac9109cd39874355aa)
