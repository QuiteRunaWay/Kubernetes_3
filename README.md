# Домашнее задание к занятию "12.3 Развертывание кластера на собственных серверах, лекция 1"
Поработав с персональным кластером, можно заняться проектами. Вам пришла задача подготовить кластер под новый проект.

## Задание 1: Описать требования к кластеру
Сначала проекту необходимо определить требуемые ресурсы. Известно, что проекту нужны база данных, система кеширования, а само приложение состоит из бекенда и фронтенда. Опишите, какие ресурсы нужны, если известно:

* База данных должна быть отказоустойчивой. Потребляет 4 ГБ ОЗУ в работе, 1 ядро. 3 копии.
* Кэш должен быть отказоустойчивый. Потребляет 4 ГБ ОЗУ в работе, 1 ядро. 3 копии.
* Фронтенд обрабатывает внешние запросы быстро, отдавая статику. Потребляет не более 50 МБ ОЗУ на каждый экземпляр, 0.2 ядра. 5 копий.
* Бекенд потребляет 600 МБ ОЗУ и по 1 ядру на копию. 10 копий.

## Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

План расчета
1. Сначала сделайте расчет всех необходимых ресурсов.
2. Затем прикиньте количество рабочих нод, которые справятся с такой нагрузкой.
3. Добавьте к полученным цифрам запас, который учитывает выход из строя как минимум одной ноды.
4. Добавьте служебные ресурсы к нодам. Помните, что для разных типов нод требовния к ресурсам разные.
5. Рассчитайте итоговые цифры.
6. В результате должно быть указано количество нод и их параметры.


# Ответ: 

### БД
|Параметр | На 1 ВМ | На кластер | Резерв (минимум 1 нода) | Служебные ресурсы на 1 ноду|
|-------- | ------- | ---------- | ----------------------- |---------|
|ОЗУ|4 Гб|12 Гб|4 Гб|4 Гб|
|Процессор|1 ядро|3 ядра|1 ядро|4 ядра|

### Кэш
|Параметр | На 1 ВМ | На кластер | Резерв (минимум 1 нода) | Служебные ресурсы на 1 ноду|
|-------- | ------- | ---------- | ----------------------- |---------|
|ОЗУ|4 Гб|12 Гб|4 Гб|4 Гб|
|Процессор|1 ядро|3 ядра|1 ядро|4 ядра|

### Фронтенд
|Параметр | На 1 ВМ | На кластер | Резерв (минимум 1 нода) | Служебные ресурсы на 1 ноду|
|-------- | ------- | ---------- | ----------------------- |---------|
|ОЗУ|50 Мб|250 Мб|50 Мб|2 Гб|
|Процессор|0,2 ядра|1 ядро|0,2 ядра|1 ядро|

### Бекенд
|Параметр | На 1 ВМ | На кластер | Резерв (минимум 1 нода) | Служебные ресурсы на 1 ноду|
|-------- | ------- | ---------- | ----------------------- |---------|
|ОЗУ|600 Мб|6000 Мб|600 Мб|2 Гб|
|Процессор|1 ядро|10 ядер|1 ядро|1 ядро|

Стандарт для ВМ под обычный софт – 1 ядро, 2 Гб, 20 Гб HDD. Следовательно будем исходить из этого. По этому:

Для Фронтенда – на 1 ноду 1 ядро, 2 Гб ОЗУ, 20 Гб HDD.

Для бекенда аналогично на 1 ноду возьмём 1 ядро и 2 Гб ОЗУ, 20 Гб HDD.

Следовательно для БД и кэша необходимо более лучшие характеристики: думаю по 4 ядра на ноду для БД и кэша, по 4 Гб ОЗУ на каждую ноду. По диску: для БД и кэша надо более быстрые диски по этому будем использовать SSD. Размеры БД и кэша так же будут постоянно расти, по этому дисковое пространство для кеша и БД сделаем по 100 Гб на ноду.

В итоге всего ресурсов на ноду и на кластер со всеми ресурсами (служебными):

### БД
|Параметр | На кластер | Резерв (минимум 1 нода) | Служебные ресурсы на 3 ноды | ВСЕГО (кластер, резерв, служебные ресурсы)|
|-------- | ------- | ---------- | ----------------------- |---------|
|ОЗУ|12 Гб|4 Гб|12 Гб|28 Гб|
|Процессор|3 ядра|1 ядро|12 ядер|16 ядер|

### Кеш
|Параметр | На кластер | Резерв (минимум 1 нода) | Служебные ресурсы на 3 ноды | ВСЕГО (кластер, резерв, служебные ресурсы)|
|-------- | ------- | ---------- | ----------------------- |---------|
|ОЗУ|12 Гб|4 Гб|12 Гб|28 Гб|
|Процессор|3 ядра|1 ядро|12 ядер|16 ядер|

