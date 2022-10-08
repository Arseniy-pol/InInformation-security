---
marp: true
---

<style>
section.titleslide h6
{
    text-align: right;
}
section.titleslide
{
    text-align: center;
}
</style>

<!-- _class: titleslide -->

#### РОССИЙСКИЙ УНИВЕРСИТЕТ ДРУЖБЫ НАРОДОВ
#### Факультет физико-математических и естественных наук  
#### Кафедра прикладной информатики и теории вероятностей 
#### ПРЕЗЕНТАЦИЯ ПО ЛАБОРАТОРНОЙ РАБОТЕ №5

###### дисциплина: Информационная безопасность
###### Преподователь: Кулябов Дмитрий Сергеевич
###### Студент: Поляков Арсений Андреевич
###### Группа: НФИбд-01-19
МОСКВА
2022 г.

---

# **Прагматика выполнения лабораторной работы**

- работа с дополнительными атрибутами

---

# **Цель работы**

Изучение механизмов изменения идентификаторов, применения SetUID и Sticky-битов.

---

# **Выполнение лабораторной работы**

# 1. Создание программы simpleid.c. и выполнение
![simpleid](Img/1.png "simpleid.c")

![compile and run](Img/2.png "compile and run")

---

# 2. Создание программы simpleid2.c. и выполнение
![simpleid2.c](Img/3.png "simpleid2.c")

![simpleid2](Img/4.png "simpleid2")

---

# 3. Установка новых аттрибутов и запуск simpledid2
![chmod](Img/5.png "chmod")

![simpleid2 run](Img/6.png "simpleid2 run")

---

# 4. Создание программы readfile.c
![readfile.c](Img/7.png "readfile.c")

---

# 5. Смена владельца readfile
![chown](Img/8.png "chown")

![cant read](Img/9.png "cant read")

---

# 6. Смена владельца readfile и установил SetU’D-бит
![readfile](Img/10.png "readfile")

![readfile read](Img/11.png "readfile read")

![/etc/shadow read](Img/12.png "/etc/shadow read")

---

# 7. Проверка sticky бит на категории tmp.

![sticky](Img/13.png "sticky")

---

# 8. Выполнение различных операций от guest2

![guest2 file01](Img/14.png "guest2 file01")

---

# 9. Снятие атрибут t (Sticky-бит) сдиректории /tmp и выполнение предыдущих шагов

![-t](Img/15.png "-t")

![guest2 file01 try 2](Img/16.png "guest2 file01 try 2")

---

# Вывод

Выполнив данную лабораторную работу, я получение практические навыков работы в консоли с дополнительными атрибутами. Рассмотрел работы механизма смены идентификатора процессов пользователей, а также влияние бита Sticky на запись и удаление файлов.