# Модель предметной области
<!-- Логическая модель, содержащая бизнес-сущности предметной области, атрибуты и связи между ними. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375782602

Используется диаграмма классов UML. Документация: https://plantuml.com/class-diagram 
-->

```plantuml
@startuml
' Логическая модель данных в варианте UML Class Diagram (альтернатива ER-диаграмме).

namespace HelloConf {

 class User
 {
   id : bigint
   firstName: string
   lastName : string
   authorities : UserAuthority[]
   email : string
   createdAt : datetime
   updatedAt : datetime
 }

 class UserAuthority
 {
   id : string
   user : User
   authority : Authority
 }

 class SpeakerProfile 
 {
   id : bigint
   user : User
   bio : string
   reports : Report[]
   createdAt : datetime
   updatedAt : datetime
 }

 class ReviewerProfile 
 {
   id : bigint
   user : User
   bio : string
   reports : Report[]
   createdAt : datetime
   updatedAt : datetime
 }

 class Report 
 {
   id : bigint
   status : ReportStatus
   speakers : SpeakerProfile[]
   reviewComments : ReviewComment[]
   performedAt : datetime
   createdAt : datetime
   updateAt : datetime
 }

 class ReviewComment
 {
   id:  bigint
   report : Report
   writtenBy : User
   createdAt : datetime
   updatedAt : datetime
 }

 class Conference
 {
   id : bigint
   name : string
   conductionDate : date
   schedule : Schedule[]
   createdAt: datetime
   updatedAt: datetime
 }

 class Schedule
 {
   id : bigint
   report : Report
   performedAt: datetime
   createdAt : datetime
   updatedAt : datetime
 }

 enum ReportStatus 
 {
   new,
   draft,
   approved
 }

 enum Authority
 {
   user,
   speaker,
   reviewer
 }

  Conference o-- Report
  SpeakerProfile o--o "0..*" Report
  ReviewComment --o Report
  Schedule .. Report
  Report *-- ReportStatus
  Schedule o-- Conference
  User <|-- SpeakerProfile
  User <|-- ReviewerProfile
  User o-- UserAuthority
  UserAuthority *-- Authority
}


@enduml
```
