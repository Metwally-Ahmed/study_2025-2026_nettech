---
## Front matter
title: "Отчёт по лабораторной работе 4"
subtitle: "Подготовка экспериментального стенда GNS3"
author: "Метвалли Ахмед Фарг Набеех"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
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
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---


# Цель работы

Установка и настройка GNS3 и сопутствующего программного обеспечения.

# Выполнение

## Настройка и запуск GNS3 с добавлением образов маршрутизаторов FRR и VyOS

1. Виртуальная машина **GNS3 VM** была запущена. После загрузки из основной операционной системы запущено приложение **GNS3**.  

2. При первом запуске GNS3 автоматически запустился мастер настройки. В разделе **Remote controller** были указаны параметры подключения к серверу GNS3:  
   - **Protocol:** HTTP  
   - **Host:** `192.168.133.131`  
   - **Port:** `80 TCP`  
   - **Username:** admin  
   - **Password:** *•••••*  

   ![Настройка подключения к контроллеру GNS3](Screenshot_1.png){ #fig:001 width=70% }

3. После подключения выполнено добавление образа маршрутизатора **FRR (FRRouting)**.  
   В окне выбора шаблонов выбрана категория **Routers**, после чего в списке выбрано устройство **FRR**.

   ![Выбор образа FRR](Screenshot_2.png){ #fig:002 width=85% }

4. В процессе установки были отображены доступные версии FRR. Для установки выбрана версия **8.2.2**, статус которой отображён как *Ready to install*.

   ![Выбор версии FRR](Screenshot_3.png){ #fig:003 width=85% }

5. После завершения установки появилось окно с краткими сведениями об устройстве, где указаны учетные данные для входа:  
   **Username:** root  
   **Password:** root  

   ![Информация об установленном FRR](Screenshot_4.png){ #fig:004 width=70% }

6. В окне **QEMU VM template configuration** выполнена настройка шаблона FRR:  
   - Категория: **Routers**  
   - RAM: **256 MB**  
   - CPU: **1 vCPU**  
   - Консоль: **telnet**  
   - Параметр *On close*: **Send the shutdown signal (ACPI)**  

   ![Настройки FRR — General](Screenshot_5.png){ #fig:005 width=85% }

7. Во вкладке **HDD** указано использование образа `frr-8.2.2.qcow2` и активирована опция *Automatically create a config disk on HDD*.

   ![Настройки FRR — HDD](Screenshot_6.png){ #fig:006 width=85% }

8. После завершения конфигурации FRR был добавлен в рабочее пространство GNS3 и успешно запущен. На консоли устройства отображается приветственное сообщение FRRouting 8.2.2, подтверждающее корректную работу маршрутизатора.

   ![Запуск маршрутизатора FRR](Screenshot_7.png){ #fig:007 width=85% }

9. Аналогичным образом был добавлен образ маршрутизатора **VyOS**. Для установки выбрана версия **1.3.3-qemu**, статус которой отображён как *Ready to install*.

   ![Выбор версии VyOS](Screenshot_8.png){ #fig:008 width=85% }

10. После установки отображены инструкции по использованию устройства. Указаны учётные данные по умолчанию:  
    **Username:** vyos  
    **Password:** vyos  

    ![Информация об установленном VyOS](Screenshot_9.png){ #fig:009 width=70% }

11. После добавления маршрутизатор **VyOS** был запущен, что подтверждается выводом консоли при загрузке операционной системы.

    ![Запуск маршрутизатора VyOS](Screenshot_10.png){ #fig:010 width=85% }

# Заключение

В ходе работы выполнена настройка и запуск виртуальной машины GNS3, добавлены и сконфигурированы образы маршрутизаторов **FRR** и **VyOS**. Оба устройства успешно установлены, загружены и готовы к использованию в сетевых топологиях для дальнейших экспериментов.
