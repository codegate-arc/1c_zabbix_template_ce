# 1c_zabbix_template
Шаблон, конфигурационный файл агента и вспомогательные скрипты для мониторинга серверов 1С Предприятия под GNU/Linux с помощью Zabbix
## Выбранная архитектура (текущая версия)
Шаблон разбит на несколько составляющих по функциональному назначению:
* Шаблон для мониторинга рабочих процессов кластера 1С Предприятия (в активной разработке);
* Шаблон для мониторанга сервера лицензирования (реализовано);
* Шаблон для мониторинга центральных серверов кластера 1С Предприятя (планируется к реализации).

На текущий момент выбрана модель сбора необходимых данных без задействования (или задействования по минимуму) сервиса <b>RAS</b>, обходясь средствами ОС.

<b>ВАЖНО:</b> Выбранный вариант архитектуры предполагает что сервер, которому добавляется данный шабон, принимает участие только в одном кластере серверов 1С Предприятия!
## Установка
На сервере Zabbix необходимо добавить (экспортировать) файлы шаблонов:
* 1c_rmngr_template.xml;
* 1c_rphost_template.xml.

После чего назначить необходимый шаблон(ы) хосту, на котором работает 1С Предприятие. В шаблонах имеются макросы, позволяющие задавать требуемые пороги реагирования Zabbix (срабатывания триггеров).

На сервере с 1С Предприятием необходимо установить все скрипты по структуре каталогов репозитория (структура каталогов соответсвует используемой в <b>CentOS</b>).

Из файла <b>/opt/1C/noarch/conf/logcfg_zabbix_template.xml</b> необходимо перенести секции <b>log</b> в файл <b>logcfg.xml</b> рабочего сервера 1С Предприятия (или просто скопировать его в каталог /opt/1C/v8.3/тип_архитектуры/conf/, если сбор ТЖ не использовался).
### Макросы для рабочего сервера
В шаблоне рабочего сервера есть следующие макросы:
* {$LOG_DIR} - каталог для хранения файлов технологического журнала;
* {$MAX_LOCK_WAIT} - пороговое значение (в секундах за час) суммарного ожидания на управляемых блокировках, по превышении которого срабатывает триггер;
* {$TOP_CALL_LIMIT} - количество выдаваемых (сохраняемых) записей в агригированных выборках по серверным вызовам.
### Макросы для сервера лицензирования
В шаблоне сервера лицензирования есть следующие макросы:
* {$LIC_UTIL_LIMIT} - значение отношения количества использованных лицензий к количеству сеансов, лицензируемых клиентскими лицензиями, активированными на данном сервере, по превышении которого срабатывает триггер с предупреждением о скором исчерпании имеющихся лицензий;
* {$RAS_PORT} - порт сервера(ов) RAS, для кластеров, в которых участвует данный сервер лицензирования.
## Мониторинг рабочих процессов (rphost)
В настоящий момент реализован сбор следующих показателей (осуществляется по всем рабочим процессам сервера):
* Количество таймаутов на управляемых блокировках;
* Количество взаимоблокировок на управляемых блокировках;
* Суммарное время ожидания на управляемых блокировках;
* ТОП серверных вызовов по суммарной длительности;
* ТОП серверных вызовов по суммарному процессорному времени;
* ТОП серверных вызовов по суммарному количеству;
## Мониторинг сервера лицензирования (rmngr)
В настоящий момент реализован сбор следующих показателей:
* Общее количество сеансов на серверах кластера, в котором участвует данный сервер лицензирования;
* Количество файлов клиентских лицензий, активированных на данном сервере;
* Количество сеансов лицензируемых клиенскими (программными) лицензиями, активированными на данном сервере;
* Количество использованных лицензий (выданных данным сервером).
