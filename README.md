# Домашнее задание к занятию 1.  «Введение в виртуализацию» "Макарцев Алексаднр Владимирович"

#### Это задание для самостоятельной отработки навыков и не предполагает обратной связи от преподавателя. Его выполнение не влияет на завершение модуля. Но мы рекомендуем его выполнить, чтобы закрепить полученные знания.  Все вопросы, возникающие в процессе выполнения заданий, пишите в учебный чат или в раздел "Вопросы по заданиям" в личном кабинете.

### Цели задания
1. Научиться запускать виртуальную машину в Yandex Cloud с минимальным расходом ресурсов.
2. Попрактиковаться в выборе платформы  и системы управления виртуализации для решения требуемых задач.

### Инструкция к выполению

1. Для выполнения задачи 1 ознакомьтесь с [инструкцией](https://github.com/netology-code/devops-materials/blob/master/cloudwork.MD) по экономии облачных ресурсов и затем выполните задачу 1 по шагам.
2. Своё решение к задачам 2,3,4 загрузите  в ваш ЛК.
   
## Задача 1

Ознакомьтесь с [инструкцией ](https://github.com/netology-code/devops-materials/blob/master/cloudwork.MD) по экономии облачных ресурсов.


1. Создайте через web-интерфейс Yandex Cloud - VPC и виртуальную машину из инструкции конфигурации "эконом-ВМ" с публичным ip-адресом. В пункте "Выбор образа/загрузочного диска" выберите вкладку "Cloud Marketplace" , щелкните "Посмотреть больше", найдите образ "Yandex Cloud Toolbox".
2. Убедитесь, что вы можете подключиться к консоли ВМ через ssh, используя публичный ip-адрес. Убедитесь, что на ВМ установлен Docker с помощью команды ```docker --version```(команду выполните от имени root пользователя) !
3. Узнайте в инструкции Яндекс, какие еще инструменты предустановлены в данном образе.
4. Оставьте ВМ работать, пока она не выключится самостоятельно! Опция "прерываемая" выключит ее не позже чем через 24 часа. 
5. Для наглядности подождите еще 1 сутки.
6. Перейдите по [ссылке ](https://console.cloud.yandex.ru/billing?section=accounts). Выберите свой платежный аккаунт. Перейдите на вкладку детализация (фильтр "По продуктам") и оцените график потребления финансов.
7. Удалите ВМ или пользуйтесь ею при выполнении последующих домашних заданий курса обучения.

---


## Задача 2

Выберите один из вариантов платформы в зависимости от задачи. Здесь нет однозначно верного ответа так как все зависит от конкретных условий: финансирование, компетенции специалистов, удобство использования, надежность, требования ИБ и законодательства, фазы луны.

Тип платформы:

- физические сервера;
- паравиртуализация;
- виртуализация уровня ОС;

Задачи:

- высоконагруженная база данных MySql, критичная к отказу;
- различные web-приложения;
- Windows-системы для использования бухгалтерским отделом;
- системы, выполняющие высокопроизводительные расчёты на GPU.

Объясните критерии выбора платформы в каждом случае.

#### Решение

| Условия использования                                      | Организация серверов                | Пояснение|
|------------------------------------------------------------|-------------------------------------|----------|
| Высоконагруженная база данных, чувствительная к отказу     | физические сервера                  | Система всего одна, поэтому виртуализация - оверхед и увеличивает латентность. В высоконагруженной системе это нежелательно, а достичь отказоустойчивости можно резервированием. Постоянно нагруженной системе потребуется максимум ресурсов хоста, "соседи" могут их отбирать.|
| Различные web-приложения                                   | виртуализация уровня ОС             | Бест практис для веб-приложений - разворачивать часто и быстро, на хосте или или кластере хостов с оркестрацией, при этом нужно обеспечить их изоляцию друг от друга. Виртуализация ОС подходит лучше всего, так свернуть приложение в контейнер и развернуть из него быстрей, чем делать это с виртуальными машинами с полноценной ОС и отдельным ядром.|
| Windows системы для использования бухгалтерским отделом    | паравиртуализация                   | Виртуализация поможет системе быть более отказоустойчивой, из предложенных вариантов возможна только паравиртуализация, но я бы для Windows выбрал полную hardware-assisted виртуализацию, т.к. преимущества паравиртуализации в современных реалиях теряются|
| Системы, выполняющие высокопроизводительные расчеты на GPU | физ.сервера/виртуализация уровня ОС | Зависит от целей. Если систем одна/мало - лучше физический сервер, который будет утилизировать все ресурсы самостоятельно. Если систем много (куча микросервисов с машинным обучением), то виртуализация средствами ОС предпочтительна, т.к. можно использовать GPU. Если же это какие-то а-ля игровые сервера, то из "не предложенных" - полная виртуализация VMWare ESXi, которая поддерживает технологию vGPU|

## Задача 3

Выберите подходящую систему управления виртуализацией для предложенного сценария. Опишите ваш выбор.

Сценарии:

1. 100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований. Преимущественно Windows based-инфраструктура, требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий.
2. Требуется наиболее производительное бесплатное open source-решение для виртуализации небольшой (20-30 серверов) инфраструктуры на базе Linux и Windows виртуальных машин.
3. Необходимо бесплатное, максимально совместимое и производительное решение для виртуализации Windows-инфраструктуры.
4. Необходимо рабочее окружение для тестирования программного продукта на нескольких дистрибутивах Linux.

#### Решение

1. VMWare ESXi и Hyper-V обладают нужным функционалом. Учитывая Windows-based ориентацию инфраструктуры, то Hyper-V подойдёт лучше, но шире функционал у VMWare ESXi 
2. KVM - наиболее производительное решение, позволяет запускать Windows и Linux, опенсорсный, нетребовательный к ресурсам и довольно функциональный. Можно также использовать Xen - он бесплатный и опен-сорсный, но по бенчмаркам медленнее
3. Лучше использовать Hyper-V, так как он наиболее адаптирован под семейство Windows и начиная с Win 10 предустановлен в ОС.
4. KVM бесплатен и входит в состав ядра линукс. Но обычно тестирование необходимо автоматизировать, а это достигается через CI/CD, поэтому правильнее будет использовать Docker-контейнеры, ну либо поды k8s. Если же это что-то десктопное и ручное, то наверное можно и VirtualBox+Vagrant для быстрого поднятия тестовых машин  

## Задача 4

Опишите возможные проблемы и недостатки гетерогенной среды виртуализации (использования нескольких систем управления виртуализацией одновременно) и что необходимо сделать для минимизации этих рисков и проблем. Если бы у вас был выбор, создавали бы вы гетерогенную среду или нет?

#### Решение

Основные проблемы гетерогенной среды: 
* увеличиваются требования к персоналу в части навыков эксплуатации разнородного ПО виртуализации,
* увеличиваются операционные расходы в случае использования нескольких проприетарных систем управления виртуализацией,
* усложняются операции по переносу виртуальных машин между различными средами и масштабирование.
 
Но, кажется, её сложно избежать. В основном организации, особенно крупные, работают в гибридной/гетерогенной среде. Часто остаются физические сервера - либо легаси, либо по требованию регуляторов (ФСБ, ФСТЭК, ЦБ). 
Затем компания чаще всего выбирает пошаговую стратегию виртуализации ИТ-инфраструктуры, постепенно внедряя решения от различных производителей. Такой подход подразумевает, что определенные типы устройств выполняют соответственные задачи, и при этом компания не зависит от продукции одного вендора.
   
Для минимизации проблем нужно либо постараться перейти к однородной инфраструктуре, 
либо уметь поддерживать все эти инфраструктуры, а также найти способ решить задачи по миграции виртуальных машин в случае, когда это понадобится, сделать процессы понятнее и прозрачнее для всех команд.
   
Если бизнес-процесс требует повышенной надежности (часть ресурсов локально, часть в облаке) или особых требований (регуляторы), то без гетерогенной среды не обойтись. 
Надо освоиться в обеих средах и выработать практику работы с ними. Если такой задачи нет, то сама по себе гетерогенная среда "ради самой себя" или потому что "так сложилось" - это скорее проблема, от которой надо уйти. 
Если такой задачи нет, то сама по себе гетерогенная среда "ради самой себя" или потому что "так сложилось" - это скорее проблема, от которой надо уйти

### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.
