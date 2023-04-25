# Контекст решения
<!-- Окружение системы (роли, участники, внешние системы) и связи системы с ним. Диаграмма контекста C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783261
-->
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

Person(visitor, "Посетитель", "Участник конференции в роли зрителя")
Person(speaker, "Докладчик", "Участник конференции, делающий доклад")
Person(committee_member, "Член ПК", "Участник конференции,проверяющий доклады")
Person(administrator, "Администратор", "Администратор Системы с максимальными правами")
System(conference, "Проведение конференций", "Автоматизирует проведение конференций: регистрация, оценка докладов, составление расписания")
System_Ext(notificator, "Система рассылки уведомлений", "Рассылает пользователям уведомления по email")

Rel(conference, notificator, "Использует")
Rel(administrator, conference, "Составляет расписание")
Rel(visitor, conference, "Регистрируется, смотрит информацию о докладах")
Rel(speaker, conference, "Подаёт заявку на доклад, делает доклад")
Rel(committee_member, conference, "Оценивает доклад")
Rel(notificator, visitor, "Отправляет уведомление")
Rel(notificator, speaker, "Отправляет уведомление")
Rel(notificator, committee_member, "Отправляет уведомление")
Rel(notificator, administrator, "Отправляет уведомление")

@enduml
```
