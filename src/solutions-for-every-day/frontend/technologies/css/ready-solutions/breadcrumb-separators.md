# Разделители для хлебных крошек

```css
[aria-label^="breadcrumb" i] li + li::before {
    content: "";
    display: block;
    width: 8px;
    height: 8px;
    border-top: 2px solid currentColor;
    border-right: 2px solid currentColor;
    rotate: 45deg;
    opacity: .8
  }
```
