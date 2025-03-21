# Обнуляющие стили

```css
/* ПЕРЕМЕННЫЕ */

/* === Шрифты === */

:root {
	/* Системные шрифты, которые используются везде и не требуют подключения */

  --font-family-system: system-ui, -apple-system, 'Segoe UI', Roboto,
    'Helvetica Neue', Helvetica, sans-serif;

	/* Время действия эффектов */

  --short-delay: 0.1s;
  --medium-delay: 0.3s;
  --long-delay: 0.5s;
  --ultra-long: 1s;

	/* Названия цветов берём отсюда: https://colornamer.robertcooper.me/ */

  --color-main:  /* вставить цвет */ ;
  --color-disco-ball: #d4d4d4;
  --color-grey-shingle: #939393;
  --color-namara-grey: #7c7c7c;
}

/* ОБЩЕЕ */

/* Перекрашиваем выделение текста в соответствии с дизайном */

::selection {
  background-color: /* вставить цвет */ ;
  color: /* вставить цвет */ ;
}

/* Убираем стандартный фокус у элементов и добавляем фокус с клавиатуры */

:focus {
  outline: none;
}

:focus-visible {
  outline: 2px dashed /* вставить цвет */;
  outline-offset: 1px;
}

/* Скрываем все пустые элементы */

:empty {
  display: none
}

/* Указываем box sizing, обнуляем отступы */

* {
  padding: 0;
  border: 0;
  margin: 0;
}

*,
*::before,
*::after {
  box-sizing: border-box;
}

/* Определяем блочную модель для старых браузеров */

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
img {
  display: block;
}

/* Выставляем основные настройки для <body> и <html> */

body {
  position: relative;
  min-height: 100vh;
  color: var(--color-main);
  font-family: var(/* вставить значение */, --font-family-system);
  line-height: 1.5;

  /* Оптимизация отображения текста */
  text-rendering: optimizeSpeed;
}

html {
  position: relative;
  height: 100%;
  font-size: 100%;
  overflow-x: hidden;
  overflow-y: scroll;
  scroll-behavior: smooth;
}

/* Стилизуем scrollbar страницы */

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

/* Добавляем класс для блокировки прокручивания контента. Резервируем место под полосу прокрутки, чтобы контент не прыгал при исчезновении скроллбара. */

html,
body {
  scrollbar-gutter: stable;
}

body.lock {
  overflow: hidden;
}

/* Добавляем для элементов вложенных цитат выбранный стиль кавычек */

body {
  quotes: '«' '»' '„' '“';
}

.quoteInside::before {
  content: open-quote;
}

.quoteInside::after {
  content: close-quote;
}

/* Все теги, имеющие атрибут "inert" делаем отличными от стилей атрибута "disabled" для правильного восприятия */

[inert],
[inert] * {
  opacity: 0.7;
  pointer-events: none;
  cursor: default;
  user-select: none;
}

/* Удаляем стандартные браузерную обводку у диалоговых окон */

dialog {
  border: none;
}

/* Удаляем стандартную стилизацию для тегов списка */

ul,
ol,
menu {
  list-style: none;
}

/* Наследуем шрифты для интеррактивных элементов */

input,
button,
textarea,
select {
  font: inherit;
}

/* Кнопки по умолчанию должны иметь указатель и не выделять текст внутри */

button {
  cursor: pointer;
  user-select: none;
}

/* Делаем стилизацию тега <progress> одинаковой для всех */

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

/* Избавляемся от стандартных разрывов и границ в <table> */

table {
  position: relative;
  border-collapse: collapse;
  border-spacing: 0;
}

/* Для удобства просмотра шапки при скроле таблицы */

thead th {
  position: sticky;
  top: 0;
  z-index: 1;
  background-color: #ff8630;
}

/* ФОРМАТИРОВАНИЕ */

/* Добавляем для тега <u> волнистое подчёркивание, чтобы ошибки бросались в глаза, и чтобы не спутать их со ссылками. */

u {
  text-decoration-style: wavy;
}

/* ДОСТУПНОСТЬ */

/* Делаем тег <mark> более доступным для скринридеров */

mark::before {
  content: ' [начало выделения] ';
}

mark::after {
  content: ' [конец выделения] ';
}

/* Добавляем удобный разделитель для тега <dt> */

dt::after {
  content: ': ';
}

/* ПОЛЬЗОВАТЕЛЬСКИЕ НАСТРОЙКИ */

/* Если поддерживается режим высокой контрастности в Windows, то заменяем цвет на системный у тего <mark> для контрастности */

@media (forced-colors: active) {
  mark {
    color: HighlightText;
    background-color: Highlight;
  }
}

/* Удаляем все анимации и переходы для людей, которые предпочитай их не использовать */

@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    scroll-behavior: auto !important;
    transition-duration: 0.01ms !important;
  }
}

/* Управляем поведением прокрутки в зависимости от предпочтений пользователя */

@media (prefers-reduced-motion: no-preference) { 
	html {   
		scroll-behavior: auto; 
	}
}
```
