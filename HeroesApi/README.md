# README.md для лабораторной работы №30

```markdown
# Лабораторная работа №30: JSON и модели данных

## Основная информация

**ФИО:** Грошев Никита Андреевич  
**Группа:** ИСП-232  
**Дата:** 02.04.2026

---

## Описание проекта

В ходе лабораторной работы создано API для управления героями (Heroes API) с демонстрацией работы с JSON в ASP.NET Core. Изучены настройки сериализации, атрибуты для управления JSON-полями и способы работы с вложенными объектами.

### Что было изучено:
- Сериализация и десериализация объектов в JSON
- Настройка имен полей через `[JsonPropertyName]`
- Исключение полей из JSON через `[JsonIgnore]`
- Глобальная настройка JSON в `Program.cs` (camelCase, enum как строка)
- Работа с вложенными объектами и коллекциями
- Фильтрация и поиск по данным
- Тестирование API через Swagger и REST Client

---

## Как запустить проект

### 1. Перейти в папку проекта
```bash
cd HeroesApi
```

### 2. Восстановить зависимости
```bash
dotnet restore
```

### 3. Запустить сервер
```bash
dotnet run
```

Сервер запустится на порту, указанном в `launchSettings.json`.

### 4. Открыть Swagger UI
```
http://localhost:5000/swagger
```

---

## Структура проекта

```
HeroesApi/
├── Controllers/
│   └── HeroesController.cs          # Контроллер героев
├── Models/
│   └── Hero.cs                       # Модель героя (с вложенным Weapon, enum Universe)
├── Data/
│   └── HeroesStore.cs                # Статическое хранилище героев
├── Program.cs                        # Настройка приложения (JSON, CORS, Swagger)
├── appsettings.json                  # Конфигурация
└── Properties/
    └── launchSettings.json           # Настройки запуска
```

---

## Модели данных

### Enum Universe
```csharp
public enum Universe
{
    Marvel,
    DC
}
```

### Класс Weapon (вложенный объект)
```csharp
public class Weapon
{
    public string Name { get; set; } = string.Empty;
    public bool IsRanged { get; set; }
}
```

### Класс Hero
```csharp
public class Hero
{
    public int Id { get; set; }

    [JsonPropertyName("name")]
    public string Name { get; set; } = string.Empty;

    public string RealName { get; set; } = string.Empty;
    public Universe Universe { get; set; }
    public int PowerLevel { get; set; }
    public List<string> Powers { get; set; } = new();
    public Weapon Weapon { get; set; } = new();

    [JsonIgnore]
    public string? InternalNotes { get; set; }
}
```

---

## Настройка JSON в Program.cs

```csharp
builder.Services.AddControllers()
    .AddJsonOptions(options => {
        // camelCase для всех имён: "realName" вместо "RealName"
        options.JsonSerializerOptions.PropertyNamingPolicy = JsonNamingPolicy.CamelCase;
        // Enum как строка: "Marvel" вместо 0
        options.JsonSerializerOptions.Converters
            .Add(new JsonStringEnumConverter());
        // Красивый JSON с отступами — удобно при разработке
        options.JsonSerializerOptions.WriteIndented = true;
    });
```

**Эффект настроек:**

| Настройка | Пример |
|-----------|--------|
| `CamelCase` | `RealName` → `"realName"` |
| `JsonStringEnumConverter` | `Universe.Marvel` → `"Marvel"` |
| `WriteIndented` | JSON с отступами для чтения |

---


## Пример ответа (GET /api/heroes/1)

```json
{
    "id": 1,
    "name": "Человек-паук",
    "realName": "Питер Паркер",
    "universe": "Marvel",
    "powerLevel": 75,
    "powers": ["паутина", "лазание по стенам", "паучье чутьё"],
    "weapon": {
        "name": "Паутинострел",
        "isRanged": true
    }
}
```

---

## Атрибуты JSON

| Атрибут | Назначение | Пример |
|---------|------------|--------|
| `[JsonPropertyName("name")]` | Задаёт имя поля в JSON | `Name` → `"name"` |
| `[JsonIgnore]` | Исключает поле из JSON | `InternalNotes` не попадает в ответ |

---

## GitHub репозиторий

[Ссылка на репозиторий](https://github.com/Hig-lime-simp/isrpo_lab30.git)

---

## Автор

**ФИО:** Грошев Никита Андреевич  
**Группа:** ИСП-232  
**Дата:** 02.04.2026
```