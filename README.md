# Sprint 3

## Задание 1. Анализ и планирование

### Определение доменов и границ контекстов (вариант AS-IS)

- Управление устройствами
  - вкл./выкл. отопления
  - настройка отопления, установка желаемой температуры

- Мониторинг температуры
  - получение текущей температуры

### Анализ архитектуры монолитного приложения

Проблемы бизнес-задач:

- Сложность добавления нового функционала;
- Невозможность масштабирования части функционала;
- Невозможно подключать устройства самостоятельно, требуется вызов мастера
- При повышении количества пользователей будет расти нагрузка с которой приложение не справится;
- В связи с предыдущей причиной, синхронное взаимодействие будет узким местом в части увеличения длительности ответа пользователю;
- При обновлении приложения требует его полной остановки, что приведет к недоступности приложения пользователю.

### Визуализируйте контекст системы

Приложена модель c4, лежит тут: c4-diagrams\current-context\C4_current_context.puml

## Задание 2. Проектирование микросервисной архитектуры

### Декомпозируйте приложение на микросервисы (варинт TO-BE)

Далее 1 "Домен" = 1 микросервису. (Вариант AS-IS разобран в задании 1)

- Домен: управление устройствами
  - поддомен: добавление новых устройств
  - поддомен: настройка устройств
    - конекст: вкл./выкл. устройств
    - конекст: перенастройка устройств

- Домен: управление сценариями
  - поддомен: настройка сценария
    - контекст: создание/перенастройка/удаление сценария
    - контекст: установка расписаний

- Домен: управление пользователями
  - поддомен: создание/удаление пользователя
  - поддомен: редактирование профиля
  - поддомен: добавление в группу "Семья"

- Домен: мониторинг устройств
  - поддомен: просмотр данных с датчиков устройства
  - поддомен: статус устройства
    - контекст: последняя дата обслуживания
    - контекст: заряд(при наличии)

- Домен: уведомления
  - поддомен: отправка уведомлений

- Домен: поддержка пользователей
  - поддомен: создание заявки
    - контекст: чат с тех. поддержкой
    - контекст: вызов мастера
  - поддомен: обработка заявки

### Определите взаимодействия между

В реализации используем:

- API Gateway - для маршрутизации запросов через одну точку входа к мискросервисам;
- Kafka - для возможности асинхронного взаимодействия;
- Базы данных - 1 БД = 1 микросервис, везде используем PostgreSQL.

### Визуализируйте архитектуры:

C4 — Уровень контейнеров (Containers).
c4-diagrams\to-be\task2.3.1-container.puml

C4 — Уровень компонентов (Components).
c4-diagrams\to-be\task2.3.2-components.puml

C4 — Уровень кода (Code).
c4-diagrams\to-be\task2.3.3-code.puml

## Задание 3. Разработка ER-диаграммы

Диаграмма лежит тут:
c4-diagrams\to-be\task3-er.puml

## Опишите связи

1. Пользователь — Дом: Один пользователь может иметь доступ к нескольким домам, дом может быть связан только с несколькими пользователями (многие-ко-многим). Так будет реализована функция "добавить в семью" для предоставления управлением умным домом для членов семьи;
2. Дом — Устройство. Один дом может содержать множество устройств, и каждое устройство принадлежит только одному дому (один-ко-многим);
3. Устройство — Тип устройства. Одно устройство может иметь только один его тип. (один-к-одному);
4. Устройство — Сценарий. Одно устройство может иметь несколько сценариев, но сценарий относится только к одному устройству (один-ко-многим).