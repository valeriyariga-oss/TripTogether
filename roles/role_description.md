# Описание ролей и сущностей

## Сущности
1. User — пользователь системы
2. Trip — поездка
3. Participant — участник поездки
4. PlanItem — элемент плана (рейс, отель, активность, трансфер)
5. AlternativeGroup — группа альтернативных предложений
6. SyncOperation — операция синхронизации
7. Conflict — зарегистрированный конфликт
8. Notification — уведомление

## Роли
1. **Owner** - владелец поездки, создатель
2. **Editor** - член группы, имеющий возможность редактировать (добавлять/изменять)
3. **Viewer** - член группы, имеющий возможность только просматривать
4. **Admin** - администратор системы

## Классический вариант CRUD таблицы - связь "сущность-роль"

| Сущность | Owner | Editor | Viewer | Admin |
|----------|-------|--------|--------|-------|
| User | CRUD | CRU | R | CRUD |
| Trip | CRUD | R | R | CRUD |
| Participant | CRUD | R | R | CRUD |
| PlanItem | CRUD | CRUD | R | CRUD |
| AlternativeGroup | CRUD | CRU | R | CRUD |
| Conflict | RU | RU | - | CRUD |
| Notification | R | R | R | CRUD |
