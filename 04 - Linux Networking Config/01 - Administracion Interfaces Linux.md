#  Administración de Interfaces y Enrutamiento en Servidores Linux

##  Objetivo de la Práctica

En entornos de centros de datos y virtualización, los servidores carecen de interfaz gráfica (GUI). El propósito de este documento es demostrar la capacidad de aprovisionar conectividad a nivel de sistema operativo de forma nativa a través de la CLI, gestionando direccionamiento estático, enrutamiento (Gateways) y resolución de nombres (DNS).

Las pruebas se documentan sobre dos distribuciones estratégicas:
1. **Ubuntu Server:** Estándar de la industria corporativa.
2. **Alpine Linux:** Distribución minimalista, estándar en el despliegue de contenedores (Docker/LXC) y microservicios.

---

## 1. Configuración de Red en Ubuntu Server (Netplan)
Ubuntu y otros sistemas modernos utilizan **Netplan** como utilidad de administración de red, la cual lee archivos estructurados en formato YAML para aplicar las configuraciones al backend (`systemd-networkd` o `NetworkManager`).

###  Despliegue de IP Estática
Se configuró la interfaz principal (`eth0`) editando el archivo de configuración en `/etc/netplan/99_config.yaml`:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      addresses:
        - 10.10.10.2/24
      routes:
        - to: default
          via: 10.10.10.1
      nameservers:
        search: [midominio.local]
        addresses: [10.10.10.1, 1.1.1.1]
```

![[captura_netplan_ubuntu.png]] _// CANDELA: Acá podés abrir tu máquina virtual de Ubuntu, hacer un `cat /etc/netplan/*.yaml` o abrir el archivo con `nano`, sacarle captura a esa consola negra y pegarla._

> **💻 Aplicación y Verificación:** Para compilar y aplicar los cambios sin necesidad de reiniciar el servidor, se ejecutó: `sudo netplan apply` Luego, se validó la asignación en la interfaz con `ip addr show eth0`.

## 2. Configuración de Red en Alpine Linux (Interfaces Tradicional)

Al ser un sistema orientado a la eficiencia, Alpine gestiona sus redes a través del método clásico de Linux, utilizando el archivo de configuración `/etc/network/interfaces`.

###  Asignación Dinámica vs Estática

Para entornos de prueba rápidos, se puede configurar la interfaz en modo DHCP. Sin embargo, para que un servidor ofrezca servicios estables, requiere una configuración estática.

Se modificó el archivo de interfaces para fijar los parámetros de red:



```Plaintext
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
```

![[captura_interfaces_alpine.png]] _// CANDELA: Si tenés a mano tu VM de Alpine, hacele un `cat /etc/network/interfaces` y sacale una captura a la terminal. Guardala en tus assets y ponela acá._

> **💻 Aplicación de Cambios:** En Alpine, el reinicio del demonio de red se invoca a través de su sistema de inicio (`OpenRC`): `sudo /etc/init.d/networking restart`

## 3. Suite iproute2: El estándar de diagnóstico local

A lo largo de todo el laboratorio, se descartó el uso de herramientas obsoletas (como `ifconfig` o `route`) en favor de la moderna suite **`iproute2`**, utilizando comandos críticos para la administración local del servidor:

- `ip a` (o `ip addr`): Para auditar el estado físico (UP/DOWN), la dirección MAC y las direcciones IP (IPv4/IPv6) asignadas a los adaptadores lógicos y físicos.
    
- `ip route`: Para consultar la tabla de ruteo interna del kernel de Linux y verificar que la ruta hacia el _Default Gateway_ esté inyectada correctamente.