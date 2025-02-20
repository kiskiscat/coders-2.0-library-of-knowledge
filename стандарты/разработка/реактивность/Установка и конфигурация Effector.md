# Установка

#### Шаг 1

Начнем с основных зависимостей, всё дополнительное поставим позже.

```shell
pnpm add effector effector-react
```

Так как effector умеет работать вообще без фреймворков для рендеринга компонентов, все хуки для react вынесены в отдельный пакет.

#### Шаг 2

Запустим dev-сервер:

```shell
pnpm dev
#  ➜  Local:   http://localhost:5173/
```

#### Шаг 3

Создадим файл `src/model.ts`

Реализуем простейший счетчик, наша задача проверить, что всё работает корректно

```typescript
import { createEvent, createStore } from 'effector';

export const incrementClicked = createEvent();
export const $counter = createStore(0);

$counter.on(incrementClicked, (counter) => counter + 1);

$counter.watch((counter) => console.info('[store] $counter', counter));
```

#### Шаг 4

Теперь напишем компонент для отображения счетчика прямо в `src/main.tsx`

```typescript
// src/main.tsx
import { useUnit } from 'effector-react';

import '@mantine/core/styles/ActionIcon.css';

import styles from './application.module.css';
import { Button } from './button';
import { Header } from './header';
import { KanbanBoard } from './kanban';
import { $counter, incrementClicked } from './model';

const Counter = () => {
  const [counter, handleIncrement] = useUnit([$counter, incrementClicked]);

  return <Button onClick={handleIncrement}>{counter}</Button>;
};

export const Application = () => {
  return (
    <>
      <Header />
      <Counter />
      <main className={styles.main}>
        <KanbanBoard />
      </main>
    </>
  );
};
```

#### Шаг 5

Если Вы видите в браузере кнопку счетчика и счетчик увеличивается, значит effector работает корректно

# Отладка

В большинстве случаев, мы не будем пользоваться `.watch()` для отладки, есть гораздо более удобный инструмент для печати логов в консоль - утилита debug из библиотеки patronum.

#### Шаг 1

Чтобы понять, как она работает, давайте установим и протестируем её

```shell
pnpm add patronum
```

#### Шаг 2

Теперь вместо `.watch()` можно использовать `debug()`

```typescript
import { debug } from 'patronum';

// ...

debug($counter, incrementClicked);
```

#### Шаг 3

Но в консоли Вы увидите что-то вроде:

```livescript
[store] 3 [getState] 0
[event] 1 SyntheticBaseEvent {...}
[store] 3 1
[event] 1 SyntheticBaseEvent {...}
[store] 3 2
[event] 1 SyntheticBaseEvent {...}
[store] 3 3
```

#### Шаг 4

Вместо чисел, мы должны увидеть название стора и события, чтобы исправить это, можно завернуть юниты переданные в `debug` в объект

```typescript
debug({ $counter, incrementClicked });
```

#### Шаг 5

Таким образом мы задали им имена именно для этой сессии отладки

```livescript
[store] $counter [getState] 0
[event] incrementClicked SyntheticBaseEvent {...}
[store] $counter 1
[event] incrementClicked SyntheticBaseEvent {...}
[store] $counter 2
[event] incrementClicked SyntheticBaseEvent {...}
[store] $counter 3
```

#### Шаг 6

Но debug имеет и другую полезную возможность - отображение трейсов, то есть последовательности срабатывания событий. Эта возможность позволяет узнать что привело к изменению стора или срабатыванию события.

```typescript
debug({ trace: true }, { $counter });
```

#### Шаг 7

Я убрал `incrementClicked` и добавил первым аргументом `{ trace: true }`.

```livescript
[store] $counter [getState] 0
[store] $counter 1
[store] $counter trace
  <- [store] $counter 1
  <- [on] $counter.on(1) 1
  <- [event] 1
```

Теперь, причина изменения стора отображается в отдельной `console.group`. Но здесь снова пропало имя события.

