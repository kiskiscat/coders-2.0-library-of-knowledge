# Как писать логику на Effector

## Введение

В прошлом уроке мы лишь прошлись по верхам, разобравшись с основными сущностями effector.

Для реализации простой логики достаточно того, что мы уже увидели, но сложная логика может потребовать больше телодвижений от разработчика, поэтому effector предлагает расширенные возможности определения и взаимодействия сущностей.

## События в effector

**Event** в Effector - это фундаментальная единица, представляющая собой действие, на которое реагирует система. События используются для обновления состояния, вызова эффектов или запуска других событий.

События именуются в формате `somethingHappened`, например: `buttonClicked`, `pageLoaded`, `appStarted`, `formSubmitted`. Встроенные события могут не всегда следовать этому паттерну, но для пользовательских событий это хороший стиль.

Пример создания события:

```ts
import { createEvent } from 'effector';

type UserSelected = {
  id: string;
};

export const userSelected = createEvent<UserSelected>();
```

В этом примере **userSelected** - это событие, которое может принимать нагрузку в виде объекта с полем `id`.

### EventCallable

Каждое событие, созданное через **createEvent**, имеет тип `EventCallable<T>` - то есть, событие можно использовать как функцию для передачи данных:

```ts
userSelected({ id: 'InGW4AcaZHDhO' });
```

События поддерживают только один аргумент. Поэтому если требуется передать больше, стоит завернуть данные в объект, а его свойства использовать для наглядного объяснения смысла значений. О причинах такого дизайн-решения поговорим позднее.

Также подобные события могут быть использованы в React через **useUnit**:

```tsx
import { useUnit } from 'effector-react';
import { userSelected } from './model';

export const SomeComponent = () => {
  const [handleUserSelection] = useUnit([userSelected]);

  return (
    <button onClick={() => handleUserSelection({ id: 'InGW4AcaZHDhO' })}>
      Select user
    </button>
  );
};
```

### Подписки

Чтобы отследить момент срабатывания того или иного события можно использовать `.watch(fn)`, но **только** для отладки.

Важно! Не нужно запускать в `.watch(fn)` какую-нибудь важную логику, так как метод для этого не предназначен, он подавляет ошибки и не позволяет подписаться на окончание выполнения.

```ts
userSelected.watch(({ id }) => console.info('Selected user with id=', id));

userSelected({ id: 'InGW4AcaZHDhO' });
//=> Selected user with id=InGW4AcaZHDhO
```

## Состояния в effector

**Store** в Effector представляет собой единицу состояния, которая отвечает за хранение данных и их реактивное обновление.

Сторы позволяют реагировать на события и эффекты, изменяя своё состояние. Стор можно представлять как реактивную переменную, рекомендуется хранить в сторах отдельные маленькие значения. Сторы именуются в формате `$oneOrTwoWords`, например: `$user`, `$isActive`, `$savedBytes`.

Пример создания стора:

```ts
import { createStore } from 'effector';

export const $count = createStore<number>(0);
```

Здесь **$count** инициализируется значением `0` и хранит его внутри объекта. Сторы могут хранить любые значения, но при обновлении нужно возвращать НОВОЕ значение. Таким образом хранение мутабельных значений, вроде инстансов классов, будет немного сложнее, поэтому рассмотрим позже.

### StoreWritable

Каждый новый стор созданный через createStore, имеет тип `StoreWritable<T>` - то есть значение стора можно модифицировать используя `.on()` и другие способы:

```ts
export const counterClicked = createEvent();

$count.on(counterClicked, (count) => count + 1);
```

При срабатывании события `counterClicked` будет вызвана функция в `.on()`, а ее результат будет записан в `$count`, если новое значение стора отличается от старого, подписчики стора получат обновление.

Первый интересный момент, `createEvent` вызван без указания типа, по умолчанию будет `EventWritable<void>`, то есть аргумент не требуется при вызове события как функции:

