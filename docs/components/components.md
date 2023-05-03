# Компонентная архитектура
<!-- Состав и взаимосвязи компонентов системы между собой и внешними системами с указанием протоколов, ключевые технологии, используемые для реализации компонентов.
Диаграмма контейнеров C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783368
-->
## Контейнерная диаграмма

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(visitor, "Зритель", "Участник, смотрящий доклады. Основной пользователь системы")
Person(speaker, "Докладчик", "Специалист, выступающий с докладом")
Person(committee_member, "Член ПК", "Специалист, отбирающий доклады и оставляющий на них отзывы")
Person(administrator, "Админстратор", "Пользователь с максимальными правами")

System_Boundary(c, "HelloConf") {
   Container(conference_service, "Конференция", "Java, Spring Boot, Thymeleaf, HTML, CSS", "Автоматизация проведения конференций")

   ContainerDb(conference_db, "Пользователи и доклады", "PostgreSQL", "", $tags = "storage")

   ContainerQueue(notification_queue, "Очередь сообщений", "RabbitMQ", "Команды на отправку уведомлений")

   Container(notification_service, "Уведомления", "Java, Spring Boot", "Сервис отправки уведомлений")

   ContainerDb(notification_db, "Журнал рассылки", "PostgreSQL", "", $tags = "storage")
}

System_Ext(mail_system, "SMTP-сервер", "Почтовый сервер")
System_Ext(youtube, "YouTube", "Стриминговый сервис. Видеохостинг")

Rel_D(visitor, conference_service, "Читает информацию о конференции, смотрит доклады", "WebUI")
Rel_D(speaker, conference_service, "Подаёт заявку на доклад, дорабатывает доклад", "WebUI")
Rel_D(committee_member, conference_service, "Оценивает доклады", "WebUI")
Rel_D(administrator, conference_service, "Ведёт расписание, выполняет администрирование", "WebUI")
Rel_R(conference_service, notification_queue, "Отправка сообщений (Уведомление)", "AMQP")
Rel_R(notification_queue, notification_service, "Передача сообщений (Уведомление)", "AMQP")
Rel_D(notification_service, notification_db, "Сохранение журнала отправки уведомлений", "JDBC, SQL")
Rel(conference_service, conference_db, "Сохранение докладов \n(Доклад)\n \nСохранение комментариев \n(Комментарий)\n \n Сохранение расписания \n(Расписание)", "JDBC, SQL")

Rel_R(notification_service, mail_system, "Отправка уведомлений", "SMTP")
Rel_U(visitor, youtube, "Просмотр видеотрансляции доклада", "HTTP")
Rel_U(speaker, youtube, "Трансляция доклада", "OBS Studio")

SHOW_LEGEND()
@enduml
```

## Список компонентов
| Компонент             | Роль/назначение                  |
|:----------------------|:---------------------------------|
| *Название компонента* | *Описание назначения компонента* |