#### Шаг 8

Можно исправить ситуацию, задавая вручную все имена при создании юнитов

```typescript
export const incrementClicked = createEvent({
  name: 'incrementClicked',
});
```

Но это не самый надежный способ, ведь тогда, при переименовании юнита придется не забыть сменить имя указанное в аргументах.

Вот был бы способ автоматически синхронизировать имя юнитов и констант, которым они присваиваются.

# Настройка babel

Плагин для babel умеет не только в установку имен всем юнитам, но сейчас важно именно это.

Можно конечно продолжить вручную устанавливать имена, но это увеличит размер бандла дополнительными строками, плюс встанет вопрос синхронизации.

Настройка выполняется в два шага:

#### Шаг 1

Cоздать файл `.babelrc` в корне проекта

```json
{
  "plugins": ["effector/babel-plugin"]
}
```

#### Шаг 2

Включить `babelrc` в плагине `@vitejs/plugin-react`

```typescript
import react from '@vitejs/plugin-react';
import { defineConfig } from 'vite';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react({ babel: { babelrc: true } })],
  css: { modules: { localsConvention: 'camelCase' } },
});
```

#### Шаг 3

Теперь можно использовать debug без оборачивания в объект

```typescript
debug({ trace: true }, $counter);
```

Имена всех юнитов будут соответствовать именам констант, которым назначаются при создании.

# Как это работает

Можно обойтись и без `patronum/debug`, тогда нужно будет реализовать функцию отладки самостоятельно

```ts
$counter.watch((counter) =>
  console.log(`[store] ${$counter.shortName}`, counter)
);
```

Здесь используется специальное свойство `.shortName`, которое заполняется как раз плагинами к компиляторам вроде babel/swc/oxs, а также вручную через `{ name }` при создании юнита.

Ключевое отличие от `debug` в том, что последний умеет выводить трейсы, а также печатает начальное состояние стора в виде конструкции

```tsx
[store] $someStore [getState] value
```

Позже мы познакомимся с понятиями Scope и .sid, вот где `debug` принесет достаточно пользы, чтобы не отказываться от его использования.

Чтобы имена не попадали в production сборку, можно использовать `api.env("production")` в файлах конфигурации babel. Например, вот так можно указать опцию `addNames` в зависимости от типа сборки

```js
// babel.config.js
module.exports = function (api) {
  api.cache(true);

  return {
    plugins: [
      [
        'effector/babel-plugin',
        {
          addNames: api.env('production') === false,
        },
      ],
    ],
  };
};
```

# Заключение

Установка и конфигурация effector для Vite приложений обычно не занимает больше 5 минут. Самое важное в этом процессе - убедиться, что все работает корректно.

Чтобы не забыть убрать `patronum/debug` из кода перед отправкой pull request, можно использовать eslint вместе с `eslint-plugin-effector`.

Вообще, в этом плагине есть несколько встроенных пресетов, которые помогут писать более чистый и безопасный код.

Детально о конфигурации линтера поговорим в следующий раз.

Ключевые изменения в проекте можно посмотреть по ссылке:

[https://github.com/frontend-vision/brello-2024.02/commit/b6f576e3512102525fd8707f48de00b0efc48216](https://github.com/frontend-vision/brello-2024.02/commit/b6f576e3512102525fd8707f48de00b0efc48216)

## Полезные ссылки

- [Installation 🚀 effector](https://effector.dev/en/introduction/installation/)
- [Usage with effector-react 🚀 effector](https://effector.dev/en/typescript/usage-with-effector-react/)
- [effector patronum](https://patronum.effector.dev/)
- [debug](https://patronum.effector.dev/operators/debug/)
- [Config Files · Babel](https://babeljs.io/docs/config-files#apienv)
- [ESLint plugin for Effector | ESLint plugin for Effector](https://eslint.effector.dev/)
- [effector/no-patronum-debug | ESLint plugin for Effector](https://eslint.effector.dev/rules/no-patronum-debug.html)