```ts
counterClicked(); // EventCallable<void>
```

Функция переданная в `.on()` называется **редюссер**, и должна быть чистой, чтобы effector мог выполнять оптимизации.

Кроме того, в редюссере может быть два аргумента: первый это состояние стора, второй - аргумент из события.

```ts
$store.on(someEvent, (storeValue, eventPayload) => newStoreValue);
```

Вот еще пример редюссера, в котором сразу вызывается деструктуризация второго аргумента, то есть значения из события `userSelected`.

```ts
$user.on(userSelected, (currentUser, { id }) => id);
```

Хранилища также могут использовать **.watch()** для отслеживания обновлений, например, для логирования:

```ts
$user.watch(({ id }) => console.info("User's current id", id));
```

**.watch()** чаще всего применяется в библиотечном коде или для отладки, но не рекомендуется для регулярного использования в бизнес-логике.

В случае сторов, функция в `.watch(fn)` будет вызвана при создании стора, а также при каждом обновлении.

## Сайд-эффекты в effector

**Effect** - это сущность для работы с **асинхронными** действиями, такими как HTTP-запросы, а также любыми операциями выбрасывающими **исключения**.

Эффекты принимают **один аргумент** и возвращают Promise, что делает их идеальными для интеграции с API или другими внешними системами. Кроме того, эффект обрабатывает выбрасываемые внутри функции исключения, что позволяет не пользоваться `try/catch` во время описания бизнес-логики.

Пример создания эффекта:

```ts
import { createEffect } from 'effector';

const fetchDataFx = createEffect(async (id: string) => {
  const response = await fetch(`/api/data/${id}`);
  return response.json();
});
```

**fetchDataFx** - это сайд-эффект, который выполняет запрос по переданному `id` и возвращает ответ в формате JSON.

Теперь стоит разобрать структуру эффекта:

```ts
const effectFx: Effect<Params, Done, Fail> = createEffect(handler);
```

Функция `createEffect` возвращает объект `Effect<Params,Done,Fail>`, который с помощью типов TypeScript описывает принимаемый аргумент `Params`, тип успешного завершения `Done` и тип ошибки `Fail` который по умолчанию является стандартной ошибкой `Error`.

Эффект можно вызывать как функцию, тогда он возвращает Promise независимо от реализации handler. Если handler выбросит ошибку, императивный вызов эффекта тоже выбросит ошибку, это поведение удобно использовать вызывая эффекты внутри других эффектов.

```ts
const result = await effectFx('91G5j00mQSz0B3VIwY');
```

При успешном завершении эффекта, будет вызвано событие `effectFx.done` с аргументом эффекта и результатом работы **handler**.

При выброшенной ошибке (или же Promise.reject), будет вызвано событие `effectFx.fail` с аргументом эффекта и ошибкой.

```ts
effectFx.done.watch(({ params, result }) =>
  console.log('Success', params, result)
);
effectFx.fail.watch(({ params, error }) =>
  console.warn('Failure', params, result)
);
```

С помощью чтения аргументов можно отличить один вызов эффекта от другого, позже будет наглядно, почему это важно.

```ts
effectFx.pending.watch((pending) => console.info('Store is pending', pending));
```

Кроме событий, в объекте эффекта имеются сторы, хорошим примером является `.pending`, который имеет тип `Store<boolean>`. После запуска эффекта, стор `.pending` будет обновлен со значением `true`, а после завершения с любым исходом обновится со значением `false`.

Полный пример жизненного цикла эффекта можно описать так:

```ts
const effectFx = createEffect(async (params) => {
  console.info('handler ran with', params);
  return 'RESULT';
});
effectFx.watch((params) => console.info('effect called with', params));
effectFx.done.watch(({ params, result }) =>
  console.log('effect finished', params, result)
);
effectFx.fail.watch(({ params, error }) =>
  console.warn('effect failed', params, result)
);
effectFx.pending.watch((pending) =>
  console.info('effect pending state is', pending)
);
//=> effect pending state is false

await effectFx('PARAMS');
//=> effect called with PARAMS
//=> effect pending state is true
//=> handler ran with PARAMS
//=> effect finished PARAMS RESULT
//=> effect pending state is false
```

