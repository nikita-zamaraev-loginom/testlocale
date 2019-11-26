# Замена по точному совпадению

Поле *Значение* таблицы замен содержит значения, подлежащие замене. Алгоритм ищет точное совпадение исходных данных со значениями данного поля. Если такое совпадение находится, то оно заменяется на значение поля *Замена*.

Пример логики алгоритма:

<table> 
<thead>
<tr><th>Значение исходных данных</th><th colspan="2">Значения строк таблицы замен</th><th>Совпадение</th><th>Новое значение</th></tr>
</thead>
<tbody>
<tr><td> </td><th>поле<br>"Замена"</th><th>поле<br>"Значение"</th><td> </td><td> </td></tr>
<tr><td rowspan="5" valign="top" align="right">48</td><td align="right">12</td><td align="left">Плохо</td><td>Нет</td><td rowspan="5" valign="top" align="left">Отлично</td></tr>
<tr><td align="right">24</td><td align="left">Удовлетворительно</td><td>Нет</td></tr>
<tr><td align="right">36</td><td align="left">Хорошо</td><td>Нет</td></tr>
<tr><td align="right">48</td><td align="left">Отлично</td><td>Да</td></tr>
<tr><td align="right">60</td><td align="left">Восхитительно</td><td>Нет</td></tr>
</tbody>
</table>

Пример с использованием данных:

Таблица замен

|Замена|Значение|
|-:|:-|
|12|Плохо|
|24|Удовлетворительно|
|36|Хорошо|
|48|Отлично|
|60|Восхитительно|

Результат проведенной замены

|Значение исходных данных|Новое значение|
|-:|:-|
|12|Плохо|
|15|15|
|24|Удовлетворительно|
|35|35|
|48|Отлично|
|73|73|

Как видно из таблицы сравнения исходных данных с итоговыми, все значения не попавшие в *Таблицу замен* не заменяются, и выдаются в исходном виде. Не попавшие в *Таблицу замен* значения обрабатываются согласно указанной *Точности* и параметру [Заменять остальное](./other-match.md).

## Применение допустимого интервала

При поиске среди вещественных и целых данных возможно указание допустимого интервала поиска. За его настройку отвечает параметр *Точность*. Интервалы рассчитываются следующим образом: `от <Значение поля замена>-<Точность> до <Значение поля замена>+<Точность>`. В случае, если с учетом интервала будут найдены несколько совпадений, то применяться будет наиболее близкое к исходному.

Пример логики алгоритма с применением интервала:

<table>
<thead>
<tr><th>Значение исходных данных</th><th colspan="2">Значения строк таблицы замен </th><th>Допустимый интервал</th><th>Интервал совпадения</th><th>Совпадение</th><th>Наиболее близкое к исходному</th><th>Новое значение</th></tr>
</thead>
<tbody>
<tr><th></th><th>поле<br>"Замена"</th><th>поле<br>"Значение"</th><th></th><th></th><th></th><th></th><th></th></tr>
<tr><td rowspan="5" valign="top" align="right">50</td><td align="right">12</td><td>Плохо</td><td rowspan="5" valign="top" align="right">20</td><td>от -8 до 32</td><td>Нет</td><td>Нет</td><td rowspan="5" valign="top" align="center">Отлично</td></tr>
<tr><td align="right">24</td><td>Удовлетворительно</td></td><td> от 4 до 44</td><td>Нет</td><td>Нет</td></tr>
<tr><td align="right">36</td><td>Хорошо</td></td><td>от 16 до 56</td><td>Да</td><td>Нет</td></tr>
<tr><td align="right">48</td><td>Отлично</td></td><td>от 28 до 68</td><td>Да</td><td>Да</td></tr>
<tr><td align="right">60</td><td>Восхитительно</td></td><td>от 40 до 80</td><td>Да</td><td>Нет</td></tr>
</tbody>
</table>

Пример с использованием данных:

Таблица замен

|Замена|Значение|
|-:|:-|
|12|Плохо|
|24|Удовлетворительно|
|36|Хорошо|
|48|Отлично|
|60|Восхитительно|

*Точность* равна `12`.

Результат проведенной замены

|Значение исходных данных|Новое значение|
|-:|:-|
|12|Плохо|
|15|Плохо|
|24|Удовлетворительно|
|35|Хорошо|
|48|Отлично|
|73|73|

Значения не попавшие, ни в *Таблицу замен*, ни в *Допустимые интервалы*, обрабатываются согласно настройке параметра [Заменять остальное](./other-match.md).