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

### 8. **Выход из системы**
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






## Работа с токенами

### 1. **GET /get_access_token**

**Описание**:  
Этот эндпоинт предназначен для получения токенов доступа и обновления по `session_token`. Если токены не найдены или передан недействительный `session_token`, возвращаются значения по умолчанию.

**Пример запроса (Python)**:

```python
import requests

# Запрос с передачей session_token как query параметра
response = requests.get('http://localhost:3000/get_access_token', params={'session_token': '1234-uuid-5678'})
print(response.json())
```

**Запрашиваемые данные**:
- Параметр `session_token` в query строке или в заголовке запроса.

**Возвращаемые данные**:
- `{ "access_token": "token_value", "refresh_token": "refresh_token_value" }` — если токены существуют.
- `{ "access_token": null, "refresh_token": null }` — если токены отсутствуют.
- `{ "access_token": "none", "refresh_token": "none" }` — если токены удалены или указано "none".

**Ошибки**:
- `400 Bad Request` — если не передан `session_token`.
- `500 Internal Server Error` — если произошла ошибка на сервере.

---

### 2. **POST /create_token**

**Описание**:  
Создает новый `session_token` с токенами доступа и обновления или удаляет их, если передан параметр "none".

**Пример запроса (Python)**:

```python
import requests

# Создание нового session_token с токенами
data = {
    'access_token': 'new_access_token',
    'refresh_token': 'new_refresh_token'
}
response = requests.post('http://localhost:3000/create_token', json=data)
print(response.json())
```

**Запрашиваемые данные**:
- Тело запроса с полями:
  - `access_token`: строка (обязательное).
  - `refresh_token`: строка (обязательное).

**Возвращаемые данные**:
- `{ "session_token": "generated_session_token" }` — возвращает сгенерированный `session_token`.

**Ошибки**:
- `500 Internal Server Error` — если произошла ошибка на сервере.

---

### 3. **POST /delete_token**

**Описание**:  
Удаляет токены по указанному `session_token`.

**Пример запроса (Python)**:

```python
import requests

# Удаление токена по session_token
data = {
    'session_token': '1234-uuid-5678'
}
response = requests.post('http://localhost:3000/delete_token', json=data)
print(response.json())
```

**Запрашиваемые данные**:
- Тело запроса с полем `session_token` или `session_token` в заголовке запроса.

**Возвращаемые данные**:
- `{ "ok": true }` — если токен был успешно удален.
- `{ "error": "not found token or can't delete" }` — если токен не найден или не удалось удалить.

**Ошибки**:
- `400 Bad Request` — если не передан `session_token`.
- `500 Internal Server Error` — если произошла ошибка на сервере.

---

### 4. **PUT /link_token**

**Описание**:  
Связывает новые токены доступа и обновления с уже существующим `session_token`. Если передан некорректный `session_token`, возвращается ошибка.

**Пример запроса (Python)**:

```python
import requests

# Связать новый токен с существующим session_token
data = {
    'access_token': 'new_access_token',
    'refresh_token': 'new_refresh_token',
    'session_token': '1234-uuid-5678'
}
response = requests.put('http://localhost:3000/link_token', json=data)
print(response.json())
```

**Запрашиваемые данные**:
- Тело запроса с полями:
  - `access_token`: строка (обязательное).
  - `refresh_token`: строка (обязательное).
  - `session_token`: строка (обязательное).

**Возвращаемые данные**:
- `{ "ok": true }` — если токены были успешно привязаны.

**Ошибки**:
- `400 Bad Request` — если не переданы обязательные параметры.
- `404 Not Found` — если не найден соответствующий `session_token`.
- `500 Internal Server Error` — если произошла ошибка на сервере.

---

### 5. **PUT /set_users_token**

**Описание**:  
Привязывает токен пользователя (`session_token`) к конкретному `userId`. Если уже существуют токены для данного пользователя, новый токен добавляется в список.

