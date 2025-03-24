H/W for the lesson "Databases, their types" - Alexey Motorin
# Домашнее задание к занятию "`Базы данных, их типы`" - `Моторин Алексей`

---

### Задание 1. СУБД

`Крупная строительная компания, которая также занимается проектированием и девелопментом, решила создать правильную архитектуру для работы с данными. Ниже представлены задачи, которые необходимо решить для каждой предметной области.

Какие типы СУБД, на ваш взгляд, лучше всего подойдут для решения этих задач и почему?`

1. `Бюджетирование проектов с дальнейшим формированием финансовых аналитических отчётов и прогнозирования рисков. СУБД должна гарантировать целостность и чёткую структуру данных.`

Для решения задачи бюджетирования с аналитическими отчетами и прогнозированием рисков оптимальным выбором будет колоночная NoSQL СУБД.

- Высокая эффективность чтения: прямое обращение к нужным столбцам, минимизация объема считываемых данных, быстрые аналитические запросы
- Эффективное сжатие данных: значительное сокращение занимаемого места, оптимизация производительности при работе с большими объемами, экономия ресурсов хранения
- Особенности для финансовых задач: высокая производительность при агрегировании данных, оптимизация для аналитических запросов, поддержка сложных вычислений.

**СУБД имеющие преимущества для финансовой аналитики**:
* ClickHouse
* Apache Kudu
* SAP HANA
* Vertica
* Snowflake

**Основные преимущества**:
- Быстрая обработка исторических данных
- Эффективное построение прогнозов
- Высокая производительность при расчете рисков
- Возможность обработки больших объемов финансовых данных


1. `* Хеширование стало занимать длительно время, какое API можно использовать для ускорения работы?`

Прежде чем использовать какие-либо API нужно провести тестирование для выбора стратегии оптимизации, например, выбор оптимального алгоритма хеширования, использование кэширования или аппаратное ускорение.

**Для ускорения процесса хеширования можно использовать**:
- Специальные криптографические библиотеки: OpenSSL API (C/C++) высокооптимизированные функции хеширования; Bouncy Castle (Java) - включает аппаратное ускорение; Crypto++ (C++) - поддерживает параллельное хеширование.
- Аппаратное ускорение: Intel AES-NI (Advanced Encryption Standard New Instructions); GPU-ускорение через CUDA или OpenCL; FPGA-решения для криптографических операций.



2. `Под каждый девелоперский проект создаётся отдельный лендинг, и все данные по лидам стекаются в CRM к маркетологам и менеджерам по продажам. Какой тип СУБД лучше использовать для лендингов и для CRM? СУБД должны быть гибкими и быстрыми.`

Для данной задачи оптимально использовать связку из двух типов СУБД, a для интеграции между системами использовать ETL-процессы для передачи данных, синхронизации, репликации и API интеграцию для передачи данных от лендингов в CRM.

**Для лендингов лучше использовать NoSQL базы данных**: MongoDB (документальная БД), Cassandra (колоночная БД), Redis (in-memory БД).
**Преимуществами будут**: высокая производительность чтения/записи, гибкая структура данных, горизонтальное масштабирование, быстрая обработка большого количества запросов.

**Для CRM лучше подойдут реляционные СУБД**: PostgreSQL, MySQL, Microsoft SQL Server
Такой подход позволит обеспечить высокую производительность лендингов, сохранить целостность данных в CRM, обеспечить гибкую масштабируемость, упростить разработку и поддержку.

При необходимости можно также рассмотреть гибридные решения, сочетающие элементы как SQL, так и NoSQL подходов, например, использование PostgreSQL с JSONB полями для хранения неструктурированных данных.



