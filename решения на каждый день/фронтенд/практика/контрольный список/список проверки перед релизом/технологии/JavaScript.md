# Best practices

- [ ] 🔴 **Отсутствие встроенного JavaScript.** У вас нет встроенного кода JavaScript (смешанного с HTML-кодом).
- [ ] 🔴 **Безопасность**.

> - 📖 [Guidelines for Developing Secure Applications Utilizing JavaScript](https://www.owasp.org/index.php/DOM_based_XSS_Prevention_Cheat_Sheet#Guidelines_for_Developing_Secure_Applications_Utilizing_JavaScript)

- [ ] 🔴 **Объединение:** Все файлы объединяются.
- [ ] 🔴 **Минификация.** Все файлы минимизируются.

> - 📖 [Minify Resources (HTML, CSS, and JavaScript)](https://developers.google.com/speed/docs/insights/MinifyResources)

- [ ] 🔴 **Линтеры**. Проанализировав код, не возникают ошибки.
- [ ] 🟡 **Неблокируемый:** Файлы JavaScript загружаются асинхронно с использованием `async` или откладываются с помощью атрибута `defer` или `type="module"` .

> - 📖 [Remove Render-Blocking JavaScript](https://developers.google.com/speed/docs/insights/BlockingJS)

- [ ] 🟡 **Оптимизированы и обновлены библиотеки JS:** Все библиотеки JavaScript, используемые в вашем проекте обновлены. до последней версии и не перегружают ваш код ненужными методами.
