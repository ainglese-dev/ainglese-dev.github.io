---
title: Config basicas vPC en Cisco Nexus
date: 2023-07-24 00:00:00 -05:00
categories: [Cisco, CCNP, DCCOR]
tags: [devasc, ccna, ccnp, windows, nexus, vpc, port-channel]     # TAG names should always be lowercase
---
**Config basica vPC domain Cisco Nexus**<br>

Configurar un dominio vPC (Virtual PortChannel) en dispositivos Cisco Nexus permite establecer una conexión de alta disponibilidad y redundancia entre switches. Los dominios vPC permiten configurar un PortChannel que se extiende a través de múltiples switches, evitando problemas de Spanning Tree Protocol y mejorando la utilización del ancho de banda. Aquí tienes un paso a paso para configurar un dominio vPC en dispositivos Cisco Nexus, junto con algunas buenas prácticas:

+ **Paso 1: Preparación**<br>
Antes de comenzar la configuración, asegúrate de que los switches Nexus sean compatibles con vPC y que las interfaces físicas que planeas utilizar en el PortChannel están correctamente conectadas y configuradas en ambos switches.

+ **Paso 2: Configurar el Peer-keepalive link**<br>
El enlace peer-keepalive es una conexión de capa 3 que se utiliza para la comunicación de estado entre los dos switches Nexus en el dominio vPC. Esto es esencial para la sincronización de la información del vPC. Aquí tienes un ejemplo de configuración básica para el peer-keepalive:

```
switch(config)# interface Ethernet1/1
switch(config-if)# description Peer-keepalive link
switch(config-if)# no shutdown
switch(config-if)# ip address <dirección_IP> <máscara_de_red>
```

+ **Paso 3: Habilitar el Feature vPC**<br>
Antes de configurar el dominio vPC, asegúrate de habilitar la función vPC en ambos switches Nexus utilizando el siguiente comando:

```
switch(config)# feature vpc
```

+ **Paso 4: Crear el dominio vPC**<br>
Ahora, crearemos el dominio vPC y asignaremos el número de dominio deseado. Este número debe ser el mismo en ambos switches Nexus:

```
switch(config)# vpc domain <numero_de_dominio>
```

+ **Paso 5: Especificar el peer switch**<br>
Dentro del contexto del dominio vPC, debes especificar el otro switch Nexus como el "peer switch" utilizando el siguiente comando:

```
switch(config-vpc-domain)# peer-switch
```

Esta configuración asegura que uno de los switches se convertirá automáticamente en el switch primario si el otro falla.

+ **Paso 6: Configurar el peer-keepalive**<br>
En el contexto del dominio vPC, especifica la dirección IP que configuraste en el enlace peer-keepalive en el Paso 2:

```
switch(config-vpc-domain)# peer-keepalive destination <dirección_IP_peer_switch>
```

{% include adsense-inarticlead.html %}

+ **Paso 7: Crear el PortChannel vPC**<br>
El siguiente paso es crear el PortChannel vPC que se extenderá entre los switches Nexus. El número del PortChannel debe ser el mismo en ambos switches:

```
switch(config)# interface port-channel <numero_de_PortChannel>
switch(config-if)# description vPC Peer-link
switch(config-if)# vpc peer-link
```

El comando `vpc peer-link` es esencial para identificar este PortChannel como el enlace de pares (peer-link) entre los switches Nexus.

+ **Paso 8: Agregar interfaces al PortChannel vPC**<br>
A continuación, debes agregar las interfaces físicas al PortChannel vPC que deseas utilizar para la comunicación entre los switches Nexus. Por ejemplo:

```
switch(config)# interface Ethernet1/2
switch(config-if)# channel-group <numero_de_PortChannel> mode active
```

Repite este paso para todas las interfaces que deseas agregar al PortChannel vPC.

+ **Paso 9: Configurar el Load Balancing**<br>
Para mejorar el rendimiento, es recomendable configurar el algoritmo de equilibrio de carga en el PortChannel vPC. Cisco Nexus utiliza la opción "src-dst-ip" como algoritmo predeterminado, que equilibra la carga basada en las direcciones IP de origen y destino. Puedes confirmar que esté configurado usando:

```
switch(config-if)# port-channel load-balance src-dst-ip
```

+ **Paso 10: Guardar la configuración**<br>
Una vez que hayas completado la configuración, asegúrate de guardar la configuración en ambos switches Nexus:

```
switch# copy running-config startup-config
```

**Buenas prácticas:**<br>
- Verifica que los dos switches Nexus estén utilizando la misma versión de sistema operativo y que sean compatibles con vPC.
- Utiliza cables de alta calidad y asegúrate de que estén correctamente conectados y no presenten daños físicos.
- Configura el peer-keepalive link en una red de administración independiente y asegura que haya redundancia en la conectividad de administración.
- Usa el comando "graceful consistency-check auto-recovery" para permitir una recuperación más rápida de los PortChannels vPC en caso de una interrupción temporal en el enlace peer-keepalive.
- Evita configurar VLANs vPC en la misma interfaz física en ambos switches Nexus para evitar problemas de convergencia del protocolo Spanning Tree.

Siguiendo estos pasos y buenas prácticas, estarás en camino de configurar un dominio vPC exitoso en dispositivos Cisco Nexus, mejorando la redundancia y alta disponibilidad en tu red.

**Referencias**

- Understand and Configure Nexus 9000 vPC with Best Practices: [enlace](https://www.cisco.com/c/en/us/support/docs/switches/nexus-9000-series-switches/218333-understand-and-configure-nexus-9000-vpc.html)

