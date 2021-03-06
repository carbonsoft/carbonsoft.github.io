---
layout: post
title: Carbon Reductor 7.5.0 204 и 800001435
---

## Повышение надёжности фильтрации (Carbon Reductor 7.5)

- **CR-448**: добавлено: в файлах модулей фильтрации выводится статистика по обеим базам данных - как buffer, так и readonly. Ранее счётчики показывались не совсем верно - в какой-то момент от buffer, в какой-то от readonly, что могло привести к ошибочному выводу о том, что база, по которой происходит поиск при фильтрации обнуляется при обновлении списков или рестарте.
- **CR-461**: добавлено: утилита `modules_ctl` используемая для взаимодействия с модулями ядра. Все скрипты, которые раньше за чем-либо лезли в /proc/ (исключение `load.sh`) теперь используют для этих целей её.
- **CR-481**: добавлено: режим для satellite, обеспечивающий отсутствие расхождения содержимого списков на reductor и satellite на время проверки фильтрации (используется только в целях агрессивного тестирования разработчиками).
- **CR-504**: исправлено: скрипты `load.sh` и `load_ip.sh` теоретически могли быть запущены в процессе обработки списков и использовать не до конца обработанные списки.

## Про "обезумевший" АС Ревизор

- **CR-470**: добавлено(диагностика): не указанный IP ревизора считается ошибкой.
- **CR-488**: добавлено(режим обслуживания): пока только в версии 7.5.0. В случае, если обновление Carbon Reductor несёт в себе обновление "прошивки" - модулей фильтрации (такие обновления обычно бывают раз в 1-3 месяца), на время процесса обновления (обычно 10-25 секунд) включается режим обслуживания. Правила файрвола не очищаются полностью, вместо этого добавляется REJECT для всех видов трафика (относительно осмысленно это только для TCP). Эти правила применяются только для ревизора, если указан его IP/подсеть, иначе - для всех абонентов. Сделано это с целью исключить пропуски фильтрации в процессе обновления Carbon Reductor.
- **CR-505**: добавлено(DNS-спуфинг): регистронезависимая фильтрация DNS для всех абонентов, если не указан IP/подсеть ревизора. Может повышать нагрузку на сервер.

## Исправления в Carbon Reductor 8

- **CR-417**: исправлено: применение собственных списков в веб-интерфейсе отдаёт 404/500 ошибку.
- **CR-445**: исправлено: L2-зеркало трафика, настроенное с помощью мастера не работало.
- **CR-462**: исправлено(веб-интерфейс): минорные ошибки в веб-интерфейсе, "файл не найден"
- **CR-469**: добавлено(диагностика): использование страницы-заглушки Carbonsoft считается ошибкой.
- **CR-484**: исправлено(https-proxy): проблемы с запуском при первой установке.

Портированием исправлений, направленных на повышение надёжности фильтрации и связанных с изменением режима работы АС Ревизор в Carbon Reductor 8 мы занимаемся прямо сейчас.

## Прочее

- **CR-519**: исправлено: satellite не удаляет пустые временные файлы на каждый домен при проверке DNS, что приводит к заполнению inodes. Проверить текущую заполненность можно командой: `df -h /tmp/`.
- **CR-514**: исправлено: плагин collectd сломался при обновлении модулей (см. CR-448). Плагин теперь поставляется в комплекте с Carbon Reductor, чтобы обеспечить его автоматическое обновление. Инструкция на github по работе с ним в скором времени будет обновлена и продублирована в документацию Carbon Reductor.