## Логические цепочки

Сейчас мы вкратце посмотрели какие базовые возможности есть у событий, сторов и эффектов, но самое интересное кроется не внутри этих сущностей, а между ними. Бизнес-логика любого приложения это взаимодействие базовых сущностей, а не только их список.

Как мы можем устроить взаимодействие, если не будем вызвать события внутри `.watch(fn)`?

События могут образовывать цепочки через **.map()**, что позволяет трансформировать данные, не вызывая события вручную:

```ts
const numberReceived: EventCallable<number> = createEvent<number>();

const oddNumberReceived: Event<boolean> = numberReceived.map(
  (num) => num % 2 === 0
);
// Типы констант здесь указаны лишь для наглядности, обычно это не требуется
```

Здесь **oddNumberReceived** будет автоматически вызвано при каждом срабатывании события **numberReceived**.

Метод **.map()** возвращает новое событие сразу же, сохраняя оригинальное событие неизменным. При этом **numberReceived** является `EventCallable<number>`, а **oddNumberReceived** - `Event<boolean>`, даже если типы не указывать явно. Более того, в большинстве случаев типы не нужно указывать вовсе, effector сам постарается их определить.

Событие `numberReceived` можно вызвать вручную, а `oddNumberReceived` - только подписаться и слушать.

Сторы, как и события, могут создавать производные сторы с помощью **.map()**:

```ts
const $number = createStore(5);
const $isOddNumber = $number.map((number) => number % 2 === 0);
```

Здесь **$isOddNumber** будет автоматически обновляться при каждом изменении **$number**. Если результат функции в **.map(fn)** совпадает с предыдущим значением, обновления не происходит, что позволяет оптимизировать производительность.

Однако, при создании объектов в таких цепочках, обновления будут происходить всегда.

Кроме `.map()` события имеют ряд других методов, например `.filter({fn})`, позволяющий определить в каких случаях второе событие нужно вызывать, а когда нет. Стор не может существовать без значения внутри, значит `.filter()` для сторов имеет не так много смысла, как для событий.

## Юниты

Кроме метода `.map()` и `.watch()`, сторы и события имеют и другие общие детали. Большинство операторов, которые принимают на вход событие, могут принимать также стор и эффект.

Например, в `.on()` можно передать другой стор:

```ts
const counterClicked = createEvent();
const $counter = createStore(0);
const $sum = createStore(0);

$counter.on(counterClicked, (counter) => counter + 1);
// $sum подписан на обновления $counter
$sum.on($counter, (sum, counter) => sum + counter);

$counter.watch((counter) => console.info('counter updated', counter));
//=> counter updated 0
$sum.watch((sum) => console.info('sum updated', sum));
//=> sum updated 0

counterClicked();
//=> counter updated 1
//=> sum updated 1

counterClicked();
//=> counter updated 2
//=> sum updated 3

counterClicked();
//=> counter updated 3
//=> sum updated 6
```

Вместо стора можно указывать также и эффект, и даже несколько сущностей разного типа. При срабатывании любого из них, будет вызвана фунция-редюссер, а затем обновлено значение стора:

```ts
$sum.on(
  [effectFx, effectFx.doneData, $counter, incrementClicked],
  (sum, amount) => sum + amount
);
```

Если внутри оператора или метода не важно, какой источник данных мы хотим слушать, используется `Unit<T>` и называется он тоже юнит.

Если же хочется записывать значение в юнит, тогда `UnitTargetable<T>`. Это разделение позволяет отличать юниты предназначенные только для чтения от юнитов, в которые можно значения передавать/записывать.

Для событий и сторов есть свои пары интерфейсов:

