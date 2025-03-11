# Производительность

- [ ] 🔴 **Отложенная загрузка.** Изображения, скрипты и CSS необходимо загружать отложено, чтобы сократить время отклика текущей страницы.
- [ ] 🔴 **Предварительная загрузка.** Ресурсы, которые понадобятся в ближайшее время (например, ленивые загруженные изображения), запрашиваются заранее во время простоя с использованием предварительной выборки.
- [ ] 🔴 **Цели для достижения.** Ваши страницы должны достигать следующих целей:

  - Первая значимая краска менее чем за 1 секунду
  - Время взаимодействия менее 5 секунд для «средней» конфигурации (Android за 200 долларов в медленной сети 3G с RTT 400 мс и скоростью передачи 400 Кбит/с) и менее 2 секунд для повторных посещений.
  - Критический размер файла менее 170 КБ в сжатом виде.

> - 🛠 [Website Page Analysis](https://tools.pingdom.com/)
> - 🛠 [WebPageTest](https://www.webpagetest.org/)
> - 📖 [Size Limit: Make the Web lighter](https://evilmartians.com/chronicles/size-limit-make-the-web-lighter)

- [ ] 🔴 **Вес страницы.** Вес каждой страницы составляет от 0 до 500 КБ.
- [ ] 🔴 **Размер файла cookie.** Если Вы используете файлы cookie, убедитесь, что размер каждого файла cookie не превышает 4096 байт, а в вашем доменном имени не более 20 файлов cookie.

> - 📖 [Cookie specification: RFC 6265](https://tools.ietf.org/html/rfc6265)
> - 📖 [Cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)

- [ ] 🟡 **Google PageSpeed показатели.** все ваши страницы были протестированы и имеют оценку не менее 85/100.

> - 🛠 [Google PageSpeed](https://developers.google.com/speed/pagespeed/insights/)
> - 🛠 [Test your mobile speed with Google](https://testmysite.withgoogle.com/)
> - 🛠 [WebPagetest - Website Performance and Optimization Test](https://www.webpagetest.org/)
> - 🛠 [GTmetrix - Website speed and performance optimization](https://gtmetrix.com/)
> - 🛠 [Speedrank - Improve the performance of your website](https://speedrank.app/)

- [ ] 🟡 **Сторонние компоненты.** Сторонние `iframe` или компоненты, использующие внешний JS (например, кнопки общего доступа), по возможности заменяются статическими компонентами, тем самым ограничивая вызовы внешних API и сохраняя конфиденциальность действий вашего пользователя.

> - 🛠 [Simple sharing buttons generator](https://simplesharingbuttons.com/)
