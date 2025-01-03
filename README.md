# Генштаб Нытья. Сегментация пятен нефти
Репозиторий для решений задачи "Сегментация пятен нефти на поверхности водоёмов" команды Генштаб Нытья

Директории:
- notebooks - ноутбук + ссылка на колаб
- slides - презентация
## Состав команды
Теги взяты из Пачки магистратуры
- Дарья Акимова @akimdar - Data Analyst
- Марина Дубровина @mainarel - Data Analyst
- Анна Ларькина @laawest007 - Data Analyst
- Александра Чиж @sandra.chig - Data Analyst
- Андрей Лудков @a.ludkov29 ML-engineer
- Семен Семин @senyasemi - ML-engineer
- Денис Астанин @densof161922 Tech Lead
- Дмитрий Гончаров @goncharovdv - ML-engineer
- Борис Смородин @bobbysmorcher - Team Lead
## Проблематика
Иногда на плавучих нефтедобывающих вышках происходят аварии, результатами которых является разлив нефти. Обычно такие аварии характеризуются заметным изменением показателей на вышке, например, перепадами давления. Для подтверждения разлива и фиксации его характеристик снаряжается специалист на водном транспорте.

Цель данной работы - разработка автоматизированного решения для разведки водной местности на предмет наличия разлива нефти на основе сегментации.

## Задачи
* Выбор референса, определение стека технологий
* Анализ, сбор, очистка открытых данных, разметка, аугментация. Формирование тренировочного и валидационного датасетов.
* Выбор и обучение модели, сопосотавление метрик с референсом.
* План дальнейшего развития.

## Определение стека технологий

Коллегиально было сформировано мнение решать задачу в рамках фреймворка Roboflow, в рамках которого возможно решить большинство задач, связанных с компьютерным зрением.

## Работа с данными

Для обучения моделей сегментации необходимы отдельные кадры с соответствующей разметкой.
Для тестирования всей системы в целом нужны наборы фотографий.

В открытом доступе, как правило, есть три типа данных для задач определения разлива нефти:

1. Табличные данные с описанием характеристик чрезвычайной ситуации. Решаемые задачи:
классификация.
2. Снимки со спутников в различных световых спектрах, например, в инфракрасном. Решаемые
задачи: детекция, сегментация, классификация.
3. Обычные фотоснимки/видеозаписи. Решаемые задачи: детекция, классификация.

В данной работе целевым типом данных является третий - набор фотографий поверхности воды
с предполагаемым разливом нефти. В результате первичного анализа открытых данных удалось
найти восемь различных наборов фотографий, либо размеченных под детекцию, либо не
размеченных вовсе.

Для разметки данных было составлено техническое задание (могут быть внесены изменения):

Необходимо выполнить отбор только валидных данных, а также разметку для задачи сегментирования области разлива нефти на воде. Область разлива будем помечать единицей, все остальное нулём.  При наличии большого кол-ва мелких областей близко друг ко другу можно выделить их как одну область разлива. Однако, большие области необходимо отделять друг от друга, чтобы сегментированная область не содержала большой процент поверхности воды.

В результате командной разметки нами был собран набор данных из ~150 фотографий. Разметка заняла ~5 часов.

Из размеченных данных автоматически (при помощи roboflow) было составлено 3 датасета: train, test, validate.

## Работа с моделью

Для решения задачи сегментации была выбрана модель Yolo V11. 

Программная реализация процесса обучения доступна в ноутбуке, находящемся в директории notebooks.

В качестве основной метрики для обучения модели была выбрана метрика mAP (mean Average Precision). AP (Average precision) – это популярная метрика измерения точности детекторов объектов таких как Faster R-CNN, SSD и т.д. Average Precision вычисляет среднюю точность для recall в диапазоне от 0 до 1.

Были обучены несколько версий моделей с различным параметрами обучения. При этом в процесе обучения для всех моделей применялся регуляризатор Adam.

Из всех моделей наиболее правдоподобные результаты показала модель с наименьшим значением метрики mAP 35%.

Именно эту модель было принято решение развернуть на roboflow в виде workflow. Работоспособность модели была протестирована при помощи запросов к этому workflow. Программная реализация находится в директории notebooks.

## Развитие

В рамках дальнейшего развития предполагается работа в следующих направлениях:
* Улучшение точности работы модели путём дополнения и актуализации обучающей выборки;
* Переход с roboflow на более производительные технологии, полностью локальная работа с моделью;
* Создание клиентской платформы и бэкенда для предоствления доступа к непосредственному взаимодействию с моделью.