**Пример запроса (Python)**:

```python
import requests

# Привязка session_token к userId
data = {
    'userId': 'user123',
    'session_token': '1234-uuid-5678'
}
response = requests.put('http://localhost:3000/set_users_token', json=data)
print(response.json())
```

**Запрашиваемые данные**:
- Тело запроса с полями:
  - `userId`: строка (обязательное).
  - `session_token`: строка (обязательное).

**Возвращаемые данные**:
- `{ "ok": true }` — если токен был успешно привязан к пользователю.

**Ошибки**:
- `400 Bad Request` — если не переданы обязательные параметры.
- `500 Internal Server Error` — если произошла ошибка на сервере.

---

### 6. **GET /get_users_token**

**Описание**:  
Возвращает все `session_token`, связанные с конкретным пользователем по `userId`.

**Пример запроса (Python)**:

```python
import requests

# Получение всех session_token для пользователя
response = requests.get('http://localhost:3000/get_users_token', params={'userid': 'user123'})
print(response.json())
```

**Запрашиваемые данные**:
- Параметр `userid` в query строке.

**Возвращаемые данные**:
- `[ "session_token1", "session_token2" ]` — если для пользователя существуют связанные токены.
- `{ "error": "not found tokens" }` — если токены не найдены для пользователя.

**Ошибки**:
- `400 Bad Request` — если не передан `userid`.
- `500 Internal Server Error` — если произошла ошибка на сервере.

---

### Общие ошибки:
- **400 Bad Request** — ошибка в запросе (отсутствие обязательных параметров).
- **404 Not Found** — объект не найден (например, токен или пользователь).
- **500 Internal Server Error** — ошибка сервера, которая произошла при обработке запроса.

--- 
Эта документация описывает работу с Redis через API для управления токенами доступа и обновления, позволяя связывать и удалять их по уникальным сессиям пользователей.



---

## Работа с предметами

Документация к части веб-сервера, работающего с субъектами и пользователями, включает описание каждого эндпоинта с примерами на Python для использования API. Для всех запросов в теле запроса передаются данные через `JSON` формат, а для аутентификации используется токен сессии.

---

### 1. `/get_data`
**Описание**: Этот эндпоинт получает данные о субъектах для пользователя. Если передан `subjectId`, возвращаются данные по конкретному предмету, иначе — данные по всем предметам, к которым имеет доступ пользователь.

**Метод**: `POST`

**Запрашиваемые данные**:
- `session_token` (строка) — токен сессии пользователя.
- `subjectId` (необязательное) — ID предмета, если необходимо получить информацию по конкретному предмету.

**Пример запроса** (Python):
```python
import requests

url = "http://yourserver.com/get_data"
headers = {"session_token": "your_session_token"}
data = {"subjectId": "12345"}  # Опционально

response = requests.post(url, headers=headers, json=data)

print(response.json())
```

**Пример ответа**:
```json
{
  "subjectId": "12345",
  "title": "Mathematics",
  "description": "Basic Math Course",
  "role": 1
}
```

**Описание**:
- Запрашивает данные о всех или одном предмете пользователя.
- Возвращает информацию о предметах, к которым пользователь имеет доступ, в зависимости от роли. Роли могут быть:
  - `0` — обычный пользователь (удаляются пользователи и тесты).
  - `1` — преподаватель (полный доступ).
  
---

### 2. `/add_subject`
**Описание**: Этот эндпоинт добавляет новый предмет. Доступ только для пользователей с ролью `1`.

**Метод**: `POST`

**Запрашиваемые данные**:
- `session_token` (строка) — токен сессии пользователя.
- `data` (объект) — информация о новом предмете, включая:
  - `title` — название предмета.
  - `description` — описание предмета.

**Пример запроса** (Python):
```python
import requests

url = "http://yourserver.com/add_subject"
headers = {"session_token": "your_session_token"}
data = {
    "title": "Physics",
    "description": "Fundamentals of Physics"
}

response = requests.post(url, headers=headers, json={"data": data})

print(response.json())
```

