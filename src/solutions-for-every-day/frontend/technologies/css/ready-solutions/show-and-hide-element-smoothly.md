# Плавно показать и скрыть элемент

```css
.parent {
  padding: 15px;
  border: 1px solid currentColor;
}

.block {
  visibility: hidden;
  opacity: 0;
  transition: visibility 0s linear 300ms, opacity 300ms;
}

.parent:hover .block {
  visibility: visible;
  opacity: 1;
  transition: visibility 0s linear 0s, opacity 300ms;
}
```
