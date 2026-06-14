# 🌐 Enterprise Network Architecture & Protocol Analysis

¡Hola! Soy **Candela Nuñez** 👩‍💻. Técnica en Redes de Datos y Telecomunicaciones.

Este repositorio es mi entorno de laboratorio. Aquí documento mis prácticas enfocadas en el diseño, configuración y resolución de problemas (*troubleshooting*) de infraestructuras de red. Mi objetivo es ir más allá de la teoría y demostrar, con capturas y comandos reales, cómo se comportan los datos desde el momento en que salen de una interfaz de red hasta que llegan a un servidor.

##  ¿Qué vas a encontrar aquí?

El proyecto está dividido en cuatro grandes áreas, abarcando las capas 2, 3 y 4 del modelo OSI. En ellas utilizo tanto equipamiento propietario (Cisco) como herramientas open-source (Linux, Wireshark).

### 📂 Índice del Repositorio

* 🔬 **[1. Análisis de Tráfico y Protocolos Core](01%20-%20Traffic%20Analysis%20and%20Protocols/README.md):** Inspección profunda de paquetes (*Deep Packet Inspection*) con Wireshark. Análisis del *Three-Way Handshake* (TCP), tráfico *stateless* (UDP), resolución ARP y diagnóstico perimetral con ICMP.
  
* 🗺️ **[02. Infraestructura y Ruteo Cisco](02%20-%20Cisco%20Routing%20Infrastructure/README.md):** Diseño de topologías LAN/WAN. Configuración de IOS desde cero, administración manual de tablas de ruteo en topologías de gran escala y pruebas de convergencia / Alta Disponibilidad con ruteo dinámico (RIPv2).

* 🧮 **[03. Planificación de Capacidad y Subnetting](03%20-%20IP%20Addressing%20and%20Subnetting/README.md):** Cálculo matemático de subredes (CIDR, VLSM), determinación de dominios de broadcast, prevención de solapamientos de red y validación lógica utilizando la consola de Linux (`ipcalc`).

* 🐧 **[04. Configuración de Redes en Linux](04%20-%20Linux%20Networking%20Config/README.md)(./04-Linux-Networking-Config):** Aprovisionamiento de conectividad a nivel de servidor sin entorno gráfico. Implementación nativa usando `Netplan` en Ubuntu Server, el método de interfaces tradicionales en Alpine Linux y diagnóstico con la suite `iproute2`.

---

## 🛠️ Stack Tecnológico
* **Sistemas Operativos:** Ubuntu Server, Alpine Linux.
* **Networking & Virtualización:** Cisco IOS, Packet Tracer, Proxmox (conceptos).
* **Seguridad y Análisis de Tráfico:** Wireshark, tcpdump.
* **Protocolos Dominados:** TCP/IP, ARP, ICMP, RIP, DNS.

---
📫 **¿Conectamos?** Me apasiona resolver problemas y optimizar recursos. Si estás buscando sumar talento a tu equipo de **Soporte IT, Infraestructura o SOC (Ciberseguridad)**, me encantaría charlar. 

Te invito a conectar conmigo en [LinkedIn](https://www.linkedin.com/in/candela-nu%C3%B1ez27/).