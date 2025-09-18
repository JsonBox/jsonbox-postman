# jsonbox-postman
Postman-коллекция для JsonBox.ru — лёгкого REST API-сервиса для хранения JSON-данных. В коллекции собраны основные запросы: регистрация, работа с данными, управление настройками и просмотр страниц (Pages). Удобный старт для разработчиков: импортируйте и используйте API за 3 минуты.

## Содержание репозитория

```
jsonbox-postman/
├─ examples/
│  └─ quickstart.md
├─ JsonBox_API_postman_collection.json
├─ LICENSE
└─ README.md
```

---

## Быстрый старт

1. Скачайте файл `JsonBox_API_postman_collection.json` (в этом репозитории он уже есть).
2. Откройте Postman (или Insomnia / Hoppscotch) → **Import** → выберите файл `JsonBox_API_postman_collection.json`.
3. Создайте окружение (Environment) и добавьте переменную `api_key`.  
   - Для просмотра страниц (`page.php`) используйте **read‑only ключ**, начинающийся с `ro_` (пример: `ro_abc123...`).  
   - Для операций записи (store/delete/set_setting и т.п.) используйте полный `api_key` (необязательно для Page-view, и возможно отключён для этой фичи — см. ниже про безопасность).

> Примечание: страничный просмотр (`page.php`) безопасно работает только с ключом **только для чтения** — `api_key_read_only` (префикс `ro_`). Полный ключ **не будет работать** для публичного веб-рендера по соображениям безопасности.

---

## Переменные Postman

- `api_key` — основной placeholder, используемый в коллекции. Замените на ваш `ro_...` или `api_key`.

---

## Ключевые запросы (коротко)

### Сохранить данные
```
POST https://jsonbox.ru/api.php?action=store
Content-Type: application/json

{
  "api_key": "YOUR_FULL_API_KEY",
  "data": { ... }
}
```

### Получить данные
```
GET https://jsonbox.ru/api.php?action=get&api_key=YOUR_API_KEY
```

### Удалить данные
```
DELETE https://jsonbox.ru/api.php?action=delete
Content-Type: application/json

{ "api_key": "YOUR_FULL_API_KEY" }
```

### Настройки
- Получить все настройки:
```
GET https://jsonbox.ru/api.php?action=get_settings&api_key=YOUR_API_KEY
```
- Изменить настройку (например, включить/выключить веб-отображение или поставить тему):
```
POST https://jsonbox.ru/api.php?action=set_setting
Content-Type: application/json

{
  "api_key": "YOUR_FULL_API_KEY",
  "setting_name": "web_display_enabled",
  "setting_value": "false"
}
```

### Перегенерировать ключ
```
POST https://jsonbox.ru/api.php?action=regenerate_key
Content-Type: application/json

{ "api_key": "YOUR_FULL_API_KEY" }
```

### Просмотр как веб-страницы (Pages)
```
https://jsonbox.ru/page.php?key=ro_YOUR_READ_ONLY_KEY&page=PAGE_SLUG
```
- `page` — необязательный параметр; если не указан — показывается первая страница из массива `pages`.
- Обязательно используйте ключ `ro_...` (read-only). Полный ключ для этой операции не используется/не поддерживается по соображениям безопасности.