- `Event<T>` + `EventCallable<T>`;
- `Store<T>` + `StoreWritable<T>`;
- кроме эффектов `Effect<Params, Done, Fail>`.

## Взаимодействие сущностей

Мы рассмотрели некоторые методы сущностей effector которые позволяют запускать цепочки вычислений, например через `.on(clock, reducer)` можно модифицировать значение стора и оповестить подписчиков, через `.map(fn)` трансформировать данные.

Но как быть, если нужно взять данные из одного стора и обновить другой стор при срабатывании какого-то стороннего события:

```ts
export const cityClicked = createEvent<{ cityName: string }>();

const $currentCityName = createStore<string | null>(null>);
export const $currentCity = createStore<City | null>(null);
export const $cities = createStore<City[]>([]);

/**
$currentCityName.on(cityClicked, (_prevCityName, { cityName }) => ? );
*/
```

В этом примере, при изменении текущего выбранного города, нужно переложить объект City из `$cities` в `$currentCity`, но именно при срабатывании события `cityClicked`.

### sample

Это оператор, позволяющих запускать юниты, не связанные друг с другом цепочкой. Пример выше, можно записать вот так:

```ts
sample({
  clock: cityClicked,
  source: $cities,
  fn: (cities, { cityName }) => cities.find((c) => c.name === cityName) ?? null,
  target: $currentCity,
});
```

Читать так:

- clock - когда будет запущен `cityClicked`,
- source - в этот момент прочитать `$cities`,
- fn - вызвать эту функцию со значениями из `$cities` и `cityClicked`,
- target - положить результат вызова fn в `$currentCity`.

Оператор очень мощный и имеет много разных вариантов записи, постепенно мы познакомимся со всеми. Например, sample может работать почти с любой комбинацией аргументов:

```ts
sample({
  clock: cityClicked,
  target: $currentCityName,
});

sample({
  clock: cityClicked,
  filter: ({ cityName }) => cityName.length > 2,
  target: $currentCityName,
});
```

Обычно, файл модели отделен от компонента и состоит из определений юнитов, а также несколько sample в порядке предполагаемого срабатывания.

Обращать внимание стоит только на аргументы sample, именно из них составляется сценарий. Пока приложение запущено, в фоне все время будут срабатывать какие-нибудь события, задача разработчика подписаться на все важные события и обрабатывать их в процессе жизни страницы.

### combine

Оператор позволяет создать новый стор, на основе состояния нескольких существующих. При обновлении каждого стора, от которого зависит combined-стора, значение внутри combined будет рассчитано заново. Пример выше можно переписать и на combine:

```ts
export const $currentCity = combine(
  $currentCityName,
  $cities,
  (cityName, cities) => cities.find((c) => c.name === cityName) ?? null
);
```

Теперь, `$currentCity` будет обновляться каждый раз, как `$currentCityName` и/или `$cities` получат обновления своих внутренних значений. При этом, устраняются гонки данных, если от одного события обновлены сразу два стора, combined при этом получит только одно обновление.

В общем виде, combine выглядит так:

```ts
$combined = combine($a, $b, $c, ..., (a, b, c, ...) => result);
```

Оператор `combine()` подпишется на обновления сторов, при инициализации вызовет функцию, переданную последним аргументом, а дальше будет отслеживать изменения сторов и перевычислять значение `$combined` на основе их состояний.

Заметили, что в некоторых примерах, имеется `?? null`?

Это всё потому, что `.find(fn)` возвращает `T | undefined`. Сейчас `undefined` используется в проектах старше 2-3 лет для пропуска обновлений стора, в некоторых случаях. В свежих версиях effector, undefined в сторе подсвечен предупреждением, которое стоит исправить, ради корректной работы логики.

**undefined это специальное значение**, которое НЕ сохраняется в сторе при обычных условиях. Поэтому, стоит его заменять на `null` везде, где оно может появиться, даже потенциально. Это относится и к редюссерам, в том числе в функциях `fn` и `combine`.

