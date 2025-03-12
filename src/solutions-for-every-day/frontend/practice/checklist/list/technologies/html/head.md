# Head

## Best practices

> - 🤠 [Стартовая страница](../../../../../технологии/HTML/готовые%20решения/Стартовая%20страница.md)

- [ ] 🔴 **Тип документа.** Тип документа - HTML5 должен находится вверху всех ваших HTML-страниц.
- [ ] 🔴 **Кодировка.** Кодировка (UTF-8) объявлена правильно.
- [ ] 🔴 **Область просмотра.** Область просмотра объявлена правильно.
- [ ] 🔴 **Заголовок.** Заголовок используется на всех страницах (SEO: Google вычисляет ширину символов, используемых в заголовке, и обрезает их между 472 и 482 пикселями. Средний лимит составляет около 55 символов).

> - 🛠 [SERP Snippet Generator](https://www.sistrix.com/serp-snippet-generator/)

- [ ] 🔴 **Описание.** Предоставляется метаописание, оно уникально и не может содержать более 150 символов.
- [ ] 🔴 **Порядок CSS.** Все файлы CSS загружаются раньше всех файлов JavaScript (за исключением случая, когда файлы JS иногда загружаются асинхронно поверх вашей страницы).
- [ ] 🔴 **Атрибут языка.** Атрибут `lang` вашего веб-сайта указывается и связан с языком текущей страницы.

```html
<html lang="en"></html>
```

- [ ] 🟡 **Favicons.** Каждый значок создан и отображается правильно. Если у вас есть только `favicon.ico`, поместите его в корень вашего сайта. Обычно вам не нужно использовать какую-либо разметку. Тем не менее, по-прежнему рекомендуется использовать ссылку на него, используя приведенный ниже пример. Сегодня формат `.png` рекомендуется вместо формата `.ico` (размеры: 32 x 32 пикселя).

> - 🛠 [Favicon Generator](https://www.favicon-generator.org/)
> - 🛠 [RealFaviconGenerator](https://realfavicongenerator.net/)
> - 📖 [Favicon Cheat Sheet](https://github.com/audreyr/favicon-cheat-sheet)
> - 📖 [Favicons, Touch Icons, Tile Icons, etc. Which Do You Need? - CSS Tricks](https://css-tricks.com/favicon-quiz/)

- [ ] 🟡 **`rel="canonical"`.** Используйте `rel="canonical"` , чтобы избежать дублирования контента.

> - 📖 [Use canonical URLs - Search Console Help - Google Support](https://support.google.com/webmasters/answer/139066?hl=en)
> - 📖 [5 common mistakes with rel=canonical - Google Webmaster Blog](https://webmasters.googleblog.com/2013/04/5-common-mistakes-with-relcanonical.html)

- [ ] 🟡 **Атрибут направления.** Указывает стандартное направление написания текста в ваше проекте

```html
<html dir="ltr"></html>
```

> - 📖 [dir - HTML - MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/dir)

- [ ] 🟡 **Критический CSS.** Весь CSS, используемый для визуализации видимой части страницы, доступен для использования. Он встраивается перед основным вызовом остального CSS и между `<style></style>` в одну минимизированную строку.

> - 🛠 [Critical by Addy Osmani on GitHub](https://github.com/addyosmani/critical)

- [ ] 🟢 **Мета-теги Apple Web App.** Присутствуют мета-теги Apple.

> - 📖 [Configuring Web Applications](https://developer.apple.com/library/content/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html)
> - 📖 [Supported Meta Tags](https://developer.apple.com/library/content/documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html)

- [ ] 🟢 **Windows Tiles.** Windows Tiles присутствуют и связаны между собой.

> - 📖 [Browser configuration schema reference](<https://msdn.microsoft.com/en-us/library/dn320426(v=vs.85).aspx>)

- [ ] 🟢 **Альтернативный язык.** Тег языка вашего веб-сайта указан и связан с языком текущей страницы.

```html
<link rel="alternate" href="https://es.example.com/" hreflang="es" />
```

- [ ] 🟢 **x-default.** Языковой тег вашего веб-сайта для международных целевых страниц.

```html
<link rel="alternate" href="https://example.com/" hreflang="x-default" />
```

> - 📖 [x-default - Google](https://webmasters.googleblog.com/2013/04/x-default-hreflang-for-international-pages.html)

- [ ] 🟢 **Условные комментарии.** Условные комментарии присутствуют для IE, если это необходимо.

> - 📖 [About conditional comments (Internet Explorer) - MSDN - Microsoft](<https://msdn.microsoft.com/en-us/library/ms537512(v=vs.85).aspx>)

- [ ] 🟢 **RSS-канал.** Если ваш проект представляет собой блог или содержит статьи, предоставьте RSS-ссылку.

### Дополнительно

#### Social meta

Визуализируйте и автоматически генерируйте метатеги социальных сетей с помощью [этого сервиса](https://metatags.io/).

**Facebook OG** и **Twitter Cards** настоятельно рекомендуются для любого веб-сайта. Другие теги социальных сетей можно рассмотреть, если Вы ориентируетесь на определенное присутствие в них и хотите обеспечить их отображение.\*

- [ ] 🟢 **Open Graph разметка для Facebook.** OG разметка проверена, и она не содержит ложной информации. Изображения должны быть не менее 600 x 315 пикселей, хотя рекомендуемый размер - 1200 x 630 пикселей.

```html
<meta property="og:type" content="website" />
<meta property="og:url" content="https://example.com/page.html" />
<meta property="og:title" content="Content Title" />
<meta property="og:image" content="https://example.com/image.jpg" />
<meta property="og:description" content="Description Here" />
<meta property="og:site_name" content="Site Name" />
<meta property="og:locale" content="en_US" />
<!-- Next tags are optional but recommended -->
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
```

> - 📖 [A Guide to Sharing for Webmasters](https://developers.facebook.com/docs/sharing/webmasters/)
> - 📖 [Best Practices - Sharing](https://developers.facebook.com/docs/sharing/best-practices/)
> - 🛠 Test your page with the [Facebook OG testing](https://developers.facebook.com/tools/debug/)

- [ ] 🟢 **Open Graph разметка для Twitter.**

```html
<meta name="twitter:card" content="summary" />
<meta name="twitter:site" content="@site_account" />
<meta name="twitter:creator" content="@individual_account" />
<meta name="twitter:url" content="https://example.com/page.html" />
<meta name="twitter:title" content="Content Title" />
<meta
  name="twitter:description"
  content="Content description less than 200 characters"
/>
<meta name="twitter:image" content="https://example.com/image.jpg" />
```

> - 📖 [Getting started with cards - Twitter Developers](https://developer.twitter.com/en/docs/tweets/optimize-with-cards/guides/getting-started)
> - 🛠 Test your page with the [Twitter card validator](https://cards-dev.twitter.com/validator)

### Производительность

- [ ] 🟢 **Разрешение DNS:** DNS сторонних сервисов, которые могут потребоваться, определяются заранее во время простоя с помощью `dns-prefetch `.

```html
<link rel="dns-prefetch" href="https://example.com" />
```

- [ ] 🟢 **Предварительное подключение:** Поиск DNS, подтверждение TCP и согласование TLS со службами, которые потребуются в ближайшее время, выполняются заранее во время простоя с помощью `предварительное соединение`.

```html
<link rel="preconnect" href="https://example.com" />
```

- [ ] 🟢 **Предварительная выборка:** Ресурсы, которые понадобятся в ближайшее время (например, отложенно загружаемые изображения), запрашиваются заранее во время простоя с использованием `prefetch` .

```html
<link rel="prefetch" href="image.png" />
```

- [ ] 🟢 **Предварительная загрузка:** Ресурсы, необходимые на текущей странице (например, скрипты, помещенные в конец `<body>`), заранее используя `предварительная загрузка`.

```html
<link rel="preload" href="app.js" />
```

> - 📖 [Difference between prefetch and preload](https://medium.com/reloading/preload-prefetch-and-priorities-in-chrome-776165961bbf)
