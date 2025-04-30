# Объект Headers

В JavaScript, объект Headers, используемый в API fetch, предоставляет методы для работы с заголовками HTTP. Хотя методы этого объекта реализованы на уровне нативного кода и не позволяют напрямую просматривать их внутреннее состояние, вы можете использовать доступные методы для получения значений заголовков.

Вот несколько способов, как можно работать с объектом Headers и получать значения заголовков:

1. Использование метода `get`. Метод `get` позволяет получить значение конкретного заголовка по его имени.

```js
const headers = new Headers();
headers.append('Content-Type', 'application/json');
headers.append('Authorization', 'Bearer token');

console.log(headers.get('Content-Type')); // 'application/json'
console.log(headers.get('Authorization')); // 'Bearer token'
```

2. Использование метода `has`. Метод `has` позволяет проверить, существует ли заголовок.

```js
console.log(headers.has('Content-Type')); // true
console.log(headers.has('Non-Existent-Header')); // false
```

3. Использование метода `forEach`. Метод `forEach` позволяет перебрать все заголовки и их значения.

```js
headers.forEach((value, key) => {
    console.log(${key}: ${value});
});
```

4. Преобразование в объект. Если вам нужно получить все заголовки в виде объекта, вы можете создать функцию, которая будет использовать метод `Object.fromEntries`.

```js
const headersObject = Object.fromEntries(headers.entries());
console.log(headersObject);
```
