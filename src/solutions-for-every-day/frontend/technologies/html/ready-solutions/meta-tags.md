# Мета-теги

## Использование разных фавиконок для разных экранов и устройств:

```html
<!-- На iPad третьего поколения с Retina-дисплеем: -->
<link
  rel="apple-touch-icon-precomposed"
  sizes="144x144"
  href="favicon144.png"
/>
<!-- iPhone с Retina-дисплеем: -->
<link
  rel="apple-touch-icon-precomposed"
  sizes="114x114"
  href="favicon114.png"
/>
<!-- iPad первого и второго поколений: -->
<link rel="apple-touch-icon-precomposed" sizes="72x72" href="favicon72.png" />
<!-- iPhone, iPod Touch и Android 2.1+ устройства без Retina дисплея: -->
<link rel="apple-touch-icon-precomposed" href="favicon57.png" />
<!-- стандартная фавиконка -->
<link rel="icon" href="favicon32.png" />
```

## Использование разных стилей, в зависимости от устройства и размера экрана:

```html
<link href="print.css" rel="stylesheet" media="print" />
<link href="mobile.css" rel="stylesheet" media="all" />
<link
  href="desktop.css"
  rel="stylesheet"
  media="screen and (min-width: 600px)"
/>
<link
  href="highres.css"
  rel="stylesheet"
  media="screen and (min-resolution: 300dpi)"
/>
```

## Стандартный набор метатегов для OG-разметки

```html
<meta property="og:title" content="Лучший сайт в интернете" />
<meta
  property="og:description"
  content="Посетите лучший сайт в интернете и познайте тщетность бытия"
/>
<meta property="og:image" content="http://best-site/thumbnail.jpg" />
<meta property="og:url" content="http://best-site/index.htm" />
```
