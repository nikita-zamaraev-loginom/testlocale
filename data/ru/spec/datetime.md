# Функции вида Дата/время


%spoiler%AddDay%spoiler%

**AddDay(Дата, Количество)**

* Дата - аргумент типа Дата/Время
* Количество - аргумент целого типа.

Функция возвращает значение аргумента Дата, увеличенного на указанное Количество дней. Количество может быть и отрицательным, тогда функция возвратит дату на указанное количество дней раньше указанной даты.

%/spoiler%

%spoiler%AddMonth%spoiler%

**AddMonth(Дата, Количество)**

* Дата - аргумент типа Дата/Время
* Количество - аргумент целого типа.

Функция возвращает значение аргумента Дата, увеличенного на указанное Количество месяцев. Количество может быть и отрицательным, тогда функция возвратит дату на указанное количество месяцев раньше указанной даты.

Если указанный день месяца больше, чем последний день получившегося месяца, то функция устанавливает дату на последний день получившегося месяца.

%/spoiler%

%spoiler%AddQuarter%spoiler%

**AddQuarter(Дата, Количество)**

* Дата - аргумент типа Дата/Время
* Количество - аргумент целого типа.

Функция возвращает значение аргумента Дата, увеличенного на указанное Количество кварталов. Количество может быть и отрицательным, тогда функция возвратит дату на указанное количество кварталов раньше указанной даты.

Если указанный день месяца больше, чем последний день месяца получившегося квартала , то функция устанавливает дату на последний день месяца получившегося квартала.

%/spoiler%

%spoiler%AddWeek%spoiler%

**AddWeek(Дата, Количество)**

* Дата - аргумент типа Дата/Время
* Количество - аргумент целого типа.

Функция возвращает значение аргумента Дата, увеличенного на указанное Количество недель. Количество может быть и отрицательным, тогда функция возвратит дату на указанное количество недель раньше указанной даты.

%/spoiler%

%spoiler%AddYear%spoiler%

**AddYear(Дата, Количество)**

* Дата - аргумент типа Дата/Время
* Количество - аргумент целого типа.

Функция возвращает значение аргумента Дата, увеличенного на указанное Количество лет. Количество может быть и отрицательным, тогда функция возвратит дату на указанное количество лет раньше указанной даты. 

%/spoiler%

%spoiler%DateTimeToStr%spoiler%

**DateTimeToStr(Дата_Время [, Формат_Даты [, Формат_Времени]])**

* Дата_Время - выражение типа дата/время.
* Формат_Даты - необязательный параметр формат даты в терминах D, M, Y.
* Формат_Времени - необязательный параметр формат времени в терминах H, N, S, Z.

Возвращает строковое представление аргумента Дата_время. Примеры формата даты:

```
"DD.MM.YY" - преобразует дату в формат "день.месяц.год" ("25.12.04") "MM/DD/YYYY" - преобразует дату в формат "месяц.день.год" ("12/25/2004")
```

Примеры формата времени:

```
"H:NN" - преобразует время в формат "часы:минуты" ("9:53") "HH:NN:SS"- преобразует время в формат "часы:минуты:секунды" ("09:05:53")
```

%/spoiler%


%spoiler%DateToStr%spoiler%

**DateToStr(Дата [, Формат_Даты])**

* Дата - выражение типа дата/время.
* Формат_Даты - необязательный параметр формат даты в терминах D, M, Y.

Возвращает строковое представление аргумента Дата. Примеры формата даты:

```
"DD.MM.YY" - преобразует дату в формат "день.месяц.год" ("25.12.04") "MM/DD/YYYY"- преобразует дату в формат "месяц.день.год" ("12/25/2004")
```

%/spoiler%

%spoiler%Day%spoiler%

**Day(Дата)**

* Дата - поле типа дата.

Возвращает день по заданной дате.

%/spoiler%

%spoiler%DayOfWeek%spoiler%

**DayOfWeek(Дата)**

* Дата - поле типа дата.

Возвращает день недели заданной даты. 

%/spoiler%

%spoiler%DaysBetween%spoiler%

**DaysBetween(Дата1, Дата2)**

* Дата1, Дата2 - поля типа дата.

Возвращает полное количество дней между двумя датами. 

%/spoiler%

%spoiler%EncodeDate%spoiler%

**EncodeDate(Год, Месяц, День)**

* Год - год в виде числа,
* Месяц - месяц в виде числа,
* День - день в виде числа.

