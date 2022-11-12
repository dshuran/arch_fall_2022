# Проект по архитектуре

## Описание задачи

В рамках проекта будет создана библиотека и спецификация для работы
с файловым хранилищем.

### Спецификация

Будут реализованы методы чтения и модификации хранилища по ключам. 
Также будет возможность проитерироваться по всем ключам хранилища

Реализуем интерфейс, чтобы хранилище могло работать с различными типами данных. 
Приложение планируем сделать на языке Java. Предусмотрим возможность различных
реализаций работы хранилища, для того чтобы можно было подключать различные хранилища для
соответствующих типов данных.

### Базовая реализация

#### Выбор библиотеки

Базовая реализация спецификации хранилища написана при помощи библиотеки
[mapdb](https://mapdb.org/). Она включает в себя встроенную базу данных, 
работающую на базе коллекций Java (получается тесная интеграция). Существует
множество полезных применений данного решения, мы же будем использовать как раз
возможность использовать её в качестве персистентного хранилища данных
с удобными ConcurrentMap Java-интерфейсом. Это позволит нам не писать
дополнительных абстракций при работе с библиотекой. Поэтому она и была выбрана.

#### Хранение данных

В данной реализации мы решили хранить данные в бинарном формате, т.к. это
более удобно и занимает меньше места на диске. 

#### Сериализация данных

Для сериализации данных используются нативные возможности Java. Достаточно
реализовать интерфейс Serializable, и базовые DTO-объекты уже могут быть
сериализованы

#### Функционал решения

Весь функционал решения сосредоточен в файле KeyValueStorageImpl.java
Он поддерживает несколько типов данных, реализует интерфейс KeyValueStorage
и под капотом использует библиотеку mapdb, описанную выше

### Тестовая система

Будет разработана тестовая система, содержащая определённый набор тестов для
проверки качества конкретной реализации хранилища. Для реализации тестов
предлагается использовать фреймворк JUnit, который хорошо зарекомендовал себя

Какие области хотим покрыть нашим набором тестов:
- Тесты чтения и записи из хранилища
- Проверка того, что данные сохраняются в хранилище между сеансами 
(персистентность)
- Проверка, что при отсутствии ключа в хранилище вернётся нулевое значение
- Проверка нескольких модификаций: многочисленные модификации хранилища
в рамках одной сессии должны выполняться успешно
- Проверка возможности работы с данными, которые были скопированы
- Проверка того, что нельзя записать данные в массив, пока он читается
- Проверка невозможности записи данных при закрытой сессии в БД


## Декомпозиция задачи

Декомпозицию производим по схеме Аббот-CRM-UML

### Метод Аббота

Метод Аббота заключается в словесном анализе предметной области и получении ее словаря
и объектно-ориентированного словаря. Согласно этому методу надо описать задачу
на обычном языке, а потом подчеркнуть существительные и глаголы.
Существительные – кандидаты на роль классов, а глаголы могут стать именами методов

1. Существительные: База данных, Ключ, Значение, Хранилище, Итератор, Итератор ключей,
Итератор значений, Итератор пар ключ-значение, Пара ключ-значение, Коллекция ключей,
Коллекция значений, Коллекция пар ключ-значение, Коллекция ключей-значений.
2. Глаголы: Читать, Записывать, Удалять, Итерироваться, Получать, Сохранять.

### CRC-карта

CRC-карта – это таблица, в которой для каждого класса описываются его зоны
ответственности

| Класс                         | Зона ответственности                                                                                                                                                |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| KeyValueStorage               | Запись, чтение и удаление данных                                                                                                                                    | 
| KeyValueStorageImpl           | Реализация абстрактного класса KeyValueStorage                                                                                                                      | 
| AbstractSingleFileStorageTest | Абстрактный класс для тестирования хранилища. Содержит готовые экземпляры тестовых сущностей и тесты к ним. Реализацию хранилища требуется определить в наследнике. | 
| KeyValueStorageFactories      | Абстрактный класс фабрики для создания хранилища                                                                                                                    | 
| StorageTestUtils              | Вспомогательные методы для тестирования хранилища                                                                                                                   |  
| Student                       | Тестовая сущность для тестирования базы данных. В базе данных используется в виде значения по определённому ключу                                                   |  
| StudentKey                    | Тестовая сущность для тестирования базы данных. В базе данных используется в виде ключа                                                                             |

### UML-диаграмма

UML-диаграмма – это диаграмма, которая показывает структуру классов и их взаимодействие.

![UML-диаграмма](uml_diagram_full.svg)