**Пример ответа**:
```json
{
  "ok": true
}
```

**Описание**:
- Добавляет новый предмет в систему.
- Только пользователи с ролью `1` (преподаватели) могут создать предмет.

---

### 3. `/update_data`
**Описание**: Этот эндпоинт обновляет данные предмета. Доступ только для пользователей с ролью `1`.

**Метод**: `POST`

**Запрашиваемые данные**:
- `session_token` (строка) — токен сессии пользователя.
- `title` — новое название предмета.
- `description` — новое описание предмета.
- `subjectId` — ID предмета, который нужно обновить.

**Пример запроса** (Python):
```python
import requests

url = "http://yourserver.com/update_data"
headers = {"session_token": "your_session_token"}
data = {
    "subjectId": "12345",
    "title": "Advanced Physics",
    "description": "Advanced topics in Physics"
}

response = requests.post(url, headers=headers, json=data)

print(response.json())
```

**Пример ответа**:
```json
{
  "ok": true
}
```

**Описание**:
- Обновляет данные существующего предмета. Пользователь должен быть преподавателем (роль `1`).

---

### 4. `/delete_data`
**Описание**: Этот эндпоинт удаляет предмет. Доступ только для пользователей с ролью `1`.

**Метод**: `POST`

**Запрашиваемые данные**:
- `session_token` (строка) — токен сессии пользователя.
- `subjectId` — ID предмета, который нужно удалить.

**Пример запроса** (Python):
```python
import requests

url = "http://yourserver.com/delete_data"
headers = {"session_token": "your_session_token"}
data = {"subjectId": "12345"}

response = requests.post(url, headers=headers, json=data)

print(response.json())
```

**Пример ответа**:
```json
{
  "ok": true
}
```

**Описание**:
- Удаляет предмет, если пользователь является создателем (преподавателем).

---

### 5. `/get_all`
**Описание**: Этот эндпоинт получает все предметы в системе.

**Метод**: `POST`

**Запрашиваемые данные**:
- `session_token` (строка) — токен сессии пользователя.

**Пример запроса** (Python):
```python
import requests

url = "http://yourserver.com/get_all"
headers = {"session_token": "your_session_token"}

response = requests.post(url, headers=headers)

print(response.json())
```

**Пример ответа**:
```json
[
  {
    "subjectId": "12345",
    "title": "Mathematics",
    "description": "Basic Math Course"
  },
  {
    "subjectId": "67890",
    "title": "Physics",
    "description": "Fundamentals of Physics"
  }
]
```

**Описание**:
- Возвращает все доступные предметы в системе, удаляя информацию о пользователях и тестах.

---

### 6. `/add_user`
**Описание**: Этот эндпоинт добавляет пользователя (ученика или преподавателя) к предмету. Доступ только для пользователей с ролью `1`.

**Метод**: `POST`

**Запрашиваемые данные**:
- `session_token` (строка) — токен сессии пользователя.
- `userId` — ID пользователя, которого нужно добавить.
- `subjectId` — ID предмета, к которому нужно добавить пользователя.

**Пример запроса** (Python):
```python
import requests

url = "http://yourserver.com/add_user"
headers = {"session_token": "your_session_token"}
data = {
    "userId": "98765",
    "subjectId": "12345"
}

response = requests.post(url, headers=headers, json=data)

print(response.json())
```

**Пример ответа**:
```json
{
  "ok": true
}
```

**Описание**:
- Добавляет пользователя к предмету, либо как ученика, либо как преподавателя (в зависимости от роли).

---

### 7. `/delete_user`
**Описание**: Этот эндпоинт удаляет пользователя из предмета. Доступ только для пользователей с ролью `1`.

**Метод**: `POST`

**Запрашиваемые данные**:
- `session_token` (строка) — токен сессии пользователя.
- `userId` — ID пользователя, которого нужно удалить.
- `subjectId` — ID предмета, из которого нужно удалить пользователя.

