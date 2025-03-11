# Linux Mint

## Команды для выполнения после установки

- sudo systemctl disable cups-browsed.service (если вы не используете принтер)
- sudo systemctl disable avahi-daemon.service (если вы не используете домашнюю локальную сеть)
- sudo systemctl disable geoclue.service (если вы не хотите, чтобы приложения получали вашу геолокацию)
- sudo systemctl disable ModemManager.service (если вы не планируете использовать модемы 2G, 3G, LTE и т.д)
- sudo apt install neofetch (утилита командной строки, которая позволяет отображать данные о системе в окне терминала Linux)

## Команды для регулярного выполнения

Для удаление "мусора", который образуется в ОС в процессе работы:

- sudo apt autoclean
- sudo apt autoremove
