# План автоматизации тестирования покупки тура "Марракэш"

## *1. Перечень автоматизируемых сценариев:*

### Возможность покупки тура _двумя_ способами:
1. Оплата по дебетовой карте
2. Оплата через кредит по данным карты

### Карты, представленные для тестирования (файл data.json):
- 4444 4444 4444 4441, status APPROVED
- 4444 4444 4444 4442, status DECLINED

### *Позитивные сценарии:*
**1.** Оплата по дебетовой карте со статусом APPROVED:
   
#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить"
3. Ввести в поле "Номер карты" - 4444 4444 4444 4441
4. Ввести в поле "Месяц" - любое число от 01 до 12
5. Ввести в поле "Год" - последние две цифры года (например: 24)
6. Ввести в поле "Владелец" - валидное имя (например: Sergey)
7. Ввести в поле "CVC/CVV" - 3-х значный номер (например: 123)
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: появилось всплывающее окно "Операция одобрена Банком", в БД в payment_entity появилась запись со статусом APPROVED

**2.** Оплата по дебетовой карте со статусом DECLINED:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить"
3. Ввести в поле "Номер карты" - 4444 4444 4444 4442
4. Ввести в поле "Месяц" - любое число от 01 до 12
5. Ввести в поле "Год" - последние две цифры года (например: 24)
6. Ввести в поле "Владелец" - валидное имя (например: Petr)
7. Ввести в поле "CVC/CVV" - 3-х значный номер (например: 123)
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: появилось всплывающее окно "Ошибка! Банк отказал в проведении операции", в БД в payment_entity появилась запись со статусом DECLINED

**3.** Оплата через кредит по данным карты со статусом APPROVED:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить в кредит"
3. Ввести в поле "Номер карты" - 4444 4444 4444 4441
4. Ввести в поле "Месяц" - любое число от 01 до 12
5. Ввести в поле "Год" - последние две цифры года (например: 24)
6. Ввести в поле "Владелец" - валидное имя (например: Vaycheslav)
7. Ввести в поле "CVC/CVV" - 3-х значный номер (например: 123)
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: появилось всплывающее окно "Операция одобрена Банком", в БД в credit_request_entity появилась запись со статусом APPROVED

**4.** Оплата через кредит по данным карты со статусом DECLINED:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить в кредит"
3. Ввести в поле "Номер карты" - 4444 4444 4444 4442
4. Ввести в поле "Месяц" - любое число от 01 до 12
5. Ввести в поле "Год" - последние две цифры года (например: 24)
6. Ввести в поле "Владелец" - валидное имя (например: Dmitriy)
7. Ввести в поле "CVC/CVV" - 3-х значный номер (например: 123)
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: появилось всплывающее окно "Ошибка! Банк отказал в проведении операции", в БД в credit_request_entity появилась запись со статусом DECLINED

### *Негативные сценарии:*

**1.** Оплата по карте, указан невалидный номер карты:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить"
3. Ввести в поле "Номер карты" - некорректный номер (например: 4444 4444 4444)
4. Ввести в поле "Месяц" - любое число от 01 до 12
5. Ввести в поле "Год" - последние две цифры года (например: 24)
6. Ввести в поле "Владелец" - валидное имя (например: Daria)
7. Ввести в поле "CVC/CVV" - 3-х значный номер (например: 123)
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: внизу поля "Номер карты" появилось сообщение об ошибке "Неверный формат"

**2.** Оплата по карте, указан невалидный месяц:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить"
3. Ввести в поле "Номер карты" - 4444 4444 4444 4441
4. Ввести в поле "Месяц" - число 00 или число 13
5. Ввести в поле "Год" - последние две цифры года (например: 24)
6. Ввести в поле "Владелец" - валидное имя (например: Sergei)
7. Ввести в поле "CVC/CVV" - 3-х значный номер (например: 123)
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: внизу поля "Месяц" появилось сообщение об ошибке "Неверно указан срок действия карты"