2. `* Можно ли эту задачу закрыть одной СУБД? И если да, то какой именно СУБД и какой реализацией?

Да, эту задачу можно закрыть одной СУБД - PostgreSQL. Вот почему: преимущества PostgreSQL для данной задачи - это универсальность: хранение структурированных данных (как в классической реляционной БД), работа с JSONB данными (для гибкой структуры лендингов), поддержка полнотекстового поиска.

**Так же стоит отметить производительность**: масштабируемость, высокую производительность чтения/записи и поддержка параллельных операций
Надежность наше всё. При использовании единого решения мы получаем меньшее точек отказа, ACID-согласованность, репликацию данных и достаточно простую поддержку.

Реализация решения.
**Для лендингов**: 
- Использование JSONB полей для хранения динамических данных форм;
- Кэширование часто используемых данных через pg_hint_plan:
- Создание временных таблиц для быстрых операций.
**Для CRM**:
- Классическая реляционная структура;
- Строгая схема данных;
- Сложные запросы и отчеты.

Структура данных:
```
CREATE TABLE leads (
    id SERIAL PRIMARY KEY,
    project_id INTEGER,
    lead_data JSONB,
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE crm_data (
    id SERIAL PRIMARY KEY,
    lead_id INTEGER,
    customer_info JSONB,
    status VARCHAR,
    ...
);
```
`
Индексы:
- **B-tree** индексы для часто используемых полей;
- **GIN**-индексы для JSONB полей;
- **Частичные индексы** для оптимизации запросов.


3. `Отдел контроля качества решил создать базу по корпоративным нормам и правилам, обучающему материалу и так далее, сформированную согласно структуре компании. СУБД должна иметь простую и понятную структуру.`

Для отдела контроля качества оптимальным выбором будет PostgreSQL с использованием JSONB полей. Ввиду простоты освоения, минимальное количество настроек, интуитивно понятным синтаксисом и обширной доступной документации.
Гибкость структуры подобного решения позволит хранить иерархические данные, даёт возможность быстрой модификации, поддерживает различные типы контента.
И конечно надежность в виде ACID-согласованности, отказоустойчивости и отработанными механизмами резервного копирования.

3. `* Можно ли под эту задачу использовать уже существующую СУБД из задач выше и если да, то как лучше это реализовать?`

Да, эту задачу можно реализовать на уже существующей PostgreSQL СУБД, используемой для лендингов и CRM. Реализация заключается в использование JSONB полей для гибкой структуры, в возможности расширения существующей схемы, в оптимизации запросов на уже настроенной системе. В итоге мы получим единые права и возможность создания отчётов по всем данным.


4. `Департамент логистики нуждается в решении задач по быстрому формированию маршрутов доставки материалов по объектам и распределению курьеров по маршрутам с доставкой документов. СУБД должна уметь быстро работать со связями.`

Для департамента логистики оптимальным выбором будет графовая СУБД. Основным преимущества графовых СУБД является скорость работы со связями, мгновенный поиск по связям между объектами, эффективная обработка графовых алгоритмов, быстрая маршрутизация. Не менее весомым преимуществом является структура данных: естественное представление связей, гибкая схема, иерархическая организация.
Можно использовать популярную и производительную графовую СУБД Neo4j с богатым функционалом.

4. `* Можно ли к этой СУБД подключить отдел закупок или для них лучше сформировать свою СУБД в связке с СУБД логистов?`

Для отдела закупок лучше создать отдельную СУБД, которая будет интегрирована с системой логистов.
**Причиной разделения является разная природа данных!**
Закупки: финансовые операции, контракты, поставщики
Логистика: маршруты, перемещения, статусы
Разная нагрузка: Закупки: периодические крупные операции
Логистика: постоянные мелкие операции

5. `* Можно ли все перечисленные выше задачи решить, используя одну СУБД? Если да, то какую именно?`

Да, все перечисленные задачи можно решить с помощью одной СУБД – PostgreSQL.

**PostgreSQL считается одной из самых универсальных СУБД с широкими техническими возможностями**: 
- **Поддержка различных типов данных**: реляционные данные, JSONB и XML, геоданные (с расширением PostGIS), бинарные данные, временные ряды.
- **Расширяемость**: пользовательские типы данных, функции на разных языках (C, Python, SQL), операторы и индексные методы, триггеры.
- **Стандарты и совместимость**: поддержка SQL и реализация большинства стандартов SQL, возможность работы с другими СУБД, совместимость с популярными фреймворками.
- **ACID-соответствие**: атомарность, согласованность, изолированность, надёжность.
- **Производительность и масштабируемость**: горизонтальное и вертикальное масштабирование.
- **Репликация, оптимизация, многопоточность, кэширование, параллельные запросы**.
- **Гибкость применения**: Веб-приложения, Хранение данных, Аналитика, Бизнес-логика. Корпоративные системы: ERP, CRM, Аналитические системы. Специальные задачи: Геоинформационные системы, Машинное обучение, Большие данные.
- **Дополнительные преимущества**: открытый исходный код и бесплатное использование, возможность модификации, активное камюнити сообщество.

---

### Задание 2 Транзакции

1. `Пользователь пополняет баланс счёта телефона, распишите пошагово, какие действия должны произойти для того, чтобы транзакция завершилась успешно. Ориентируйтесь на шесть действий.`

**Давайте разберем процесс пополнения баланса телефона пошагово**:

1) **Инициализация транзакции**
- Пользователь инициирует операцию пополнения;
- Система создает уникальный идентификатор транзакции; 
- Запускается таймер для отслеживания времени выполнения

2)	**Проверка параметров**
- Система проверяет корректность номера телефона
- Проверяет наличие достаточного баланса на карте
- Проверяет соответствие суммы лимитам
- Проверяет актуальность данных карты

