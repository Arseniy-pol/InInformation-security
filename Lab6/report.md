---
title: "Лабораторная работа №6"
author: "Поляков Арсений Андреевич"
subtitle: "Мандатное разграничение прав в Linux"
output:
  word_document: default
  pdf_document: default
toc-title: Содержание
toc: yes
toc_depth: 2
lof: yes
fontsize: 12pt
linestretch: 1.5
papersize: a4paper
documentclass: scrreprt
polyglossia-lang: russian
polyglossia-otherlangs: english
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase
indent: yes
pdf-engine: lualatex
header-includes:
- \linepenalty=10
- \interlinepenalty=0
- \hyphenpenalty=50
- \exhyphenpenalty=50
- \binoppenalty=700
- \relpenalty=500
- \clubpenalty=150
- \widowpenalty=150
- \displaywidowpenalty=50
- \brokenpenalty=100
- \predisplaypenalty=10000
- \postdisplaypenalty=0
- \floatingpenalty = 20000
- \raggedbottom
- \usepackage{float}
- \floatplacement{figure}{H}
---


# Цель работы

Целью данной лабораторной работы является развить навыки администрирования ОС Linux. Получить первое практическое знакомство с технологией SELinux1. Проверить работу SELinx на практике совместно с веб-сервером Apache.

# Выполнение лабораторной работы

1. Входим в систему с полученными учётными данными. Проверили, что SELinux работает в режиме enforcing политики targeted с помощью команд **getenforce** и **sestatus**.
   
![Выполнение команд getenforce и sestatus](Img/1.png)

2. Запустили веб-сервер и обратились к нему с помощью команды:
service httpd status 
   
![Выполнение команды status](Img/2.png)

3. Найшли веб-сервер Apache в списке процессов с помощью команды **ps auxZ | grep httpd**. Контекст безопасности - unconfined_u:unconfined_r:unconfined_t. 
   
![Выполнение команды ps auxZ | grep httpd](Img/3.png)

4. Посмотрели текущее состояние переключателей SELinux для Apache с помощью команды .
   
![Выполнение команды sestatus -ez](Img/4.png)

5. Посмотрели статистику по политике с помощью команды **seinfo**. Определили, что множество пользователей = 8; ролей = 14; типов = 5002.
   
![Статистика по политике](Img/5.png)

6. Определили тип файлов и поддиректорий, находящихся в директории /var/www, с помощью команды **ls -lZ /var/www**.
   
![Выполнение команды ls -lZ /var/www](Img/6.png)

7. Необходимо было определить тип файлов, находящихся в директории /var/www/html, с помощью команды **ls -lZ /var/www/html**. Но в данной директории файлов не обнаружилось. 
   
![Выполнение команды ls -lZ /var/www/html](Img/7.png)

9. Создали от имени суперпользователя html-файл /var/www/html/test.html следующего содержания: 
   
![Содержимое файла test.html](Img/8.png)

10. Проверили контекст созданного файла - httpd_sys_content_t.
   
![Контекст файла test.html](Img/10.png)

11.  Обратитились к файлу через веб-сервер, введя в браузере адрес http://127.0.0.1/test.html и убедились, что файл был успешно отображён. 
   
![Обращение к файлу test.html через веб-сервер](Img/11.png)

12.   Изучили справку man httpd_selinux. Тип файла test.html - контекст созданного файла - httpd_sys_content_t. 
   
![Контекст файла test.html](Img/12.png)

13.  Изменили контекст файла /var/www/html/test.html с httpd_sys_content_t на samba_share_t:
chcon -t samba_share_t /var/www/html/test.html
ls -Z /var/www/html/test.html
И проверили, что контекст поменялся.
   
![Изменение контекста файла /var/www/html/test.html](Img/13.png)

14. Пробуем ещё раз получить доступ к файлу через веб-сервер, введя в браузере адрес http://127.0.0.1/test.html. В результате получили ошибку.
   
![Обращение к файлу test.html через веб-сервер после изменения контекста](Img/14.png)

15.  Проанализируем ситуацию. Почему файл не был отображён, если права доступа позволяют читать этот файл любому пользователю?
ls -l /var/www/html/test.html
Просмотрим log-файлы веб-сервера Apache и системный лог-файл:
tail /var/log/messages
В системе оказались запущенны процессы **setroubleshootd** и **audtd**. 
   
![Вывод команд ls -l /var/www/html/test.html и tail /var/log/messages](Img/15.png)

16. Попробуем запустить веб-сервер Apache на прослушивание ТСР-порта 81. Для этого в файле /etc/httpd/httpd.conf находим строчку Listen 80 и заменяем её на Listen 81.
   
![Запуск веб-сервера Apache на прослушивание ТСР-порта 81](Img/16.png)
 
17. Выполним перезапуск веб-сервера Apache. Произошёл сбой? Нет. 
18. Проанализируем лог-файлы:
tail -nl /var/log/messages
Просмотрим файлы /var/log/http/error_log,
/var/log/http/access_log и /var/log/audit/audit.log. 
   
![Перезапуск веб-сервера Apache](Img/17.png)

19. Выполним команду **semanage port -a -t http_port_t -р tcp 81**. Вылетает ValueError в связи с тем, что порт уже определен. После этого проверим список портов командой **semanage port -l | grep http_port_t** и убедились, что порт 81 появился в списке.
   
![Проверка установления 81 порта tcp](Img/18.png)

20. Попробуем запустить веб-сервер Apache ещё раз. 
   
![Перезапуск веб-сервера Apache](Img/20.png)
  
21. Вернули контекст httpd_sys_cоntent__t к файлу /var/www/html/test.html: **chcon -t httpd_sys_content_t /var/www/html/test.html** 
   
![Возвращение контекста httpd_sys_cоntent__t к файлу test.html](Img/25.png)

После этого пробуем получить доступ к файлу через веб-сервер, введя в браузере адрес http://127.0.0.1:81/test.html. В результате увидели содержимое файла — слово «test». 
   
![Обращение к файлу test.html через веб-сервер](Img/21.png)

22. Исправим обратно конфигурационный файл apache, вернув Listen 80. 
   
![Исправление конфигурационного файла apache](Img/22.png)

23. Удалим привязку http_port_t к 81 порту: **semanage port -d -t http_port_t -p tcp 81** и проверим, что порт 81 удалён. Данная команда не была выполнена. 
   
![Удаление привязки http_port_t к 81 порту](Img/23.png)

24.  Удалим файл /var/www/html/test.html: **rm /var/www/html/test.html**.
   
![Удаление файла test.html](Img/24.png)

# Вывод

В ходе выполнения лабораторной работы мы развили навыки администрирования ОС Linux. Получили первое практическое знакомство с технологией SELinux1. Проверили работу SELinx на практике совместно с веб-сервером
Apache.

# Библиография

1. Кулябов Д. С., Королькова А. В., Геворкян М. Н. Мандатное разграничение прав в Linux [Текст] / Кулябов Д. С., Королькова А. В., Геворкян М. Н. - Москва: - 5 с. [^1]: Мандатное разграничение прав в Linux. 
2. Справочник 70 основных команд Linux: полное описание с примерами (https://eternalhost.net/blog/sozdanie-saytov/osnovnye-komandy-linux)
  