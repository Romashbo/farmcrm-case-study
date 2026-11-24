# Диаграммы процессов и взаимодействий

[К кейсу](../README.md) · [ER-модель](../architecture/data-model.md)

## Sequence — оплата подписки (AS-IS)

```mermaid
sequenceDiagram
 actor U as Пользователь
 participant UI as Web UI
 participant API as FarmCRM
 participant DB as PostgreSQL
 participant P as ЮKassa
 U->>UI: Выбирает тариф
 UI->>API: Создать оплату
 API->>DB: Создать ожидающий платёж
 API->>P: Запрос оплаты
 P-->>API: Ссылка на оплату
 API-->>UI: Ссылка
 U->>P: Оплата
 P-->>API: Уведомление
 API->>P: Проверка статуса
 alt Подтверждённая оплата
  API->>DB: Транзакционная активация при первом подтверждении
 else Нет подтверждения
  Note over API,DB: Не активировать
 end
```

## Activity — удаление фермы (AS-IS)

```mermaid
flowchart TD
 A([Запрос удаления]) --> B{Доступ разрешён?}
 B -->|Нет| E[Отказать]
 B -->|Да| C{Есть животные?}
 C -->|Нет| F[Удалить ферму]
 C -->|Да| D{Выбрана другая своя ферма?}
 D -->|Нет| E
 D -->|Да| G[Перенести животных]
 G --> F
 F --> H{Операции успешны?}
 H -->|Да| I[COMMIT]
 H -->|Нет| J[ROLLBACK]
 I --> K([Результат])
 J --> K
 E --> K
```

## Sequence — движение медикамента (AS-IS)

```mermaid
sequenceDiagram
 actor U as Пользователь
 participant API as FarmCRM
 participant DB as PostgreSQL
 U->>API: Списать препарат
 API->>DB: Начать транзакцию и заблокировать остаток
 alt Недостаточный остаток
  API->>DB: ROLLBACK
  API-->>U: Ошибка
 else Допустимый расход
  API->>DB: Изменить остаток и записать движение
  API->>DB: COMMIT
  API-->>U: Успех
 end
```

## Состояния напоминания (TO-BE, предлагаемая модель)

```mermaid
stateDiagram-v2
 [*] --> pending
 pending --> pending: Перенести дату
 pending --> completed: Выполнить
 pending --> cancelled: Отменить
 completed --> [*]
 cancelled --> [*]
```

Допустимость повторного открытия и судьбу связанного события следует согласовать. Это целевая модель, а не исчерпывающее описание текущего API.

## Карта возможностей

```mermaid
flowchart LR
 U((Владелец)) --> F[Управлять фермами]
 U --> A[Вести карточки животных]
 U --> H[Планировать уход]
 U --> R[Учитывать продукцию и препараты]
 U --> B[Управлять подпиской]
 H --> E[Фиксировать выполнение]
 ADM((Администратор)) --> P[Управлять пользователями и тарифами]
```

Диаграммы Mermaid — презентационные, не формальная BPMN-нотация.
