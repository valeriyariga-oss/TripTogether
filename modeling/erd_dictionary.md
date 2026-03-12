# Словарь данных (Data Dictionary)

| Сущность | Мнемоника | Название | Описание | Тип данных |
|----------|-----------|----------|----------|------------|
| **User** | id_user | ID пользователя | Уникальный идентификатор пользователя | int |
| | email | Электронная почта | Адрес электронной почты для входа и уведомлений | varchar |
| | password_hash | Хеш пароля | Хешированный пароль пользователя | varchar |
| | name | Имя | Имя пользователя | varchar |
| | avatar_url | Ссылка на аватар | URL изображения профиля (опционально) | varchar |
| | created_at | Дата регистрации | Дата и время регистрации пользователя | timestamp |
| | updated_at | Дата обновления | Дата последнего обновления профиля | timestamp |
| | last_login_at | Последний вход | Дата и время последнего входа в систему | timestamp |
| **Trip** | id_trip | ID поездки | Уникальный идентификатор поездки | int |
| | title | Название | Название поездки | varchar |
| | description | Описание | Краткое описание поездки | text |
| | start_date | Дата начала | Дата начала поездки | date |
| | end_date | Дата окончания | Дата окончания поездки | date |
| | status | Статус | Статус поездки (планирование / активная / завершённая) | varchar |
| | created_by | Создатель | ID пользователя, создавшего поездку (FK → User.id_user) | int |
| | created_at | Дата создания | Дата и время создания поездки | timestamp |
| | updated_at | Дата обновления | Дата последнего изменения поездки | timestamp |
| **Participant** | id_participant | ID участника | Уникальный идентификатор записи об участии | int |
| | trip_id | ID поездки | Ссылка на поездку (FK → Trip.id_trip) | int |
| | user_id | ID пользователя | Ссылка на пользователя (FK → User.id_user) | int |
| | role | Роль | Роль в поездке (owner / editor / viewer) | varchar |
| | invited_at | Дата приглашения | Дата отправки приглашения | timestamp |
| | joined_at | Дата принятия | Дата принятия приглашения пользователем | timestamp |
| | status | Статус участника | Статус (приглашён / активен / удалён) | varchar |
| **PlanItem** | id_plan_item | ID элемента | Уникальный идентификатор элемента плана | int |
| | trip_id | ID поездки | Ссылка на поездку (FK → Trip.id_trip) | int |
| | created_by | Создатель | ID пользователя, создавшего элемент (FK → User.id_user) | int |
| | type | Тип элемента | Тип (flight / hotel / activity / other) | varchar |
| | title | Название | Краткое название элемента | varchar |
| | description | Описание | Подробное описание | text |
| | start_datetime | Дата и время начала | Начало события в UTC | timestamp |
| | end_datetime | Дата и время окончания | Окончание события в UTC | timestamp |
| | timezone | Часовой пояс | Часовой пояс места события | varchar |
| | status | Статус | Статус (предложен / подтверждён / отклонён / альтернатива) | varchar |
| | details | Детали | JSON с дополнительными полями (номер рейса, адрес отеля и т.п.) | json |
| | price | Цена | Стоимость (опционально) | decimal |
| | currency | Валюта | Валюта цены (опционально) | varchar |
| | booking_url | Ссылка на бронь | URL подтверждения бронирования | varchar |
| | alternative_group_id | ID группы альтернатив | Ссылка на группу альтернатив (FK → AlternativeGroup.id_alternative_group), NULL если не в группе | int |
| | created_at | Дата создания | Дата и время создания элемента | timestamp |
| | updated_at | Дата обновления | Дата последнего изменения | timestamp |
| **AlternativeGroup** | id_alternative_group | ID группы | Уникальный идентификатор группы альтернатив | int |
| | trip_id | ID поездки | Ссылка на поездку (FK → Trip.id_trip) | int |
| | title | Название группы | Например, "Варианты отеля в Париже" | varchar |
| | status | Статус | Статус группы (активна / разрешена / архивирована) | varchar |
| | resolved_item_id | Выбранный вариант | ID элемента, выбранного как итоговый (FK → PlanItem.id_plan_item), NULL пока не выбран | int |
| | created_at | Дата создания | Дата и время создания группы | timestamp |
| | resolved_at | Дата разрешения | Дата выбора итогового варианта | timestamp |
| **Conflict** | id_conflict | ID конфликта | Уникальный идентификатор конфликта | int |
| | trip_id | ID поездки | Ссылка на поездку (FK → Trip.id_trip) | int |
| | plan_item_id | ID элемента | Элемент, по которому возник конфликт (FK → PlanItem.id_plan_item) | int |
| | description | Описание | Описание конфликтной ситуации | text |
| | status | Статус | Статус (новый / в процессе / разрешён / отменён) | varchar |
| | created_at | Дата возникновения | Дата и время возникновения конфликта | timestamp |
| | resolved_at | Дата разрешения | Дата разрешения конфликта | timestamp |
| | resolved_by | Кто разрешил | ID пользователя, разрешившего конфликт (FK → User.id_user) | int |
| **Notification** | id_notification | ID уведомления | Уникальный идентификатор уведомления | int |
| | user_id | ID пользователя | Получатель уведомления (FK → User.id_user) | int |
| | type | Тип | Тип уведомления (new_item / item_changed / conflict / trip_deleted и др.) | varchar |
| | title | Заголовок | Заголовок уведомления | varchar |
| | body | Текст | Текст уведомления | text |
| | data | Данные | JSON с дополнительными данными | json |
| | is_read | Прочитано | Флаг, прочитано ли уведомление | bool |
| | created_at | Дата создания | Дата и время отправки уведомления | timestamp |
| **SyncOperation** | id_sync_op | ID операции | Уникальный идентификатор операции синхронизации | int |
| | user_id | ID пользователя | Пользователь, выполнивший операцию (FK → User.id_user) | int |
| | device_id | ID устройства | Идентификатор устройства, с которого выполнена операция | varchar |
| | operation_type | Тип операции | Тип (create / update / delete) | varchar |
| | entity_type | Тип сущности | Тип изменяемой сущности (trip / plan_item / и т.д.) | varchar |
| | entity_id | ID сущности | Идентификатор изменяемой сущности | int |
| | data | Данные | JSON с состоянием на момент операции | json |
| | status | Статус | Статус синхронизации (pending / synced / failed) | varchar |
| | created_at | Дата создания | Дата и время создания операции (оффлайн) | timestamp |
| | synced_at | Дата синхронизации | Дата и время отправки на сервер | timestamp |