**3.** Оплата по карте, указан год с истекшим сроком действия:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить"
3. Ввести в поле "Номер карты" - 4444 4444 4444 4441
4. Ввести в поле "Месяц" - любое число от 01 до 12
5. Ввести в поле "Год" - последние две цифры года (например: 23 или 30)
6. Ввести в поле "Владелец" - валидное имя (например: Vasiliy)
7. Ввести в поле "CVC/CVV" - 3-х значный номер (например: 123)
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: внизу поля "Год" появилось сообщение об ошибке "Истёк срок действия карты" или "Неверно указан срок действия карты"

**4.** Оплата по карте, указано некорректное имя:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить"
3. Ввести в поле "Номер карты" - 4444 4444 4444 4441
4. Ввести в поле "Месяц" - любое число от 01 до 12
5. Ввести в поле "Год" - последние две цифры года (например: 24)
6. Ввести в поле "Владелец" - некорректное имя (например: abc или 098)
7. Ввести в поле "CVC/CVV" - 3-х значный номер (например: 123)
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: внизу поля "Владелец" появилось сообщение об ошибке "Неверное имя" 

**5.** Оплата по карте, указано некорректное значение CVC/CVV:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить"
3. Ввести в поле "Номер карты" - 4444 4444 4444 4441
4. Ввести в поле "Месяц" - любое число от 01 до 12
5. Ввести в поле "Год" - последние две цифры года (например: 24)
6. Ввести в поле "Владелец" - валидное имя (например: Alexander)
7. Ввести в поле "CVC/CVV" - 2-х значный номер (например: 12)
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: внизу поля "CVC/CVV" появилось сообщение об ошибке "Неверный формат"

**6.** Оплата по карте, указаны пустые поля:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить"
3. Оставить все поля пустыми
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: под каждым из полей появляется сообщение "Неверный формат", только под полем "Владелец" появляется сообщение "Поле обязательно для заполнения"

**7.** Купить тур в кредит и указать невалидный номер карты:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить в кредит"
3. Ввести в поле "Номер карты" - некорректный номер (например: 4444 4444 4444)
4. Ввести в поле "Месяц" - любое число от 01 до 12
5. Ввести в поле "Год" - последние две цифры года (например: 24)
6. Ввести в поле "Владелец" - валидное имя (например: Vladimir)
7. Ввести в поле "CVC/CVV" - 3-х значный номер (например: 123)
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: внизу поля "Номер карты" появилось сообщение об ошибке "Неверный формат"

**8.** Кредит по данным карты, указан невалидный месяц:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить в кредит"
3. Ввести в поле "Номер карты" - 4444 4444 4441
4. Ввести в поле "Месяц" - число 00 или число 13
5. Ввести в поле "Год" - последние две цифры года (например: 24)
6. Ввести в поле "Владелец" - валидное имя (например:Yulia)
7. Ввести в поле "CVC/CVV" - 3-х значный номер (например: 123)
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: внизу поля "Месяц" появилось сообщение об ошибке "Неверно указан срок действия карты"

**9.** Кредит по данным карты, указан год с истекшим сроком действия:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить в кредит"
3. Ввести в поле "Номер карты" - 4444 4444 4444 4441
4. Ввести в поле "Месяц" - любое число от 01 до 12
5. Ввести в поле "Год" - последние две цифры года (например: 23 или 30)
6. Ввести в поле "Владелец" - валидное имя (например: Alexander)
7. Ввести в поле "CVC/CVV" - 3-х значный номер (например: 123)
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: внизу поля "Год" появилось сообщение об ошибке "Истёк срок действия карты" или "Неверно указан срок действия карты"

**10.** Кредит по данным карты, указано некорректное имя:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить в кредит"
3. Ввести в поле "Номер карты" - 4444 4444 4444 4441
4. Ввести в поле "Месяц" - любое число от 01 до 12
5. Ввести в поле "Год" - последние две цифры года (например: 24)
6. Ввести в поле "Владелец" - некорректное имя (например: ab или 098)
7. Ввести в поле "CVC/CVV" - 3-х значный номер (например: 123)
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: внизу поля "Владелец" появилось сообщение об ошибке "Неверное имя" 

**11.** Кредит по данным карты, указано некорректное значение CVC/CVV:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить в кредит"
3. Ввести в поле "Номер карты" - 4444 4444 4444 4441
4. Ввести в поле "Месяц" - любое число от 01 до 12
5. Ввести в поле "Год" - последние две цифры года (например: 24)
6. Ввести в поле "Владелец" - валидное имя (например: Alexander)
7. Ввести в поле "CVC/CVV" - 2-х значный номер (например: 12)
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: внизу поля "CVC/CVV" появилось сообщение об ошибке "Неверный формат"