Функция возвращает дату, сформированную из указанных аргументов. 

%/spoiler%

%spoiler%EncodeDateTime%spoiler%

**EncodeDateTime(Год, Месяц, День, Часы, Минуты, Секунды)**

* Год - год в виде числа,
* Месяц - месяц в виде числа,
* День - день в виде числа,
* Часы - часы в виде числа,
* Минуты - минуты в виде числа,
* Секунды - секунды в виде числа.

Функция возвращает дату и время, сформированные из указанных аргументов. 

%/spoiler%

%spoiler%EncodeTime%spoiler%

**EncodeTime(Часы, Минуты, Секунды)**

* Часы - часы в виде числа,
* Минуты - минуты в виде числа,
* Секунды - секунды в виде числа.

Функция возвращает время, сформированное из указанных аргументов. 

%/spoiler%

%spoiler%Hour%spoiler%

**Hour(ДатаВремя)**

* ДатаВремя - поле типа дата/время.

Возвращает час по заданной дате/времени. 

%/spoiler%

%spoiler%Minute%spoiler%

**Minute(ДатаВремя)**

* ДатаВремя - поле типа дата/время.

Возвращает минуты по заданной дате/времени. 

%/spoiler%

%spoiler%Month%spoiler%

**Month(Дата)**

* Дата - поле типа дата.

Возвращает месяц по заданной дате. 

%/spoiler%

%spoiler%MonthsBetween%spoiler%

**MonthsBetween(Дата1, Дата2)**

* Дата1, Дата2 - поля типа дата.

Возвращает полное количество месяцев между двумя датами. 

%/spoiler%

%spoiler%Now%spoiler%

**Now()**

* Аргументы отсутствуют.

Возвращает текущую дату и время. Так как текущая дата и время - это время вычисления выражения, которое считается каждый раз при получении значения этого выражения, например, при просмотре результата в виде таблицы или при выполнении экспорта данных, то можно, при наличии параметра выражения «Кэшировать рассчитанные значения выражения» включить эту опцию. 

%/spoiler%

%spoiler%Second%spoiler%

**Second(ДатаВремя)**

* ДатаВремя - поле типа дата/время.

Возвращает секунды по заданной дате/времени. 

%/spoiler%

%spoiler%StartOfTheWeek%spoiler%

**StartOfTheWeek(Дата)**

* Дата - аргумент типа Дата/Время.

Функция возвращает дату начала указанной недели в соответствии со стандартом ISO 8601, по которому неделя начинается с понедельника и заканчивается воскресеньем. 

%/spoiler%

%spoiler%StrToDate%spoiler%

**StrToDate(Аргумент [, Формат])**

* Аргумент - строковое выражение, содержащее дату/время
* Формат - необязательный параметр формат даты/время в терминах D, M, Y, H, N, S, Z

Функция конвертирует строку, содержащую дату в формат типа «Дата/Время». Примеры формата:

<pre>"DD.MM.YY" - говорит, что Аргумент содержит строки вида "25.12.04"; "DD/MM/YY/HH:NN:SS" - говорит, что Аргумент содержит строки вида "25/12/04/12:44:54";</pre>

%/spoiler%

%spoiler%Today%spoiler%

**Today()**

* Аргументы отсутствуют.

Возвращает текущую дату. Так как текущая дата - это дата вычисления выражения, которое считается каждый раз при получении значения этого выражения, например, при просмотре результата в виде таблицы или при выполнении экспорта данных, то можно, при наличии параметра выражения «Кэшировать рассчитанные значения выражения» включить эту опцию. 

%/spoiler%

%spoiler%Week%spoiler%

**Week(Дата)**

* Дата - поле типа дата.

Возвращает номер недели в году по заданной дате в соответствии со стандартом ISO 8601, по которому неделя начинается с понедельника и заканчивается воскресеньем. Первая неделя года начинается с понедельника, для дней с 1 января по первый понедельник возвращается номер последней недели предыдущего года. 

%/spoiler%

%spoiler%Year%spoiler%

**Year(Дата)**

* Дата - поле типа дата.

Возвращает год по заданной дате. 

%/spoiler%

%spoiler%YearsBetween%spoiler%

**YearsBetween(Дата1, Дата2)**

* Дата1, Дата2 - поля типа дата.

Возвращает полное количество лет между двумя датами.

%/spoiler%