# Безопасность

## Проверьте свою работу

> - 🛠 [securityheaders.io](https://securityheaders.io/)
> - 🛠 [Observatory by Mozilla](https://observatory.mozilla.org/)

## Best practices

- [ ] 🔴 **HTTPS.** HTTPS используется на каждой странице и для всего внешнего контента (плагинов, изображений...).

> - 🛠 [Let's Encrypt - Free SSL/TLS Certificates](https://letsencrypt.org/)
> - 🛠 [Free SSL Server Test](https://www.ssllabs.com/ssltest/index.html)

- [ ] 🔴 **Подделка межсайтового запроса (CSRF).** Вы гарантируете, что запросы, сделанные на стороне вашего сервера, являются законными и исходят с вашего веб-сайта.

> - 📖 [Cross-Site Request Forgery (CSRF) Prevention Cheat Sheet - OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)

- [ ] 🔴 **Межсайтовый скриптинг (XSS).** На вашей странице или веб-сайте нет возможных проблем XSS.

> - 📖 [XSS (Cross Site Scripting) Prevention Cheat Sheet - OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
> - 📖 [DOM based XSS Prevention Cheat Sheet - OWASP](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html)

- [ ] 🔴 **X-Frame-Options (XFO):** На вашей странице или веб-сайте нет возможных проблем XFO.

> - 📖 [RFC7034 - HTTP Header Field X-Frame-Options](https://tools.ietf.org/html/rfc7034)

- [ ] 🟡 **Политика безопасности контента:** Определяет, как контент загружается на ваш сайт и откуда его разрешено загружать.

> - 📖 [CSP Cheat Sheet - OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html)
> - 📖 [Content Security Policy Reference](https://content-security-policy.com/)

- [ ] 🟡 **HTTP Strict Transport Security (HSTS).** Заголовок HTTP имеет значение «Strict-Transport-Security».

> - 🛠 [Check HSTS preload status and eligibility](https://hstspreload.org/)
> - 📖 [HTTP Strict Transport Security Cheat Sheet - OWASP](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html)
> - 📖 [Transport Layer Protection Cheat Sheet - OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html)
