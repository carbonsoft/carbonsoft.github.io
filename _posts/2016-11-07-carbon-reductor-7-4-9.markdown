---
layout: post
title:  "Carbon Reductor 7.4.9"
date:   2016-11-07 14:28:06 +0500
---
В Carbon Reductor 7.4.9 всё внимание было сконцентрировано на стабилизации продукта, так как принятие закона <http://asozd2.duma.gov.ru/main.nsf/(Spravka)?OpenAgent&RN=1102471-6>
не за горами.

- **CR-0**. Исправлено(управление проектом): недавно нам пришлось обновиться на последнюю версию Jira. Она довольно похорошела за несколько лет, многие вещи стали удобнее, поэтому релизы теперь будут основываться на сгенерированных отчётах в таком формате. Тем не менее, пока что мы оставляем здесь только действительно важные пользователям моменты, а также дополняем их подробным описанием.
- **CR-73**. Исправлено(разбор реестра): так как реестр стал довольно большим, https ресурсов в нём всё больше и больше, имеется несколько альтернативных методов блокировки по IP (DNS-фильтрация и SNI) резолвер теперь ставит в очередь на запросы не более 500 доменов, потому что большее число может приводить к превышению выделенного на его работу таймаута, одновременным запускам и другим проблемам. Логика работы резолвера такова, что спустя некоторое время после первого запуска он в основном запускает резолв ресурсов, которые действительно необходимо отрезолвить, то есть часто меняющие свои IP-адреса.
- **CR-207**. Исправлено(обработка списков): была возможность повредить кэш сигнатур при многократном одновременном запуске скрипта их обновления, что могло привести к отсутствию блокировки части HTTP-ресурсов.
- **CR-245**. Исправлено(обработка списков): новая система обработки белых списков неправильно обрабатывала URL, содержащие пробелы, что приводило к отсутствию блокировки в оригинальном виде (не фильтровались запросы к ним, совершённые с помощью CURL и старых версий IE).
- **CR-247**. Исправлено(модули фильтрации): segfault вспомогательного модуля фильтрации для прокси-серверов.
- **CR-254**. Исправлено(миграция списков): благодаря собранным данным об использовании списков (только названия и размер) удалось сделать обновление на версию 7.4 практически незаметным. Автоматически будут удалены только временные файлы от старой обработки списков, правильно названные файлы будут аккуратно переименованы и размещены в нужном месте, а о тех, с которыми не удалось разобраться автоматически администратор будет уведомлён по почте + заявкой в хелпдеске.
- **CR-263**. Исправлено(утилиты для отладки): автоматическое удаление дампов requests/replies после их объединения в duplex экономит место на диске.
- **CR-269**. Добавлено(веб-интерфейс): отображение новой структуры списков в веб-интерфейсе. Теперь списки разбиты по группам, для категорий: "обработанные", "Роскомнадзор", "Carbon Soft", "Резолвер" отображаются только существующие списки, для провайдеров отображаются все возможные списки, включая белые, с возможностью редактирования.
- **CR-276**. Исправлено(техподдержка): не создавались автоматические задачи в хелпдеске. (Однако уведомления о проблемах администраторам по e-mail продолжали и продолжают отправляться).
- **CR-278**. Исправлено(обновление): обновление с версии 7.3 на версию 7.4 приводило к приблизительно минутному downtime фильтрации. Теперь такие задачи как обнволение веб-интерфейса и миграция списков происходят до остановки фильтрации.
- **CR-279**. Исправлено(диагностика): не работала проверка содержимого собственных списков, в диагностике были указаны пути до старых собственных списков провайдера.
- **CR-281**. Исправлено(обработка списков): ресурсы, которые необходимо блокировать по домену снова дополнительно блокируются модулем HTTP для подстраховки, так как число провайдеров, корректно настроивших фильтрацию по DNS довольно мало. Тем не менее, мы очень рекомендуем настроить её в ближайшие сроки.
- **СR-290**. Исправлено(RPM): из репозиториев удалены все устаревшие дефолтные файлы.
- **CR-291**. Исправлено(обработка списков): сохранение обработной совместимости с системами интеграции с DNS серверами провайдера. Запуск резолвера создаёт симлинк на список всех блокируемых доменов, даже если сам резолвер отключен.
- **CR-293**. Улучшено(диагностика): человеческий формат времени в ошибке диагностики "Проверка актуальности списков".
- **CR-294**. Исправлено(обработка списков): минюст не блокировался. Сохранялся в файл со старым расширением.
- **CR-297**. Исправлено(веб-интерфейс): добавление кириллических записей в собственные списки через веб-интерфейс вызывало ошибку 502 Bad Gateway.
