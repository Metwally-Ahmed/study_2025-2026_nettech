---
## Front matter
lang: ru-RU
title: Сетевые технологии
subtitle: Лабораторная работа №6 
author:
  - Метвалли Ахмед Фарг Набеех
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 10 ноября 2025

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


# Цель работы

## Постановка задачи

- Построить топологии
- Настроить IPv4 / IPv6
- Проверить связность
- Проанализировать трафик Wireshark

# Выполнение 

## Топология

![Топология](Screenshot_1.png){width=90%}

## Адресация IPv4

| Устройство | IP / Mask | Gateway |
|------------|------------|----------|
| PC1        | 172.16.20.10/25 | 172.16.20.1 |
| PC2        | 172.16.20.138/25 | 172.16.20.129 |
| Server     | 64.100.1.10/24 | 64.100.1.1 |

## Настройка IPv4 на PC1

![PC1 IPv4](Screenshot_2.png){width=90%}

## Настройка IPv4 на PC2

![PC2 IPv4](Screenshot_3.png){width=90%}

## Настройка IPv4 на сервере

![Server IPv4](Screenshot_4.png){width=90%}

## Настройка IPv4 на маршрутизаторе FRR

![FRR config](Screenshot_5.png){width=85%}

## Просмотр конфигурации FRR

![FRR running config](Screenshot_6.png){width=85%}

## Проверка связности IPv4

![Ping PC1-PC2 / Server](Screenshot_8.png){width=90%}

## Адресация IPv6

| Устройство | IPv6 / Prefix |
|------------|----------------|
| PC3        | 2001:db8:c0de:12::a/64 |
| PC4        | 2001:db8:c0de:13::a/64 |
| Server     | 2001:db8:c0de:11::a/64 |

## IPv6 на PC3

![PC3 IPv6](Screenshot_10.png){width=90%}

## IPv6 на PC4

![PC4 IPv6](Screenshot_11.png){width=90%}

## IPv6 на сервере

![Server IPv6](Screenshot_12.png){width=90%}

## IPv6 настройка на VyOS

![VyOS config](Screenshot_13.png){width=90%}

## Проверка интерфейсов VyOS

![VyOS interfaces](Screenshot_14.png){width=85%}

## Проверка связности IPv6

![Ping IPv6 PC3-PC4](Screenshot_15.png){width=90%}

## Ping IPv4 и IPv6 на сервере Dual Stack

![Ping dual-stack server](Screenshot_16.png){width=90%}

## Wireshark — ARP / ICMP / ICMPv6

![Wireshark IPv4](Screenshot_17.png){width=100%}

## Wireshark — ICMP IPv4

![Wireshark ICMP](Screenshot_18.png){width=100%}

## Wireshark — ICMPv6 + RA

![Wireshark ICMPv6](Screenshot_19.png){width=100%}

## Самостоятельное задание

![New topology](Screenshot_20.png){width=90%}

## IPv4 и IPv6 PC1 в задании

![PC1 config](Screenshot_21.png){width=90%}

## IPv4 и IPv6 PC2 в задании

![PC2 config](Screenshot_22.png){width=90%}

## Конфигурация маршрутизатора VyOS

![VyOS config](Screenshot_23.png){width=90%}

## Интерфейсы VyOS

![VyOS interfaces](Screenshot_24.png){width=90%}

## Проверка ping/trace

![ping trace result](Screenshot_25.png){width=90%}

# Итоги

## Вывод

- Выполнена настройка IPv4, IPv6 и Dual Stack
- Устройства связаны в обеих подсетях
- Сервер Dual Stack доступен по обоим протоколам
- Wireshark подтвердил работу ARP, ICMP, ICMPv6

