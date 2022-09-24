---
# Front matter
title: "Лабораторная работа 3"
author: "Поляков Арсений Андреевич, НФИбд-01-19"

# Generic otions
lang: ru-RU
toc-title: "Содержание"

# Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

# Pdf output format
toc: true # Table of contents
toc_depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
### Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Misc options
indent: true
header-includes:
  - \linepenalty=10 # the penalty added to the badness of each line within a paragraph (no associated penalty node) Increasing the value makes tex try to have fewer lines in the paragraph.
  - \interlinepenalty=0 # value of the penalty (node) added after each line of a paragraph.
  - \hyphenpenalty=50 # the penalty for line breaking at an automatically inserted hyphen
  - \exhyphenpenalty=50 # the penalty for line breaking at an explicit hyphen
  - \binoppenalty=700 # the penalty for breaking a line at a binary operator
  - \relpenalty=500 # the penalty for breaking a line at a relation
  - \clubpenalty=150 # extra penalty for breaking after first line of a paragraph
  - \widowpenalty=150 # extra penalty for breaking before last line of a paragraph
  - \displaywidowpenalty=50 # extra penalty for breaking before last line before a display math
  - \brokenpenalty=100 # extra penalty for page breaking after a hyphenated line
  - \predisplaypenalty=10000 # penalty for breaking before a display
  - \postdisplaypenalty=0 # penalty for breaking after a display
  - \floatingpenalty = 20000 # penalty for splitting an insertion (can only be split footnote in standard LaTeX)
  - \raggedbottom # or \flushbottom
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

<h1 align="center">
<p>РОССИЙСКИЙ УНИВЕРСИТЕТ ДРУЖБЫ НАРОДОВ 
<p>Факультет физико-математических и естественных наук  
<p>Кафедра прикладной информатики и теории вероятностей
<p>ОТЧЕТ ПО ЛАБОРАТОРНОЙ РАБОТЕ №3
<br></br>
<h2 align="right">
<p>дисциплина: Информационная безопасность
<p>Преподователь: Кулябов Дмитрий Сергеевич
<p>Студент: Поляков Арсений Андреевич
<p>Группа: НФИбд-01-19
<br></br>
<br></br>
<h1 align="center">
<p>МОСКВА
<p>2022 г.
</h1>

# **Цель работы**

Получение практических навыков работы в консоли с атрибутами файлов для групп пользователей.

# **Теоретическое введение**

useradd - добавление пользователя

passwd - установка пароля

pwd - местоположение по файловой системе

whoami - узнать логин

id - информация о пользователе

chmod - изменение атрибутов файла

newgrp - регистрация пользователя в группе

# **Выполнение лабораторной работы**

1. На виртуальной машине создал нового пользователя guest2 и задал для него пароль. Пользователь guest уже был создан

![create user](img/1.png "create user")

2. Добавил пользователя guest2 в группу guest

![add in group](img/2.png "add in group")

3. Осуществил вход в систему от двух пользователей на двух разных консолях:
guest на первой консоли и guest2 на второй консоли

![login1](img/3.png "login1")

![login2](img/4.png "login2")

4. Для обоих пользователей командой pwd определил директорию, в которой я нахожусь. Сравнил её с приглашениями командной строки - они совпадают 

![pwd1](img/5.png "pwd1")

![pwd2](img/6.png "pwd2")

5. Уточнил имя вашего пользователя, его группу, кто входит в неё и к каким группам принадлежит он сам.Guest имеет идентификатор 1001(guest), группа пользователя 1001(guest), состоит в группе 1001(guest). Guest2 имеет идентификатор 1002(guest2), группа пользователя 1002(guest2), состоит в группе 1002(guest2) и 1001(guest).
Определил командами groups guest и groups guest2, в какие группы входят пользователи guest и guest2.
Команда groups guest выдаёт группу guest. Команда groups guest2 выдаёт группу guest2.
Сравнил вывод команды groups с выводом команд id -Gn и id -G. Команда id -Gn выдает имя групп, команда id -G выдает id групп. 

![info1](img/7.png "info1")

![info2](img/8.png "info2")

6. Сравнил полученную информацию с содержимым файла /etc/group. Пользователь guest2 состоит в группе guest

![/etc/group](img/9.png "/etc/group")

7. От имени пользователя guest2 выполнил регистрацию пользователя guest2 в группе guest 

![newgrp](img/10.png "newgrp")

8. От имени пользователя guest изменил права директории /home/guest,
разрешив все действия для пользователей группы 

![chmod g+rwx](img/11.png "chmod g+rwx")

9. От имени пользователя guest снял с директории /home/guest/dir1
все атрибуты и проверил правильность снятия атрибутов 

![chmod 000](img/12.png "chmod 000")

10. Экспериментальным путём заполнил таблицу с правами и возможностями 

![excel1](img/13.png "excel1")

![excel2](img/14.png "excel2")

11. В отдельной таблице указал минимальные права для определённых действий 

![min requirments](img/15.png "min requirments")

# Выводы

Выполнив данную лабораторную работу, я создал второго нового пользователя, получил практические навыки работы в консоли с атрибутами файлов для групп пользователей.

# Список литературы

1. Кулябов, Д.С. - Лабораторная работа № 3. Дискреционное разграничение прав в Linux. Два пользователя
https://esystem.rudn.ru/pluginfile.php/1651885/mod_resource/content/4/003-lab_discret_2users.pdf
