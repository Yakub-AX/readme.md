# Документация Проекта

<details open>
  <summary><strong><font size="5">1. Введение</font></strong></summary>

### 1.1 Обзор Проекта

Этот проект представляет собой современное веб-приложение, созданное с использованием следующих технологий:

- **Feature-Sliced Design (FSD)**
- **Tailwind CSS**
- **TypeScript**
- **Redux**
- **React**
- **Vite**
</details>

<details open>
  <summary><strong><font size="5">2. Структура Проекта</font><strong></summary>

### 2.1 Основные Уровни

- **app/**: Содержит основные настройки приложения, глобальные компоненты и провайдеры. Например, глобальные стили, настройки роутинга или провайдеры контекста.
- **shared/**: Хранит общие для всего приложения модули и компоненты, которые могут использоваться в разных частях приложения. Это могут быть утилиты, типы данных, базовые UI-компоненты и стили.
- **entities/**: Модули, представляющие ключевые сущности предметной области, такие как пользователи, продукты, заказы и т.д.
- **features/**: Отвечает за конкретные функциональные возможности приложения, такие как форма авторизации, фильтры, поиск и т.д.
- **widgets/**: Независимые функциональные блоки, которые могут объединять несколько компонентов и использоваться на различных страницах приложения. Например, навигационное меню, хедер или футер.
- **pages/**: Страницы приложения, которые состоят из виджетов, фич и сущностей. Каждая страница — это самостоятельный модуль, который отвечает за отрисовку определенного раздела приложения.

### 2.2. Касательно изменений которые мы внесли для арихиектуры:

1. **В Каждом слое за исключением (shared, app) иерархия строится модульно, то есть если у нас есть модуль `user` в entities то в каждом следуешем слое(features, widgets etc.) будет создавать модуль `user` и раобота будет вестись в этом модуле**

   Пример:

   ```
   src/
   ├── entities/
   │   └── user/
   │       ├── model/
   │       └── ui/
   │
   ├── features/
   │   └── user/
   │       └── ui/
   │
   ├── widgets/
   │   └── user/
   │       └── ui/
   │
   ├── pages/
   │   └── user/
   │       └── ui/
   ```

2. **_Слой shared тоже был изменен исходя из наших нужнд в разработке, добавился новй сегмент `shared/components`_**

   Основная задача этого сегмента заключается в том что он реализует переиспользуемые компоненты специфичные только для проекта

   A `shared/ui` реализует компоненты общего назначение

   **_Пример_**:

   ```
   shared/components: <GeneralInfo/>, <Customize/>

   shared/ui: <Button/>, <Input/>
   ```

### 2.3 Правила Наименования

В проекте используются следующие соглашения по наименованию:

- **Компоненты**: Именуются в стиле **`PascalCase`**. Пример: `UserProfile`, `NavBar`.
- **Папки**: Именуются в стиле **`kebab-case`**. Пример: `user-profile`, `nav-bar`.

</details>

<details open>
<summary><strong><font size="5">3. Использование</font></strong></summary>

### 3.1 Настройка окружений:

Проект использует два основных окружения для разработки: **staging** и **development**. Каждое окружение имеет свой конфигурационный файл:

- **Staging окружение**: `.env.staging.local`
- **Development окружение**: `.env.development.local`

## Конфигурационные файлы

Для настройки вашего окружения создайте соответствующие `.env` файлы в корневой директории проекта:

### Staging окружение

Для staging окружения используйте следующий файл:

```bash
.env.staging.local
```

Этот файл должен содержать все переменные окружения, специфичные для staging окружения.

### Development окружение

Для development окружения используйте следующий файл:

```bash
.env.development.local
```

Этот файл должен содержать все переменные окружения, специфичные для development окружения.

## Переключение между окружениями

Чтобы переключиться между окружениями, убедитесь, что используется правильный `.env` файл. Это можно сделать либо вручную, скопировав нужный файл в `.env.local`, либо с помощью скриптов сборки для конкретного окружения.

### Примеры использования:

- Чтобы запустить приложение в режиме **staging**:

  ```bash
  npm run start:staging
  ```

- Чтобы запустить приложение в режиме **development**:

  ```bash
  npm run start:development
  ```

Убедитесь, что ваши процессы сборки и деплоя используют правильные `.env` файлы в зависимости от окружения.

</details>
<details open>

<summary><strong><font size="5">4. Основные правила кода</font></strong></summary>

## 4.1 Публичные поля:

**_Публичные поля создаются только на уровне `slice` и нигде более пример:_**

```
src/
│
└── features/
   │
   ├── user/
   │   ├── ui/
   │   └── index.ts
   │
   └── product/
       ├── ui/
       └── index.ts
```

## 4.2 Наименование Enum:

**_Наименование Enum полей просиходит только через `PascalCase` примеры:_**

Правильно ✅:

```ts
enum someCoolEnum {
  BrokerAgent = "BrokerAgent",
  BrokerAgency = "BrokerAgency",
}
```

Неправильно ❌:

```ts
enum someCoolEnum {
  broker_agent = "broker_agent",
  broker_agency = "broker_agency",
}
```

## 4.3 Cоздание функции внутри JSX:

**_Не создаем функцию внутри JSX(`cоздаем функцию над return`) примеры:_**

Правильно ✅:

```tsx
const renderList = useCallback(
  (data) => {
    return (
      <>
        <h1>{data.title}</h1>
        <button onClick={data.trigger}>Click me</button>
      </>
    );
  },
  [props],
);

retrun(
  <div>
    <p> List rendering: </p>
    {props.map(renderList)}
  </div>,
);
```

Неправильно ❌:

```tsx
retrun(
  <div>
    <p> List rendering: </p>
    {props.map((data) => {
      return (
        <>
          <h1>{data.title}</h1>
          <button onClick={data.trigger}>Click me</button>
        </>
      );
    })}
  </div>,
);
```

## 4.4 Render функции:

**_Render функции оборачиваем в `useCallback` примеры:_**

Правильно ✅:

```tsx
const renderList = useCallback(
  (data) => {
    return (
      <>
        <h1>{data.title}</h1>
        <button onClick={data.trigger}>Click me</button>
      </>
    );
  },
  [props],
);

retrun(
  <div>
    <p> List rendering: </p>
    {props.map(renderList)}
  </div>,
);
```

Неправильно ❌:

```tsx
const renderList = (data) => {
  return (
    <>
      <h1>{data.title}</h1>
      <button onClick={data.trigger}>Click me</button>
    </>
  );
};

retrun(
  <div>
    <p> List rendering: </p>
    {props.map(renderList)}
  </div>,
);
```

</details>
