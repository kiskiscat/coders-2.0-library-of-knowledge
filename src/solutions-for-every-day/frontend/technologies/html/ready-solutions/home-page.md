# Стартовая страница

## Источники

- https://github.com/Konfuze/HEAD
- https://github.com/thedaviddias/Front-End-Checklist/blob/master/README.md

## Стандартный набор

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <!-- Заголовок документа -->
    <title>Page Title less than 55 characters</title>

    <!-- Эти 3 meta-тега *должны* быть расположены первыми в `<head>` -->
    <meta charset="utf-8" />
    <!-- Сообщает Internet Explorer использовать самый новый движок для рендера -->
    <meta http-equiv="x-ua-compatible" content="ie=edge" />
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1, viewport-fit=cover"
    />

    <!-- Позволяет указывать, откуда могут грузиться ресурсы -->
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'" />
    <!-- Короткое описание страницы (ограничение - 150 символов) -->
    <meta name="description" content="Описание страницы" />

    <!-- Фавиконка стандартный формат -->
    <link
      rel="icon"
      type="image/x-icon"
      href="https://example.com/favicon.ico"
    />
    <!-- Фавиконка рекомендуемый формат -->
    <link rel="icon" type="image/png" href="https://example.com/favicon.png" />
    <!-- Фавиконка нерекомендуемый формат (для старых браузеров пойдёт)-->
    <link
      rel="icon"
      type="image/svg+xml"
      href="https://example.com/favicon.svg"
    />

    <!-- Фавиконка для устройств с Apple Touch (от 200x200px) -->
    <link rel="apple-touch-icon" href="/custom-icon.png" />

    <!-- Чтобы запустить веб-приложение в полноэкранном режиме -->
    <meta name="apple-mobile-web-app-capable" content="yes" />
    <!-- Стиль строки состояния. Не имеет эффекта, если у вас нет предыдущего метатега-->
    <meta name="apple-mobile-web-app-status-bar-style" content="black" />

    <!-- Microsoft Tiles -->
    <meta name="msapplication-config" content="/browserconfig.xml" />
    <link rel="manifest" href="/mlwmanifest.json" />

    <!-- Помогает предотвратить проблемы с дублированием контента -->
    <!-- Позволяет избежать дублирование индексации контента -->
    <link
      rel="canonical"
      href="https://example.com/2010/06/9-things-to-do-before-entering-social-media.html"
    />

    <!-- Critical CSS -->

    <style></style>

    <!-- Остальной CSS -->
    <link rel="stylesheet" src="css/styles.css" />

    <link rel="alternate" href="https://es.example.com/" hreflang="es" />
  </head>
  <body>
    <!-- <script defer src="scripts/lightswitch.js"></script>-->
  </body>
</html>
```

## Рекомендуется

```html
<!-- Контролирует поведение поисковых систем при индексации страницы -->
<meta
  name="robots"
  content="index,follow,noodp"
/><!-- Все поисковые системы -->
<meta
  name="googlebot"
  content="index,follow"
/><!-- Указание отдельно для Google -->

<!-- Позволяет указать Google не предлагать перевести эту страницу -->
<meta name="google" content="notranslate" />

<!-- Позволяет указать Google не предлагать перевести эту страницу -->
<meta name="google" content="notranslate" />

<!-- Подтверждает авторство страницы в Google Search Console -->
<meta name="google-site-verification" content="verification_token" />

<!-- Используется для указания ПО, которое сгенерировало эту страницу -->
<meta name="generator" content="program" />

<!-- Короткое описание тематики вашего сайта -->
<meta name="subject" content="тематика вашего сайта" />

<!-- 
Очень короткое предложение (до 10 слов), описывающее назначение сайта. 
Используется преимущественно для академических работ 
-->
<meta name="abstract" content="" />

<!-- Полный URL страницы -->
<meta name="url" content="https://example.com/" />

<meta name="directory" content="submission" />

<!-- Указывает что контент сайта соответствует возрастному рейтингу `General` -->
<meta name="rating" content="General" />

<!-- Позволяет контролировать процесс передачи реферальных данных  -->
<meta name="referrer" content="never" />

<!-- Отключает автоматическое определение и форматирование телефонных номеров -->
<meta name="format-detection" content="telephone=no" />

