# Best practices

- [ ] 🔴 **Используются CSS переменные, вместо обычной подстановки значений при дубляже**
- [ ] 🔴Использовать осмысленные и логичные имена для классов и идентификаторов, чтобы другие разработчики легко понимали структуру. Можно использовать методологию БЭМ.
- [ ] 🔴 **Используются единицы измерения `rem`**
- [ ] 🔴 **Используются однородные и принятые подходы к однотипным задачам**
- [ ] 🔴 **Адаптивный веб-дизайн.** На веб-сайте используется адаптивный веб-дизайн. Все страницы были протестированы с правильными точками останова.
- [ ] Медиа-запросы (`@media`) используем для адаптации стилей под разные размеры экранов. Ширины для медиазапросов:

- 320px
- 768px
- 1024px
- 1280px
- 1600px

Их может быть больше/меньше в зависимости от вашей аналитики.

> - 🛠 [Am I Responsive?](http://ami.responsivedesign.is/)
> - 🛠 [Mobile Friendly Test](https://search.google.com/test/mobile-friendly)
> - 🛠 [Responsinator](https://www.responsinator.com/)

- [ ] 🔴 **Соответствие W3C.** CSS был протестирован, и соответствующие ошибки были исправлены.

> - 🛠 [CSS Validator](https://jigsaw.w3.org/css-validator/)

- [ ] 🔴 **Линтеры**. Проанализировав код, не возникают ошибки.
- [ ] 🔴 **Браузеры.** Все страницы были протестированы минимум в трёх современных браузерах: Safari, Firefox, Chrome.
- [ ] 🔴 **Сброс CSS.** Используются стили "обнуления".

> - 🤠 [Обнуляющие стили](../../../../технологии/CSS/готовые%20решения/Обнуляющие%20стили.md)

- [ ] 🔴 **Минификация.** Все файлы минимизируются.
- [ ] 🔴 **Встроенный CSS**. Любой ценой избегайте встраивания CSS в теги `<style>` или использования встроенного CSS.
- [ ] 🟡 **Неблокируемый.** CSS-файлы должны быть неблокирующими, чтобы загрузка DOM не занимала много времени.

> - 📖 [Optimize-resource-loading](https://web.dev/learn/performance/optimize-resource-loading?continue=https%3A%2F%2Fweb.dev%2Flearn%2Fperformance&hl=ru#article-https://web.dev/learn/performance/optimize-resource-loading&hl=ru)
> - 📖 [Example of preload CSS using loadCSS](https://gist.github.com/thedaviddias/c24763b82b9991e53928e66a0bafc9bf)

- [ ] 🟢 **Неиспользуемый CSS:** Удалите неиспользуемый CSS.

> - 🛠 [UnCSS Online](https://uncss-online.com/)
> - 🛠 [PurgeCSS](https://github.com/FullHuman/purgecss)
> - 🛠 [Chrome DevTools Coverage](https://developer.chrome.com/docs/devtools/coverage/)

# Доступность

- [ ] 🔴 **Стиль фокуса.** Если фокус отключен, он заменяется видимым состоянием в CSS.

> - 🤠 [Правильный фокус интерактивного элемента](../../../../технологии/CSS/готовые%20решения/Правильный%20фокус%20интерактивного%20элемента.md)

- [ ] 🔴Для кнопок и ссылок всегда необходимо использовать псевдоклассы `:hover`, `:active`, и `:focus`, чтобы пользователи могли интуитивно понять, что это интерактивный элемент.
- [ ] 🔴 **Линтеры**. Проанализировав код, не возникают ошибки.
- [ ] 🔴 **Вендорные префиксы.** Префиксы поставщиков CSS используются и генерируются в соответствии с совместимостью вашего браузера.

> - 🛠 [Autoprefixer CSS online](https://autoprefixer.github.io/)

- [ ] 🔴 **Направление чтения.** Все страницы необходимо протестировать на наличие языков LTR и RTL, если они нуждаются в поддержке.

> - 📖 [Building RTL-Aware Web Apps & Websites: Part 1 - Mozilla Hacks](https://hacks.mozilla.org/2015/09/building-rtl-aware-web-apps-and-websites-part-1/)
> - 📖 [Building RTL-Aware Web Apps & Websites: Part 2 - Mozilla Hacks](https://hacks.mozilla.org/2015/10/building-rtl-aware-web-apps-websites-part-2/)
> - 📖 [Logical-properties](https://web.dev/learn/css/logical-properties?hl=ru)
