# 🔬 Análisis de Capa 2: Trama Ethernet y Resolución ARP

## 🎯 Resumen de la Práctica
En este entorno de laboratorio se analizó el comportamiento del tráfico de red a nivel de Capa de Enlace de Datos (Capa 2) y Capa de Red (Capa 3). El objetivo principal fue auditar el intercambio de paquetes durante el descubrimiento de la puerta de enlace (Gateway) utilizando el protocolo ARP, y analizar la estructura de la trama Ethernet.

---

## 1. Identificación de la Puerta de Enlace y Conectividad
Antes de analizar el tráfico, es fundamental conocer la topología lógica local. Se procedió a identificar la ruta por defecto del sistema y a verificar conectividad mediante ICMP.

> **💻 Comando utilizado:** `route -n` e `ip route` para identificar el Gateway, seguido de un `ping` continuo.

![Pasted image 20260611041748](Pasted%20image%2020260611041748.png)


---

## 2. Inspección de la Trama Ethernet (Capa 2)
Utilizando **Wireshark** en modo promiscuo sobre la interfaz conectada al switch, se capturó el tráfico generado por el comando ping. Al inspeccionar el encabezado de la capa de enlace (Ethernet II), pudimos identificar claramente el direccionamiento físico.

![](../assets/Pasted%20image%2020260611044702.png)




**Análisis Técnico:**
* La trama Ethernet encapsula el paquete de Capa 3 (IPv4).
* Se puede observar el direccionamiento físico (MAC) de origen (nuestra interfaz) y el de destino (la interfaz del router/gateway).

---

## 3. Comportamiento del Protocolo ARP
Para que el equipo local pudiera enviar el ping al Gateway, primero necesitó resolver su dirección MAC. Esto se logró mediante el protocolo **ARP (Address Resolution Protocol)**. En la captura se observan los dos tipos de operaciones principales:


**Explicación del flujo:**
1. **ARP Request:** Es un mensaje de tipo **Broadcast** (`ff:ff:ff:ff:ff:ff`). Nuestro equipo pregunta a toda la red: *"¿Quién tiene esta IP? Díganmelo a mí"*. Al ser broadcast, todos los equipos en el dominio de difusión lo reciben.
2. **ARP Reply:** El router destino recibe el broadcast, reconoce su IP y responde directamente a la MAC de nuestra PC con un mensaje **Unicast**, entregando su dirección física.

---

## 4. Auditoría de la Tabla ARP Local
Una vez que el intercambio ARP es exitoso, el sistema operativo guarda temporalmente esta asociación en caché para no saturar la red con preguntas constantes.

> **💻 Comando utilizado:** `arp -a`

![imagen_consola_arp_a.png](imagen_consola_arp_a.png)
*// CANDELA: Acá pega la captura final de tu Word 1.2 (la Imagen 8) donde se ve la salida de tu consola mostrando la tabla ARP con la MAC Address guardada (ej. `? (192.168.1.1) at 00:1a:2b...`).*

**Conclusión:**
Revisar esta tabla es el primer paso en el diagnóstico de red (*troubleshooting*). Desde la perspectiva de ciberseguridad, monitorear cambios irregulares en esta tabla local es vital para detectar posibles ataques de **ARP Spoofing / Man-in-the-Middle**.