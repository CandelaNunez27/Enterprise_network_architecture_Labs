# 🛠️ Administración de Cisco IOS y Configuración Base

## 🎯 Objetivo de la Práctica
El objetivo de este laboratorio es familiarizarse con el sistema operativo de Cisco (IOS), comprendiendo su arquitectura interna de memoria y dominando la navegación por la línea de comandos (CLI). Además, se realiza el levantamiento inicial de interfaces de red en una topología de conexión directa entre dos routers.

---

## 1. Arquitectura de Memoria en Routers Cisco
Para administrar correctamente un equipo Cisco, primero identificamos cómo almacena y ejecuta su configuración operativa:

* **RAM:** Almacena la tabla de enrutamiento y el archivo `running-config` (la configuración que está activa en ese momento). Es volátil.

* **NVRAM (Non-Volatile RAM):** Almacena el archivo `startup-config`. Es la configuración permanente que carga el router al encenderse.

* **Flash:** Memoria no volátil donde se guarda la imagen completa del sistema operativo (Cisco IOS).

* **ROM:** Contiene instrucciones de arranque básico (Bootstrap) y software de diagnóstico inicial.

---

## 2. Navegación en la Línea de Comandos (CLI)
Para aplicar configuraciones, es necesario escalar privilegios dentro de la terminal de IOS. Los modos principales utilizados son:

1. **Modo Usuario (`Router>`):** Acceso limitado, ideal para diagnósticos básicos (como un simple ping).
2. **Modo Privilegiado (`Router#`):** Se accede con el comando `enable`[cite: 3]. Permite visualizar configuraciones con comandos como `show running-config` o `show interfaces`[cite: 3].
3. **Modo Configuración Global (`Router(config)`):** Se accede con `configure terminal`[cite: 3]. Desde aquí se aplican cambios que afectan a todo el equipo (como cambiar el nombre con `hostname` o deshabilitar la resolución de dominios con `no ip domain-lookup`)[cite: 3].
4. **Modo Configuración de Interfaz (`Router(config-if)`):** Utilizado para asignar IPs y encender puertos específicos[cite: 3].

---

## 3. Topología del Laboratorio
Se armó una red básica conectando dos routers Cisco espalda con espalda (*back-to-back*), cada uno conectado a una red LAN independiente[cite: 3].

![[topologia_basica_routers_cisco.png]]
*// CANDELA: Acá pega la segunda imagen de tu archivo "ACT 1.3", esa donde sale la topología de Packet Tracer con los routers ISR4331 (Router1 y Router2) conectados por el medio y las PCs abajo.*

**Esquema de Direccionamiento:**
* **Red LAN 1 (PC1 a Router 1):** `192.168.1.0/24`[cite: 3]
* **Red WAN (Enlace entre Routers):** `10.0.0.0/24`[cite: 3]
* **Red LAN 2 (PC2 a Router 2):** `192.168.2.0/24`[cite: 3]

---

## 4. Comandos de Configuración Inicial
A continuación, se detalla el proceso ejecutado en la terminal para levantar el **Router 1**, asignar sus direcciones IP e indicarle por dónde llegar a la red remota[cite: 3].

```bash
# Ingreso y configuración global
Router> enable
Router# configure terminal
Router(config)# hostname Router1

# Configuración de la interfaz LAN (Conexión a PC1)
Router1(config)# interface GigabitEthernet0/0/0
Router1(config-if)# ip address 192.168.1.254 255.255.255.0
Router1(config-if)# no shutdown
Router1(config-if)# exit

# Configuración de la interfaz WAN (Conexión a Router2)
Router1(config)# interface GigabitEthernet0/0/1
Router1(config-if)# ip address 10.0.0.1 255.255.255.0
Router1(config-if)# no shutdown
Router1(config-if)# exit

# Configuración de ruta estática para alcanzar la LAN 2
Router1(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

(El mismo procedimiento estructurado se replicó en el Router 2, invirtiendo las redes correspondientes[cite: 3]).

## 5. Verificación de Estados de Interfaz

Tras aplicar las configuraciones, se validó el estado físico y lógico de los puertos. Para que la comunicación sea exitosa, la interfaz debe reportar: `FastEthernet0/0 is up, line protocol is up`[cite: 3]. Si el estado figura como `administratively down`, significa que faltó ejecutar el comando `no shutdown`[cite: 3].