**Пример запроса** (Python):
```python
import requests

url = "http://yourserver.com/delete_user"
headers = {"session_token": "your_session_token"}
data = {
    "userId": "98765",
    "subjectId": "12345"
}

response = requests.post(url, headers=headers, json=data)

print(response.json())
```

**Пример ответа**:
```json
{
  "ok": true
}
```

**Описание**:
- Удаляет пользователя из предмета. Только пользователи с ролью `1` могут выполнить эту операцию.

---

### 8. `/add_user_self`
**Описание**: Этот эндпоинт позволяет пользователю (ученику или преподавателю) самостоятельно добавить себя к предмету.

**Метод**: `POST`

**Запрашиваемые данные**:
- `session_token` (строка) — токен сессии пользователя.
- `subjectId` — ID предмета, к которому пользователь хочет присоединиться.

**Пример запроса** (Python):
```python
import requests

url = "http://yourserver.com/add_user_self"
headers = {"session_token": "your_session_token"}
data = {"subjectId": "12345"}

response = requests.post(url, headers=headers, json=data)

print(response.json())
```

**Пример ответа**:
```json
{
  "ok": true
}
```

**Описание**:
- Позволяет пользователю добавлять себя к предмету, если это разрешено.

---

### 9. `/delete_user_self`
**Описание**: Этот эндпоинт позволяет пользователю (ученику или преподавателю) самостоятельно удалить себя из предмета.

**Метод**: `POST`

**Запрашиваемые данные**:
- `session_token` (строка) — токен сессии пользователя.
- `subjectId` — ID предмета, из которого пользователь хочет выйти.

**Пример запроса** (Python):
```python
import requests

url = "http://yourserver.com/delete_user_self"
headers = {"session_token": "your_session_token"}
data = {"subjectId": "12345"}

response = requests.post(url, headers=headers, json=data)

print(response.json())
```

**Пример ответа

**:
```json
{
  "ok": true
}
```

**Описание**:
- Позволяет пользователю удалять себя из предмета.

---

Эти эндпоинты составляют основную функциональность для работы с пользователями и предметами в системе, включая создание, обновление, удаление данных и управление пользователями.

---


## Работа с тестами

---

### 1. **`POST /get_data`**

#### Описание:
Этот эндпоинт используется для получения данных тестов по заданному `subjectId` и `testId`. Если `testId` не указан, возвращаются все тесты, связанные с предметом.

#### Пример запроса:

```python
import requests

url = "http://yourserver.com/get_data"
headers = {
    "session_token": "your_session_token"
}
data = {
    "subjectId": 1,
    "testId": 2
}

response = requests.post(url, headers=headers, json=data)
print(response.json())
```

#### Ожидаемые данные запроса:
- `session_token` (обязательный): Токен сессии, полученный после авторизации.
- `subjectId` (обязательный): Идентификатор предмета.
- `testId` (необязательный): Идентификатор теста.

#### Ожидаемые данные ответа:
- В случае успеха: Данные тестов. Если указаны `testId`, возвращается только один тест.
- В случае ошибок (например, если не найден токен или тест): Сообщение об ошибке.

---

### 2. **`POST /add_test`**

#### Описание:
Эндпоинт для добавления нового теста. Требуется авторизация, а также подтверждение, что пользователь является администратором (роль 1).

#### Пример запроса:

```python
import requests

url = "http://yourserver.com/add_test"
headers = {
    "session_token": "your_session_token"
}
data = {
    "subjectId": 1,
    "name": "New Test",
    "description": "Description of the new test",
    "questionsId": [1, 2, 3]
}

response = requests.post(url, headers=headers, json=data)
print(response.json())
```

#### Ожидаемые данные запроса:
- `session_token` (обязательный): Токен сессии.
- `data`: Объект с данными теста, который нужно добавить:
  - `subjectId`: Идентификатор предмета.
  - `name`: Название теста.
  - `description`: Описание теста.
  - `questionsId`: Массив идентификаторов вопросов для теста.