### merge

Оператор объединяет реакцию на несколько юнитов в одно событие, что упрощает обработку множества различных триггеров единым способом. В общем виде выглядит вот так.

```ts
const mergedEvent = merge([firstEvent, anotherEvent, $someStore, someFx]);
```

Здесь стоит сразу описать направление срабатывания юнитов: merge позволяет **отреагировать** на срабатывание любого юнита из аргументов, то есть только слушать. Главное, чтобы типы юнитов из списка совпадали, иначе обработка может быть сложной.

Если сработает ОДИН ИЗ firstEvent, anotherEvent, или появится новое значение в $someStore, или будет запущен someFx, то с этим же значением в аргументе будет вызвано событие `mergedEvent`.

Причем, стоит обратить внимание, что юниты могут быть расположены где угодно в проекте, а также вызваны могут быть абсолютно любым кодом. Если Вы можете их импортировать, значит их может вызывать кто-то еще и на эти вызовы кем-то еще ваш код тоже будет реагировать.

## Интеграция с React

Для работы с Effector в React можно использовать хук **useUnit**, который упрощает доступ к состояниям сторов и вызову событий. По умолчанию `useUnit` может принимать один или несколько юнитов.

```ts
const [user, handleButtonClick] = useUnit([$user, buttonClicked]);
```

Рекомендуется всегда заворачивать события в `useUnit` при использовании событий effector в React-компонентах или хуках.

Кроме того, когда событий и сторов на один компонент становится очень много, их можно:

1. переписать в виде нескольких вызовов `useUnit`,
2. декомпозировать на несколько небольших компонентов,
3. оформить в виде объекта.

```ts
const Form = () => {
  const [handleSubmit] = useUnit([formSubmitted]);
  return (
    <form onSubmit={handleSubmit}>
      <UserName />
    </form>
  );
};

const UserName = () => {
  const [name, formPending, disabled] = useUnit([$userName, $formPending, $formDisabled]);
  const [handleNameChange] = useUnit([userNameChanged]);

  return (/* ... */);
}
```

Кроме этого хука, `effector-react` пакет имеет в своём арсенале еще пару хуков для упрощения взаимодействия с React, но об этом позже.

## Структура файлов и поток данных

Самым распространенным способом взаимодействия React и effector является разделение на два файла: компонент и модель. Рекомендуется начинать с реализации основной логики страницы, а декомпозицией на более сложные слои заниматься позже,

```txt
./src/pages/
  - page.tsx
  - model.ts
```

Файл с определением компонентов страницы `page.tsx` импортирует события и сторы из `model.ts`, пользуется ими и старается не завязываться на детали реализации модели.

Поток данных выглядит как-то так:

```txt
page.tsx -- calls events -> model.ts
model.ts -- changes stores -> page.tsx
```

Модель определяет **начальные значения** сторов, на которых страница рендерится первый раз. Когда пользователь взаимодействует со страницей, компонент **вызывает события** из модели. Модель **реагирует на вызовы**, а именно срабатывают `.on()`, `sample()`, `merge()` и прочие, итогом становится **изменение состояние** в сторах. Компонент **подписан на сторы** через `useUnit` и автоматически применяет изменения к DOM страницы.

В этом жизненном цикле нет потребности во взаимодействии сайд-эффектов и компонентов. Упрощенный процесс приносит несколько значимых результатов по сравнению с логикой на хуках:

1. модель и компонент по отдельности выглядят проще, чем объединение логики работы с данными и особенностей хуков React,
2. и модель и компонент теперь могут быть очень легко протестированы,
3. внесение изменений упрощено, так как разделение кода на две части, позволяет сфокусироваться на чем-то одном за раз.

## Разберем пример

Основной способ построения логики на effector - это составление таких связей между юнитами, чтобы при их запуске был достигнут необходимый результат.

