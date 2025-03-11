# Что такое Effector

## Введение

В этом уроке мы погрузимся в основы effector, его ключевые возможности и API. Мы разберём фундаментальные понятия, такие как события, сторы и сайд-эффекты, и посмотрим, как с их помощью можно выстраивать последовательные сценарии.

Effector не только помогает упростить код, но и позволяет разработчику сосредоточиться на бизнес-логике, оставив сложность обработки данных за пределами компонентов.

Разработчики использующие React привыкли, что логику необходимо размещать внутри хуков `useEffect`, `useCallback` снабжая внушительным количеством инструкций, помогающих фреймворку избегать деоптимизаций. Effector предлагает подход, когда React выступает лишь слоем представления, используя его самые сильные стороны, а логика находится в отдельном жизненном цикле, не мешая рендерингу компонентов.

## События

События - основа реактивности в effector. Это механизм, через который мы можем сигнализировать о том, что произошло действие (например, нажатие кнопки или отправка формы).

События создаются с помощью createEvent и позволяют отслеживать конкретные моменты в работе приложения. Важной особенностью событий является их способность запускать цепочки операций, создавая таким образом сценарии взаимодействий.

```ts
export const buttonClicked = createEvent();
export const formSubmitted = createEvent();
```

События не хранят в себе состояние, но можно подписаться на вызов событий:

```ts
buttonClicked.watch(() => console.info('button clicked'));
formSubmitted.watch(() => console.info('form submitted'));
```

Запустить события довольно просто, можно вызывать их как функцию:

```ts
buttonClicked();
//=> button clicked

formSubmitted();
//=> form submitted
```

## Взаимодействия

Взаимодействие между событиями - это важная часть построения реактивной логики. Effector позволяет настраивать связи между событиями, обеспечивая гибкость в обработке пользовательских сценариев.

Например, с помощью оператора `sample` можно связать одно событие с другим, чтобы при срабатывании первого автоматически вызывалось второе. Это позволяет выстраивать последовательные и предсказуемые цепочки действий, создавая более сложное поведение приложения.

```ts
sample({
  clock: buttonClicked,
  target: formSubmitted,
});
```

Когда сработает `buttonClicked`, оператор sample отреагирует на это и вызовет `formSubmitted`.

```ts
buttonClicked();
//=> button clicked
//=> form submitted
```

Задача разработчика, разобраться какой пользовательский сценарий необходимо реализовать, выразить его в виде событий и связать всё через несколько sample.

## Хранение значений

Для большинства приложений важно не только отслеживать события, но и хранить состояние в рамках сессии, которое может изменяться со временем. Effector использует концепцию реактивных переменных, называемых Store (стор), для хранения значений и отслеживания их обновлений.

Каждый раз, когда значение в сторе меняется, все подписчики получают актуальное значение, что делает обновление данных прозрачным и управляемым.

```ts
const $counter = createStore(0);

$counter.updates.watch((counter) =>
  console.log('counter updated with', counter)
);
```

Один стор - это одно реактивное значение, все его подписчики будут получать обновления каждый раз, как его внутреннее значение обновляется на новое.

Изменять значение можно например через `.on()`:

```ts
$counter.on(buttonClicked, (counter) => counter + 1);

buttonClicked();
//=> button clicked
//=> counter updated with 1
//=> form submitted
```

Кроме того, используя `sample` можно замещать значение в сторе:

```ts
const appStarted = createEvent();
const $initialized = createStore(false);

sample({
  clock: appStarted,
  fn: () => true,
  target: $initialized,
});

appStarted.watch(() => console.log('app started'));
$initialized.updates.watch((init) => console.info('initialized?', init));

appStarted();
//=> app started
//=> initialized? true
```

В этом примере с `sample` есть свойство `fn:`, которое позволяет подставить/преобразовать данные перед установкой значения в стор.

При этом, повторные вызовы не изменят значение стора:

```ts
appStarted();
//=> app started

// Стор $initialized уже имеет true, значит обновляться не будет.
// Мемоизация из коробки!
```

## Сайд-эффекты

Сайд-эффекты - это действия, которые выходят за рамки обычного вычисления и могут включать асинхронные операции, такие как загрузка данных с сервера.

В effector для выполнения таких операций используется специальная конструкция `createEffect`, которая позволяет отслеживать успешное выполнение, ошибки и статус выполнения эффекта.

```ts
const fetchTodoFx = createEffect(async (listId: string) => {
  const response = await fetch('/todos' + listId);
  if (!response.ok) throw new Error('Failed to load todos');
  return await response.json();
});
```

Эффекты декларируют два результата завершения: успешный и с ошибкой:

```ts
fetchTodoFx.done.watch(({ result }) =>
  console.info('fetchTodoFx finished with', result)
);
fetchTodoFx.fail.watch(({ error }) =>
  console.warn('fetchTodoFx failed with', error)
);
```

А запускать эффекты можно как внутри других эффектов:

```ts
const anotherLoaderFx = createEffect(async () => {
  await fetchTodoFx('example-list-id');
});
```

Так и через sample:

```ts
sample({
  clock: $someId,
  target: fetchTodoFx,
});
```

## Экосистема

Кроме основных операторов и функций, effector предлагает целую экосистему библиотек для решения повседневных задач: комбинирование сторов, сохранение данных в локальном хранилище, управление кешированием и повторными попытками запросов.

Эти библиотеки расширяют стандартные возможности effector и делают его более мощным и удобным для разработки комплексных приложений.

Например, вот так можно комбинировать разные сторы с помощью effector:

```ts
const $pageLoaded = createStore(false);
const $todosLoaded = createStore(false);

export const $finished = combine(
  $pageLoaded,
  $todosLoaded,
  (pageLoaded, todosLoaded) => pageLoaded && todosLoaded
);
```

А вот так с помощью effector patronum:

```ts
import { and } from 'patronum';

const $pageLoaded = createStore(false);
const $todosLoaded = createStore(false);

export const $finished = and($pageLoaded, $todosLoaded);
```

Кроме этого, имеются библиотеки для сохранения данных в локальных хранилищах вроде LocalStorage, SessionStorage, и тд.

```ts
import { persist } from 'effector-storage/local';

persist({
  store: $someId,
  key: 'todo-id',
  pickup: appStarted,
});
```

А вот целая экосистема для выполнения запросов, мутаций, проведения валидации данных в рантайме, управления повторными вызовами и кешированием:

```ts
import { createJsonQuery, isNetworkError, retry } from '@farfetched/core';

const itemsQuery = createJsonQuery({
  request: { url: '/api/items', method: 'GET' },
  response: { contract: ItemsResponseContract },
});

retry(itemsQuery, {
  times: 3,
  delay: 1000,
  filter: isNetworkError,
});

itemsQuery.finished.success.watch((items) => {
  console.log('Items loaded', items);
});
itemsQuery.finished.error.watch((error) => {
  console.error('Failed to load items', error);
});

itemsQuery.refresh();
```

## Заключение

Effector позволяет выстраивать архитектуру приложения, начиная с бизнес-логики, что облегчает поддержку и тестирование кода. Логика приложения вынесена за пределы компонентов, что позволяет оставить в них только рендеринг, убирая необходимость использования множества хуков вроде `useEffect` и `useMemo`.

В результате код становится более чистым и понятным, а его компоненты можно использовать независимо и эффективно тестировать. Effector помогает создавать масштабируемые и поддерживаемые приложения, что особенно важно для работы с реактивными интерфейсами.