#### Ожидаемые данные ответа:
- В случае успеха: `{ "ok": true }`
- В случае ошибки: Сообщение об ошибке, например, если роль пользователя не является администратором.

---

### 3. **`POST /update_data`**

#### Описание:
Эндпоинт для обновления данных теста. Пользователь должен быть администратором.

#### Пример запроса:

```python
import requests

url = "http://yourserver.com/update_data"
headers = {
    "session_token": "your_session_token"
}
data = {
    "testId": 2,
    "data": {
        "name": "Updated Test Name",
        "description": "Updated description",
        "activate": True,
        "questionsId": [1, 2, 4]
    }
}

response = requests.post(url, headers=headers, json=data)
print(response.json())
```

#### Ожидаемые данные запроса:
- `session_token` (обязательный): Токен сессии.
- `testId` (обязательный): Идентификатор теста, который необходимо обновить.
- `data`: Новый объект данных теста:
  - `name`: Название теста.
  - `description`: Описание.
  - `activate`: Статус активации теста.
  - `questionsId`: Массив идентификаторов вопросов.

#### Ожидаемые данные ответа:
- В случае успеха: `{ "ok": true }`
- В случае ошибки: Сообщение об ошибке, например, если роль пользователя не является администратором.

---

### 4. **`POST /get_full_data`**

#### Описание:
Возвращает полные данные теста с вопросами и попытками для администратора.

#### Пример запроса:

```python
import requests

url = "http://yourserver.com/get_full_data"
headers = {
    "session_token": "your_session_token"
}
data = {
    "testId": 2
}

response = requests.post(url, headers=headers, json=data)
print(response.json())
```

#### Ожидаемые данные запроса:
- `session_token` (обязательный): Токен сессии.
- `testId` (обязательный): Идентификатор теста.

#### Ожидаемые данные ответа:
- В случае успеха: Объект с полными данными теста, включая вопросы и попытки.
- В случае ошибки: Сообщение об ошибке, если не найден тест или попытки.

---

### 5. **`POST /start_test`**

#### Описание:
Запускает тест для пользователя, проверяя его права доступа и доступность теста.

#### Пример запроса:

```python
import requests

url = "http://yourserver.com/start_test"
headers = {
    "session_token": "your_session_token"
}
data = {
    "subjectId": 1,
    "testId": 2
}

response = requests.post(url, headers=headers, json=data)
print(response.json())
```

#### Ожидаемые данные запроса:
- `session_token` (обязательный): Токен сессии.
- `subjectId` (обязательный): Идентификатор предмета.
- `testId` (обязательный): Идентификатор теста.

#### Ожидаемые данные ответа:
- В случае успеха: `{ "attemptId": <id_attempt> }` — Идентификатор попытки.
- В случае ошибки: Сообщение об ошибке, если тест не доступен или пользователь уже имеет активную попытку.

---

### 6. **`POST /save_answers`**

#### Описание:
Сохраняет ответы пользователя на вопросы в тесте.

#### Пример запроса:

```python
import requests

url = "http://yourserver.com/save_answers"
headers = {
    "session_token": "your_session_token"
}
data = {
    "attemptId": 1,
    "answers": [1, 2, 3]
}

response = requests.post(url, headers=headers, json=data)
print(response.json())
```

#### Ожидаемые данные запроса:
- `session_token` (обязательный): Токен сессии.
- `attemptId` (обязательный): Идентификатор попытки.
- `answers` (обязательный): Массив ответов на вопросы.

#### Ожидаемые данные ответа:
- В случае успеха: `{ "ok": true }`
- В случае ошибки: Сообщение об ошибке, если попытка уже завершена или данные некорректны.

---

### 7. **`POST /stop_attempt`**

#### Описание:
Завершает активную попытку теста.

