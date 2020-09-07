---
publish: 2020-01-16
---

# Алгоритмы кластеризации на службе Data Mining

**Данный материал — попытка систематизировать и дать целостный взгляд на последние достижения в области разработки эффективных подходов к кластеризации данных. Целью материала не являлось подробное описание всех алгоритмов кластеризации. Наоборот, обзорный характер статьи и затронутая проблематика помогут сориентироваться в огромном количестве алгоритмов кластеризации.**

## Введение

[Кластеризация](https://wiki.loginom.ru/articles/clustering.html) – объединение в группы схожих объектов – является одной из фундаментальных задач в области анализа данных и [Data Mining](https://wiki.loginom.ru/articles/data-mining.html). Список прикладных областей, где она применяется, широк: сегментация изображений, маркетинг, [борьба с мошенничеством](https://wiki.loginom.ru/articles/antifraud.html), [прогнозирование](https://wiki.loginom.ru/articles/forecasting.html), [анализ текстов](https://wiki.loginom.ru/articles/text-mining.html) и многие другие. На современном этапе кластеризация часто выступает первым шагом при анализе данных. После выделения схожих групп применяются другие методы, для каждой группы строится отдельная модель.

Задачу кластеризации в том или ином виде формулировали в таких научных направлениях, как статистика, распознавание образов, [оптимизация](https://wiki.loginom.ru/articles/optimization.html), [машинное обучение](https://wiki.loginom.ru/articles/machine-learning.html). Отсюда многообразие синонимов понятию кластер – [класс](https://wiki.loginom.ru/articles/class.html), таксон, сгущение.

На сегодняшний момент число методов разбиения групп объектов на кластеры довольно велико – несколько десятков алгоритмов и еще больше их модификаций. Однако нас интересуют алгоритмы кластеризации с точки зрения их применения в Data Mining.

## Кластеризация в Data Mining

Кластеризация в Data Mining приобретает ценность тогда, когда она выступает одним из этапов анализа данных, построения законченного аналитического решения. Аналитику часто легче выделить группы схожих объектов, изучить их особенности и построить для каждой группы отдельную модель, чем создавать одну общую модель на всех данных. Таким приемом постоянно пользуются в маркетинге, выделяя группы клиентов, покупателей, товаров и разрабатывая для каждой из них отдельную стратегию.

Очень часто данные, с которыми сталкивается технология Data Mining, имеют следующие важные особенности:

* высокая размерность (тысячи полей) и большой объем (сотни тысяч и миллионы записей) таблиц баз данных и [хранилищ данных](https://wiki.loginom.ru/articles/data-warehouse.html) (сверхбольшие базы данных);
* наборы данных содержат большое количество числовых и [категорийных атрибутов](https://wiki.loginom.ru/articles/categorical-data.html).

Все [атрибуты](https://wiki.loginom.ru/articles/attribute.html) или признаки объектов делятся на числовые (numerical) и категорийные (categorical). Числовые атрибуты – это такие, которые могут быть упорядочены в пространстве, соответственно категорийные – которое не могут быть упорядочены. Например, атрибут «возраст» – числовой, а «цвет» – категорийный. Приписывание атрибутам значений происходит во время измерений выбранным типом шкалы, а это, вообще говоря, представляет собой отдельную задачу.

Большинство алгоритмов кластеризации предполагают сравнение объектов между собой на основе некоторой меры близости (сходства). Мерой близости называется величина, имеющая предел и возрастающая с увеличением близости объектов. Меры сходства «изобретаются» по специальным правилам, а выбор конкретных мер зависит от задачи, а также от шкалы измерений. В качестве меры близости для числовых атрибутов очень часто используется [евклидово расстояние](https://wiki.loginom.ru/articles/euclid-distance.html), вычисляемое по формуле:

{% math %}D(x, y)=\sqrt{\sum_{i}{(x-y)^2}}{% endmath %}

Для категорийных атрибутов распространена мера сходства Чекановского-Серенсена и Жаккара {% math %}( \mid t_1 \cap t_2\mid/\mid t_1\cup t_2 \mid){% endmath %}.

Потребность в обработке больших массивов данных в Data Mining привела к формулированию требований, которым, по возможности, должен удовлетворять алгоритм кластеризации. Рассмотрим их:

1. Минимально возможное количество проходов по базе данных;
2. Работа в ограниченном объеме оперативной памяти компьютера;
3. Работу алгоритма можно прервать с сохранением промежуточных результатов, чтобы продолжить вычисления позже;
4. Алгоритм должен работать, когда объекты из базы данных могут извлекаться только в режиме однонаправленного курсора (т.е. в режиме навигации по записям).

Алгоритм, удовлетворяющий данным требованиям (особенно второму), будем называть [масштабируемым](https://wiki.loginom.ru/articles/scalable-algorithm.html) (scalable). Масштабируемость – важнейшее свойство алгоритма, зависящее от его вычислительной сложности и программной реализации. Имеется и более емкое определение. Алгоритм называют масштабируемым, если при неизменной емкости оперативной памяти с увеличением числа записей в базе данных время его работы растет линейно.

Но далеко не всегда требуется обрабатывать сверхбольшие массивы данных. Поэтому на заре становления теории кластерного анализа вопросам масштабируемости алгоритмов внимания практически не уделялось. Предполагалось, что все обрабатываемые данные будут умещаться в оперативной памяти, главный упор всегда делался на улучшение качества кластеризации.

Трудно соблюсти баланс между высоким качеством кластеризации и масштабируемостью. Поэтому в идеале в арсенале Data Mining должны присутствовать как эффективные алгоритмы кластеризации микромассивов (microarrays), так и масштабируемые для обработки сверхбольших баз данных (large databases).

## Алгоритмы кластеризации: блеск и нищета

Итак, уже можно классифицировать кластерные алгоритмы на масштабируемые и немасштабируемые. Продолжим классификацию.

По способу разбиения на кластеры алгоритмы бывают двух типов: иерархические и неиерархические. Классические иерархические алгоритмы работают только с категорийными атрибутами, когда строится полное дерево вложенных кластеров. Здесь распространены агломеративные методы построения иерархий кластеров – в них производится последовательное объединение исходных объектов и соответствующее уменьшение числа кластеров. Иерархические алгоритмы обеспечивают сравнительно высокое качество кластеризации и не требуют предварительного задания количества кластеров. Большинство из них имеют сложность {% math %}O(n^{2}){% endmath %}.

Неиерархические алгоритмы основаны на оптимизации некоторой целевой функции, определяющей оптимальное в определенном смысле разбиение множества объектов на кластеры. В этой группе популярны алгоритмы семейства [k-средних](https://wiki.loginom.ru/articles/k-means.html) (k-means, fuzzy c-means, Густафсон-Кесселя), которые в качестве целевой функции используют сумму квадратов взвешенных отклонений координат объектов от центров искомых кластеров.

Кластеры ищутся сферической либо эллипсоидной формы. В канонической реализации минимизация функции производится на основе метода множителей Лагранжа и позволяет найти только ближайший [локальный минимум](https://wiki.loginom.ru/articles/local-minimum.html). Использование методов глобального поиска ([генетические алгоритмы](https://wiki.loginom.ru/articles/genetic-algorithm.html)) значительно увеличит вычислительную сложность алгоритма.

![Рисунок 1. Гистограммы двух разбиений](k-means.svg)

Среди неиерархических алгоритмов, не основанных на расстоянии, следует выделить EM-алгоритм (Expectation-Maximization). В нем вместо центров кластеров предполагается наличие функции плотности вероятности для каждого кластера с соответствующим значением [математического ожидания](https://wiki.loginom.ru/articles/expectation-value.html) и [дисперсией](https://wiki.loginom.ru/articles/variance.html). В смеси распределений (рис. 2) ведется поиск их параметров (средние и [стандартные отклонения](https://wiki.loginom.ru/articles/mean-square-deviation.html)) по принципу [максимума правдоподобия](https://wiki.loginom.ru/articles/mle.html). Алгоритм EM и есть одна из реализаций такого поиска. Проблема заключается в том, что перед стартом алгоритма выдвигается гипотеза о виде распределений, которые оценить в общей совокупности данных сложно.

![Рисунок 2. Распределения и их смесь](distribution.svg)

Еще одна проблема появляется тогда, когда атрибуты объекта смешанные – одна часть имеет числовой тип, а другая часть – категорийный. Например, пусть требуется вычислить расстояние между следующими объектами с атрибутами (Возраст, Пол, Образование):

{23, муж, высшее}, (1)

{25, жен, среднее}, (2)

Первый атрибут является числовым, остальные – категорийными. Если мы захотим воспользоваться классическим иерархическим алгоритмом с какой-либо мерой сходства, нам придется каким-то образом произвести дискредитацию атрибута «Возраст». Например, так:

{до 30 лет, муж, высшее}, (1)

{до 30 лет, жен, среднее}, (2)

При этом часть информации, мы, безусловно, потеряем. Если же мы будем определять расстояние в евклидовом пространстве, то возникнут вопросы с категорийными атрибутами. Понятно, что расстояние между «Пол муж» и «Пол жен» равно 0, т.к. значения этого признака находятся в шкале наименований. А атрибут «Образование» можно измерить как в шкале наименований, так и в шкале порядка, присвоив каждому значению определенные балл.

Какой вариант выбрать? А что делать, если категорийные атрибуты важнее числовых? Решение этих проблем ложится на плечи аналитика. Кроме того, при использовании алгоритма k-средних и ему подобных возникают трудности с пониманием центров кластеров у категорийных атрибутов, априорным заданием количества кластеров.

Алгоритм оптимизации целевой функции в неиерархических алгоритмах, основанных на расстояниях, носит итеративный характер, и на каждой итерации требуется рассчитывать матрицу расстояний между объектами. При большом числе объектов это неэффективно и требует серьезных вычислительных ресурсов. Вычислительная сложность 1й итерации алгоритма k-means оценивается как {% math %}O(kmn){% endmath %}, где {% math %}k,m,n{% endmath %} – количество кластеров, атрибутов и объектов соответственно. Но итераций может быть очень много! Придется делать много проходов по набору данных.

Имеет массу недостатков в k-means сам подход с идеей поиска кластеров сферической или эллипсоидной формы. Подход хорошо работает, когда данные в пространстве образуют компактные сгустки, хорошо отличимые друг от друга. А если данные имеют вложенную форму, то ни один из алгоритмов семейства k-means никогда не справится с такой задачей. Также алгоритм плохо работает в случае, когда один кластер значительно больше остальных, и они находятся близко друг от друга – возникает эффект «расщепления» большого кластера (рис. 3).

![Рисунок 3. Эффект расщепления большого кластера](splitting.svg)

Впрочем, исследования в области совершенствования алгоритмов кластеризации идут постоянно. Разработаны интересные расширения алгоритма k-means для работы с категорийными атрибутами (k-modes) и смешанными атрибутами (k-prototypes). Например, в k-prototypes расчет расстояний между объектами осуществляется по-разному в зависимости от типа атрибута.

На рынке масштабируемых алгоритмов кластеризации борьба идет за снижение каждого «дополнительного» прохода по набору данных во время работы алгоритма. Разработаны масштабируемые аналоги k-means и EM (scalable k-means и scalable EM), масштабируемые агломеративные методы (CURE, CACTUS). Эти современные алгоритмы требуют всего несколько (от двух до десяти) сканирований базы данных до получения финальной кластеризации.

Получение масштабируемых алгоритмов основано на идее отказа от локальной функции оптимизации. Парное сравнение объектов между собой в алгоритме k-means есть не что иное, как локальная оптимизация, т.к. на каждой итерации необходимо рассчитывать расстояние от центра кластера до каждого объекта. Это ведет к большим вычислительным затратам.

При задании глобальной функции оптимизации добавление новой точки в кластер не требует больших вычислений: оно рассчитывается на основе старого значения, нового объекта и так называемых кластерных характеристик (clusters features). Конкретные кластерные характеристики зависят от того или иного алгоритма. Так появились алгоритмы BIRCH, LargeItem, [CLOPE](https://loginom.ru/blog/clope) и многие другие.

Таким образом, не существует единого универсального алгоритма кластеризации. При использовании любого алгоритма важно понимать его достоинства и недостатки, учитывать природу данных, с которыми он лучше работает и способность к масштабируемости.

## Литература

1. Bradley, P., Fayyad, U., Reina, C. Scaling Clustering Algorithms to Large Databases, Proc. 4th Int'l Conf. Knowledge Discovery and Data Mining, AAAI Press, Menlo Park, Calif., 1998.
2. Zhang, T., Ramakrishnan, R., Livny, M. Birch: An Efficient Data Clustering Method for Large Databases, Proc. ACM SIGMOD Int’l Conf. Management of Data, ACM Press, New York, 1996.
3. Paul S. Bradley, Usama M. Fayyad, Cory A. Reina Scaling EM (Expectation-Maximization) Clustering to Large Databases, Microsoft Research, 1999.
4. Z. Huang. Clustering large data sets with mixed numeric and categorical values. In The First Pacific-Asia Conference on Knowledge Discovery and Data Mining, 1997.
5. Milenova, B., Campos, M. Clustering large databases with numeric and nominal values using orthogonal projections, Oracle Data Mining Technologies, 2002.
6. Z. Huang. A fast clustering algorithm to claster very large categorical data sets in Data Mining. Research Issues on on Data Mining and KDD, 1997.
7. Wang, K., Xu, C.. Liu, B. Clustering transactions using large items. In Proc. CIKM’99, Kansas, Missouri, 1999.
8. Guha S., Rastogi R.,Shim K. CURE: An Efficient Clustering Algorithm for Large Databases, Proc. ACM SIGMOD Int’l Conf. Management of Data, ACM Press, New York, 1998.
9. Ganti V., Gerhke J., Ramakrishan R. CACTUS – Clustering Categorical Data Using Summaries. In Proc KDD’99, 1999.
10. J. Bilmes. A Gentle Tutorial on the EM Algorithm and its Application to Parameter Estimation for Gaussian Mixture and Hidden Markov Models, Tech. Report ICSI-TR-97-021, 1997.
11. Добыча данных в сверхбольших базах данных / В. Ганти, Й. Герке, Р. Рамакришнан // Открытые системы, №9-10, 1999.
12. Барсегян и др. Методы и модели анализа данных: OLAP и Data Mining. – СПб., 2004.