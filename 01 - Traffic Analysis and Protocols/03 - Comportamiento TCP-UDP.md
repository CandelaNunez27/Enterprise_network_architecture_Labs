#  Análisis de Capa de Transporte: TCP vs UDP

##  Objetivo
Este laboratorio tiene como finalidad auditar empíricamente las diferencias operativas entre los dos principales protocolos de la Capa de Transporte: el **Protocolo de Control de Transmisión (TCP)** y el **Protocolo de Datagramas de Usuario (UDP)**. Mediante la captura de tráfico real, se analiza el establecimiento de sesiones, la transferencia de datos y el cierre de conexiones.

---

## 1. Análisis de Tráfico sin Conexión (UDP)
Para evaluar el comportamiento del tráfico UDP, se generó actividad de resolución de nombres de dominio (DNS) consultando un host externo.

A diferencia de TCP, UDP envía los datagramas directamente hacia el puerto destino sin establecer un canal previo, priorizando la velocidad y la baja sobrecarga (overhead) sobre la fiabilidad de entrega.

![](../assets/Pasted%20image%2020260612001707.png)
![](../assets/Pasted%20image%2020260612001750.png)

**Observaciones Técnicas:**
* Se identificaron los puertos de origen (efímeros) y el puerto de destino.
* En la cabecera UDP capturada no se observan campos de control de flujo ni números de secuencia, confirmando su naturaleza "sin estado" (stateless).

---

## 2. Implementación de Servicios TCP (Servidor Web)
Para forzar y analizar una conexión orientada a sesión, se levantó un servidor web **Apache2** en una máquina virtual con **Ubuntu Linux**. 

> **💻 Despliegue del servicio:**
> `sudo apt install apache2`
> `sudo systemctl start apache2.service`[cite: 8, 22]

Una vez que el servicio estuvo escuchando peticiones en el puerto 80, se procedió a interceptar la comunicación desde un cliente HTTP[cite: 8, 22].

---

## 3. Inspección del Three-Way Handshake (TCP)
La principal característica de TCP es garantizar la entrega fiable y ordenada de los datos[cite: 8, 22]. Al iniciar la captura, se aisló el proceso de negociación inicial conocido como *Saludo de 3 Vías*.

![[imagen_wireshark_tcp_handshake.png]]
*// CANDELA: Acá pega la captura de tu ACT 1.9 (punto 8) donde resaltaste en amarillo los tres primeros paquetes: `[SYN]`, `[SYN, ACK]`, `[ACK]`.*

**Análisis de la Negociación:**
1. **`[SYN]` (Cliente -> Servidor):** El cliente inicia la conexión solicitando sincronizar números de secuencia[cite: 8, 22].
2. **`[SYN, ACK]` (Servidor -> Cliente):** El servidor confirma la recepción (`ACK`) y envía su propio parámetro de sincronización (`SYN`)[cite: 8, 22].
3. **`[ACK]` (Cliente -> Servidor):** El cliente acusa recibo, estableciendo la conexión formal para comenzar a transmitir los datos[cite: 8, 22].

---

## 4. Transmisión HTTP y Control de Flujo (Window Size)
Una vez establecido el canal TCP, se evidenció el envío del primer paquete de datos en texto claro: una petición `GET / HTTP/1.1`[cite: 8, 22]. 

![[imagen_tcp_window_size.png]]
*// CANDELA: Acá pega la captura detallada de Wireshark de tu ACT 1.9 (punto 8) donde sale el panel inferior remarcando en rojo/naranja la bandera `Flags: 0x002 (SYN)` y `Window: 64240`.*

**Tamaño de Ventana (Window Size):** 
En la captura se identificó el parámetro de la Ventana TCP[cite: 8, 22]. Este campo es crucial para el **Control de Flujo**, ya que le indica al emisor cuántos bytes está dispuesto a recibir y procesar el receptor antes de que sus búferes se saturen. 

---

## 5. Finalización Ordenada de la Conexión (TCP Teardown)
Finalmente, se capturó el proceso de cierre de la sesión. Para garantizar que ningún paquete se pierda en tránsito, TCP utiliza un proceso de 4 pasos (o 3 pasos combinados) intercambiando banderas de finalización.

![[imagen_wireshark_tcp_fin.png]]
*// CANDELA: Acá pega la última captura de Wireshark de tu ACT 1.9 (punto 8) donde se ven las líneas con la bandera `[FIN, ACK]` y el `[ACK]` final cerrando el puerto.*

**Mecanismo de Desconexión:**
* El equipo que desea terminar la comunicación envía un segmento `FIN`[cite: 8, 22].
* El extremo remoto responde con un `ACK` de confirmación[cite: 8, 22].
* Luego envía su propio segmento `FIN`[cite: 8, 22].
* El cliente responde con el `ACK` definitivo, cerrando la conexión y liberando los puertos en ambos sistemas[cite: 8, 22].