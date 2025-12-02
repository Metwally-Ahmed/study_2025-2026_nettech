---
## Front matter
title: "Отчёт по лабораторной работе 7"
subtitle: " Адресация IPv4 и IPv6. Настройка DHCP"
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

Получение навыков настройки службы DHCP на сетевом оборудовании для распределения адресов IPv4 и IPv6.

# Выполнение

## Настройка маршрутизатора VyOS и DHCP-сервера в GNS3

### Построение топологии

1. В рабочем поле **GNS3** размещены и соединены устройства согласно требуемой топологии: оконечный хост **PC1-ahmedfarg**, коммутатор **ahmedfarg-sw-01** и маршрутизатор **ahmedfarg-gw-01**.  
   Все интерфейсы переведены во включённое состояние.

   ![Топология сети](Screenshot_1.png){ #fig:001 width=60% }

### Базовая настройка VyOS

2. На маршрутизаторе **VyOS** выполнена установка образа и первичная конфигурация. После загрузки выполнен переход в режим конфигурирования.  
   Присвоены имя хоста и доменное имя.

   ![Настройка hostname и домена](Screenshot_2.png){ #fig:002 width=80% }

3. Создан новый пользователь **ahmedfarg** с паролем, после чего изменения сохранены командами *commit* и *save*.

### Настройка интерфейсов и DHCP-сервера

4. На интерфейсе **eth0** маршрутизатора настроен IPv4-адрес **10.0.0.1/24**.

5. Выполнена конфигурация DHCP-сервера:
   - указано доменное имя,
   - добавлен DNS-сервер,
   - создана shared-network,
   - настроена подсеть **10.0.0.0/24**,
   - задан диапазон выдачи адресов: **10.0.0.2 — 10.0.0.253**.

   ![Настройка DHCP-сервера](Screenshot_3.png){ #fig:003 width=85% }

### Получение IPv4-адреса клиентом PC1

6. На хосте **PC1-ahmedfarg** выполнена команда получения IP-адреса через DHCP.  
   Клиент получил адрес **10.0.0.2/24**, шлюз **10.0.0.1**, DNS-сервер **10.0.0.1**, доменное имя **ahmedfarg.net**.  
   На экране отображены декодированные сообщения DHCP: Discover → Offer → Request → ACK.

   ![Декодированные DHCP-пакеты на PC1](Screenshot_4.png){ #fig:004 width=85% }

### Проверка сетевой конфигурации и связности

7. Просмотр параметров IPv4 на PC1 показал корректную конфигурацию.  
   Выполненная проверка `ping 10.0.0.1 -c 2` подтвердила успешный обмен пакетами.

   ![Проверка IP-состояния и ping](Screenshot_5.png){ #fig:005 width=85% }

### Статистика и выданные адреса на DHCP-сервере

8. На маршрутизаторе просмотрены статистика и список выданных адресов.  
   Пул содержит 252 адреса, один из которых выдан устройству **PC1**.

   ![Статистика и leases DHCP](Screenshot_6.png){ #fig:006 width=85% }

9. Просмотр журнала DHCP-сервера подтвердил последовательность сообщений DHCPDISCOVER, DHCPOFFER, DHCPREQUEST и DHCPACK.

   ![Журнал DHCP-сервера](Screenshot_7.png){ #fig:007 width=85% }

### Анализ DHCP-трафика в Wireshark

10. Анализ трафика на канале между коммутатором и маршрутизатором показал корректный процесс назначения адреса:
    - клиент инициирует *DHCP Discover*,
    - сервер отправляет *Offer*,
    - клиент направляет *Request*,
    - сервер подтверждает выдачу адреса *ACK*,
    - клиент отправляет Gratuitous ARP для проверки уникальности IP.

    В сообщениях DHCP детализированы параметры: маска, шлюз, DNS, доменное имя, время аренды.

    ![DHCP-пакеты в Wireshark](Screenshot_8.png){ #fig:008 width=95% }

## Настройка IPv6-адресации и DHCPv6 Stateless в GNS3

### Построение топологии

1. В рабочем пространстве GNS3 сеть была дополнена новыми устройствами согласно топологии: к маршрутизатору **ahmedfarg-gw-01** добавлены коммутаторы **ahmedfarg-sw-02**, **ahmedfarg-sw-03**, а также два узла — **PC2-ahmedfarg** и **PC3-ahmedfarg** на базе Kali Linux CLI.  
   Все устройства подключены в соответствии с требуемой схемой.

   ![Топология](Screenshot_9.png){ #fig:009 width=70% }

### Настройка IPv6 на маршрутизаторе

2. На маршрутизаторе **ahmedfarg-gw-01** в режиме конфигурирования настроены IPv6-адреса на интерфейсах `eth1` и `eth2`:

- `eth1`: `2000::1/64`
- `eth2`: `2001::1/64`

   После задания параметров выполнены просмотр интерфейсов, commit и сохранение конфигурации.

   ![Настройка интерфейсов IPv6](Screenshot_10.png){ #fig:010 width=80% }

### Настройка DHCPv6 (Stateless) и RA

3. Выполнена настройка Router Advertisements (RA) на интерфейсе `eth1`.  
   Установлен префикс `2000::/64` и активирован флаг `other-config-flag`, указывающий, что информация, кроме адресов, предоставляется через DHCPv6.

4. Создана shared-network **ahmedfarg-stateless** для DHCPv6-сервера.  
   Заданы общие параметры — DNS-сервер (`2000::1`) и доменное имя (`ahmedfarg.net`).

   ![Настройка RA и DHCPv6 Stateless](Screenshot_11.png){ #fig:011 width=80% }

5. Конфигурация DHCPv6 и RA отображена командой просмотра настроек.

   ![Вывод конфигурации](Screenshot_12.png){ #fig:012 width=80% }

### Проверка сетевой конфигурации на PC2

6. На узле **PC2-ahmedfarg** отображены IPv6-интерфейсы и таблица маршрутизации.  
   На этом этапе адрес от RA получен не был, маршрутизация отсутствовала.

   ![Начальная конфигурация IPv6 на PC2](Screenshot_13.png){ #fig:013 width=80% }

7. Попытка выполнить ping `2000::1` завершается ошибкой — отсутствие маршрута.

### Получение настроек по DHCPv6 Stateless

8. На PC2 выполнен запрос параметров DHCPv6.  
   Клиент получил конфигурационную информацию (DNS, домен), но без назначения IPv6-адреса, что соответствует Stateless-режиму.

   ![Работа DHCPv6 client](Screenshot_14.png){ #fig:014 width=80% }

9. После получения параметров интерфейс PC2 получил IPv6-адрес по SLAAC с префиксом `2000::/64`.  
   Маршруты обновлены, появилась дефолтная запись.  

   ![Обновлённая конфигурация и ping](Screenshot_15.png){ #fig:015 width=80% }

10. Выполнена проверка DNS — файл `/etc/resolv.conf` содержит данные, выданные DHCPv6 (`nameserver 2000::1`, `search ahmedfarg.net`).

### Проверка DHCPv6 на маршрутизаторе

11. На маршрутизаторе выполнен просмотр выдачи адресов DHCPv6.  
    Так как Stateless DHCPv6 **не назначает IP-адреса**, таблица leases пуста — поведение корректное.

   ![DHCPv6 leases](Screenshot_16.png){ #fig:016 width=60% }

### Анализ DHCPv6 трафика в Wireshark

12. На захваченном трафике отображена работа DHCPv6:

- Neighbor Solicitation/Advertisement — получение SLAAC-адреса;  
- DHCPv6 Information-Request — клиент запрашивает только параметры;  
- DHCPv6 Reply — сервер передаёт DNS и domain-search;  
- Присутствуют ICMPv6 RA-пакеты от маршрутизатора (`prefix 2000::/64`).

   В DHCPv6 Reply содержатся:
   - Client Identifier (DUID),  
   - Server Identifier,  
   - DNS recursive name server,  
   - Domain Search List.

   ![DHCPv6 трафик Wireshark](Screenshot_17.png){ #fig:017 width=95% }

## Настройка DHCPv6 Stateful в GNS3

### Конфигурация маршрутизатора для DHCPv6 Stateful

13. На маршрутизаторе **ahmedfarg-gw-01** выполнена настройка DHCPv6 с отслеживанием состояния (Stateful).  
    На интерфейсе `eth2` включён флаг `managed-flag`, уведомляющий узлы о необходимости получения IPv6-адреса по DHCPv6.

    Далее создана shared-network **ahmedfarg-stateful** с подсетью `2001::/64`, а также:
    - указан DNS-сервер `2001::1`;
    - добавлен домен `ahmedfarg.net`;
    - задан диапазон адресов `2001::100 — 2001::199`.

    ![Настройка DHCPv6 Stateful](Screenshot_18.png){ #fig:018 width=85% }

### Проверка настройки на PC3 до получения адреса

14. На маршрутизаторе был просмотрен список выданных адресов DHCPv6, в данный момент пустой.

15. На узле **PC3-ahmedfarg** были изучены текущие сетевые параметры IPv6: интерфейс имеет лишь локальные IPv6-адреса, глобальный адрес отсутствует; маршрут по умолчанию не настроен.

    ![Начальные параметры IPv6 PC3](Screenshot_19.png){ #fig:019 width=80% }

16. Файл `/etc/resolv.conf` до получения настроек DHCPv6 содержит только базовые параметры.

### Получение IPv6-адреса по DHCPv6 Stateful

17. На PC3 выполнен запрос полноценной stateful-конфигурации DHCPv6.  
    Клиент получил адрес из диапазона **2001::198**, а также параметры DHCPv6 — DNS и доменное имя.

    ![Получение stateful DHCPv6 настроек](Screenshot_20.png){ #fig:020 width=85% }

### Проверка настроек после получения DHCPv6

18. После получения адреса по DHCPv6 интерфейс PC3 получил глобальный адрес в сети `2001::/64`, маршрут по умолчанию был обновлён, появились корректные записи DNS.  
    Проверка связности с маршрутизатором показала успешный отклик.

    ![Параметры IPv6 и маршрутизация после DHCPv6](Screenshot_21.png){ #fig:021 width=85% }

    Файл `/etc/resolv.conf` обновлён DHCPv6-сервером и содержит:
    - search: `ahmedfarg.net`
    - nameserver: `2001::1`

### Просмотр выданных адресов на маршрутизаторе

19. На маршрутизаторе вновь просмотрен список DHCPv6 leases.  
    Теперь таблица содержит выданные адреса, включая адрес PC3:

    - `2001::198` — активный DHCPv6 stateful-адрес
    - `2001::199` — также активный адрес (выдан второму устройству)

    ![Полученные DHCPv6 leases](Screenshot_22.png){ #fig:022 width=85% }

### Анализ DHCPv6 Stateful в Wireshark

20. На захваченном трафике видно корректное прохождение DHCPv6 stateful-процедуры:

- Solicit — клиент запрашивает параметры и адрес;
- Advertise — сервер предлагает доступный адрес;
- Request — клиент запрашивает конкретный адрес;
- Reply — сервер подтверждает назначение IPv6-адреса и всех опций.

    В DHCPv6 Reply содержатся:
    - Client Identifier (DUID);
    - Server Identifier;
    - Identity Association for Non-temporary Address (IA_NA), включая назначенный адрес (`2001::199`);
    - DNS recursive name server (`2001::1`);
    - Domain Search List (`ahmedfarg.net`).

    ![DHCPv6 Stateful трафик](Screenshot_23.png){ #fig:023 width=95% }

# Заключение

В ходе работы была развернута виртуальная среда **GNS3**, добавлены маршрутизаторы **FRR** и **VyOS**, а также создана и протестирована базовая топология с коммутатором и виртуальными ПК.
Настроена IP-адресация, проверена связность между узлами, выполнен анализ трафика в **Wireshark**.