3)	**Блокировка средств**
- Система блокирует необходимую сумму на счете
- Создается запись о временной блокировке
- Отправляется запрос на подтверждение блокировки
- Устанавливается время действия блокировки

4)	**Обработка платежа**
- Формируется платежный запрос
- Данные шифруются и передаются в процессинг
- Проводится проверка на мошенничество
- Выполняется авторизация платежа

5)	**Зачисление средств**
- Средства переводятся на счет мобильного оператора
- Создается запись о зачислении в системе оператора
- Обновляется баланс телефона
- Формируется подтверждающий документ

6)	**Завершение транзакции**
- Система получает подтверждение от всех участников
- Отменяет временную блокировку средств
- Создает финальную запись в истории операций
- Отправляет уведомление пользователю

**Важные технические аспекты**:
- Все данные передаются по защищенному протоколу
- Используется механизм двухфакторной аутентификации
- Применяется система контроля версий данных
- Ведется логирование всех операций

**При возникновении ошибок**:
- Система определяет тип ошибки
- Формирует соответствующее сообщение
- Предлагает варианты решения
- При необходимости отменяет транзакцию

**После успешного завершения**:
- Пользователь получает SMS/push-уведомление
- Информация о транзакции сохраняется в истории
- Обновляется баланс в приложении
- Создается электронный чек
- 
**Особенности безопасности**:
- Шифрование данных при передаче
- Контроль целостности данных
- Мониторинг подозрительной активности
- Резервное копирование данных
- 
**При необходимости отмены**:
- Система проверяет статус транзакции
- Формирует запрос на отмену
- Возвращает заблокированные средства
- Обновляет статус операции

Это стандартная последовательность действий, которая может немного варьироваться в зависимости от конкретного банка и способа пополнения.

При этом важно понимать, что хотя процесс включает несколько этапов, для пользователя он происходит практически мгновенно - обычно требуется всего несколько секунд для завершения всей операции.

1. `* Какие действия должны произойти, если пополнение счёта телефона происходило бы через автоплатёж?`

**Давайте разберем процесс автопополнения баланса телефона пошагово**:

1.	**Подготовка к автоплатежу**
- Пользователь настраивает параметры автоплатежа в приложении/системе.
- Указывает: номер телефона, сумму пополнения, минимальный порог баланса, периодичность, способ оплаты.
- Связывает банковскую карту с сервисом
- Подтверждает настройки через SMS/push-уведомление

2.	**Мониторинг баланса**
- Система регулярно проверяет баланс телефона
- Устанавливает триггеры на заданный минимальный порог
- Ведет логирование изменений баланса
- Сохраняет историю проверок

3.	**Инициализация автоплатежа**
- При достижении минимального порога: создается запрос на пополнение, формируется идентификатор транзакции, блокируются необходимые средства.
- Система проверяет: доступность средств на карте, актуальность настроек, статус карты.

4.	**Обработка платежа**
- Система формирует платежный запрос
- Шифрует данные и передает в процессинг
- Проводит проверку на мошенничество
- Выполняет авторизацию платежа
- Блокирует сумму для перевода

5.	**Зачисление средств**
- Средства переводятся на счет оператора
- Обновляется баланс телефона
- Создается запись о зачислении
- Формируется подтверждающий документ

6.	**Завершение операции**
- Система получает подтверждение от всех участников
- Отменяет временную блокировку средств
- Создает запись в истории операций
- Отправляет уведомление пользователю

**Особенности автоплатежа**:
- Возможность настройки расписания
- Установка лимитов и ограничений
- Автоматический контроль баланса
- История всех операций

**Дополнительные функции**:
- Приостановка автоплатежа
- Изменение параметров
- Отключение сервиса
- Настройка уведомлений

**Безопасность**:
- Двухфакторная аутентификация при настройке
- Шифрование данных
- Мониторинг подозрительной активности
- Возможность отмены операции

**При возникновении проблем**:
- Система отправляет уведомление
- Предлагает варианты решения
- Приостанавливает автоплатеж
- Формирует заявку в службу поддержки

**После успешного завершения**:
- Пользователь получает уведомление
- Обновляется информация в приложении
- Создается запись в истории
- Сохраняется электронный чек

**Важные моменты**:
- Возможность настройки исключения определенных дней
- Установка максимального лимита расходов
- Выбор способа уведомлений
- Возможность синхронизации с другими сервисами

При достижении минимального порога система автоматически запускает процесс пополнения, следуя заранее заданным настройкам и правилам безопасности.

---

### Задание 3 SQL vs NoSQL

`Приведите ответ в свободной форме........`

1. ` Напишите пять преимуществ SQL-систем по отношению к NoSQL.`

**5 ключевых преимуществ SQL-систем по сравнению с NoSQL:**

