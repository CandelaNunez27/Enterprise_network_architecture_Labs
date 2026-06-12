#  Enrutamiento Dinámico (RIPv2) y Convergencia de Red

##  Objetivo de la Práctica
El propósito de este laboratorio es implementar un protocolo de enrutamiento dinámico de tipo *Vector-Distancia* para automatizar el descubrimiento de rutas. El objetivo principal es evaluar la capacidad de **Convergencia y Alta Disponibilidad (HA)** de la red frente a la caída física de enlaces, comparando su rendimiento contra infraestructuras de enrutamiento estrictamente estático.

---

## 1. Topología en Anillo (Ring Topology)

Para realizar las pruebas de redundancia, se construyó una topología cerrada en forma de anillo compuesta por cinco routers Cisco. Esta arquitectura garantiza que existan múltiples caminos lógicos para llegar desde cualquier origen a cualquier destino.

![[topologia_anillo_5_routers.png]]
*// CANDELA: Acá pega la imagen número 1 de tu archivo "AT 1.10", donde se ve el círculo con los 5 routers (Router A, B, C, D, E) y sus respectivas PCs.*

---

## 2. El Problema del Ruteo Estático ante Fallos

Inicialmente, la topología se configuró utilizando rutas estáticas y por defecto en sentido horario. Se comprobó conectividad total, pero al simular un fallo físico (desconectando el enlace directo entre el Router A y el Router B), la red colapsó parcialmente.

**Análisis de la Falla:**
* Al caer el enlace, los paquetes entre redes opuestas (ej. PC2 a PC5) dejaron de llegar.
* La comunicación asimétrica falló: Un router podía enviar el mensaje por un camino alternativo, pero el router destino, al tener rutas estáticas estrictas, intentaba responder por el enlace caído.
* **Conclusión:** El ruteo estático no se adapta automáticamente a los cambios topológicos, requiriendo intervención manual ante cada interrupción del servicio.

---

## 3. Implementación de RIPv2 (Routing Information Protocol)

Para dotar a la red de inteligencia y automatización, se eliminaron las rutas estáticas y se habilitó el protocolo dinámico **RIP en su versión 2**. 

A diferencia del ruteo estático, en RIP no le decimos al router *"cómo llegar al destino"*, sino que le indicamos *"cuáles son sus propias redes conectadas"*. El protocolo se encarga de anunciar estas redes a sus vecinos mediante mensajes *multicast*.

> **💻 Comandos de configuración ejecutados en IOS:**

```bash
Router(config)# router rip
Router(config-router)# version 2
Router(config-router)# network 192.168.1.0  # Anunciando la red LAN
Router(config-router)# network 10.0.0.0     # Anunciando la red WAN vecina
```


_(Esta configuración se aplicó en los 5 nodos del anillo, declarando las redes directamente conectadas de cada uno)._

## 4. Auditoría de Convergencia y Prueba de Redundancia

Una vez que RIP propagó las tablas de ruteo basándose en la métrica de _Cantidad de Saltos (Hops)_, se realizó la prueba de fuego: **simular nuevamente la caída del enlace entre Router A y Router B**.

![[prueba_ping_redundancia.png]] _// CANDELA: Acá podés agregar una captura de consola si tenés algún ping exitoso de la AT 1.10 luego de borrar el enlace, o simplemente dejar el texto explicativo de abajo._

**Resultados de la Prueba de Alta Disponibilidad:** A pesar de la interrupción física del enlace principal, la comunicación no se perdió. El protocolo RIP detectó la ausencia de actualizaciones de estado, recalculó las métricas y actualizó dinámicamente las tablas de todos los routers, encontrando el camino alternativo en sentido antihorario a través del anillo.

**Métricas del Protocolo:**

- **Tipo:** Vector-Distancia.
    
- **Métrica evaluada:** Cantidad de saltos (máximo 15 saltos permitidos).
    
- **Adaptabilidad:** Altamente resiliente ante caídas de hardware en redes de tamaño pequeño/mediano.