### Фронтенд
|Параметр | На кластер | Резерв (минимум 1 нода) | Служебные ресурсы на 3 ноды | ВСЕГО (кластер, резерв, служебные ресурсы)|
|-------- | ------- | ---------- | ----------------------- |---------|
|ОЗУ|250 Мб|50 Мб|6 Гб|6,3 Гб|
|Процессор|1 ядро|0,2 ядра|3 ядра|4,2 ядера|


### Бекенд
|Параметр | На кластер | Резерв (минимум 1 нода) | Служебные ресурсы на 3 ноды | ВСЕГО (кластер, резерв, служебные ресурсы)|
|-------- | ------- | ---------- | ----------------------- |---------|
|ОЗУ|6000 Мб|600 Мб|6 Гб|12,6 Гб|
|Процессор|10 ядер|1 ядро|3 ядра|14 ядер|

#### ИТОГО, всего для ВМ требуется (по заданию): 

Процессор: 50,2 ядра

ОЗУ: 74,9 Гб ОЗУ

Хранилище: 1140 Гб. (Хранилище: HDD – 340 Гб (фронтенд+бекенд, 15 нод по заданию + по 1 ноде каждого (итого 2) под резерв. Всего фронтенд и бекенд с резервом – 17 нод), SSD – 3*100 БД + 100 резерв, 3*100 кэш + 100 резерв. Получается 8 нод по 100 Гб – 800 Гб.)

В расчетах 1000 Мб = 1 Гб.

Должен еще отметить, что на практике, если даже приложение будет требовать 0,2 ядра процессора, то мне в любом случае выделят 1 – это минимум. Следовательно, принимая во внимание тот факт, что мы всегда будем округлять ресурсы в большую сторону, то можно сказать, что для фронтенда у нас минимум будет 1 ядро для приложения на ноде. Если округлить цифры, то есть наши 4,2 на кластер с резервом мы можем смело выделять 5 ядер. И тогда наши 50,2 округляем в большую сторону, т.е. до 51 ядра.
ОЗУ аналогично, тут конечно проще, чем с ядрами, тут можно выделить сколько угодно, но всё же думаю округление до 75 Гб будет смотреться логично завершенной просьбой на выделение ресурса.

По поводу места – считаю, что место должно выделяться из общего хранилища с возможностью его дальнейшего расширения (как и ядра с ОЗУ, соответственно). Следовательно для БД и кэша, я бы всё же выделил не 800 Гб как положено, а 1 Тб, с запасом на будущее и если нет проблем с хранилищем в текущий момент.
Окончательные итоговые цифры, которые я бы выделил под кластер на новый проект:

## ИТОГО предлагаю окончательные цифры, которые включают в себя все ресурсы:

Процессор: 51 ядро. ОЗУ: 75 Гб. Хранилище 1340 Гб. Количество нод: БД: 4 ноды Кэш: 4 ноды Фронтенд: 6 нод Бекенд: 11 нод

# Доработка.

После дополнительного анализа и комментария проверяющего Андрея Копылова, предлагаю решение в таком виде: 

управляющих нод у нас будет ```3``` (для построения отказоустойчивого кластера, в первоначальных лекциях смотрели на примере отказоустойчивости etcd, не четное количество должно быть), характеристики одной ноды вот такие: дискового пространства ```100 Гб``` (в лекции было рекомендовано от 50 Гб, но предполагаю, что это значение у нас будет расти, по этому сразу 100 возьмём), ```4 ядра``` (рекомендовано от 2 ядер, но у нас нагрузка будет достаточно большая и количество сервисов у нас будет значительное, берем так же с запасом), ОЗУ: предполагаю, что достаточно будет ```4 Гб``` на каждую ноду.

### Итого для master нод у нас будет выделено ресурсов: 300 Гб дискового пространства, 12 ядер процессора и 12 Гб ОЗУ.

рабочих нод у нас будет ```6``` (берем из расчета, как было сказано на лекции, что на 1 master ноду обычно идёт 2 worker ноды, следовательно чтобы внутри они могли перебрасывать ресурсы между собой), характеристики одной ноды: дискового пространства тут надо чуть больше, возьмём ```150 Гб``` на ноду, ```2 ядра``` должно хватить + ```2 Гб``` на каждую ноду;

### Итого для worker нод у нас будет выделено следующее количество ресурсов: 900 Гб дискового пространства, 12 ядер процессора и 12 Гб ОЗУ.
