# Контейнер для контента и страницы

```css
.page-container {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  flex: 1 1 100%;
  width: 100%;
  min-height: 100vh;
}


.content-container {
  width: clamp(360px, 90%, 1024px);
  margin-left: auto;
  margin-right: auto;
}
```
