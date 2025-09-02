# Фильтр светлой и тёмной темы

```css
/* Для светлой темы */
.logos-container img {
  filter: brightness(0);
  opacity: 0.6;
}

/* Для тёмной темы */
.logos-container img {
  filter: invert(1) saturate(0) brightness(4);
  opacity: 0.6;
}
```