**12.** Кредит по данным карты, указаны пустые поля:

#### Шаги:
1. Открыть в браузере страницу http://localhost:8080/
2. Кликнуть по кнопке "Купить в кредит"
3. Оставить все поля пустыми
8. Кликнуть по кнопке "Продолжить"

*Ожидаемый результат*: под каждым из полей появляется сообщение "Неверный формат", только под полем "Владелец" появляется сообщение "Поле обязательно для заполнения"

## *2. Перечень используемых инструментов с обоснованием выбора:*

- **IntelliJ IDEA 2021.1.3** - среда разработки, мощная и удобная IDE для java-разработки с поддержкой всех последних технологий и фреймворков.
- **Java 11** язык для написания авто-тестов, имеет набор готового ПО для разработки и запуска приложений.
- **Gradle** понадобится для сборки проекта, для управления подключенными зависимостями, а так же для генерации отчётов о тестировании. Он прост в использовании благодаря тому, что билд скрипты короче и чище чем у Ant и Maven.
- **JUnit 5** для написания и запуска тестов. Не требует контроля пользователя во время исполнения тестов, может запускать одновременно несколько тестов, сообщает обо всех ошибках в ходе тестирования, предоставляет готовый набор методов для сравнения ожидаемого и фактического результатов.
- **Docker** — это программное обеспечение, которое дает возможность на определенном участке памяти изолированно установить необходимую ОС (операционную систему), версию Java, настроить переменные окружения, установить различные зависимости и дать доступ только при определенных условиях (также это инструмент, позволяющий запускать мультиконтейнерные приложения, чтобы не устанавливать на компьютер необходимые для работы приложения Node.js и СУБД).
- **Selenide**, чтобы протестировать GUI. Помогает делать стабильные тесты, решая почти все проблемы с таймаутами, автоматически управляет браузером и делает скриншоты если тест упал.
- **Lombok** позволяет оптимизировать код авто-тестов, добавляет в Java новые «ключевые слова» и превращает аннотации в Java-код, уменьшая усилия на разработку и обеспечивая некоторую дополнительную функциональность.
- **Faker** для генерации тестовых данных. Эту библиотеку можно использовать для создания широкого спектра реально выглядящих данных от адресов до телефонных номеров.
- **Rest Assured** - java-библиотека для тестирования REST API, позволяет автоматизировать тестирование get и post запросов.
- **Allure** - фреймворк для создания отчетов о тестировании, для наглядного отображения прохождения тестов и ошибок.
- **Git и GitHub** для хранения кода. Git достаточно прост и удобен для управления исходным кодом, очень распространенная система контроля версий, поэтому достаточно хорошо взаимодействует с различными ОС. GitHub специализированный веб-сервис с удобным интерфейсом, интегрирован с Git.

## *3. Перечень и описание возможных рисков при автоматизации:*

- Невозможность запуска веб-приложения и работы с ним: приложения не загружается по указанному адресу, происходит ошибка на стороне сервера (статус 500)
- Проблемы с составлением селекторов элементов из-за отсутствия уникальных идентификаторов
- Если программисты запилили новую фичу, а нас тестировщиков не предупредили
- Возможна смена кода страницы либо css-селекторов, придется править код
- Нет реальной документации к сервису
- Проблемы с интернет соединением

## *4. Интервальная оценка с учётом рисков (в часах):*

### Необходимое время на тестирование веб-сервера составляет 41 час, с учетом рисков - 55 часов:

- подготовка окружения, инфраструктуры, развертывание БД - 8 часов
- написание авто-тестов, тестирование - 30 часов
- формирование и анализ отчетов - 8 часов

## *5. План сдачи работ:*

- Планирование автоматизации тестирования: с 21.11.2024 до 22.11.2024
- Непосредственно сама автоматизация: с 23.11.2024 до 07.12.2024
- Подготовка отчётных документов по итогам автоматизированного тестирования: с 08.12.2024 до 10.12.2024
- Подготовка отчётных документов по итогам автоматизации: с 10.12.2024 до 11.12.2024
