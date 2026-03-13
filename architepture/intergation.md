# Модуль интеграций с внешними API

## 1. Назначение
Модуль отвечает за взаимодействие с внешними сервисами:
- Поиск авиабилетов
- Поиск отелей и жилья
- Поиск активностей и экскурсий
- (опционально) Погода, трансферы

## 2. Принципы проектирования модуля интеграций

 **Изоляция сбоев** <br>
 Проблемы одного API не влияют на работу других модулей системы, чтобы падение одного блока не ломало другие <br>
**Резервирование** <br>
Для каждого типа данных минимум 2 независимых провайдера для обеспечения отказоустойчивости при недоступности основного API <br>
**Кэширование** <br>
Результаты поиска сохраняются в Redis с TTL для снижения нагрузки на внешние API и ускорение ответов <br>
**Retry-механизм** <br>
Автоматический повтор запроса при временных ошибках (таймаут, 5xx) необходим для повышения вероятности успешного ответа <br>
**Circuit Breaker** <br>
Временное отключение проблемного API при накоплении ошибок для предотвращения каскадных сбоев и бесполезных повторных запросов <br>
**Таймауты** <br>
Ограничение времени ожидания ответа от API (например, 5 секунд) для освобождения ресурсов при зависании внешнего сервиса <br>

## 3. Примеры запросов и ответов

### 3.1 Авиабилеты 
**Запрос:**<br>
GET https://api.travelpayouts.com/aviasales/v3/prices_for_dates <br>
    ?origin=MOW <br>
    &destination=LED <br>
    &departure_at=2025-06-01 <br>
    &return_at=2025-06-10 <br>
    &currency=rub <br>
    &limit=10 <br>
    &token=YOUR_API_KEY <br>

**Параметры запроса:**<br>
origin - код города вылета <br>
destination - код города назначения <br>
departure_at - дата вылета <br>
retunrn_at - дата возращения <br>
currency - валюта <br>
limit - кол-во выводимых результатов <br>
token - API ключ <br>

**Ответ:**<br>
{<br>
  "success": true, <br>
  "data": [ <br>
    { <br>
      "flight_number": "SU10", <br>
      "airline": "SU", <br>
      "origin": "MOW", <br>
      "destination": "LED", <br>
      "departure_at": "2025-06-01T10:30:00+03:00", <br>
      "return_at": "2025-06-10T15:20:00+03:00", <br>
      "price": 8500, <br>
      "currency": "rub", <br>
      "link": "https://aviasales.ru/search/MOW2206LED1?t=SU10" <br>
    }, <br>
    { <br>
      "flight_number": "DP651", <br>
      "airline": "DP", <br>
      "origin": "MOW", <br>
      "destination": "LED", <br>
      "departure_at": "2025-06-01T08:15:00+03:00", <br>
      "return_at": "2025-06-10T19:40:00+03:00", <br>
      "price": 4200, <br>
      "currency": "rub", <br>
      "link": "https://aviasales.ru/..." <br>
    } <br>
  ] <br>
} <br>

### 3.2 Ж/д билеты 
**Запрос:**<br>
GET https://api.tutu.ru/railway/v1/search <br>
    ?from=c149 <br>
    &to=c200 <br> 
    &date=2025-06-01 <br>
    &limit=10 <br>
**Параметры**<br>
from - откуда (код вокзала) <br>
to - куда (код вокзала) <br>
date - дата  <br>
limit - кол-во выводимых результатов <br>

**Ответ:**<br>
{<br>
  "success": true,<br>
  "data": [ <br>
    {<br>
      "train_number": "002А",<br>
      "train_name": "Красная стрела",<br>
      "departure": {<br>
        "station": "Москва (Ленинградский вокзал)",<br>
        "datetime": "2025-06-01T23:55:00+03:00"<br>
      },<br>
      "arrival": {<br>
        "station": "Санкт-Петербург (Московский вокзал)",<br>
        "datetime": "2025-06-02T08:00:00+03:00"<br>
      },<br>
      "duration": 485,<br>
      "price": {<br>
        "min": 2500,<br>
        "max": 8500,<br>
        "currency": "rub"<br>
      },<br>
      "bookingUrl": "https://tutu.ru/..."<br>
    },<br>
    {<br>
      "train_number": "004А",<br>
      "train_name": "Экспресс",<br>
      "departure": {<br>
        "station": "Москва (Ленинградский вокзал)",<br>
        "datetime": "2025-06-01T06:40:00+03:00"<br>
      },<br>
      "arrival": {<br>
        "station": "Санкт-Петербург (Московский вокзал)",<br>
        "datetime": "2025-06-01T10:30:00+03:00"<br>
      },<br>
      "duration": 230,<br>
      "price": {<br>
        "min": 1500,<br>
        "max": 3200,<br>
        "currency": "rub"<br>
      },<br>
      "bookingUrl": "https://tutu.ru/..."<br>
    }<br>
  ]<br>
}<br>

