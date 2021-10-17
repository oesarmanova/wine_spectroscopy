# wine_spectroscopy

# 0. О базе вин.

Причесанную базу, более-менее готвую к обработке, можно найти тут: http://213.131.1.132:25621/owncloud/index.php/apps/files/?dir=/wine_problem&fileid=280534 под именем wine_output_data.csv 
Исходная база находится там же, но под именем Вина_исходная_сводная_таблица.xlxs

База содержит 303 образца белых вин. У этих образцов контроллировались следующие параметры:

| параметр | значение | комментарий |
| --- | --- | --- |
| name | название вина | малоинформативный параметр |
| country | страна производства | - |
| region | регион производства внутри страны | Многие регионы я округляла в сторону бОльших территорий. Например, Центральная долина (Чили) включает себя 4 региона: долину Майпо, регион Рапель (находится в провинции Кольчагуа), долину Курико и долину Мауле. Если вино принадлежит к одному из этих 4х регионов, то я фиксирую, что оно произведено в Центральной долине. Для 22 образцов этот параметр неизвестен. |
| harvest year | год сбора винограда | - |
| bottling date | дата розлива | - |
| grape variety | сорт винограда | wgv - неидентифицированные белые сорта винограда. Зачастую, такое пишут, когда виноградный мусор из бочек, заливают спиртом, добавляют сахару и готовят напиток богов (но это не точно). muscat - собирательное название для целого семейства, включающего 200 сортов винограда, но на этикетках, как правило, более детальной информации найти не удается. |
| type | тип | Здесь идет деление на тихое (still), игристое (sparkling) вино, портвейн (porto) и винные напитки (winy beverage) из фруктового сусла. |
| sweetness | сладость | Внимание стоит обратить на образцы, у которых type != still. Для портвейнов и игристых своя градация по сладости. На данный момент портвены идут в категории 'sweet', брюты в категории 'dry' |
| ethanol, min. % | минимальное количество этанола в вине, объемные проценты | Часто на бутылке указывают диапазон значений этанола. |
| ethanol, max. % | максимальное количество этанола в вине, объемные проценты | комментарий |
| protein, g | содержание белков | Для 60 образцов этот параметр неизвестен. |
| fat, g | содержание жиров | Для 60 образцов этот параметр неизвестен. |
| carbohydrate, g | содержание углеводов | Для 60 образцов этот параметр неизвестен. |
| energy, kJ | энергетическая ценность, кДж | Возможно, имеет смысл рассматривать энергетическая ценность только в каких-то одних единицах измерения. |
| energy, kcal | энергетическая ценность, ккал | Возможно, имеет смысл рассматривать энергетическая ценность только в каких-то одних единицах измерения. |
| sugar, min. g/L | минимальное содержание сахара | Для 178 образцов этот параметр неизвестен. |
| sugar, max. g/L | максимальное содержание сахара | Для 178 образцов этот параметр неизвестен. |
| price, rub/mL | цена | Образцы закупались в разных местах, поэтому относиться к этой величине с осторожностью. Для 98 образцов этот параметр неизвестен.|
| comments | комментарии | Указана дополнительная информация. Например, за сколько дне до измерения была вскрыта бутылка. |

