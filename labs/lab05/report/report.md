---
## Front matter
title: "Отчёт по лабораторной работе 5"
subtitle: "Простые сети в GNS3. Анализ трафика"
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

Построение простейших моделей сети на базе коммутатора и маршрутизаторов FRR и VyOS в GNS3, анализ трафика посредством Wireshark.

# Выполнение

## Моделирование простейшей сети на базе коммутатора в GNS3

### Построение топологии и настройка IP-адресов

1. В приложении **GNS3** создан новый проект и размещены устройства: коммутатор **msk-ahmedfarg-sw-01** и два виртуальных ПК — **PC1-ahmedfarg** и **PC2-ahmedfarg**.  
   Устройства соединены кабелями Ethernet, формируя простую локальную сеть.

   ![Топология сети в GNS3](Screenshot_1.png){ #fig:001 width=70% }

2. На каждом ПК была открыта консоль **VPCS**. Для просмотра доступных команд введена команда `?`, отобразившая список поддерживаемых операций.

   ![Просмотр списка команд VPCS](Screenshot_2.png){ #fig:002 width=70% }

3. Выполнена настройка IP-адресов:  
   - для **PC1-ahmedfarg**: `ip 192.168.1.11/24 192.168.1.1`  
   - для **PC2-ahmedfarg**: `ip 192.168.1.12/24 192.168.1.1`  

   После настройки параметры были сохранены командой `save`. Проверка связи с помощью `ping` показала успешный обмен ICMP-пакетами между устройствами.

   ![Настройка IP и проверка связи между ПК](Screenshot_3.png){ #fig:003 width=85% }

### Анализ трафика в Wireshark

4. Для анализа сетевого обмена запущен захват трафика на линке между **PC1** и коммутатором. В окне **Wireshark** зафиксированы ARP-пакеты, определяющие соответствие IP- и MAC-адресов.  
   На скриншоте видно, что оба устройства отправляют *Gratuitous ARP*-запросы с целью обновления ARP-таблиц в сети.

   ![Захват ARP-пакетов в Wireshark](Screenshot_4.png){ #fig:004 width=85% }

5. Далее был выполнен обмен ICMP-пакетами (эхо-запрос и эхо-ответ) при выполнении команды `ping` с **PC2** на **PC1**.  
   На захвате видно тип пакета *Echo (ping) request/reply* и соответствующие IP-адреса источника и назначения.

   ![ICMP-эхо-запрос и ответ](Screenshot_5.png){ #fig:005 width=85% }

6. При выполнении `ping` с флагом `-2` был отправлен UDP-эхо-запрос.  
   Анализ показал использование протокола **UDP (port 7)** для передачи данных, при этом в поле *Payload* отображается содержимое передаваемого сообщения.

   ![UDP-эхо-запрос в Wireshark](Screenshot_6.png){ #fig:006 width=85% }

7. При использовании опции `-3` команда `ping` выполнялась в режиме **TCP**.  
   В захваченных пакетах Wireshark зафиксированы типичные фазы установления TCP-соединения: *SYN*, *SYN-ACK* и *ACK*, подтверждающие корректную работу транспортного уровня.

   ![TCP-эхо-запрос и установка соединения](Screenshot_7.png){ #fig:007 width=85% }

8. В терминале VPCS была изучена справка по команде `ping`, где представлены возможные режимы работы: ICMP (`-1`), UDP (`-2`), TCP (`-3`).  
   Все три режима были протестированы, что подтвердило корректное взаимодействие на уровнях **ICMP**, **UDP** и **TCP**.

   ![Проверка различных режимов ping в VPCS](Screenshot_8.png){ #fig:008 width=80% }

## Моделирование простейшей сети на базе маршрутизатора FRR в GNS3

### Построение топологии и настройка устройств

1. В приложении **GNS3** создан новый проект. В рабочей области размещены устройства: коммутатор **msk-ahmedfarg-sw-01**, маршрутизатор **msk-ahmedfarg-gw-01** и конечное устройство **PC1-ahmedfarg**.  
   Все элементы соединены между собой кабелями Ethernet, формируя простейшую сеть.

   ![Топология сети с маршрутизатором FRR](Screenshot_9.png){ #fig:009 width=70% }

2. В консоли **VPCS** для узла **PC1-ahmedfarg** выполнена настройка IP-адреса:  
   ```
   ip 192.168.1.10/24 192.168.1.1
   save
   show ip
   ```
   В результате устройство получило адрес `192.168.1.10/24` с шлюзом `192.168.1.1`.

   ![Настройка IP-адреса для PC1](Screenshot_10.png){ #fig:010 width=75% }

3. На маршрутизаторе **FRR** выполнена базовая настройка. В режиме конфигурации был изменён hostname и задан IP-адрес интерфейса `eth0`:  
   ```
   configure terminal
   hostname msk-ahmedfarg-gw-01
   interface eth0
   ip address 192.168.1.1/24
   no shutdown
   exit
   write memory
   ```
   Конфигурация была сохранена и успешно применена.

   ![Настройка маршрутизатора FRR](Screenshot_11.png){ #fig:011 width=80% }

4. Для проверки настроек маршрутизатора использованы команды:  
   ```
   show running-config
   show interface brief
   ```
   В выводе показано, что интерфейс **eth0** активен и имеет адрес `192.168.1.1/24`.

   ![Проверка конфигурации FRR](Screenshot_12.png){ #fig:012 width=80% }

### Проверка связи и анализ трафика

5. На **PC1-ahmedfarg** выполнена проверка связи с маршрутизатором. Команда `ping 192.168.1.1` показала успешные ответы, что подтверждает корректность настройки IP-сети.

   ![Проверка связи между PC1 и маршрутизатором](Screenshot_13.png){ #fig:013 width=80% }

6. В **Wireshark** был запущен захват пакетов на линии между коммутатором и маршрутизатором.  
   Анализ показал обмен ICMP-пакетами *Echo Request* и *Echo Reply*, а также предварительные ARP-запросы, использовавшиеся для определения MAC-адресов.  
   Это подтверждает успешное взаимодействие между конечным устройством и маршрутизатором.

   ![Захват ICMP и ARP пакетов в Wireshark](Screenshot_14.png){ #fig:014 width=85% }

## Моделирование простейшей сети на базе маршрутизатора VyOS в GNS3

### Построение топологии и настройка устройств

1. В приложении **GNS3** создан новый проект. В рабочей области размещены устройства: коммутатор **msk-ahmedfarg-sw-01**, маршрутизатор **msk-ahmedfarg-gw-01** на базе **VyOS** и конечное устройство **PC1-ahmedfarg**.  
   Все элементы соединены между собой кабелями Ethernet, формируя простейшую сеть.

   ![Топология сети с маршрутизатором VyOS](Screenshot_15.png){ #fig:015 width=70% }

2. На ПК **PC1-ahmedfarg** через консоль **VPCS** была выполнена настройка IP-адреса:
   ```
   ip 192.168.1.10/24 192.168.1.1
   save
   show ip
   ```
   В результате узлу был присвоен адрес `192.168.1.10/24`, а шлюзом по умолчанию назначен `192.168.1.1`.

3. На маршрутизаторе **VyOS** после входа в систему выполнен переход в режим конфигурации командой `configure`.  
   Затем заданы имя устройства и IP-адрес интерфейса `eth0`:
   ```
   set system host-name msk-ahmedfarg-gw-01
   set interfaces ethernet eth0 address 192.168.1.1/24
   compare
   commit
   save
   ```
   Изменения применены и сохранены.

   ![Настройка маршрутизатора VyOS](Screenshot_16.png){ #fig:016 width=80% }

4. Для проверки корректности конфигурации маршрутизатора была выполнена команда `show interfaces`.  
   В выводе отображается активный интерфейс **eth0** с IP-адресом `192.168.1.1/24` и соответствующим MAC-адресом.

   ![Просмотр интерфейсов VyOS](Screenshot_17.png){ #fig:017 width=70% }

### Проверка связи и анализ трафика

5. Проверка связи между устройствами выполнена с помощью команды `ping 192.168.1.1` на **PC1-ahmedfarg**.  
   Устройство успешно получает ответы от маршрутизатора, что подтверждает правильность конфигурации сети.

   ![Проверка связи между ПК и маршрутизатором](Screenshot_18.png){ #fig:018 width=80% }

6. В **Wireshark** запущен захват пакетов на соединении между коммутатором и маршрутизатором.  
   Анализ показал корректный обмен **ARP** и **ICMP Echo Request/Reply** пакетами между устройствами.  
   Это демонстрирует, что сетевой стек функционирует правильно и маршрутизатор отвечает на запросы.

   ![Анализ ICMP и ARP пакетов в Wireshark](Screenshot_19.png){ #fig:019 width=85% }

# Заключение

В ходе работы была выполнена настройка и запуск виртуальной машины **GNS3**, а также добавлены и сконфигурированы образы маршрутизаторов **FRR** и **VyOS**.  
Построены и протестированы простейшие сетевые топологии с использованием коммутатора Ethernet и виртуальных ПК.  
Настроена IP-адресация, выполнена проверка связности между узлами и проведён анализ сетевого трафика с помощью **Wireshark**.  