Давайте начнем с упрощенного примера, разберем принципы по которым он построен и определим что тут происходит:

```ts
// model.ts
import {
  createEvent,
  createEffect,
  createStore,
  combine,
  sample,
} from 'effector';

export const pageStarted = createEvent();
export const nameChanged = createEvent<string>();
export const formSubmitted = createEvent();

const settingsLoadFx = createEffect(async () => {
  const response = fetch('/settings');
  return await response.json();
});
const settingsUpdateFx = createEffect(async (settings: { name: string }) => {
  const response = await fetch('/settings', {
    method: 'PATCH',
    body: JSON.stringify(settings),
    headers: { 'Content-Type': 'application/json' },
  });
  if (!response.ok) {
    throw new Error('Failed to update settings');
  }
  return await response.json();
});

export const $name = createStore('');

export const $pending = combine(
  settingsLoadFx.pending,
  settingsUpdateFx.pending,
  (settingsLoading, settingsUpdating) => settingsLoading || settingsUpdating
);

sample({
  clock: pageStarted,
  filter: settingsLoadFx.pending.map((pending) => !pending),
  target: [settingsLoadFx, $name.reinit],
});

$name.on(settingsLoadFx.done, (_, { result }) => result.name);

settingsLoadFx.fail.watch(({ params, error }) => {
  console.warn('[settingsLoadFx] failed', params, error);
});

$name.on(nameChanged, (_, name) => name);

sample({
  clock: formSubmitted,
  source: { name: $name },
  target: settingsUpdateFx,
});

$name.on(settingsUpdateFx.done, (_, { result }) => result.name);

settingsUpdateFx.fail.watch(({ params, error }) => {
  console.warn('[settingsUpdateFx] failed', params, error);
});
```

_Это несколько упрощенный пример, в реальных проектах некоторые детали скрыты под фабриками, библиотечными операторами и инфраструктурным кодом, поэтому код может выглядеть сильно короче. Прежде чем использовать всё это многообразие, стоит разобраться как работает effector сам по себе._

Первое на что стоит обратить внимание это список экспортов. Видно, что все экспорты сгруппированы по типам сущностей и находятся в самом начале файла.

Далее, смотрим на список из `sample`, `.on()` и `.watch()`. Определяем в каком порядке они срабатывают:

```ts
sample({
  clock: pageStarted,
  filter: settingsLoadFx.pending.map((pending) => !pending),
  target: [settingsLoadFx, $name.reinit],
});
```

1. в первом sample по порядку, в поле clock расположен `pageStarted`. Вероятно исходный код предполагает, что с этого события начнется исполнение сценария. Важно отметить, что нет никакого контроля для вызывающей стороны, потенциально можно вызывать события в любом порядке.
2. в этом же sample, поле `filter:` определяет будет ли выполняться дальнейшая работа. Если в момент срабатывания события pageStarted, эффект settingsLoadFx еще выполняется, то дальнейший запуск юнитов не произойдет, на этом работа этого sample окончится.
3. если же условие в filter в момент срабатывания clock будет true, тогда sample, запустит всё, что
4. будет запущен эффект `settingsLoadFx`, и как только эффект запустится, сбросит значение стора `$name` до начального, с помощью `.reinit`.

**Другими словами:** при запуске страницы, загрузить настройки и сбросить значение имени, но только если загрузка настроек уже не выполняется.

```ts
const settingsLoadFx = createEffect(async () => {
  const response = fetch('/settings');
  return await response.json();
});
// ...
$name.on(settingsLoadFx.done, (_, { result }) => result.name);

settingsLoadFx.fail.watch(({ params, error }) => {
  console.warn('[settingsLoadFx] failed', params, error);
});
```

