# Использование encodeURIComponent и encodeURI

## Пояснение

В JavaScript методы `encodeURIComponent()` и `encodeURI()` используются для кодирования URI (Uniform Resource Identifier), что обеспечивает безопасную передачу данных в URL.

1. **`encodeURIComponent()`**: Этот метод кодирует все символы, кроме следующих: алфавитно-цифровые символы, "-", "\_", ".", "!", "~", "\*", "'", "(", ")". Применяется для кодирования отдельных компонентов URI, таких как параметры запроса. Например:

   javascriptCopy code

   `var url = "http://example.com/search?q=" + encodeURIComponent(query);`

   В этом примере `encodeURIComponent()` используется для кодирования значения переменной `query` перед добавлением её в URL. Это предотвращает возможные проблемы, такие как наличие символов, которые могут нарушить структуру URL.

2. **`encodeURI()`**: Этот метод используется для кодирования всего URI, кроме специальных символов, обозначающих разделители компонентов URL (например, ":" или "/"). Применяется, когда нужно кодировать всю строку URI, а не отдельные её компоненты. Например:

   javascriptCopy code

   `var uri = "http://example.com/path/to/page.html?foo=" + encodeURIComponent(value); var encodedURI = encodeURI(uri);`

   В этом примере `encodeURI()` используется для кодирования всего URI, чтобы гарантировать, что URL будет правильно интерпретироваться веб-браузером.

Без использования этих методов специальные символы в URL могут привести к неправильной интерпретации URI или вызвать его некорректное отображение в веб-браузере.

## Прямое преобразование

**С `encodeURIComponent()`:**

```js
// Заданное значение параметра запроса
var query = 'это+тест?';

// Создание URL с использованием encodeURIComponent() для кодирования значения параметра запроса
var url = 'http://example.com/search?q=' + encodeURIComponent(query);

console.log(url);
// Вывод: http://example.com/search?q=%D1%8D%D1%82%D0%BE%2B%D1%82%D0%B5%D1%81%D1%82%3F
```

В этом примере метод `encodeURIComponent()` используется для кодирования значения параметра запроса `query`, содержащего специальные символы (плюс и вопросительный знак), чтобы они были корректно интерпретированы в URL.

**С `encodeURI()`:**

```js
// Заданный URI
var uri = 'http://example.com/path/to/page.html?foo=bar#section';

// Кодирование всего URI с использованием encodeURI()
var encodedURI = encodeURI(uri);

console.log(encodedURI);
// Вывод: http://example.com/path/to/page.html?foo=bar#section
```

В этом примере метод `encodeURI()` применяется для кодирования всего URI, который содержит специальные символы, такие как `#` для обозначения фрагмента URI. Этот метод обеспечивает правильное кодирование URI в целом, сохраняя структуру URL.

## Обратное преобразование

```js
// Закодированный URL
var encodedUrl =
  'http://example.com/search?q=%D1%8D%D1%82%D0%BE%2B%D1%82%D0%B5%D1%81%D1%82%3F';

// Создаем объект URL для получения части с параметрами запроса
var url = new URL(encodedUrl);

// Извлекаем часть URL с параметрами запроса
var searchParams = url.searchParams;

// Получаем значение параметра 'q'
var queryValue = searchParams.get('q');

console.log(queryValue);
// Вывод: это+тест?
```