#### Пример запроса:

```python
import requests

url = "http://yourserver.com/stop_attempt"
headers = {
    "session_token": "your_session_token"
}
data = {
    "attemptId": 1
}

response = requests.post(url, headers=headers, json=data)
print(response.json())
```

#### Ожидаемые данные запроса:
- `session_token` (обязательный): Токен сессии.
- `attemptId` (обязательный): Идентификатор попытки.

#### Ожидаемые данные ответа:
- В случае успеха: `{ "ok": true }`
- В случае ошибки: Сообщение об ошибке, если попытка уже завершена.

---

### 8. **`POST /get_question_attempt`**

#### Описание:
Возвращает вопросы для конкретной попытки теста для пользователя.

#### Пример запроса:

```python
import requests

url = "http://yourserver.com/get_question_attempt"
headers = {
    "session_token": "your_session_token"
}
data = {
    "attemptId": 1
}

response = requests.post(url, headers=headers, json=data)
print(response.json())
```

#### Ожидаемые данные запроса:
- `session_token` (обязательный): Токен сессии.
- `attemptId` (обязательный): Идентификатор попытки.

#### Ожидаемые данные ответа:
- В случае успеха: Объект с данными теста, вопросами и попытками.
- В случае ошибки: Сообщение об ошибке, если попытка не найдена или пользователь не имеет доступа к тесту.

---

### 9. `/get_question_attempt_result`

**Описание:**
Этот эндпоинт используется для получения результата попытки теста, основываясь на `attemptId`. Для авторизации используется `session_token`, который необходимо передать в теле запроса или в заголовке.

**Пример запроса на Python:**

```python
import requests

url = "http://yourserver.com/get_question_attempt_result"
data = {
    "attemptId": "12345", 
    "session_token": "your_session_token"
}

response = requests.post(url, json=data)
print(response.json())
```

**Запрашиваемые данные:**
- `attemptId`: Идентификатор попытки теста.
- `session_token`: Токен сессии пользователя (можно передавать как часть тела запроса или в заголовке).

**Возвращаемые данные:**
- Информация о попытке, тесте и вопросах.
- Структура ответа:
    ```json
    {
        "attempt": { ... },
        "data": {
            "name": "Test Name",
            "description": "Test Description"
        },
        "questions": [{...}, {...}]
    }
    ```

---

### 10. `/get_active_attempt`

**Описание:**
Этот эндпоинт используется для получения активной попытки теста для пользователя. Параметры запроса включают `testId` и `session_token`.

**Пример запроса на Python:**

```python
import requests

url = "http://yourserver.com/get_active_attempt"
data = {
    "testId": "67890", 
    "session_token": "your_session_token"
}

response = requests.post(url, json=data)
print(response.json())
```

**Запрашиваемые данные:**
- `testId`: Идентификатор теста.
- `session_token`: Токен сессии пользователя.

**Возвращаемые данные:**
- Идентификатор активной попытки.
    ```json
    {
        "attemptId": "123456"
    }
    ```

---

### 11. `/get_attempts`

**Описание:**
Этот эндпоинт позволяет получить все попытки теста для определенного пользователя. Требует `testId` и `session_token`.

**Пример запроса на Python:**

```python
import requests

url = "http://yourserver.com/get_attempts"
data = {
    "testId": "67890", 
    "session_token": "your_session_token"
}

response = requests.post(url, json=data)
print(response.json())
```

**Запрашиваемые данные:**
- `testId`: Идентификатор теста.
- `session_token`: Токен сессии пользователя.

**Возвращаемые данные:**
- Список попыток теста.
    ```json
    [
        { "attemptId": "1", "status": "completed" },
        { "attemptId": "2", "status": "in_progress" }
    ]
    ```

---

### 12. `/add_question`

**Описание:**
Эндпоинт для добавления нового вопроса в тест. Требует данных вопроса, идентификатора предмета и токена сессии.

**Пример запроса на Python:**