### 3.3 Отели
**Запрос:**<br>
GET https://booking-com.p.rapidapi.com/v1/hotels/search
    ?dest_id=-2950142
    &checkin_date=2025-06-01
    &checkout_date=2025-06-05
    &adults_number=2
    &room_number=1
    &currency=RUB
    &locale=ru
**Параметры**<br>
dest_id - id города <br>
checkin_date - дата заезда <br>
checkout_date - дата dstplf  <br>
adults_number - кол-во взрослых <br>
room_number - кол-во номеров <br>
currency - валюта <br>
locale - язык <br>

**Ответ:**<br>
{<br>
  "success": true,<br>
  "result": [<br>
    {<br>
      "hotel_id": 123456,<br>
      "name": "Отель Индиго Санкт-Петербург",<br>
      "address": "ул. Чайковского, 17",<br>
      "rating": 8.9,<br>
      "review_count": 1245,<br>
      "price": 8500,<br>
      "currency": "RUB",<br>
      "photo": "https://...",<br>
      "amenities": ["Wi-Fi", "Завтрак", "Парковка"],<br>
      "bookingUrl": "https://booking.com/..."<br>
    },<br>
    {<br>
      "hotel_id": 789012,<br>
      "name": "Азимут Отель Санкт-Петербург",<br>
      "address": "Лермонтовский пр., 43/1",<br>
      "rating": 8.2,<br>
      "review_count": 3421,<br>
      "price": 5200,<br>
      "currency": "RUB",<br>
      "photo": "https://...",<br>
      "amenities": ["Wi-Fi", "Ресторан", "Тренажёрный зал"],<br>
      "bookingUrl": "https://booking.com/..."<br>
    }<br>
  ]<br>
}<br>

## 4. Обработка ошибок

### 4.1 Общие ошибки для всех API
| HTTP код | Название | message в JSON ответе | Действие модуля |
|----------|----------|----------------------|-----------------|
| **400** | Bad Request | `"message": "Invalid date format. Use YYYY-MM-DD"` | Логировать ошибку, вернуть клиенту "Проверьте введённые данные" |
| **401** | Unauthorized | `"message": "Invalid API key or token expired"` | Уведомить администратора, проверить ключ |
| **403** | Forbidden | `"message": "Access denied. Insufficient permissions"` | Логировать, вернуть "Нет доступа к ресурсу" |
| **404** | Not Found | `"message": "Hotel with id 123456 not found"` | Вернуть клиенту "Ничего не найдено" |
| **429** | Too Many Requests | `"message": "Rate limit exceeded. Try again in 30 seconds"` | Увеличить задержку, повторить через `retry_after` |
| **500** | Internal Server Error | `"message": "Internal server error. Please try again later"` | Retry (до 3 раз), затем Circuit Breaker |
| **503** | Service Unavailable | `"message": "Service temporarily unavailable due to maintenance"` | Переключиться на резервный провайдер |

### JSON для каждого вида ошибок
**400 Bad Request**<br>
{<br>
  "error": {<br>
    "code": 400,<br>
    "message": "Invalid date format. Use YYYY-MM-DD"<br>
  }<br>
}<br>
**401 Unauthorized**<br>
{<br>
  "error": {<br>
    "code": 401,<br>
    "message": "Invalid API key or token expired"<br>
  }<br>
}<br>
**403 Forbidden**<br>
{<br>
  "error": {<br>
    "code": 403,<br>
    "message": "Access denied. Insufficient permissions"<br>
  }<br>
}<br>
**404 Not Found**<br>
{<br>
  "error": {<br>
    "code": 404,<br>
    "message": "Hotel with id 123456 not found"<br>
  }<br>
}<br>
**429 Too Many Requests**<br>
{<br>
  "error": {<br>
    "code": 429,<br>
    "message": "Rate limit exceeded. Try again in 30 seconds",<br>
    "retry_after": 30<br>
  }<br>
}<br>
**500 Internal Server Error**<br>
{<br>
  "error": {<br>
    "code": 500,<br>
    "message": "Internal server error. Please try again later"<br>
  }<br>
}<br>
**503 Service Unavailable**<br>
{<br>
  "error": {<br>
    "code": 503,<br>
    "message": "Service temporarily unavailable due to maintenance"<br>
  }<br>
}<br>
