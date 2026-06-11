# 🌐 Enterprise Network Architecture & Protocol Analysis

¡Hola! 👩‍💻. Este repositorio documenta mis laboratorios prácticos y entornos de prueba enfocados en el diseño, configuración y resolución de problemas (troubleshooting) de infraestructuras de red.

El objetivo de este repositorio es demostrar mis habilidades prácticas en las capas 2, 3 y 4 del modelo OSI, utilizando tanto equipamiento propietario (Cisco) como herramientas open-source (Linux, Wireshark).

## 🛠️ Stack Tecnológico y Herramientas
* **OS & Networking:** Ubuntu/Alpine Linux (Netplan, iproute2, tcpdump).
* **Routing & Switching:** Cisco IOS, Packet Tracer, MikroTik (Conceptos).
* **Análisis de Tráfico:** Wireshark (Filtros, análisis de payload, TCP/UDP/ICMP).
* **Protocolos Dominados:** TCP/IP, ARP, ICMP, RIP, OSPF, DNS, DHCP.

---

## 📂 Estructura del Laboratorio

### 1. [Traffic Analysis & Core Protocols](./01-Traffic-Analysis-and-Proto)
Análisis profundo del comportamiento de los paquetes en la red para identificar cuellos de botella y comprender la comunicación cliente-servidor.
* Captura e inspección del *Three-Way Handshake* (TCP) y sesiones UDP.
* Resolución de direcciones MAC e IP mediante **ARP** y análisis de tablas de vecinos (`ip neigh`).
* Diagnóstico avanzado con **ICMP** (`ping`, `traceroute`) e interpretación de mensajes *Destination Unreachable*.
* Identificación de dominios de colisión y difusión (Broadcast/Multicast/Unicast).

### 2. [Cisco Routing & Infrastructure](./02-Cisco-Routing-Infrastructure)
Diseño de topologías LAN/WAN y configuración de routers mediante Cisco CLI.
* Implementación de **Rutas Estáticas** y configuración de *Default Gateways*.
* Despliegue de **Ruteo Dinámico (RIP)** y convergencia de red ante fallas de enlaces.
* Administración de Cisco IOS: RAM, NVRAM, Flash, y modos de configuración global/privilegiado.
* Análisis y construcción manual de Tablas de Ruteo.

### 3. [IP Addressing & Subnetting](./03-IP-Addressing-and-Subnetting)
Planificación lógica de la red para optimizar la asignación de direcciones.
* Cálculo de subredes (Subnetting) para redes IPv4 (Clases A, B y C).
* Determinación de direcciones de Red, Broadcast y rangos de hosts utilizables.

### 4. [Linux Networking Configuration](./04-Linux-Networking-Config)
Administración de interfaces de red directamente desde la terminal de servidores Linux.
* Configuración de IP estática y dinámica utilizando **Netplan** (archivos `.yaml`).
* Gestión de interfaces y tablas de ruteo locales con la suite `iproute2` (`ip addr`, `ip route`, `ip link`).

---

## 🎯 Caso de Uso Destacado
Uno de los ejercicios más críticos documentados aquí es la **simulación de caída de enlaces en una topología en anillo**. Al configurar el protocolo de ruteo dinámico RIP, logré verificar cómo los routers convergen automáticamente y encuentran caminos alternativos para mantener la conectividad entre los hosts, una habilidad crucial para mantener la **Alta Disponibilidad (HA)** en entornos corporativos.

---
📫 **¿Conectamos?** Puedes encontrarme en [LinkedIn](https://www.linkedin.com/in/candela-nu%C3%B1ez27/) para hablar sobre Infraestructura, Soporte IT y Ciberseguridad.