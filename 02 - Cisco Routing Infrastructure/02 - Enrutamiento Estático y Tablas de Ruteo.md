# 🗺️ Enrutamiento Estático y Análisis de Tablas de Ruteo

## 🎯 Objetivo de la Práctica
El propósito de este laboratorio es comprender la lógica de reenvío de paquetes (forwarding) en la Capa 3 mediante la construcción y análisis de **Tablas de Enrutamiento**. Además, se implementan rutas estáticas manuales en equipos Cisco para interconectar redes LAN segmentadas y se configura una ruta por defecto (Default Gateway) para garantizar la salida a Internet.

---

## 1. Topología Multi-Sucursal
Se desplegó una infraestructura con múltiples routers interconectados mediante enlaces WAN, donde cada router administra su propia red de área local (LAN) aislada (ej. `192.168.1.0/24`, `192.168.2.0/24`, etc.).

![[topologia_rutas_estaticas.png]]
*// CANDELA: Acá pega la imagen principal de la topología de tu "ACT 1.6", donde se ven los routers conectados en cadena con las nubes/LANs colgando abajo.*

---

## 2. Lógica de la Tabla de Enrutamiento
Antes de aplicar comandos, es vital planificar lógicamente el tráfico. Un router solo conoce las redes que tiene directamente conectadas. Para cualquier otro destino, necesita una instrucción precisa que contenga tres elementos clave:
1. **Red Destino:** La IP de la red a la que queremos llegar.
2. **Máscara (Genmask):** El tamaño de esa red destino.
3. **Gateway / Siguiente Salto:** La dirección IP de la interfaz del router vecino que nos ayudará a llegar a ese destino (o en su defecto, nuestra propia interfaz de salida).

![[tabla_ruteo_manual.png]]
*// CANDELA: Acá hace un recorte y pega una de las tablas que rellenaste en tu archivo "ACT 1.5" (por ejemplo, la Tabla 8 del Router 4, para que se vea cómo estructuraste mentalmente los destinos y gateways).*

---

## 3. Implementación de Rutas Estáticas en IOS
Para interconectar las subredes internas, ingresamos al modo de configuración global de cada router y aplicamos el enrutamiento manual. 

La sintaxis utilizada para indicar a un router cómo alcanzar una red remota es:
`ip route [red_destino] [mascara_destino] [ip_siguiente_salto]`

> **💻 Ejemplo Práctico (Conectando sucursales):**
> Para que el router remoto (T9) pueda responderle a la red LAN 1 (`192.168.1.0/24`), se configuró la siguiente ruta estática apuntando a la IP de la interfaz WAN de su router vecino:

```bash
Router(config)# ip route 192.168.1.0 255.255.255.0 10.0.0.1
```

_(Este proceso se repitió sistemáticamente en cada nodo de la red para garantizar el enrutamiento bidireccional, evitando así el error de "Destination Unreachable")._

## 4. Configuración de Salida a Internet (Default Gateway)

En un entorno corporativo, es inviable conocer las direcciones de todos los servidores de Internet. Para solucionar esto, se implementa una **Ruta por Defecto** (o Ruta de Último Recurso).

Esta regla le indica al router: _"Si llega un paquete con un destino que no está en tu tabla de enrutamiento, envíalo por este camino hacia el Proveedor de Servicios (ISP)"_.

> **💻 Comando de configuración del Default Gateway:**

Bash

```
# Red destino 0.0.0.0 con máscara 0.0.0.0 engloba cualquier IP de Internet
Router(config)# ip route 0.0.0.0 0.0.0.0 [IP_DEL_ROUTER_PROVEEDOR]
```

## 5. Auditoría y Verificación

Una vez aplicadas las políticas de enrutamiento, se valida la configuración leyendo la tabla activa del router.

> **💻 Comando utilizado:** `show ip route`

