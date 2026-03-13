# Модель данных

## Сущности:
1. User — пользователь системы
2. Trip — поездка
3. Participant — участник поездки
4. PlanItem — элемент плана (рейс, отель, активность, трансфер)
5. AlternativeGroup — группа альтернативных предложений
6. SyncOperation — операция синхронизации
7. Conflict — зарегистрированный конфликт
8. Notification — уведомление

## Атрибуты сущностей:
**User**:
id_user - уникальный идентификатор пользователя (PK)
user_email - почта пользователя
user_password - пароль пользователя
create_at - когда создан
update_at - когда обновялось
last_login - время последнего входа в систему

**Participant**
id_participant - уникальный идентификатор участника (PK)
trip_id - уникальный идентификатор поездки (FK)
user_id - уникальный идентификатор пользователя (FK)
role - роль
joined_at - когда добавлен
status - статус участника

**Trip**
id_trip - уникальный идентификатор поездки (PK)
name_trip - название поездки
trip_descrip - описание поездки
start_date - дата начала поездки
end_date - дата окончания поездки
status - статус поездки
create_by - кем создана
created_at - когда создана
update_at - когда обновлено

**PlanItem**
id_planItem - уникальный идентификатор элемента (PK)
trip_id - уникальный идентификатор поездки, к которой относится (FK)
created_by - кем создан
type - тип
name_planItem - название элемента
planItem_descrip - описание элемента
start_datetime - дата начала
end_datetime - дата окончания
timezone - временная зона
status_planItem - статус
planItem_details - детали
price - цена
currency - валюта
booking_url - ссылка на бронирование
id_alternativ_group - уникальный идентификатор группы
created_at - когда создано
update_at - когда обновлено

**AlternativGroup**
id_alternativ_group - уникальный идентификатор группы (PK)
trip_id - уникальный идентификатор поездки, к которой относится группа альтернативных сценариев (FK)
alternative_name - название группы
alternative_status - статус
id_resolved_item - уникальный идентификатор решения
created_at - когда создано
resolved_at - когда разрешена

**Notification**
id_notification - уникальный идентификатор самого сообщения (PK)
user_id - уникальный идентификатор пользователя, которому предназначено сообщение (FK)
type_notific - тип уведомления
title_notific - название сообщения
body_notific - тело сообщения, сам текст
created_at - когда создано
is_read - статус сообщения (прочитано или нет)

**Conflict**
id_conflict - уникальный идентификатор конфликта (PK)
trip_id - уникальный идентификатор поездки (FK)
id_planItem - уникальный идентификатор элемента плана поездки (FK)
conflict_descrip - краткое описание конфликта
conflict_status - статус конфликта
created_at - когда создан
resolved_at - когда разрешен
resolved_by - кем разрешен
