---
## Front matter
lang: ru-RU
title: Сетевые технологии
subtitle: Лабораторная работа №3
author:
  - Метвалли Ахмед Фарг Набеех
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 3 октября 2025

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цели и задачи работы

## Цель лабораторной работы

Изучить работу Wireshark и провести анализ кадров Ethernet, пакетов ICMP/ARP, а также транспортных протоколов TCP, UDP, QUIC.

# Выполнение лабораторной работы

## Анализ кадров канального уровня

![Результат ipconfig](Screenshot_1.png){ #fig:001 width=80% }

## Ping и фильтрация трафика

![Фильтр arp or icmp](Screenshot_2.png){ #fig:002 width=80% }

## ICMP запрос

![ICMP Echo Request](Screenshot_3.png){ #fig:003 width=80% }

## ICMP ответ

![ICMP Echo Reply](Screenshot_4.png){ #fig:004 width=80% }

## ARP-запрос

![ARP Request](Screenshot_5.png){ #fig:005 width=80% }

## Ping внешнего узла

![Ping ya.ru](Screenshot_6.png){ #fig:006 width=80% }

## Анализ ICMP при ping

![ICMP при ping ya.ru](Screenshot_7.png){ #fig:007 width=80% }

# Анализ транспортного уровня

## HTTP-запрос

![HTTP-запрос](Screenshot_9.png){ #fig:009 width=80% }

## HTTP-ответ

![HTTP-ответ](Screenshot_10.png){ #fig:010 width=80% }

## DNS-запросы

![DNS-запросы](Screenshot_11.png){ #fig:011 width=80% }

## DNS-ответы

![DNS-ответы](Screenshot_12.png){ #fig:012 width=80% }

## QUIC Initial

![QUIC Initial](Screenshot_13.png){ #fig:013 width=80% }

## QUIC Payload

![QUIC Payload](Screenshot_14.png){ #fig:014 width=80% }

# TCP Handshake

## Трёхстороннее рукопожатие

![TCP Handshake](Screenshot_15.png){ #fig:015 width=80% }

## График потока TCP

![График потока TCP](Screenshot_16.png){ #fig:016 width=80% }

# Выводы по работе

## Вывод

В ходе работы были проанализированы кадры Ethernet, пакеты ARP и ICMP, протоколы транспортного уровня (HTTP, DNS, QUIC), а также процесс установления соединения TCP.  
Wireshark подтвердил корректность работы сетевых протоколов и позволил отследить их взаимодействие.
