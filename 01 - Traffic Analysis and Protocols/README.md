# 🔬 Laboratorio 01: Protocol Analysis & Traffic Inspection (Wireshark)

## 🎯 Objetivo
El propósito de este laboratorio es analizar el comportamiento real de los protocolos fundamentales de la suite TCP/IP en las capas de Enlace, Red y Transporte mediante la captura e inspección de paquetes en vivo. Esto permite comprender los flujos de comunicación cliente-servidor y realizar diagnósticos de conectividad (*troubleshooting*).

## 🛠️ Herramientas Utilizadas
* **Analizador de Protocolos:** Wireshark.
* **Entorno de Pruebas:** Ubuntu Linux Terminal / Laboratorio Físico.
* **Herramientas de Diagnóstico:** `ping`, `route`, `traceroute`, `resolvectl`.

---

## 💻 Desarrollos y Análisis Prácticos

### 1. Descubrimiento de Capa 2 y Resolución de Direcciones (ARP & MAC)
Se analizó el funcionamiento del protocolo **ARP (Address Resolution Protocol)** al iniciar solicitudes de comunicación hacia el Gateway de la red.
* **Mecanismo de Broadcast:** Inspección de tramas *ARP Request* dirigidas a la dirección MAC de difusión (`ff:ff:ff:ff:ff:ff`) para averiguar la dirección física del router.
* **Mecanismo de Unicast:** Verificación de las tramas *ARP Reply* enviadas directamente desde el Gateway para asociar su IP con su dirección MAC.
* **Inspección de Tablas Locales:** Mapeo de la memoria caché local del sistema mediante el uso del comando de consola:

```bash
arp -a
# O su alternativa moderna en suites iproute2:
ip neigh show
```