![вина_распределение_по_странам](https://user-images.githubusercontent.com/79655674/137643952-3532d336-19f0-4eca-85f6-5a9cbab75d34.png)
Распределение примеров по странам

![вино_распределение_по_винограду](https://user-images.githubusercontent.com/79655674/137644562-fd596aeb-5dc4-4fff-963a-dd5cb9065843.png)
Распределение примеров по сорту винограда

![вино_распределение_по_сладости](https://user-images.githubusercontent.com/79655674/137644885-cda9c20d-bc1d-4204-9a93-91e279b27524.png)
Распределение примеров по сладости

##### Классы во всех категориях нескоменсированы.

# 1. Флуоресцентная спектроскопия

### 1.1. Эксперимент

3D спектры флуоресценции были получены на спектрофлуориметре Shimadzu RF-6000. Параметры регистрации приведены в Таблице 1.
Спектры вин регистрировались сразу после вскрытия бутылки, если в таблице не указано отдельно. **Дополнительно образцы НЕ разбавлялись водой.**

Таблица 1. Параметры регистрации 3D спектров флуоресценции.
| Parameter	| |
| --- | --- |
| EX Wavelength Start:	| 250.0 nm |
| EX Wavelength End:	| 500.0 nm |
| EX Wavelength increment:	| 5.0 nm |
| EM Wavelength Start:	| 250.0 nm |
| EM Wavelength End:	| 800.0 nm |
| EX Wavelength increment:	| 1.0 nm |
| Scan Speed:	| 6000 nm/min |	
| EX Bandwidth:	| 3.0 nm |
| EM Bandwidth: |	3.0 nm |

![plot](https://github.com/oesarmanova/wine_spectroscopy/blob/images/Fig_1.png)

На рисунке представлены 3D спектры флуоресценции двух образцов под номерами 1 (слева) и 100 (справа).
Самое интенсивное и широкое возмущение - флуоресценция вина. Дополнительные характерные области на рисунке:
1. Сигнал от накачки
2. Второй порядок от накачки
3. Шумы

### 1.2. Датасет

Набор данных состоит из 303 уникальных 3D спектров флуоресценции неразбавленных белых вин.
Каждый 3d спектр с номером num записан в отдельный файл 'winenum_CorrectionData.csv'. 

![plot](https://github.com/oesarmanova/wine_spectroscopy/blob/images/data_3d_fluor_table.png)

Каждый пример представляет собой 2d массив, содержащий значения интенсивности флуоресценции вин в спектральном канале при возбуждении излучением на определенной длине волны. Каждая строка массива представляет из себя спектр флуоресценции вина в диапазоне от 250 до 800 нм с шагом 1 нм (551 признаков) при возбужденнии на некоторой длине волны в диапазоне от 250 до 500 нм. Тогда каждый столбец массива - спектр возбуждения флуоресценции в диапазоне от 250 до 500 нм с шагом 5 мн (51 признак).

Иными словами, каждый пример содержит 51 спектр флуоресценции вин.

**Предобработка данных НЕ проводилась.**

Датасет находится по ссылке http://213.131.1.132:25621/owncloud/index.php/apps/files/?dir=/wine_problem/3d_wine_csv&fileid=280536

Дополнительно были повторно (примерно месяц-полтора после открытия бутылок) записаны 3d спектры флуоресценции 33 вин.
Номера прописанных повторно вин: 1, 3, 6, 13, 15, 20, 22, 34, 47, 64, 65, 68, 70, 74, 83, 90, 95, 104, 107, 110, 111, 117, 121, 123, 128, 135, 158, 172, 174, 183, 197, 206, 217.
Их спектры флуоресценции находятся по ссылке: http://213.131.1.132:25621/owncloud/index.php/apps/files/?dir=/wine_problem/3d_wine_povtor_csv&fileid=280842

# 2. Оптическое поглощение

### 2.1. Эксперимент

Спектры оптического поглощения регистрировались с помощью спектрофотометра Shimadzu UV-1800 в диапазоне от 190 до 800 нм с шагом 1 нм.
Спектры вин регистрировались сразу после вскрытия бутылки, если в таблице не указано отдельно. **Образцы разбавлялись дистиллятом в 20 раз.** Регистрация спектров поглощения проводилась относительно дистиллята.

Данные по ссылке: http://213.131.1.132:25621/owncloud/index.php/f/280876

![image](https://user-images.githubusercontent.com/79655674/136787901-3b680540-b95a-458f-abf1-8290b747f20b.png)

### 2.2. Датасет

Набор данных состоит из 303 уникальных спектров оптического поглощения разбавленных в 20 раз белых вин + спектры поглощения 33 вин, записанные повторно (см. п. 1.2.).
Каждый пример представляет собой вектор (1х611).

![image](https://user-images.githubusercontent.com/79655674/136786848-cc877055-c89c-4889-a799-e82bd26b68d5.png)

Таким образом, в таблице данных выше каждыая строка - пример, столбцы соответствуют признакам (длинам волн). В нулевом столбце записаны номера проб. Если номер пробы имеет вид 'wine_num_povtor', значит этот спектр соответсвует образцу, записанному повторно спустя ~1.5 месяца после вскрытия бутылки.

**Предобработка данных заключалась в занулении отрицательных значений оптической плотности.**

Данные по ссылке: http://213.131.1.132:25621/owncloud/index.php/f/280876

# 3. Спектросокпия КР

### 3.1. Эксперимент

Спектры КР вин были зарегистрированы на экспериментальной установке, состоящей из:
- Монохроматора – Acton 2500i, решетка 900 штр/мм, фокусное расстояние 500 мм, ширина входной щели 50 мкм. 
- Детектора – CCD камеры Horiba Jobin Yvon, модель Syncerity.
- Диодного лазера.

Возбуждение комбинационного рассеяния (КР) происходило на длине волны 532 нм (мощность 270-290 мВт), спектры КР регистрировались в трех диапазонах с центрами 560, 600, 650 нм: 
- (560) 36.9 - 1770.7 см<sup>-1</sup>;
- (600) 1347.4 - 2855.6 см<sup>-1</sup>;
- (650) 2775.2 -  4019.0 см<sup>-1</sup>. 

Спектры, зарегистрированные в каждом диапазоне содержат 1024 спектральных каналов.

Время накопления сигнала было 0.1-1 с (в большей части случаев 1 с), 5 циклов без усреднения. Время накопления спектров выбиралось, исходя из интенсивности люминесценции - у сухих вин она не слишком большая, поэтому время накопления 1 с, у некоторых сладких мускатных вид она большая, поэтому время накопления было меньше - 0.5 с или еще меньше. 
Спектры вин регистрировались сразу после вскрытия бутылки, если в таблице не указано отдельно. **Дополнительно образцы НЕ разбавлялись водой.**

![image](https://user-images.githubusercontent.com/79655674/136800095-61159643-5ae1-4eed-97aa-26024627f645.png)

Область 2812-3040 см<sup>-1</sup> - валентные колебания спирта.
Область 3040-3780 см<sup>-1</sup> - валентные колебания воды.


### 3.2. Датасет

**Предобработка данных:**
  1. *Уборка случайных выбросов.* Поскольку каждый спектр регистрировался 5 раз подряд, то случайные выбросы в спектральном канале данного спектра заменялись средним арифметическим значением интенсивности в данном канале в остальных четырех спектрах.
  2. *Усреднение значений интенсивности в каждом канале по 5 зарегистрированным спектрам.*
  3. *Вычитание фона.* Т.к. фон - всегда зашумлённая константа, а в области фильтра (самое начало диапазона 560) сигнал гасится практически до нуля, то из спектров КР каждого образца (уже усреднённых) в трех диапазонах вычиталось одно и то же минимальное значение интенсивности КР в области фильтра. 
  4. *Спектры делились на время накопления и мощность лазера.*
  5. *Нормировка на валентную полосу воды.* Все спектры вин были отнормированы на интегральную интенсивность валентной полосы воды дистиллята, спектр которого снимался в каждой серии эксперимента (каждый день). То есть все вина, снятые в один день, нормировались на дистиллят, снятый в этот же день. Кроме образца 222. Этот образец был один в тот день. Для него не снимался отдельный дистиллят, поэтому использовался спектр дистиллята из соседней серии эксперимента для нормировки. Учитывая, что валентная полоса воды при возбуждении на 532 занимает больше ¾ диапазона 650, то интегральная интенсивность валентной полосы вычислялась как сумма значений интенсивности по всему этому диапазону.

Данные доступны по ссылке: http://213.131.1.132:25621/owncloud/index.php/apps/files/?dir=/wine_problem/raman_wine_csv&fileid=280877

