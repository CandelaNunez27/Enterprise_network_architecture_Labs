# Administración de Cisco IOS y Configuración Base

##  Objetivo de la Práctica

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

2. **Modo Privilegiado (`Router#`):** Se accede con el comando `enable`. Permite visualizar configuraciones con comandos como `show running-config` o `show interfaces`.

3. **Modo Configuración Global (`Router(config)`):** Se accede con `configure terminal`. Desde aquí se aplican cambios que afectan a todo el equipo (como cambiar el nombre con `hostname` o deshabilitar la resolución de dominios con `no ip domain-lookup`).

4. **Modo Configuración de Interfaz (`Router(config-if)`):** Utilizado para asignar IPs y encender puertos específicos.

---

## 3. Topología del Laboratorio

Se armó una red básica conectando dos routers Cisco espalda con espalda (*back-to-back*), cada uno conectado a una red LAN independiente.

![](../assets/Pasted%20image%2020260612030717.png)

**Esquema de Direccionamiento:**
* **Red LAN 1 (PC1 a Router 1):** `192.168.1.0/24`
* **Red WAN (Enlace entre Routers):** `10.0.0.0/24`
* **Red LAN 2 (PC2 a Router 2):** `192.168.2.0/24`

---

## 4. Comandos de Configuración Inicial

A continuación, se detalla el proceso ejecutado en la terminal para levantar el **Router 1** y **Router2**, asignar sus direcciones IP e indicarle por dónde llegar a la red remota.

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

```bash
# Ingreso y configuración global
Router> enable
Router# configure terminal
Router(config)# hostname Router2

# Configuración de la interfaz LAN (Conexión a PC2)
Router1(config)# interface GigabitEthernet0/0/1
Router1(config-if)# ip address 192.168.2.254 255.255.255.0
Router1(config-if)# no shutdown
Router1(config-if)# exit

# Configuración de la interfaz WAN (Conexión a Router1)
Router1(config)# interface GigabitEthernet0/0/0
Router1(config-if)# ip address 10.0.0.2 255.255.255.0
Router1(config-if)# no shutdown
Router1(config-if)# exit

# Configuración de ruta estática para alcanzar la LAN 1
Router1(config)# ip route 192.168.1.0 255.255.255.0 10.0.0.1
```


## 5. Verificación de Estados de Interfaz

Tras aplicar las configuraciones, se validó el estado físico y lógico de los puertos. Para que la comunicación sea exitosa, la interfaz debe reportar: `FastEthernet0/0 is up, line protocol is up`. Si el estado figura como `administratively down`, significa que faltó ejecutar el comando `no shutdown`.