1.	**Строгая целостность данных**
- Наличие первичных и внешних ключей
- Контроль типов данных
- Механизмы проверки ограничений
- Триггеры для бизнес-логики
- Гарантированное соблюдение связей между данными

2.	**Соответствие ACID**
- Атомарность операций
- Согласованность данных
- Изолированность транзакций
- Долговечность изменений
- Надежное выполнение операций даже при сбоях

3.	**Развитая экосистема**
- Множество инструментов администрирования
- Обширная документация
- Большое сообщество разработчиков
- Множество фреймворков и библиотек
- Хорошая поддержка от производителей

4.	**Стандартизированный язык запросов**
- Единый SQL-диалект
- Переносимость между разными СУБД
- Широкие возможности для анализа данных
- Поддержка сложных JOIN-операций
- Возможность создания подзапросов
- Управление структурированными данными

5.	**Четкая схема базы данных**
- Фиксированная структура таблиц
- Эффективное хранение связанных данных
- Оптимизированные индексы
- Быстрая работа с небольшими наборами данных

**Эти преимущества делают SQL-системы особенно подходящими для**:
- Финансовых приложений
- Систем электронной коммерции
- Корпоративных приложений
- Транзакционных систем
- Аналитических платформ

Важно отметить, что выбор между SQL и NoSQL должен основываться на конкретных требованиях проекта, так как у каждой технологии есть свои сильные и слабые стороны.


1. `* Какие, на ваш взгляд, преимущества у NewSQL систем перед SQL и NoSQL.`

**NewSQL системы имеют следующие ключевые преимущества перед SQL и NoSQL:**

**Преимущества перед SQL**:

1.	**Горизонтальное масштабирование**
- Возможность добавления узлов для увеличения производительности
- Линейное увеличение производительности с добавлением ресурсов
- Эффективное распределение нагрузки

2.	**Высокая доступность**
- Автоматическая репликация данных
- Отказоустойчивость
- Минимальное время простоя

3.	**Распределенная архитектура**
- Многоуровневая структура (административный, транзакционный и storage уровни)
- Оптимизация производительности
- Гибкая настройка конфигурации

**Преимущества перед NoSQL**:

1.	**Сохранение ACID-свойств**
- Атомарность операций
- Согласованность данных
- Изолированность транзакций
- Долговечность изменений

2.	**Поддержка SQL**
-  Знакомый язык запросов
- Возможность выполнения сложных запросов
- Поддержка JOIN-операций
- Работа со структурированными данными

3.	**Транзакционная обработка в реальном времени**
- Полная поддержка онлайн-транзакций
- Высокая производительность
- Низкое время отклика

4.	**Облачная совместимость**
- Нативная поддержка облачных сред
- Гибкая конфигурация
- Оптимизация для распределенных вычислений

5.	**Гибкость масштабирования**
- Возможность масштабирования как вертикально, так и горизонтально
- Автоматическая балансировка нагрузки
- Оптимизация использования ресурсов

6.	**Улучшенная обработка сложных запросов**
- Распределенные вычисления
- Оптимизация запросов
- Эффективное использование индексов

Практическое применение NewSQL особенно эффективно в высоконагруженных транзакционных системах, финансовых приложениях, систем электронной коммерции, распределенных системах с высокими требованиями к согласованности, приложениях, требующих масштабируемости NoSQL и надежности SQL.

NewSQL представляет собой компромиссное решение, объединяющее лучшие черты обеих технологий, что делает его особенно привлекательным для современных высоконагруженных приложений с высокими требованиями к надежности и масштабируемости.



### Задание 4 Кластеры

1. `Необходимо производить большое количество вычислений при работе с огромным количеством данных, под эту задачу выделено 1000 машин.`

На основе какого критерия будете выбирать тип СУБД и какая модель распределённых вычислений здесь справится лучше всего и почему?

Основным критерием выбора СУБД в данном случае является способность системы к горизонтальному масштабированию и параллельной обработке данных. 

**Необходимость эффективного использования 1000 машин требует:**
- Возможности распределения нагрузки между узлами
- Автоматического балансирования ресурсов
- Способности к линейному масштабированию производительности.

Оптимальная модель распределенных вычислений – MapReduce на базе Hadoop экосистемы, которая позволяет на базе HDFS распределить хранение данных, использовать YARN для управления ресурсами кластера, использовать MapReduce для обработки данных.

**Преимущества MapReduce**:
- Разделение большого массива данных на части
- Параллельная обработка каждой части на отдельном узле
- Финальное объединение результатов
- Эффективное использование вычислительных ресурсов
- Автоматическое распределение нагрузки
- Обработка данных “на месте” (data locality)

При необходимости можно также рассмотреть гибридные решения, сочетающие элементы NewSQL и NoSQL для достижения оптимального баланса между консистентностью данных и масштабируемостью.
