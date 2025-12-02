---
## Front matter
lang: ru-RU
title: Сетевые технологии
subtitle: Лабораторная работа №7  
author:
  - Метвалли Ахмед Фарг Набеех
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 28 ноября 2025

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

## Основная цель

Получение навыков настройки DHCP для распределения адресов **IPv4** и **IPv6** в среде **GNS3**.

# Ход выполнения

## Топология сети

![Топология DHCPv4](Screenshot_1.png){ width=75% }

## Первичная настройка маршрутизатора VyOS

![Hostname и домен](Screenshot_2.png){ width=75% }

## Конфигурация DHCPv4 на маршрутизаторе

![Настройка DHCPv4](Screenshot_3.png){ width=80% }

## Клиент PC1 получает адрес по DHCP

![DHCP-пакеты PC1](Screenshot_4.png){ width=80% }

## Проверка IP и связности

![Ping PC1 → R1](Screenshot_5.png){ width=80% }

## Статистика DHCP-сервера

![Leases DHCPv4](Screenshot_6.png){ width=80% }

## Анализ DHCPv4 и ARP в Wireshark

![DHCPv4 Wireshark](Screenshot_8.png){ width=90% }

## Топология IPv6

![Топология IPv6](Screenshot_9.png){ width=75% }

## Настройка IPv6 интерфейсов

![IPv6 интерфейсы](Screenshot_10.png){ width=75% }

## Настройка Router Advertisements и DHCPv6 Stateless

![RA + DHCPv6 Stateless](Screenshot_11.png){ width=80% }

## Конфигурация RA/DHCPv6 на маршрутизаторе

![Статическая конфигурация](Screenshot_12.png){ width=80% }

## Начальные параметры IPv6 на PC2

![Начальные IPv6 PC2](Screenshot_13.png){ width=75% }

## Получение DHCPv6 Stateless параметров

![DHCPv6 Stateless клиент](Screenshot_14.png){ width=80% }

## SLAAC + DNS после DHCPv6

![SLAAC и маршруты PC2](Screenshot_15.png){ width=80% }

## DHCPv6 Stateless — leases

![Leases DHCPv6 Stateless](Screenshot_16.png){ width=60% }

## Анализ DHCPv6 Stateless в Wireshark

![DHCPv6 Stateless Wireshark](Screenshot_17.png){ width=90% }

## DHCPv6 Stateful на маршрутизаторе

![Конфигурация DHCPv6 Stateful](Screenshot_18.png){ width=80% }

## Начальное состояние PC3

![Начальные IPv6 PC3](Screenshot_19.png){ width=80% }

## Получение stateful IPv6-адреса

![DHCPv6 Stateful клиент](Screenshot_20.png){ width=80% }

## IPv6 после получения DHCPv6 адреса

![IPv6 и маршруты PC3](Screenshot_21.png){ width=80% }

## Список выданных IPv6-адресов

![Leases DHCPv6 Stateful](Screenshot_22.png){ width=80% }

## Анализ DHCPv6 Stateful в Wireshark

![DHCPv6 Stateful Wireshark](Screenshot_23.png){ width=90% }

# Заключение

Настроены службы **DHCPv4**, **DHCPv6 Stateless** и **DHCPv6 Stateful** в виртуальной среде **GNS3**.  
Все клиенты успешно получили адреса и сетевые параметры.  
Анализ трафика в **Wireshark** подтвердил корректность работы протоколов DHCP и ICMP в сетях IPv4 и IPv6.

