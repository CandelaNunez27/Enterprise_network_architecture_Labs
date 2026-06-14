# 📊 Planificación de Capacidad y Subnetting (IPv4)

## 🎯 Objetivo de la Práctica
El propósito de este documento es validar la capacidad de diseño lógico de redes mediante el cálculo matemático de subredes (Subnetting). Esto incluye determinar la máscara de red adecuada según los requisitos de host de una infraestructura, calcular direcciones de red/broadcast para evitar errores de asignación, y validar configuraciones utilizando la herramienta `ipcalc` en entornos Linux.

---

## 1. Auditoría de Direccionamiento y Cálculo de Subred
El primer paso en la asignación de IPs es identificar correctamente el identificador de red a partir de una dirección y su máscara.

> **💻 Caso Práctico:** Se nos asigna la IP de host `10.122.200.77` con una máscara `255.255.255.224` (Notación CIDR: `/27`).
> **Resolución Teórica:** Al aplicar la operación *AND lógica* entre la IP y la máscara, determinamos que la **dirección de subred es `10.122.200.64`**.

Para validar estos cálculos en la administración diaria de servidores, se utiliza la utilidad `ipcalc` desde la consola de Linux, la cual nos brinda un desglose completo del bloque de red:

![[captura_ipcalc_linux.png]]
*// CANDELA: Abrí tu máquina virtual Linux (Ubuntu/Alpine), abrí la terminal, escribí el comando `ipcalc 10.122.200.77/27` y sacale una captura a lo que te responde (te va a mostrar en colores la Netmask, Network, HostMin, HostMax y Broadcast). Guardá esa imagen en tu carpeta de assets.*

---

## 2. Planificación de Capacidad (Capacity Planning)
Al diseñar una infraestructura, la máscara de red debe ajustarse estrictamente a la necesidad del entorno para no desperdiciar direcciones IPv4.

### 🏢 Escenario A: Sucursal Estándar (254 Hosts)
Para una oficina que requiere conectar aproximadamente 250 dispositivos (PCs, impresoras, teléfonos IP), tomando como base la red `192.168.32.0`, la máscara de red óptima es:
* **Máscara Decimal:** `255.255.255.0`
* **Notación CIDR:** `/24`
* **Hosts utilizables:** $(2^8) - 2 = 254$ hosts.
* **Dirección de Broadcast:** `192.168.32.255`

### 🏭 Escenario B: Infraestructura Corporativa (+1000 Hosts)
Para el despliegue de una red en la sede central que requiere asignar IPs a más de 1000 dispositivos partiendo del bloque `128.128.32.0`, se requiere ampliar la porción de host reduciendo los bits de red.
* **Máscara Decimal Óptima:** `255.255.252.0`
* **Notación CIDR:** `/22`
* **Hosts utilizables:** $(2^{10}) - 2 = 1022$ hosts.

---

## 3. Identificación de Errores Críticos (Troubleshooting)
Un error común en la configuración de interfaces (ej. al configurar `Netplan` o `/etc/network/interfaces`) es asignar una dirección reservada a un host.

**Evaluación de Broadcast:**
En una red con máscara `255.255.255.240` (o `/28`), los bloques de red saltan de 16 en 16. Por lo tanto, si un equipo intenta asignarse la IP `75.32.75.15` o `129.130.17.143`, el sistema arrojará un error, ya que **corresponden a las direcciones de Broadcast de sus respectivas subredes** (el último de los 16 números de ese bloque), las cuales están reservadas para transmitir a todos los nodos del dominio.

---

## 🧠 Conclusiones Técnicas
Dominar el subnetting y herramientas como `ipcalc` permite segmentar grandes infraestructuras en dominios de difusión más pequeños. Esto no solo mejora dramáticamente el rendimiento al reducir el "ruido" ARP en la red, sino que es el pilar fundamental para aplicar políticas de seguridad efectivas en Firewalls y Listas de Control de Acceso (ACLs) entre VLANs.