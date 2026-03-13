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
id_user - уникальный идентификатор пользователя (PK) <br>
user_email - почта пользователя<br>
user_password - пароль пользователя<br>
create_at - когда создан<br>
update_at - когда обновялось<br>
last_login - время последнего входа в систему<br>

**Participant**
id_participant - уникальный идентификатор участника (PK)<br>
trip_id - уникальный идентификатор поездки (FK)<br>
user_id - уникальный идентификатор пользователя (FK)<br>
role - роль<br>
joined_at - когда добавлен<br>
status - статус участника<br>

**Trip**
id_trip - уникальный идентификатор поездки (PK)<br>
name_trip - название поездки<br>
trip_descrip - описание поездки<br>
start_date - дата начала поездки<br>
end_date - дата окончания поездки<br>
status - статус поездки<br>
create_by - кем создана<br>
created_at - когда создана<br>
update_at - когда обновлено<br>

**PlanItem**
id_planItem - уникальный идентификатор элемента (PK)<br>
trip_id - уникальный идентификатор поездки, к которой относится (FK)<br>
created_by - кем создан<br>
type - тип<br>
name_planItem - название элемента<br>
planItem_descrip - описание элемента<br>
start_datetime - дата начала<br>
end_datetime - дата окончания<br>
timezone - временная зона<br>
status_planItem - статус<br>
planItem_details - детали<br>
price - цена<br>
currency - валюта<br>
booking_url - ссылка на бронирование<br>
id_alternativ_group - уникальный идентификатор группы<br>
created_at - когда создано<br>
update_at - когда обновлено<br>

**AlternativGroup**
id_alternativ_group - уникальный идентификатор группы (PK)<br>
trip_id - уникальный идентификатор поездки, к которой относится группа альтернативных сценариев (FK)<br>
alternative_name - название группы<br>
alternative_status - статус<br>
id_resolved_item - уникальный идентификатор решения<br>
created_at - когда создано<br>
resolved_at - когда разрешена<br>

**Notification**
id_notification - уникальный идентификатор самого сообщения (PK)<br>
user_id - уникальный идентификатор пользователя, которому предназначено сообщение (FK)<br>
type_notific - тип уведомления<br>
title_notific - название сообщения<br>
body_notific - тело сообщения, сам текст<br>
created_at - когда создано<br>
is_read - статус сообщения (прочитано или нет)<br>

**Conflict**
id_conflict - уникальный идентификатор конфликта (PK)<br>
trip_id - уникальный идентификатор поездки (FK)<br>
id_planItem - уникальный идентификатор элемента плана поездки (FK)<br>
conflict_descrip - краткое описание конфликта<br>
conflict_status - статус конфликта<br>
created_at - когда создан<br>
resolved_at - когда разрешен
resolved_by - кем разрешен
