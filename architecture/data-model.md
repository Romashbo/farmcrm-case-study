# ER-модель (AS-IS, логическое представление)

[К кейсу](../README.md)

Модель восстановлена из схемы и миграций. Названия сущностей обобщены для публичной презентации; диаграмма не заменяет точный DDL.

```mermaid
erDiagram
 ACCOUNT ||--o{ FARM : owns
 ACCOUNT ||--o{ ANIMAL : owns
 FARM ||--o{ FARM_GROUP : contains
 FARM o|--o{ ANIMAL : hosts
 FARM_GROUP o|--o{ ANIMAL : groups
 BREED o|--o{ ANIMAL : classifies
 ANIMAL o|--o{ ANIMAL : parent
 ANIMAL o|--o{ EVENT : records
 FARM o|--o{ EVENT : context
 EVENT ||--o{ EVENT_OFFSPRING : links
 ANIMAL ||--o{ EVENT_OFFSPRING : offspring
 ANIMAL ||--o{ HEALTH_RECORD : history
 EVENT o|--o{ HEALTH_RECORD : source
 ANIMAL ||--o{ REMINDER : schedules
 EVENT o|--o{ REMINDER : completion
 FARM ||--o{ EGG_RECORD : produces
 FARM ||--o{ MEAT_RECORD : produces
 FARM ||--o{ MEDICATION : stores
 MEDICATION ||--o{ MEDICATION_MOVEMENT : tracks
 ACCOUNT ||--o{ SUBSCRIPTION : subscribes
 PLAN o|--o{ SUBSCRIPTION : defines
 SUBSCRIPTION o|--o{ PAYMENT : paid_by
 ACCOUNT ||--o{ PAYMENT : pays
```

| Сущность | Назначение |
| --- | --- |
| Account | Владение данными, роль платформы |
| Farm / Farm group | Хозяйство и загоны |
| Animal / Breed | Карточка, бирки, порода, родители |
| Event / Health record | История операций и здоровья |
| Reminder | План, статус, связь с событием |
| Egg / Meat record | Производственные операции |
| Medication / Movement | Остаток и журнал изменений |
| Plan / Subscription / Payment | Монетизация и история оплат |

### Ключевые правила модели

- Основная бирка уникальна в аккаунте.
- Карточка содержит текущее местоположение, событие — исторический контекст.
- События и напоминания моделируют факт и план раздельно.
- Остатки и движения изменяются согласованно.
- Модель подписок сохраняет несколько записей на аккаунт; текущий план определяется бизнес-логикой.