```python
import requests

url = "http://yourserver.com/add_question"
data = {
    "data": {
        "question": "What is 2+2?", 
        "options": ["2", "3", "4", "5"], 
        "answer": "4"
    },
    "subjectId": "101", 
    "session_token": "your_session_token"
}

response = requests.post(url, json=data)
print(response.json())
```

**Запрашиваемые данные:**
- `data`: Данные нового вопроса (включает текст вопроса, варианты ответа и правильный ответ).
- `subjectId`: Идентификатор предмета.
- `session_token`: Токен сессии пользователя.

**Возвращаемые данные:**
- Ответ от сервера о добавлении вопроса.
    ```json
    {
        "ok": true,
        "questionId": "123456"
    }
    ```

---

### 13. `/update_question`

**Описание:**
Этот эндпоинт используется для обновления существующего вопроса. Требует данных вопроса, его идентификатор и токен сессии.

**Пример запроса на Python:**

```python
import requests

url = "http://yourserver.com/update_question"
data = {
    "data": {
        "question": "What is 2+2?", 
        "options": ["2", "3", "4", "5"], 
        "answer": "4"
    },
    "questionId": "123456", 
    "session_token": "your_session_token"
}

response = requests.post(url, json=data)
print(response.json())
```

**Запрашиваемые данные:**
- `data`: Новые данные для обновления вопроса.
- `questionId`: Идентификатор вопроса.
- `session_token`: Токен сессии пользователя.

**Возвращаемые данные:**
- Ответ от сервера о результате обновления.
    ```json
    {
        "ok": true,
        "message": "Question updated successfully"
    }
    ```

---

### 14. `/get_questions`

**Описание:**
Этот эндпоинт используется для получения вопросов, связанных с тестом или предметом. Запрос может включать `questionId` или `subjectId`.

**Пример запроса на Python:**

```python
import requests

url = "http://yourserver.com/get_questions"
data = {
    "subjectId": "101", 
    "session_token": "your_session_token"
}

response = requests.post(url, json=data)
print(response.json())
```

**Запрашиваемые данные:**
- `questionId`: Идентификатор вопроса (если нужно получить только один вопрос).
- `subjectId`: Идентификатор предмета (если нужно получить все вопросы для предмета).
- `session_token`: Токен сессии пользователя.

**Возвращаемые данные:**
- Вопросы, связанные с `subjectId` или `questionId`.
    ```json
    [
        { "questionId": "1", "question": "What is 2+2?", "options": ["2", "3", "4", "5"], "answer": "4" },
        { "questionId": "2", "question": "What is 3+3?", "options": ["3", "4", "6", "9"], "answer": "6" }
    ]
    ```

---

### 15. `/delete_data`

**Описание:**
Эндпоинт для удаления теста. Требует идентификатора теста и токена сессии.

**Пример запроса на Python:**

```python
import requests

url = "http://yourserver.com/delete_data"
data = {
    "testId": "67890", 
    "session_token": "your_session_token"
}

response = requests.post(url, json=data)
print(response.json())
```

**Запрашиваемые данные:**
- `testId`: Идентификатор теста.
- `session_token`: Токен сессии пользователя.

**Возвращаемые данные:**
- Ответ о результате удаления.
    ```json
    {
        "ok": true
    }
    ```

---

### 15. `/delete_question`

**Описание:**
Этот эндпоинт используется для удаления вопроса. Требует идентификатора вопроса и токена сессии.

**Пример запроса на Python:**

```python
import requests

url = "http://yourserver.com/delete_question"
data = {
    "questionId": "123456", 
    "session_token": "your_session_token"
}

response = requests.post(url, json=data)
print(response.json())
```

**Запрашиваемые данные:**
- `questionId`: Идентификатор вопроса.
- `session_token`: Токен сессии пользователя.

**Возвращаемые данные:**
- Ответ о результате удаления.
    ```json
    {
        "ok": true
    }
    ```

