# Текст с троеточием

```css
/* Для однострочного текста */

.text-collapseShort {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* Для многострочного текста */

.text-collapseLong {
  display: -webkit-box;
  overflow: hidden;
  -webkit-line-clamp: 2; /* кол-во строк */
  -webkit-box-orient: vertical;
}
```