<!-- Полностью отключает предзагрузку DNS-адресов -->
<meta http-equiv="x-dns-prefetch-control" content="off" />

<!-- Сохраняет cookie в веб-браузере для клиентской аутентификации -->
<meta http-equiv="set-cookie" content="name=value; expires=date; path=url" />

<!-- Определяет, то, что страница должна грузиться в определённом фрейме -->
<meta http-equiv="Window-Target" content="_value" />

<!-- Geo-теги -->
<meta name="ICBM" content="latitude, longitude" />
<meta name="geo.position" content="latitude;longitude" />
<meta
  name="geo.region"
  content="country[-state]"
/><!-- Country code (ISO 3166-1): mandatory, state code (ISO 3166-2): optional; eg. content="US" / content="US-NY" -->
<meta
  name="geo.placename"
  content="city/town"
/><!-- eg. content="New York City" -->

<!-- Ссылка на JSON-файл, который описывает установку веб-приложения -->
<link rel="manifest" href="manifest.json" />

<!-- Ссылка на автора документа -->
<link rel="author" href="humans.txt" />

<!-- Cсылка на страницу с авторскими правами, согласно которым предоставляется текущий документ -->
<link rel="copyright" href="copyright.html" />

<!-- Информация об авторе -->
<link rel="me" href="https://google.com/profiles/thenextweb" type="text/html" />
<link rel="me" href="mailto:name@example.com" />
<link rel="me" href="sms:+15035550125" />

<!-- Загружает внешний HTML-файл в текущий файл -->
<link rel="import" href="component.html" />

<!-- Prefetching, preloading, prebrowsing -->
<link rel="dns-prefetch" href="//example.com/" />
<link rel="preconnect" href="https://www.example.com/" />
<link rel="prefetch" href="https://www.example.com/" />
<link rel="prerender" href="https://example.com/" />
<link rel="subresource" href="styles.css" />
<link rel="preload" href="image.png" />
```

## Не рекомендуется

```html
<!-- Тег использовался для указания языка страницы, но не поддерживается в полной мере. Рекомендуется использование <html lang=""> -->
<meta name="language" content="en" />

<!-- Google игнорирует это содержимое, а Bing расценивает это как признак спама -->
<meta
  name="keywords"
  content="ключевые,слова,разделённые,запятыми,без,пробелов"
/>
<!-- Нет подтверждения, что хотя бы одна поисковая система использует этот тег -->

<meta name="revised" content="Sunday, July 18th, 2010, 5:15 pm" />

<!-- Даёт простой способ спам-ботам собирать email-адреса -->
<meta name="reply-to" content="email@example.com" />

<!-- Лучше использовать тег <link rel="author"> или файл humans.txt -->
<meta name="author" content="name, email@example.com" />
<meta name="designer" content="" />
<meta name="owner" content="" />

<!-- Указывает поисковым ботам, когда нужно будет переиндексировать эту страницу. Не поддерживается, т. к. поисковые системы используют случайные интервалы для переиндексации страницы -->
<meta name="revisit-after" content="7 days" />

<!-- Указывает через сколько времени и на какой адрес браузер должен осуществить редирект -->
<!-- W3C не рекомендует использование этого тега. Google рекомендует использовать 301 редирект на сервере -->
<meta http-equiv="refresh" content="300; url=https://example.com/" />

<!-- Тематика вашего сайта -->
<meta name="topic" content="" />

<!-- Краткое описание компании или предназначения сайта -->
<meta name="summary" content="" />

<!-- Устаревший тег, т. к. он указывает на то же, что и meta-тег keywords -->
<meta name="classification" content="business" />

<!-- Уточняет, что ваш сайт будет отображаться для всех стран и всех языков -->
<meta name="coverage" content="Worldwide" />

<!-- Контролирует то, что пользователь может получить доступ в Интернете -->
<meta http-equiv="Pics-label" content="value" />

<!-- Управление кешированием -->
<!-- Будет лучше, если Вы настроите управление кешем на стороне сервера  -->
<meta http-equiv="Expires" content="0" />
<meta http-equiv="Pragma" content="no-cache" />
<meta http-equiv="Cache-Control" content="no-cache" />

<link rel="shortcut icon" href="path/to/favicon.ico" />
```
