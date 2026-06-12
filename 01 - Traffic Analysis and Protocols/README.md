# 🕵️‍♀️ Laboratorio 01: Traffic Analysis & Core Protocols

## 📌 Descripción de la Sección

Este directorio contiene entornos prácticos enfocados en la auditoría y análisis de tráfico de red. El objetivo principal es salir de la teoría del Modelo OSI y observar cómo se comportan los paquetes reales en las capas de Enlace, Red y Transporte utilizando herramientas de inspección profunda.

## 🗂️ Índice de Prácticas

* **[01 - Análisis de Capa 2: MAC y ARP](01%20-%20Analisis%20ARP%20y%20MAC.md):** Inspección de direccionamiento físico (MAC), funcionamiento de solicitudes/respuestas ARP en entornos de red local y análisis de tablas de vecinos.

* **[02 - Análisis y Diagnóstico de Red con ICMP](02%20-%20Análisis%20y%20Diagnóstico%20de%20Red%20con%20ICMP):** Monitoreo de mensajes de control bidireccionales, rastreo de saltos lógicos mediante la manipulación del TTL (`traceroute`) y diagnóstico estructurado de fallas (interpretación de mensajes *Destination Unreachable*).

* **[03 - Análisis de Capa de Transporte: TCP vs UDP](03%20-%20Análisis%20de%20Capa%20de%20Transporte:%20TCP%20vs%20UDP):** Comparativa de protocolos en entornos reales. Captura del *Three-Way Handshake* de TCP hacia un servidor web local (Apache2), análisis del control de flujo (Window Size) y rutinas de finalización, contrastado con el tráfico *stateless* de UDP en resoluciones DNS.

## 🛠️ Herramientas Empleadas
* **Analizador de Protocolos:** Wireshark en modo promiscuo.
* **Comandos de Diagnóstico:** `ping`, `traceroute`, `arp`, `route`, utilidades de la suite `iproute2`.
* **Entorno y Servicios Simulados:** Ubuntu Server, Apache2 (HTTP), consultas de resolución DNS.

