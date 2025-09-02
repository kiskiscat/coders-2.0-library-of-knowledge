# Обнуляющие стили

```css
/* ПЕРЕМЕННЫЕ */

/* === Шрифты === */

:root {
  /* Системные шрифты, не требуют подключения. */
  --font-family-system:
    system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Helvetica,
    sans-serif;

  /* Время действия эффектов. */
  --short-delay: 0.1s;
  --medium-delay: 0.3s;
  --long-delay: 0.5s;
  --ultra-long: 1s;

  /* Цвета. Названия можно подобрать на https://colornamer.robertcooper.me/. */
  --color-main: /* вставить цвет */;
  --color-disco-ball: #d4d4d4;
  --color-grey-shingle: #939393;
  --color-namara-grey: #7c7c7c;
}

/* ОБЩЕЕ */

/* Перекрашиваем выделение текста. */
::selection {
  background-color: /* вставить цвет */;
  color: /* вставить цвет */;
}

/* Убираем стандартный фокус и добавляем фокус с клавиатуры. */
:focus {
  outline: none;
}

:focus-visible {
  outline: 2px dashed /* вставить цвет */;
  outline-offset: 1px;
}

/* Обнуляем отступы и границы. */
* {
  padding: 0;
  border: 0;
  margin: 0;
}

/* Применяем box-sizing. */
*,
*::before,
*::after {
  box-sizing: border-box;
}

/* Делаем блочными все HTML5-элементы. */
article,
aside,
audio,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section,
img,
summary {
  display: block;
}

/* Базовые стили для <body> и <html>. */
body {
  position: relative;
  min-height: 100vh;
  color: var(--color-main);
  font-family: var(--font-family-system);
  line-height: 1.5;
  scroll-behavior: smooth;
  text-rendering: optimizespeed;
}

html {
  position: relative;
  overflow: hidden scroll;
  height: 100%;
  font-size: 100%;
}

/* Стилизация полосы прокрутки. */
body::-webkit-scrollbar {
  width: 15px;
}

body::-webkit-scrollbar-track {
  background: var(--color-disco-ball);
  box-shadow: 0 0 2px rgb(0 0 0 / 20%) inset;
}

body::-webkit-scrollbar-thumb {
  border: 3px solid var(--color-disco-ball);
  border-radius: 8px;
  background: var(--color-grey-shingle);
}

body::-webkit-scrollbar-thumb:hover {
  background: var(--color-namara-grey);
}

/* Резервируем место под скроллбар. */
html,
body {
  scrollbar-gutter: stable;
}

/* Блокировка прокрутки при открытом модальном окне. */
body.lock {
  overflow: hidden;
}

/* Настраиваем стили кавычек. */
body {
  quotes: '«' '»' '„' '“';
}

.quoteInside::before {
  content: open-quote;
}

.quoteInside::after {
  content: close-quote;
}

/* Стили для атрибута inert. */
[inert],
[inert] * {
  cursor: default;
  opacity: 0.7;
  pointer-events: none;
  user-select: none;
}

/* Убираем рамку у fieldset. */
fieldset {
  border: none;
}

/* Убираем рамку у dialog. */
dialog {
  border: none;
}

/* Убираем маркеры списков. */
ul,
ol,
menu {
  list-style: none;
}

/* Наследуем шрифты для интерактивных элементов. */
input,
button,
textarea,
select {
  font: inherit;
}

/* Убираем системный стиль у поиска. */
input[type='search'] {
  -webkit-appearance: none;
  appearance: none;
}

/* Разрешаем изменение размера textarea. */
textarea {
  resize: vertical;
  overflow: auto;
}

/* Убираем системный стиль у select. */
select {
  -webkit-appearance: none;
  appearance: none;
  background-color: transparent;
}

/* Настройки кнопок по умолчанию. */
button {
  cursor: pointer;
  user-select: none;
}

/* Унификация стилей тега progress. */
progress {
  border: none;
  background-color: #ff8630;
}

progress::-moz-progress-bar {
  border: none;
  background-color: #ff8630;
}

progress::-webkit-progress-bar {
  border: none;
  background-color: #ff8630;
}

progress::-webkit-progress-value {
  background-color: #ff8630;
}

/* ТАБЛИЦЫ */

/* Убираем отступы и объединяем границы. */
table {
  position: relative;
  border-collapse: collapse;
  border-spacing: 0;
}

/* Фиксируем шапку таблицы при прокрутке. */
thead th {
  position: sticky;
  top: 0;
  z-index: 1;
  background-color: #ff8630;
}

/* ФОРМАТИРОВАНИЕ */

/* Волнистое подчёркивание для <u>. */
u {
  text-decoration-style: wavy;
}

/* ДОСТУПНОСТЬ */

/* Обозначаем <mark> для скринридеров. */
mark::before {
  content: ' [начало выделения] ';
}

mark::after {
  content: ' [конец выделения] ';
}

/* Разделитель для <dt>. */
dt::after {
  content: ': ';
}

/* ПОЛЬЗОВАТЕЛЬСКИЕ НАСТРОЙКИ */

/* Цвета для режима высокой контрастности. */
@media (forced-colors: active) {
  mark {
    color: HighlightText;
    background-color: Highlight;
  }
}

/* Отключение анимаций при предпочтении пользователя. */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    scroll-behavior: auto !important;
    transition-duration: 0.01ms !important;
  }
}

/* Включение плавной прокрутки при отсутствии предпочтений. */
@media (prefers-reduced-motion: no-preference) {
  html {
    scroll-behavior: auto;
  }
}
```
