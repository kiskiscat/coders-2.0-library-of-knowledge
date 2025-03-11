# Hover эффект

## Стандартный эффект

### CSS вариант

```css
/* Чтобы не писать дополнительные обнуления ховер-стилей для тач-устройств, состояние :hover удобно задавать внутри медиавыражений с условием по типу взаимодействия с устройством. */

.interactive {
  color: #ffffff;
  text-decoration-color: #2e9aff;
  transition: background-color 0.5s linear;
}


@media (any-hover: hover) {
  .interactive:not([disabled]):hover {
    background-color: #2e9aff;
    transition: background-color 0.1s linear;
  }
}
```

### SCSS вариант

```scss
.interactive {
  &:not(:disabled) {
    @media (any-hover: hover) {
      &:hover {
        background-color: #2e9aff;
        transition: background-color 0.1s linear;
      }
    }
  }
}
```

## Эффект с быстрым появлением и медленным исчезновением

### CSS вариант

```css
.interactive {
  color: #ffffff;
  text-decoration-color: #2e9aff;
  transition: background-color 0.5s linear;
}

@media (any-hover: hover) {
  .interactive:not([disabled]):hover {
    background-color: #2e9aff;
    transition: background-color 0.1s linear;
  }
}
```

### SCSS вариант

```scss
.interactive {
  &:not(:disabled) {
    @media (any-hover: hover) {
      &:hover {
        background-color: #2e9aff;
        transition: background-color 0.1s linear;
      }
    }
  }
}
```
