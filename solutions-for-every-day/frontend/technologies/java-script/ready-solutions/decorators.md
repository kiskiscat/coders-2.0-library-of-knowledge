# Декораторы

## Переадресация вызова выполняется с помощью apply

```js
let wrapper = function (original, arguments) {
  return original.apply(this, arguments);
};
```

## "Шпион"

Использование:

```js
function work(a, b) {
  alert(a + b); // произвольная функция или метод
}

work = spy(work);

work(1, 2); // 3
work(4, 5); // 9

for (let args of work.calls) {
  alert('call:' + args.join()); // "call:1,2", "call:4,5"
}
```

Реализация:

```js
function spy(func) {
  function wrapper(...args) {
    wrapper.calls.push(args);
    return func.apply(this, args);
  }

  wrapper.calls = [];

  return wrapper;
}
```

## "Задерживающий"

Использование:

```js
function f(x) {
  alert(x);
}

// создаём обёртки
let f1000 = delay(f, 1000);
let f1500 = delay(f, 1500);

f1000('test'); // показывает "test" после 1000 мс
f1500('test'); // показывает "test" после 1500 мс
```

Реализация:

```js
function delay(f, ms) {
  return function () {
    setTimeout(() => f.apply(this, arguments), ms);
  };
}

// Или

function delay(f, ms) {
  return function (...args) {
    let savedThis = this; // сохраняем this в промежуточную переменную

    setTimeout(function () {
      f.apply(savedThis, args); // используем её
    }, ms);
  };
}
```

## "Debounce"

Использование:

```js
let a = debounce(alert, 100);

a(1); // выполняется немедленно
a(2); // проигнорирован

let b = debounce(alert, 1000);

setTimeout(() => b(3), 100); // выполняется
setTimeout(() => b(4), 1100); // проигнорирован (прошло только 900 мс от последнего вызова)
setTimeout(() => b(5), 1500); // выполняется
```

Реализация:

```js
function debounce(f, ms) {
  let isCooldown = false;

  return function (...args) {
    if (isCooldown) return;

    f.apply(this, args);

    isCooldown = true;

    setTimeout(() => (isCooldown = false), ms);
  };
}
```

## "Throttle"

Использование:

```js
function f(a) {
  console.log(a);
}

// f1000 передаёт вызовы f максимум раз в 1000 мс
let f1000 = throttle(f, 1000);

f1000(1); // показывает 1
f1000(2); // (ограничение, 1000 мс ещё нет)
f1000(3); // (ограничение, 1000 мс ещё нет)

// когда 1000 мс истекли ...
// ...выводим 3, промежуточное значение 2 было проигнорировано
```

Реализация:

```js
function throttle(func, ms) {
  let isThrottled = false;
  let savedArgs = null;
  let savedThis = null;

  function wrapper(...args) {
    if (isThrottled) {
      // (2)
      savedArgs = args;
      savedThis = this;

      return;
    }

    func.apply(this, args); // (1)

    isThrottled = true;

    setTimeout(function () {
      isThrottled = false; // (3)

      if (savedArgs) {
        wrapper.apply(savedThis, savedArgs);

        savedArgs = null;
        savedThis = null;
      }
    }, ms);
  }

  return wrapper;
}
```
