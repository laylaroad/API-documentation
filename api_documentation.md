# API-документация: сервис бронирования билетов "Полетели.ру"

## 1. Введение

Этот API позволяет разработчикам интегрировать сервис бронирования авиабилетов "Полетели.ру" в свои приложения. С его помощью можно искать рейсы, получать информацию о билетах и оформлять бронирования.

**Базовый URL:**  
`https://api.poleteli.ru/v1/`

## 2. Аутентификация

Для доступа к API требуется API-ключ, который передаётся в заголовке `Authorization`.

### Пример запроса:

```http
GET /flights/search HTTP/1.1
Host: api.poleteli.ru
Authorization: Bearer YOUR_API_KEY
```

## 3. Методы API 

### 3.1. Поиск рейсов

Метод: `GET /flights/search`
Описание: Возвращает список доступных рейсов по заданным параметрам.

### Параметры запроса:

| Параметр     | Тип    | Описание                        | Обязательный |
|-------------|--------|----------------------------------|--------------|
| `origin`    | string | Код аэропорта отправления (IATA) | ✅ Да        |
| `destination` | string | Код аэропорта назначения (IATA)  | ✅ Да      |
| `date`      | string | Дата вылета (YYYY-MM-DD)         | ✅ Да        |
| `passengers` | int    | Количество пассажиров            | ❌ Нет      |



### Пример запроса:

```GET /flights/search?origin=DME&destination=JFK&date=2024-07-10&passengers=1
Host: api.poleteli.ru
Authorization: Bearer YOUR_API_KEY
```

### Пример ответа:

```
{
  "flights": [
    {
      "id": "12345",
      "airline": "Aeroflot",
      "departure": "2024-07-10T10:00:00Z",
      "arrival": "2024-07-10T18:00:00Z",
      "price": 450.00,
      "currency": "USD"
    }
  ]
}
```

### 3.2. Получение информации о рейсе

Метод: `GET /flights/{id}`
Описание: Возвращает детали рейса по его идентификатору.

### Пример запроса:

```
GET /flights/12345
Host: api.poleteli.ru
Authorization: Bearer YOUR_API_KEY
```

### Пример ответа:

```
{
  "id": "12345",
  "airline": "Aeroflot",
  "departure": "2024-07-10T10:00:00Z",
  "arrival": "2024-07-10T18:00:00Z",
  "duration": "8h",
  "price": 450.00,
  "currency": "USD",
  "baggage": "20kg",
  "cancellation_policy": "Refundable with a 10% fee"
}
```

### 3.3. Бронирование билетов

Метод: `POST /bookings/create`
Описание: Оформляет бронирование билетов.

Тело запроса:
```
{
  "flight_id": "12345",
  "passenger": {
    "first_name": "Иван",
    "last_name": "Петров",
    "passport": "123456789"
  },
  "payment": {
    "card_number": "4111111111111111",
    "expiry_date": "12/26",
    "cvv": "123"
  }
}
```

### Пример ответа:

```
{
  "booking_id": "98765",
  "status": "confirmed",
  "total_price": 450.00,
  "currency": "USD"
}
```

## 4. Ошибки и коды ответов

| Код      | Значение             | Описание                   |
| 200      | OK                   | Запрос выполнен успешно    | 
| 400      | Bad Request          | Ошибка в параметрах запроса| 
| 401      | Unauthorized         | Неверный API-ключ          | 
| 404      | Not Found            | Ресурс не найден           | 
| 500      | Internal Server Error| Внутренняя ошибка сервера  | 


## 5. Контакты и поддержка

Если у вас возникли вопросы, обратитесь в службу поддержки:
📧 Email: support@poleteli.ru
📞 Телефон: +7 (800) 123-45-67

