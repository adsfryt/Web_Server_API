# Web_Server_API

## Работа с пользователем

Эта документация описывает API для взаимодействия с сервером, выполняющим различные операции с данными пользователей, включая получение и обновление данных, регистрацию и 
логин через социальные сети (например, Yandex и GitHub), а также управление сессиями.

---

### 1. **Получение данных пользователя**
#### **GET /get_data**

##### Описание:
Этот эндпоинт используется для получения данных текущего пользователя по его токену сессии. Токен сессии передается либо через query-параметры, либо через заголовок `session_token`.

##### Параметры запроса:
- `session_token` (строка) — Токен сессии, полученный при аутентификации пользователя.

##### Пример запроса на Python:
```python
import requests

session_token = "your_session_token"
url = "http://yourserver.com/get_data"

response = requests.get(url, headers={"session_token": session_token})
print(response.json())
```

##### Пример ответа:
```json
{
    "login": "login",
    "userId": "id",
    "email": "default_email",
    "state": "some",
    "role": -1,
    "firstName": "first_name",
    "lastName": "last_name",
    "patronymic": "none",
    "activate": false,
    "subjectsId": [],
    "method": "yandex",
    "code": "none"
}
```

##### Ответ:
- **200 OK** — Возвращаются данные пользователя.
- **400 Bad Request** — Токен не найден.
- **403 Forbidden** — Неверный токен.
- **500 Internal Server Error** — Ошибка на сервере.

---

### 2. **Получение публичных данных пользователя**
#### **GET /get_public_data**

##### Описание:
Этот эндпоинт используется для получения публичных данных пользователя по его `userId`. Необходим токен сессии для авторизации.

##### Параметры запроса:
- `userId` (строка) — Идентификатор пользователя.
- `session_token` (строка) — Токен сессии.

##### Пример запроса на Python:
```python
import requests

session_token = "your_session_token"
user_id = "user_id_to_fetch"
url = f"http://yourserver.com/get_public_data?userId={user_id}"

response = requests.get(url, headers={"session_token": session_token})
print(response.json())
```

##### Пример ответа:
```json
{
    "login": "login",
    "userId": "id",
    "role": "role",
    "firstName": "first_name",
    "lastName": "last_name",
    "patronymic": "none",
    "activate": false,
    "subjectsId": [],
    "method": "yandex"
}
```

##### Ответ:
- **200 OK** — Возвращаются публичные данные пользователя.
- **400 Bad Request** — Токен сессии не найден.
- **403 Forbidden** — Неверный токен.
- **500 Internal Server Error** — Ошибка на сервере.

---

### 3. **Обновление данных пользователя**
#### **POST /update_data**

##### Описание:
Этот эндпоинт позволяет обновить данные пользователя, такие как имя, фамилия, отчество и логин.

##### Параметры запроса:
- `session_token` (строка) — Токен сессии.
- `patronymic` (строка) — Отчество.
- `firstName` (строка) — Имя.
- `lastName` (строка) — Фамилия.
- `login` (строка) — Логин.

##### Пример запроса на Python:
```python
import requests

session_token = "your_session_token"
url = "http://yourserver.com/update_data"
data = {
    "session_token": session_token,
    "patronymic": "Иванович",
    "firstName": "Иван",
    "lastName": "Иванов",
    "login": "ivan123"
}

response = requests.post(url, json=data)
print(response.json())
```

##### Ответ:
- **200 OK** — Данные успешно обновлены.
- **400 Bad Request** — Токен сессии не найден.
- **403 Forbidden** — Неверный токен.
- **500 Internal Server Error** — Ошибка на сервере.

---

### 4. **Подтверждение роли пользователя**
#### **POST /submit_role**

##### Описание:
Этот эндпоинт используется для отправки роли пользователя (например, администратор).

##### Параметры запроса:
- `session_token` (строка) — Токен сессии.
- `role` (строка) — Роль пользователя (например, "student" или "teacher").

##### Пример запроса на Python:
```python
import requests

session_token = "your_session_token"
url = "http://yourserver.com/submit_role"
data = {
    "session_token": session_token,
    "role": "admin"
}

response = requests.post(url, json=data)
print(response.json())
```

##### Ответ:
- **200 OK** — Роль успешно отправлена.
- **400 Bad Request** — Токен сессии не найден.
- **403 Forbidden** — Неверный токен.
- **500 Internal Server Error** — Ошибка на сервере.

---

### 5. **Подтверждение кода**
#### **POST /submit_code**

##### Описание:
Этот эндпоинт используется для отправки подтверждающего кода, например, для регистрации.

