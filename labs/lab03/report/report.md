---
## Front matter
title: "Отчёт по лабораторной работе 3"
subtitle: "Анализ трафика в Wireshark"
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

Изучение посредством Wireshark кадров Ethernet, анализ PDU протоколов транспортного и прикладного уровней стека TCP/IP.

# Выполнение

# Выполнение

## Анализ кадров канального уровня в Wireshark

1. На устройство установлен и запущен **Wireshark**. Для анализа был выбран активный беспроводной сетевой интерфейс, начат захват трафика.

2. С помощью команды **ipconfig** в консоли Windows определён IP-адрес устройства и шлюз по умолчанию.  
   Устройство имеет адрес `192.168.212.42`, а шлюз по умолчанию — `192.168.212.16`.  

   ![Результат ipconfig](Screenshot_1.png){ #fig:001 width=80% }

3. Выполнена команда **ping** шлюза по умолчанию (`192.168.212.16`). На экране отобразились ответы от шлюза.  

4. В Wireshark был применён фильтр `arp or icmp`. В списке пакетов отобразились только пакеты ARP и ICMP.  

   ![Фильтр arp or icmp](Screenshot_2.png){ #fig:002 width=85% }

5. Анализ ICMP эхо-запроса:  
   - Длина кадра — 74 байта.  
   - Тип кадра Ethernet II.  
   - MAC-адрес источника: `Intel_6f:7b:ec (f8:fe:5e:6f:7b:ec)`.  
   - MAC-адрес шлюза: `72:18:c7:62:fa:43`.  
   - Тип MAC-адресов — индивидуальные (unicast).  

   ![ICMP Echo Request](Screenshot_3.png){ #fig:003 width=85% }

6. Анализ ICMP эхо-ответа:  
   - Длина кадра — 74 байта.  
   - Тип кадра Ethernet II.  
   - MAC-адрес источника: `72:18:c7:62:fa:43`.  
   - MAC-адрес получателя: `f8:fe:5e:6f:7b:ec`.  
   - Тип MAC-адресов — индивидуальные (unicast).  

   ![ICMP Echo Reply](Screenshot_4.png){ #fig:004 width=85% }

7. Анализ ARP-запроса:  
   - Длина кадра — 42 байта.  
   - Отправитель: `192.168.212.42`.  
   - Запрос: «Кто имеет IP 192.168.212.16?»  
   - Тип MAC-адресов — индивидуальные.  

   ![ARP Request](Screenshot_5.png){ #fig:005 width=85% }

8. Далее был выполнен ping внешнего узла `ya.ru`. Ответы пришли успешно, что подтверждает работоспособность соединения.  

   ![Ping ya.ru](Screenshot_6.png){ #fig:006 width=75% }

9. В Wireshark зафиксированы пакеты ICMP при обмене с внешним сервером (`5.255.255.242`). Отображены эхо-запросы и ответы, а также промежуточные ARP-запросы.  

   ![ICMP при ping ya.ru](Screenshot_7.png){ #fig:007 width=85% }  
   ![ICMP Reply от ya.ru](Screenshot_8.png){ #fig:008 width=85% }


## Анализ протоколов транспортного уровня в Wireshark

1. На устройстве был запущен **Wireshark**, выбран активный сетевой интерфейс и начат захват трафика.

2. В браузере был открыт сайт, работающий по протоколу **HTTP**. Для получения достаточного количества пакетов осуществлён переход по разделам сайта.

3. В Wireshark применён фильтр `http`. На экране отобразились HTTP-запросы и ответы, передаваемые по протоколу **TCP**:  
   - Зафиксирован запрос `GET /hypertext/WWW/TheProject.html`.  
   - Сервер ответил кодом `200 OK` и передал HTML-документ.  
   - Дополнительно передавались файлы: `favicon.ico` и графические данные.  
   - Для каждого пакета видны исходные и целевые порты TCP, порядковые и подтверждающие номера сегментов.  

   ![HTTP-запрос](Screenshot_9.png){ #fig:009 width=85% }  
   ![HTTP-ответ](Screenshot_10.png){ #fig:010 width=85% }

4. Далее был применён фильтр `dns`. В захвате зафиксированы **UDP-пакеты** с DNS-запросами и ответами:  
   - Запросы на разрешение имени `yandex.ru` и других доменов.  
   - Ответы содержат несколько IP-адресов для одного доменного имени.  
   - Применялись стандартные порты: источник — динамический порт клиента, назначение — `53/UDP`.  

   ![DNS-запросы](Screenshot_11.png){ #fig:011 width=85% }  
   ![DNS-ответы](Screenshot_12.png){ #fig:012 width=85% }

5. После этого был применён фильтр `quic`. В списке пакетов отобразились сессии по протоколу **QUIC** (поверх UDP):  
   - Видны начальные пакеты соединения `Initial` с идентификаторами сессий.  
   - Присутствуют зашифрованные сегменты `Protected Payload`.  
   - Передача данных осуществляется через порт `443/UDP`.  
   - QUIC обеспечивает функции транспортного уровня, аналогичные TCP, но с более высокой производительностью.  

   ![QUIC Initial](Screenshot_13.png){ #fig:013 width=85% }  
   ![QUIC Payload](Screenshot_14.png){ #fig:014 width=85% }

6. Захват трафика был завершён.

## Анализ handshake протокола TCP в Wireshark

1. На устройстве был запущен **Wireshark**, выбран активный сетевой интерфейс и начат захват пакетов.

2. Для анализа установленного соединения было выполнено обращение к сайту по протоколу **HTTP**. В процессе обмена зафиксированы пакеты TCP, включая этап установки соединения (handshake).

3. Анализ TCP handshake:  
   - Первое сообщение (SYN) инициирует соединение, устанавливается начальный порядковый номер (Sequence Number).  
   - Второе сообщение (SYN, ACK) подтверждает приём SYN от клиента и устанавливает собственный порядковый номер.  
   - Третье сообщение (ACK) завершает процесс трёхстороннего рукопожатия, соединение считается установленным.  

   На скриншоте ниже виден процесс установления соединения по TCP, включая `SYN`, `SYN-ACK` и `ACK`:  

   ![TCP Handshake](Screenshot_15.png){ #fig:015 width=85% }

4. В ходе обмена были также зафиксированы HTTP-запросы и ответы поверх установленного TCP-соединения. Например:  
   - `GET /hypertext/WWW/TheProject.html HTTP/1.1`  
   - Ответ сервера: `HTTP/1.1 304 Not Modified`  

5. Для наглядности в меню **Статистика → График Потока** был построен график TCP-сессии. На нём отображается трёхстороннее рукопожатие:  
   - Первый сегмент `SYN` от клиента.  
   - Ответный сегмент `SYN, ACK` от сервера.  
   - Заключительный сегмент `ACK` от клиента.  
   После этого происходит передача HTTP-запроса и ответа.  

   ![График потока TCP](Screenshot_16.png){ #fig:016 width=85% }

6. Захват трафика был остановлен.


# Заключение

В ходе работы был проанализирован процесс установления соединения по протоколу TCP.  
С помощью Wireshark зафиксированы пакеты трёхстороннего рукопожатия (SYN, SYN-ACK, ACK), а также последующая передача данных HTTP.  
