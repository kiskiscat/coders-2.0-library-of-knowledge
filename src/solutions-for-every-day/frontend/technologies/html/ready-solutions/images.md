# Изображения

## Картинка с SVG

Если изображение имеет тип файла SVG, также включите [`role="img"`](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/Img_role) , что необходимо из-за [ошибок](https://bugs.webkit.org/show_bug.cgi?id=240656) [VoiceOver](https://bugs.webkit.org/show_bug.cgi?id=216364):

```html
<img src="switch.svg" alt="light switch" role="img" />
```

## Отображение для разных медиа выражений

```html
<picture>
  <source
    srcset="picture-wide@1x.png 1x, picture-wide@1x.png 2x"
    media="(min-width: 600px)"
    type="image/webp"
  />
  <source
    srcset="picture-wide@1x.png 1x, picture-wide@1x.png 2x"
    media="(min-width: 600px)"
  />
  <source srcset="picture@1x.webp 1x, picture@1x.webp 2x" type="image/webp" />
  <img
    src="picture@1x.png"
    srcset="picture@2x.png 2x"
    alt="Девушка пьёт кофе"
  />
</picture>
```