En la salida de este comando, las rutas aprendidas manualmente aparecen marcadas con la letra **`S`** (Static), mientras que la ruta de salida a Internet aparece como **`S*`** (Static Candidate Default). La conectividad end-to-end se verificó exitosamente mediante tráfico ICMP (ping) entre las computadoras de los extremos opuestos de la topología.















# 🗺️ Enrutamiento Estático: Análisis y Resolución de Topologías Complejas

## 🎯 Objetivo de la Práctica

El propósito de este laboratorio es comprender la lógica de reenvío de paquetes (forwarding) en la Capa 3. Para ello, nos centraremos en una topología de red a gran escala, construyendo sus Tablas de Enrutamiento e implementando rutas estáticas para lograr conectividad total.

---

## 1. Topología del Laboratorio

Para este desafío se analizó una infraestructura amplia con múltiples routers interconectados, donde cada nodo administra subredes específicas. La complejidad de este escenario radica en la cantidad de saltos necesarios para comunicar los equipos que se encuentran en los extremos opuestos de la red.

![[topologia_caso_c_packet_tracer.png]]
*// CANDELA: Acá recorta y pega la imagen general de la topología del Caso C que tenés en tu archivo "ACT 1.5" (donde se ven todos los routers y las PCs conectadas).*

---

## 2. Lógica de la Tabla de Enrutamiento
Antes de configurar los equipos en la consola, es fundamental planificar el tráfico de forma teórica. Un router solo conoce las redes que tiene directamente conectadas en sus interfaces. Para alcanzar destinos remotos en el Caso C, se estructuraron rutas que contienen tres elementos clave:
1. **Destino:** La red a la que queremos llegar.
2. **Genmask (Máscara):** El tamaño de esa red destino.
3. **Gateway (Siguiente Salto):** La dirección IP de la interfaz del router vecino por donde se debe enviar el paquete.

![[tabla_ruteo_caso_c.png]]
*// CANDELA: Acá pega una de las tablas grandes que armaste para el Caso C en tu archivo "ACT 1.5" (por ejemplo, la Tabla 10 o la Tabla 11).*

---

## 3. Implementación de Rutas Estáticas en IOS
Con la planificación lista, ingresamos al modo de configuración global de los routers para enseñarles los caminos hacia las redes remotas. 

La sintaxis utilizada en Cisco IOS es:
`ip route [red_destino] [mascara_destino] [ip_gateway]`

> **💻 Ejemplo de configuración:**
> Para que los paquetes puedan viajar entre las distintas LANs y no sean descartados en el camino, se aplicaron rutas estáticas apuntando a las interfaces de los routers vecinos (utilizando las IPs de los enlaces punto a punto, como las redes `10.0.0.X` o `172.22.X.X` documentadas en las tablas).

```bash
Router(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
````

_(Este proceso se repitió sistemáticamente en cada nodo clave del Caso C para asegurar respuestas bidireccionales y evitar el error "Destination Unreachable")._

## 4. Configuración de Salida a Internet (Default Gateway)

Para que los equipos de esta gran topología tengan salida a Internet a través del router del proveedor, se implementó una **Ruta por Defecto** (o Ruta de Último Recurso).

Esta regla le indica al router: _"Si llega un paquete con un destino que no está en tu tabla local, envíalo por este camino"_.

> **💻 Comando de configuración:**

Bash

```
# La red destino 0.0.0.0 con máscara 0.0.0.0 engloba cualquier IP desconocida
Router(config)# ip route 0.0.0.0 0.0.0.0 [IP_DEL_ROUTER_PROVEEDOR]
```

## 5. Auditoría y Verificación

Tras configurar todos los saltos del Caso C, validamos que la información se haya guardado correctamente en la memoria de los equipos.

> **💻 Comando utilizado:** `show ip route`

En esta salida, las redes directamente conectadas figuran con la letra **`C`**, las rutas estáticas aprendidas manualmente con la letra **`S`**, y la ruta por defecto hacia internet con **`S*`**. Finalmente, se verificó la conectividad de extremo a extremo mediante `ping` entre las PCs más alejadas de la topología.