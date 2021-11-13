# Описание
Сайт для размещения мероприятий для студентов СПБГУ (преимущественно на территории ПУНКА). Так как сейчас все анонсы мероприятий ведутся или в группах ВК или на сайте СПБГУ - о них мало кто знает. Будет проще, если можно будет собрать всех их в одном месте. Также каждый желающий может создать свое мероприятие/встречу и пригласить на него всех желающих.
## Наименование
**PUNK Events**
## Предметная область
Сайт для создания/просмотра мероприятий
# Данные
## Для каждого элемента данных - ограничения
**User**:

| name | type | constrains |
| ---- | ---- | ---------- |
| id   | Integer|  primary_key|
| uuid| UUID| unique, not null|
| email| String(64)|not null|
| full_name | String(64)| not null |


**Event**:

| name | type | constrains |
| ---- | ---- | ---------- |
| id   | Integer|  primary_key|
| uuid| UUID| unique, not null|
| creator| UUID|not null, FOREIGN KEY (User.uuid)|
| participants| ARRAY(UUID) |FOREIGN KEYS (User.uuid)|
| title | String(64)| not null |
| description | String(256)||
| location | String(64)| not null |
| scope | String(64)| not null |
| created_datetime| DateTime| not null |
| event_datetime | DateTime| not null |
| is_approved | Bool| not null, default=False |


**Comment**

| name | type | constrains |
| ---- | ---- | ---------- |
| id   | Integer|  primary_key|
| uuid| UUID| unique, not null|
| data| String(256)|not null|
| user_uuid | UUID| not null, FOREIGN KEY (User.uuid) |
| event_uuid | UUID|not null, FOREIGN KEY (Event.uuid) |
| created_datetime| DateTime| not null |

**moderator**

| name | type | constrains |
| ---- | ---- | ---------- |
| id   | Integer|  primary_key|
| user_uuid| UUID| FOREIGN KEY (User.uuid), not null|


## Общие ограничения целостности
- Между таблицами Event и User будет построено отношение `Many to many`, а между **Event -- Comment** и **User -- Comment** - `One to many`
- Ограничение `not Null` где это необходимо
- Каждый пользователь может создать в день **не больше 5 мероприятий**. Не больше 5 комментариев к одному мероприятию.
- Админ может создавать сколько угодно мероприятий в день и писать любое количество комментариев 

# Пользовательские роли
## Для каждой роли - наименование, ответственность, количество пользователей в этой роли?
Admin (Кол-во: 1)
- Создание/удаление/редактирование любых мероприятий
- Назначение модераторов (Доступ к таблице `Moderator`)
- Удаление пользователей
- Написание комментариев к любому мероприятию
- Проверка созданных мероприятий на запрещенные темы/слова/выражения и удаление таковых
- Оповещение пользователя об удалении созданного мероприятия (указание причины)

Moderator (Кол-во: 1-10)
- Создание/удаление/редактирование любых мероприятий
- Написание комментариев к любому мероприятию
- Проверка созданных мероприятий на запрещенные темы/слова/выражения и удаление таковых
- Оповещение пользователя об удалении созданного мероприятия (указание причины)

User (Логин **только по st**, Кол-во: сколько позволит машина)
- Создание/удаление/редактирование своих мероприятий
- Написание комментариев к любому мероприятию
- Подписка на мероприятия

# UI / API 
- UI - `React`
- API - `Fastapi`
- Средство развертывания - `Docker` 
# Технологии разработки
Agile Model
## Язык программирования
- Backend - Python, sql
- Frontend - JS, css, html
## СУБД
Postgresql