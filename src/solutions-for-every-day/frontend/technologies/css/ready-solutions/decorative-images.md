# Декоративные изображения

```css
/* Первый способ */

.image {
  width: 100%;
  height: 100%;
  display: block;
  object-fit: cover;
}

/*
 * Второй способ
 * Соотношение сторон для padding-bottom
 *
 * 16:9 - 56.25%
 * 4:3 - 75%
 * 3:2 - 66.66%
 * 8:5 - 62.5%
*/

.image {
  width: 100%;
  height: 0;
  padding-bottom: 56.25%;
}

/* Третий способ (кроссбраузерный) */

.image {
  display: block;
  width: 100%;
  height: 100%;

  /* Фолбэк */
  background-image: url('puppy.png');

  /* Для Chrome и Safari */
  background-image: -webkit-image-set(
    url('puppy@2x.png') 2x,
    url('puppy@1x.png') 1x
  );

  /* Для Firefox */
  background-image: image-set(
    url('puppy@2x.avif') type('image/avif') 2x,
    url('puppy@1x.avif') type('image/avif') 1x,
    url('puppy@2x.webp') type('image/webp') 2x,
    url('puppy@1x.webp') type('image/webp') 1x,
    url('puppy@2x.png') 2x,
    url('puppy@1x.png') 1x
  );
}
```
