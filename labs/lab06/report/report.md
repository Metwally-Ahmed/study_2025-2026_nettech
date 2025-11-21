---
## Front matter
title: "Отчёт по лабораторной работе 6"
subtitle: "Адресация IPv4 и IPv6. Двойной стек"
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

Изучение принципов распределения и настройки адресного пространства на устройствах сети.

# Выполнение

## Настройка сети с двойным стеком IPv4/IPv6 в GNS3

### Построение топологии и подготовка проекта

1. Запущено приложение GNS3 и создан новый проект.  
   На рабочей области размещена топология, включающая маршрутизаторы **msk-ahmedfarg-gw-01**, **msk-ahmedfarg-gw-02**, коммутаторы, узлы **PC1**, **PC2**, **Server**.

   ![Топология сети](Screenshot_1.png){ #fig:001 width=75% }

2. Устройствам присвоены имена согласно шаблону:
   - коммутаторы: `msk-ahmedfarg-sw-0x`
   - маршрутизаторы: `msk-ahmedfarg-gw-0x`
   - VPCS: `PCx-ahmedfarg`

3. Между сервером и ближайшим коммутатором включён захват трафика.

### Настройка IPv4 на PC1, PC2 и Server

1. На **PC1** задан адрес и шлюз по умолчанию:  
   `ip 172.16.20.10/25 172.16.20.1` → `save`  
   Просмотр конфигурации IPv4 и IPv6 выполнен командами `show ip` и `show ipv6`.

   ![PC1 – конфигурация](Screenshot_2.png){ #fig:002 width=70% }

2. На **PC2** выполнена аналогичная настройка:  
   `ip 172.16.20.138/25 172.16.20.129` → `save`

   ![PC2 – конфигурация](Screenshot_3.png){ #fig:003 width=70% }

3. На **Server** настроен IPv4-адрес:  
   `ip 64.100.1.10/24 64.100.1.1` → `save`

   ![Server – конфигурация](Screenshot_4.png){ #fig:004 width=70% }

### Настройка IPv4 на маршрутизаторе FRR

1. На маршрутизаторе **msk-ahmedfarg-gw-01** выполнено присвоение IP-адресов интерфейсам:

   - интерфейс `eth0` → `172.16.20.1/25`
   - интерфейс `eth1` → `172.16.20.129/25`
   - интерфейс `eth2` → `64.100.1.1/24`

   Конфигурация сохранена командой `write memory`.

   ![Конфигурация интерфейсов](Screenshot_5.png){ #fig:005 width=70% }

2. Проверен вывод текущей конфигурации (`show running-config`) и состояния интерфейсов (`show interface brief`):

   ![Running-config](Screenshot_6.png){ #fig:006 width=70% }
   ![show interface brief](Screenshot_7.png){ #fig:007 width=70% }

### Проверка сетевой связности

1. На **PC1** выполнены проверки:
   - `ping` до PC2
   - `ping` до сервера
   - `trace` до обоих узлов

   ![Ping и trace на PC1](Screenshot_8.png){ #fig:008 width=85% }

2. **PC2** также успешно отправляет эхо-запросы на PC1 и сервер:

   ![Ping на PC2](Screenshot_9.png){ #fig:009 width=70% }

## Настройка адресации IPv6 и проверка работы Dual Stack

### IPv6 на узлах PC3, PC4 и Server

1. Назначение адресов:
   - **PC3** — `2001:db8:c0de:12::a/64`
   - **PC4** — `2001:db8:c0de:13::a/64`
   - **Server** — `2001:db8:c0de:11::a/64`

2. Просмотр конфигурации:
   - **PC3**: `show ip`, `show ipv6`  
     ![PC3 – IPv4/IPv6](Screenshot_10.png){ #fig:010 width=70% }
   - **PC4**: `show ip`, `show ipv6`  
     ![PC4 – IPv4/IPv6](Screenshot_11.png){ #fig:011 width=70% }
   - **Server**: `show ip`, `show ipv6`  
     ![Server – IPv4/IPv6](Screenshot_12.png){ #fig:012 width=70% }

### Конфигурация маршрутизатора VyOS (msk-ahmedfarg-gw-02)

1. Установка образа выполнена через диалог `install image` и перезагрузка устройства.
2. Установлено имя хоста `msk-user-gw-02` (в работе использовано имя `msk-ahmedfarg-gw-02`).
3. Назначены IPv6-адреса интерфейсам и включены RA:
   - `eth0` → `2001:db8:c0de:12::1/64`, анонс префикса `2001:db8:c0de:12::/64`
   - `eth1` → `2001:db8:c0de:13::1/64`, анонс префикса `2001:db8:c0de:13::/64`
   - `eth2` → `2001:db8:c0de:11::1/64`, анонс префикса `2001:db8:c0de:11::/64`

   ![VyOS – ввод команд конфигурации](Screenshot_13.png){ #fig:013 width=80% }

4. Фиксация и сохранение конфигурации, проверка параметров интерфейсов:
   ![VyOS – commit/save и show interfaces](Screenshot_14.png){ #fig:014 width=65% }

### Проверка доступности по IPv6

1. **PC3**:
   - `ping` до `2001:db8:c0de:13::a` (PC4) — успешно  
   - `ping` до `2001:db8:c0de:11::a` (Server) — успешно  
   ![PC3 – ping по IPv6](Screenshot_15.png){ #fig:015 width=70% }

2. **Server**:
   - `ping` до `172.16.20.10` (PC1 по IPv4) — успешно  
   - `ping` до `2001:db8:c0de:13::a` (PC4 по IPv6) — успешно  
   ![Server – ping IPv4 и IPv6](Screenshot_16.png){ #fig:016 width=70% }

### Разделение доменов IPv4 и IPv6

- Узлы только с IPv6 (**PC3**, **PC4**) **не достигают** узлов только с IPv4 (**PC1**, **PC2**), что подтверждает отсутствие межстековой маршрутизации без Dual Stack.
- Сервер двойного стека (**Server**) успешно общается с обеими подсетями:  
  по IPv4 — с `172.16.20.0/25`, по IPv6 — с `2001:db8:c0de::/48`.

### Анализ захваченного трафика на линке «Server ↔ коммутатор»

1. **ARP (IPv4)**  
   В кадрах ARP видны:
   - MAC-адрес отправителя (например, MAC маршрутизатора/сервера)  
   - IPv4-адрес отправителя (`64.100.1.1` или `64.100.1.10`)  
   - Запрашиваемый IPv4-адрес назначения (кто «имеет» `64.100.1.10`)  
   Это позволяет определить соответствие IP↔MAC в подсети и увидеть, кто инициирует разрешение адресов.

   ![Wireshark – ARP и ICMPv4](Screenshot_17.png){ #fig:017 width=90% }

2. **ICMP (IPv4)**  
   Для эхо-запросов/ответов отображаются:
   - Пара IP-адресов источника/назначения (`64.100.1.10` ↔ `172.16.20.10`)  
   - Поля ICMP (тип/код), идентификатор, номер последовательности  
   - TTL/Time to live на уровне IP  
   Эти данные подтверждают успешную маршрутизацию между подсетями IPv4 и позволяют оценить задержку/путь.

   ![Wireshark – ICMPv4 кадр детально](Screenshot_18.png){ #fig:018 width=85% }

3. **ICMPv6 (IPv6)**  
   В захвате видно:
   - Источник/назначение IPv6 (например, `2001:db8:c0de:12::1a` → `2001:db8:c0de:11::a`)  
   - Поля ICMPv6 (Echo Request/Reply), «Hop Limit» (аналог TTL)  
   - Наличие **Router Advertisement** от VyOS, что подтверждает рассылку префиксов и автоконфигурацию SLAAC
   Эти пакеты позволяют убедиться в корректной работе IPv6-сегмента, RA и эхо-обмене между узлами.

   ![Wireshark – ICMPv6 и RA](Screenshot_19.png){ #fig:019 width=90% }


## Задание для самостоятельного выполнения

Топология с двумя локальными подсетями и маршрутизатором VyOS, разделяющим сегменты IPv4 и IPv6.

![Топология сети](Screenshot_20.png){ #fig:020 width=70% }

---

### Характеристика подсетей

IPv4
- **Подсеть 1:** `10.10.1.96/27`  
  Диапазон хостов: `10.10.1.97–10.10.1.126`, широковещательный: `10.10.1.127`, минимальный адрес (шлюз): `10.10.1.97`.
- **Подсеть 2:** `10.10.1.16/28`  
  Диапазон хостов: `10.10.1.17–10.10.1.30`, широковещательный: `10.10.1.31`, минимальный адрес (шлюз): `10.10.1.17`.

IPv6
- **Подсеть 1:** `2001:db8:1:1::/64`  
  Хосты: `2001:db8:1:1::1 – 2001:db8:1:1:ffff:ffff:ffff:ffff`, минимальный адрес (маршрутизатор): `2001:db8:1:1::1`.
- **Подсеть 2:** `2001:db8:1:4::/64`  
  Хосты: `2001:db8:1:4::1 – 2001:db8:1:4:ffff:ffff:ffff:ffff`, минимальный адрес (маршрутизатор): `2001:db8:1:4::1`.


### Таблица адресации

| Устройство | Интерфейс | IPv4/Mask        | Шлюз IPv4   | IPv6/Prefix              | Примечание |
|---|---|---|---|---|---|
| msk-ahmedfarg-gw-01 | eth0 | 10.10.1.97/27  | — | 2001:db8:1:1::1/64 | Подсеть 1 |
| msk-ahmedfarg-gw-01 | eth1 | 10.10.1.17/28  | — | 2001:db8:1:4::1/64 | Подсеть 2 |
| PC1-ahmedfarg | e0 | 10.10.1.100/27 | 10.10.1.97 | 2001:db8:1:1::a/64 | Хост в подсети 1 |
| PC2-ahmedfarg | e0 | 10.10.1.20/28  | 10.10.1.17 | 2001:db8:1:4::a/64 | Хост в подсети 2 |

Скриншоты подтверждения настроек хостов:
- PC1: `show ip`, `show ipv6` → ![PC1 – конфигурация](Screenshot_21.png){ #fig:021 width=60% }
- PC2: `show ip`, `show ipv6` → ![PC2 – конфигурация](Screenshot_22.png){ #fig:022 width=60% }

### Настройка IP на маршрутизаторе и хостах

VyOS (msk-ahmedfarg-gw-01)
- Назначены адреса интерфейсам и включены Router Advertisements для обоих префиксов:
  - `eth0` → `10.10.1.97/27`, `2001:db8:1:1::1/64`, RA для `2001:db8:1:1::/64`
  - `eth1` → `10.10.1.17/28`, `2001:db8:1:4::1/64`, RA для `2001:db8:1:4::/64`

![VyOS – ввод конфигурации](Screenshot_23.png){ #fig:023 width=70% }  
![VyOS – commit/save и show interfaces](Screenshot_24.png){ #fig:024 width=60% }

Хосты
- **PC1:** IPv4 `10.10.1.100/27` gw `10.10.1.97`; IPv6 `2001:db8:1:1::a/64`.
- **PC2:** IPv4 `10.10.1.20/28`  gw `10.10.1.17`; IPv6 `2001:db8:1:4::a/64`.

### Проверка связности

С PC1
- `ping` до `10.10.1.20` — успешно.  
- `trace` до `10.10.1.20` — маршрут через `10.10.1.97`.  
- `ping` до `2001:db8:1:4::a` — успешно.  
- `trace` до `2001:db8:1:4::a` — маршрут через `2001:db8:1:1::1`.

![PC1 – ping/trace v4 и v6](Screenshot_25.png){ #fig:025 width=70% }

Аналогичные проверки с PC2 дают успешные ответы, что подтверждает корректную маршрутизацию между подсетями по IPv4 и IPv6.

# Заключение

В ходе лабораторной работы была развёрнута и настроена среда моделирования сетей с использованием приложения **GNS3** и виртуальной машины **GNS3 VM**.  
Были добавлены и сконфигурированы сетевые устройства: маршрутизаторы **FRR** и **VyOS**, коммутаторы и виртуальные ПК (**VPCS**).  

Полученные результаты подтвердили корректность работы маршрутизации между подсетями и функционирование механизма **Dual Stack**, при котором один хост может взаимодействовать как в IPv4-, так и в IPv6-сетях.  
Цель лабораторной работы достигнута.
