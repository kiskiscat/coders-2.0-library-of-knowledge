# Проверочные стили

```css
/* ФОРМЫ И ИНТЕРРАКТИВНЫЕ ЭЛЕМЕНТЫ */

/*  Тег <form> обязан иметь атрибут "novalidate" */

form:not([novalidate]) {
  border-bottom: 5px solid blue !important;
  color: blue !important;
  background-color: blue !important;
}

/*  Тег <label> обязан иметь атрибут "for" */

label:not([for]) {
  border-bottom: 5px solid blue !important;
  color: blue !important;
  background-color: blue !important;
}

/*  Тег <input>, <textarea> и <select> не должны иметь атрибут "size" */

input[size],
textarea[size],
select[size] {
  border-bottom: 5px solid red !important;
  color: red !important;
  background-color: red !important;
}

/* Для ссылок у которого нет обязательных атрибутов, сбрасываем стиль до дефолтного */

a:not([href]) {
  border-bottom: 5px solid red !important;
  color: red !important;
  text-decoration-skip-ink: auto;
}

/*  Тег <textarea> не должны иметь атрибуты "rows" и "cols" */

textarea[rows],
textarea[cols] {
  border-bottom: 5px solid red !important;
  color: red !important;
  background-color: red !important;
}

/*  Тег <input> c "type='file" обязан иметь атрибут "accept" */

input[type='file']:not([accept]) {
  border-bottom: 5px solid red !important;
  color: red !important;
  background-color: red !important;
}

/*  Тег <input>, <textarea> и <select> обязаны иметь атрибут "name" и "id" */

input:not([name]),
input:not([id]),
textarea:not([name]),
textarea:not([id]),
select:not([name]),
select:not([id]) {
  border-bottom: 5px solid blue !important;
  color: blue !important;
  background-color: blue !important;
}

/* ТАБЛИЦЫ */

/* Таблица обязательно должна иметь семантичные теги <thead>, <tbody>, <caption> */

table:not(:has(caption)),
table:not(:has(tbody)),
table:not(:has(thead)) {
  border-bottom: 5px solid red !important;
  color: red !important;
  background-color: red !important;
}

/* ИЗОБРАЖЕНИЯ */

/*  Тег <img>, у которого нет обязательных атрибутов, сбрасываем до дефолтных стилей */

img:not([src]),
img:not([alt]),
img:not([width]),
img:not([height]) {
  border-bottom: 5px solid red !important;
  color: red !important;
  background-color: red !important;
}

/*  Бывают специфические случаи, когда тегу <img> можно не ставить обязательные атрибуты. Учтём это! */

img.itIsOk {
  border-bottom: revert;
  color: revert;
  background-color: revert;
}

/* БЕЗОПАСНОСТЬ */

a[target='_blank']:not([rel='noopener noreferrer']) {
  border-bottom: 5px solid green !important;
  background-color: green !important;
  color: green !important;
}
```