5. эффект `settingsLoadFx` завершается с двумя возможными исходами. Если всё ок, и исключений не было выброшено, то будет вызвано событие `.done`, в этом случае стор `$name` реагирует на это и сохраняет имя из загруженных настроек.
6. данные со строки `return await response.json()` попадают напрямую в событие `.done` в поле result в объекте `{ params, result }`, где params это аргумент с которым эффект был вызван.
7. в случае, если функция в эффекте `settingsLoadFx` выбрасывает исключение, будет вызвано событие `.fail`, в котором можно найти объект `{ params, error }`, где params это аргумент, а `error` это объект исключения, которое выбросила функция в эффекте.

**Другими словами:** когда настройки загрузятся, имя нужно сохранить в сторе, а если не удается загрузить настройки, то нужно напечатать об этом в консоль.

Важно отметить, что стор `$name` и `.watch()` отреагируют на завершение эффекта `settingsLoadFx`, даже если он был запущен по любой другой причине, не только `pageStarted`.

```ts
$name.on(nameChanged, (_, name) => name);
```

8. когда сработает событие `nameChanged`, стор `$name` возьмет из него значение и полностью заменит свое на новое. Первый аргумент редюссера получает значение стора в момент срабатывания события, здесь записан как `_`, это такое шаблонное имя, которое обозначает, что аргумент не используется, но присутствует. Просто чтобы не писать шаблонное и не наглядное state, data, value, previousName и прочее.

**Другими словами:** когда имя обновлено, сохранить его в стор.

```ts
sample({
  clock: formSubmitted,
  source: { name: $name },
  target: settingsUpdateFx,
});
```

1. этот sample существует одновременно со всеми остальными, но реагирует на **другое** событие. Когда сработает событие `formSubmitted`, sample возьмет данные из стора `$name` и отправит их в эффект `settingsUpdateFx` и тоже не будет дожидаться его завершения.
2. главное отличие этого sample - это поле `source: { name: $name }`. При срабатывании события `formSubmitted`, sample прочитает `$name`, а перед отправкой в `settingsUpdateFx`, завернет значение из стора `$name` в объект `{ name }`. Эту конструкцию, можно записать разными способами, которые мы позже разберем, но именно этот синтаксис существует ради некоторого упрощения.

**Другими словами:** когда форма будет отправлена, возьмет имя из стора $name и отправит в виде объекта в эффект `settingsUpdateFx`.

В этом примере, можно выделить две фазы сценария:

1. запуск страницы, во время которого загружаются настройки;
2. взаимодействие пользователя, когда срабатывают `nameChanged` и `formSubmitted`

Обработку и отображение ошибок разберем в следующий раз.

Наивная реализация компонентов React может выглядеть как-то так:

```tsx
// page.tsx
import { useEffect } from 'react';
import { useUnit } from 'effector-react';
import {
  pageStarted,
  nameChanged,
  formSubmitted,
  $name,
  $error,
  $pending,
} from './model';

export const SettingsPage = () => {
  const [startPage, submitForm] = useUnit([pageStarted, formSubmitted]);
  const [pending] = useUnit([$pending]);

  useEffect(() => {
    startPage();
  }, []);

  if (pending) {
    return <div>Loading...</div>;
  }

  function handleSubmit(event) {
    event.preventDefault();
    submitForm();
  }

  return (
    <form onSubmit={handleSubmit}>
      <NameField />
    </form>
  );
};

const NameField = () => {
  const [name, pending] = useUnit([$name, $pending]);
  const [changeName] = useUnit([nameChanged]);

  return (
    <input
      disabled={$pending}
      value={name}
      onChange={(e) => changeName(e.target.value)}
    />
  );
};
```

## Заключение

Effector предоставляет мощные инструменты для управления бизнес-логикой, сохраняя при этом гибкость и предсказуемость.

Основные сущности - **Event**, **Store** и **Effect** - помогают управлять базовыми составляющими. Операторы вроде **sample**, **combine** и **merge** позволяет организовывать взаимодействие между сущностями и разными частями приложения.

В дальнейшем мы разберемся как писать более сложную логику, какие подходы и практики существуют и что делать на архитектурном уровне.
