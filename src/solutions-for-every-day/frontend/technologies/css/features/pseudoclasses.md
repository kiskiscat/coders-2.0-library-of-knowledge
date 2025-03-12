# Псевдоклассы

## Последовательность

Если Вы задаёте стили для разных состояний ссылок, следует придерживаться определённого порядка в объявлении. Этот порядок легко запомнить при помощи аббревиатуры LVFHA и мнемоники LoVe Fears HAte:

    :link
    :visited
    :focus
    :hover
    :active

## :visited

Для предотвращения злодеяний браузеры приняли решение, что ограничат стили, которые будут срабатывать для псевдокласса `:visited`. Любые другие стили будут игнорироваться. Так что не удивляйтесь, если что-то из написанного вами кода не будет работать. Вот список доступных свойств:

- color
- background-color
- border-color
- border-bottom-color или border-block-end-color
- border-left-color или border-inline-start-color
- border-right-color или border-inline-end-color
- border-top-color или border-block-start-color
- outline-color
- column-rule-color
- fill
- stroke
