Предположим что уже имеется подготовленный набор отдельных SVG-иконок. Перейдём к формированию SVG-спрайта.

1. Для начала создадим отдельный файл с расширением `.svg`. Внутрь созданного файла добавим обёртку для SVG-файла состоящую из тега [`<svg>`](https://doka.guide/html/svg/) и атрибута `xmlns`, указывающего используемый диалект [XML](https://doka.guide/tools/xml/#konflikty-imenovaniya-tegov):

```xml
<svg xmlns="http://www.w3.org/2000/svg"> ... </svg>
```

2. Далее каждая SVG-иконка помещается внутрь тега [`<symbol>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/symbol), который предоставляет возможность группировать элементы. При этом данные объекты не отображаются до тех пор пока на них не будут ссылаться при помощи тега [`<use>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/use). Код ниже сокращён для удобства чтения:

```xml
<svg xmlns="http://www.w3.org/2000/svg">
  <symbol id="social-facebook" viewBox="0 0 6 12">
    <path d="M1.09903 2.72854C1.09903 ..." />
  </symbol>

  <symbol id="social-twitter" viewBox="0 0 16 11">
    <path d="M13.3758 4.62011C14.2231 ..." />
  </symbol>
</svg>
```

3. Каждый элемент `<symbol>` содержит атрибут `id` с уникальным идентификатором, который будет использоваться для ссылки на иконку в HTML. Также, каждая иконка содержит атрибут [`viewBox`](https://doka.guide/html/svg/#atributy), который определяет размеры окна отображения. Управляя данным атрибутом, можно указать в какой части холста находится конкретная иконка. Это позволяет использовать в спрайте иконки разных размеров и масштабировать их. Чтобы использовать иконку из спрайта на странице, нужно добавить внутрь тега [`<svg>`](https://doka.guide/html/svg/) тег `<use>` с обязательным атрибутом `href`. Данный атрибут должен ссылаться на файл спрайта с указанием `id` символа, содержащего нужное изображение:

```xml
<svg class="social-icon" viewBox="0 0 24 24" width="24" height="24">
  <use href="sprite.svg#social-vk" x="0" y="0"></use>
</svg>
```

4. Полученные таким образом элементы можно стилизовать с помощью CSS, как и обычные HTML-элементы. Вот пример:

```css
.social-icon {
  background-color: black;
  fill: white;
  transition: fill 0.3s ease-in-out;
}

.social-icon:hover,
.social-icon:focus {
  fill: rgb(160 123 80);
}
```
