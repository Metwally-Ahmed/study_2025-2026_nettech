---
## Front matter
lang: ru-RU
title: Сетевые технологии
subtitle: Лабораторная работа №5  
author:
  - Метвалли Ахмед Фарг Набеех
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 30 октября 2025

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

Построение простейших моделей сети на базе коммутатора и маршрутизаторов **FRR** и **VyOS** в GNS3, а также анализ сетевого трафика с помощью **Wireshark**.

# Моделирование сети на базе коммутатора

## Построение топологии

![Топология сети в GNS3](Screenshot_1.png){ width=80% }

## Настройка IP-адресов

![Проверка связи](Screenshot_3.png){ width=80% }

## Анализ трафика ARP и ICMP

![ARP-пакеты в Wireshark](Screenshot_4.png){ width=80% }

## Анализ трафика ARP и ICMP

![ICMP-пакеты в Wireshark](Screenshot_5.png){ width=80% }

# Моделирование сети на базе маршрутизатора FRR

## Топология сети

![Топология FRR](Screenshot_9.png){ width=80% }

## Настройка маршрутизатора FRR

![Конфигурация FRR](Screenshot_11.png){ width=80% }

## Проверка связи и анализ ICMP

![Проверка связи с FRR](Screenshot_13.png){ width=80% }

## Проверка связи и анализ ICMP

![Захват пакетов Wireshark](Screenshot_14.png){ width=80% }

# Моделирование сети на базе маршрутизатора VyOS

## Топология сети

![Топология VyOS](Screenshot_15.png){ width=80% }

## Настройка маршрутизатора VyOS

![Настройка VyOS](Screenshot_16.png){ width=80% }

## Проверка связи и анализ ICMP

![Ping с ПК к VyOS](Screenshot_18.png){ width=80% }

## Проверка связи и анализ ICMP

![Анализ ICMP и ARP в Wireshark](Screenshot_19.png){ width=80% }

# Вывод

Оба маршрутизатора — **FRR** и **VyOS** — успешно функционируют, обеспечивая корректный обмен пакетами в локальной сети.  
Полученные результаты подтверждают правильность конфигурации и работу сетевых протоколов в среде **GNS3**.