##### Параметры запроса:
- `session_token` (строка) — Токен сессии.
- `code` (строка) — Код подтверждения.

##### Пример запроса на Python:
```python
import requests

session_token = "your_session_token"
url = "http://yourserver.com/submit_code"
data = {
    "session_token": session_token,
    "code": "your_confirmation_code"
}

response = requests.post(url, json=data)
print(response.json())
```

##### Ответ:
- **200 OK** — Код успешно подтвержден.
- **400 Bad Request** — Токен сессии не найден или код неверный.
- **500 Internal Server Error** — Ошибка на сервере.

---

### 6. **Получение кода для подтверждения**
#### **GET /get_code**

##### Описание:
Этот эндпоинт используется для получения кода подтверждения для регистрации или других целей.

##### Параметры запроса:
- `session_token` (строка) — Токен сессии.

##### Пример запроса на Python:
```python
import requests

session_token = "your_session_token"
url = "http://yourserver.com/get_code"

response = requests.get(url, headers={"session_token": session_token})
print(response.json())
```

##### Ответ:
- **200 OK** — Возвращается код.
- **400 Bad Request** — Токен сессии не найден.
- **500 Internal Server Error** — Ошибка на сервере.

---

### 7. **Привязка аккаунта к внешним сервисам**
#### **GET /link_registration**

##### Описание:
Этот эндпоинт используется для получения получения ссылки, которую нужно вернуть пользователю для регистрации через Yandex или GitHub.

##### Параметры запроса:
- `session_token` (строка) — Токен сессии.
- `type` (строка) — Тип внешнего сервиса для привязки (например, `yandex` или `github`).

##### Пример запроса на Python:
```python
import requests

session_token = "your_session_token"
url = "http://yourserver.com/link_registration"
params = {"session_token": session_token, "type": "github"}

response = requests.get(url, params=params)
print(response.json())
```

##### Ответ:
- **200 OK** — Привязка прошла успешно.
- **400 Bad Request** — Тип сервиса не определен.
- **500 Internal Server Error** — Ошибка на сервере.

---

### 8. **Регистрация через внешние сервисы**
#### **POST /login**

##### Описание:
Этот эндпоинт используется для регистрации пользователя через внешние сервисы, такие как Yandex или GitHub.

##### Параметры запроса:
- `session_token` (строка) — Токен сессии.
- `type` (строка) — Тип внешнего сервиса для регистрации.

##### Пример запроса на Python:
```python
import requests

session_token = "your_session_token"
url = "http://yourserver.com/registration"
data = {
    "session_token": session_token,
    "type": "github"
}

response = requests.post(url, json=data)
print(response.json())
```

##### Ответ:
- **200 OK** — Вход прошел успешно.
- **400 Bad Request** — Токен сессии не найден или код неверный.
- **500 Internal Server Error** — Ошибка на сервере.

---

### 9. **Выход из системы**
#### **POST /logout**

##### Описание:
Этот эндпоинт используется для выхода пользователя из системы. Он удаляет токен сессии и токены доступа.

##### Параметры запроса:
- `session_token` (строка) — Токен сессии.

##### Пример запроса на Python:
```python
import requests

session_token = "your_session_token"
url = "http://yourserver.com/logout"
data = {"session_token": session_token}

response = requests.post(url, json=data)
print(response.json())
```

##### Ответ:
- **200 OK** — Пользователь успешно вышел.
- **400 Bad Request** — Токен сессии не найден.
- **500 Internal Server Error** — Ошибка на сервере.

---

### Таблица запрашиваемых данных

| Эндпоинт              | Метод   | Параметры запроса                    | Ответ                                      |
|-----------------------|---------|--------------------------------------|--------------------------------------------|
| `/get_data`           | GET     | `session_token`                      |  200 OK - данные пользователя               |
| `/get_public_data`    | GET     | `userId`, `session_token`            | 200 OK - публичные данные пользователя     |
| `/update_data`        | POST    | `session_token`, `patronymic`, `firstName`, `lastName`, `login` | 200 OK - данные обновлены                 |
| `/submit_role`        | POST    | `session_token`, `role`              | 200 OK - роль успешно назначена            |
| `/submit_code`        | POST    | `session_token`, `code`              | 200 OK - код подтвержден                   |
| `/get_code`           | GET     | `session_token`                      | 200 OK - возвращен код                     |
| `/link_registration`  | GET     | `session_token`, `type`              | 200 OK - привязка аккаунта прошла успешно  |
| `/registration`       | POST    | `session_token`, `type`, `code`      | 200 OK - регистрация прошла успешно        |
| `/logout`             | POST    | `session_token`                      | 200 OK - пользователь вышел из системы     |

---

