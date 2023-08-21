---
title: Esenciales de HSRP en Cisco Nexus
date: 2023-07-22 00:00:00 -05:00
categories: [Cisco, CCNP, DCCOR]
tags: [devasc, ccna, ccnp, windows, nexus]     # TAG names should always be lowercase
---
**Esenciales de HSRP en Cisco Nexus**

HSRP (Hot Standby Router Protocol) es un protocolo de enrutamiento que proporciona redundancia y alta disponibilidad en redes locales. La versión 2 de HSRP ofrece algunas mejoras sobre la versión original, como soporte para direcciones IPv6 y mayor eficiencia en el tráfico multicast. A continuación, te proporciono un paso a paso para configurar HSRP versión 2 en un dispositivo Cisco con el sistema operativo NX-OS:

+ __Paso 1:__ _Acceder al modo de configuración_

Conéctate al dispositivo Cisco NX-OS a través de una conexión de consola o SSH utilizando un software de terminal, como PuTTY. Luego, ingresa tus credenciales de inicio de sesión.

+ __Paso 2:__ _Ingresar a la interfaz de interfaz de VLAN_

Escribe el siguiente comando para ingresar al modo de configuración de la interfaz VLAN que deseas configurar con HSRP:

```
configure terminal
interface vlan <numero_de_vlan>
```

Reemplaza `<numero_de_vlan>` con el número de la VLAN en la que deseas configurar HSRP.

+ __Paso 3:__ _Configurar la dirección IP de la interfaz VLAN_

Asigna una dirección IP a la interfaz VLAN utilizando el siguiente comando:

```
ip address <direccion_ip> <mascara_de_red>
```

Reemplaza `<direccion_ip>` con la dirección IP que deseas asignar a la interfaz VLAN y `<mascara_de_red>` con la máscara de red correspondiente.

+ __Paso 4:__ _Habilitar HSRP versión 2 en la interfaz VLAN_

Para habilitar HSRP versión 2 en la interfaz VLAN, utiliza el siguiente comando:

```
hsrp version 2
```

+ __Paso 5:__ _Configurar el grupo HSRP_

Ahora, debes configurar el grupo HSRP y asignarle una dirección IP virtual. El grupo HSRP representa un conjunto de routers que se comportarán como uno solo para proporcionar redundancia. Utiliza el siguiente comando:

```
hsrp <numero_de_grupo> [ipv4|ipv6] <direccion_ip_virtual>
```

Reemplaza `<numero_de_grupo>` con un número entre 0 y 255 que identificará al grupo HSRP. Si deseas configurar HSRP para IPv4, usa `ipv4` o `ipv6` para IPv6. Luego, asigna la `<direccion_ip_virtual>` que será la dirección IP virtual compartida por todos los routers en el grupo HSRP.

+ __Paso 6:__ _Configurar la prioridad del router HSRP (opcional)_

Si deseas establecer un router preferido dentro del grupo HSRP, puedes configurar la prioridad. El router con la prioridad más alta dentro del grupo será el activo (active) en caso de empate. Utiliza el siguiente comando:

```
hsrp <numero_de_grupo> priority <prioridad>
```

Reemplaza `<prioridad>` con un valor entre 1 y 254. El valor predeterminado es 100.

+ __Paso 7:__ _Configurar el temporizador HSRP (opcional)_

Si es necesario, puedes ajustar el temporizador HSRP para cambiar la frecuencia de los mensajes de control del protocolo. Utiliza los siguientes comandos para configurar el temporizador de hello y holdtime respectivamente:

```
hsrp <numero_de_grupo> timers <hello_tiempo> <hold_tiempo>
```

Reemplaza `<hello_tiempo>` con el intervalo en segundos entre los mensajes de hello (por defecto 3 segundos) y `<hold_tiempo>` con el tiempo en segundos que un router esperará para considerar que el router activo ha fallado (por defecto 10 segundos).

+ __Paso 8:__ _Salir y guardar la configuración_

Después de haber realizado todas las configuraciones, asegúrate de salir del modo de configuración y guardar los cambios en la configuración de arranque:

```
exit
copy running-config startup-config
```

¡Eso es todo! Ahora tienes HSRP versión 2 configurado en la interfaz VLAN seleccionada en tu dispositivo Cisco NX-OS. Repite estos pasos en otros routers que desees agregar al mismo grupo HSRP para proporcionar redundancia y alta disponibilidad.

**Referencias**

- Cisco Nexus 9000 Series NX-OS Unicast Routing Configuration Guide, Release 9.3(x): [enlace](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/93x/unicast/configuration/guide/b-cisco-nexus-9000-series-nx-os-unicast-routing-configuration-guide-93x/b-cisco-nexus-9000-series-nx-os-unicast-routing-configuration-guide-93x_chapter_010010.html)

