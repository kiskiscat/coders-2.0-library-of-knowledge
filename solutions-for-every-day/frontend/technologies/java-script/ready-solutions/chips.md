# Фишки

## Корректное округление

```js
function exactRound(number) {
  Math.round(number * 10) / 10;
}
```

## Корректная рандомизация

```js
function randomFloat(min, max) {
  return min + Math.random() * (max - min);
}

function randomInteger(min, max) {
  let rand = min - 0.5 + Math.random() * (max - min + 1);

  return Math.round(rand);
}
```
