---
title: Resumen de config telemetria Cisco Nexus
date: 2023-07-25 00:00:00 -05:00
categories: [Cisco, CCNP, DCCOR]
tags: [devasc, ccna, ccnp, windows, nexus, vpc, port-channel, telemetry]     # TAG names should always be lowercase
---

**Resumen de config telemetria Cisco Nexus**<br>
Instalar y habilitar la telemetría en dispositivos Cisco Nexus 9k te permite monitorear y recopilar información valiosa sobre el rendimiento y el estado del switch. Puedes utilizar la telemetría para obtener estadísticas, métricas y registros para el análisis y solución de problemas. A continuación, te proporciono un post con los pasos para instalar y habilitar la telemetría en un Cisco Nexus 9k:

**Paso 1: Verificar la compatibilidad**<br>
Antes de comenzar, asegúrate de que tu dispositivo Cisco Nexus 9k sea compatible con la telemetría y que esté ejecutando una versión de software que lo soporte.

**Paso 2: Habilitar el módulo de telemetría**<br>
Primero, asegúrate de que el módulo de telemetría (TAC) esté habilitado en tu switch Nexus. Verifica que tengas una licencia válida para el módulo y habilita la función de telemetría utilizando el siguiente comando:

```
switch# feature telemetry
```

**Paso 3: Configurar un destino de telemetría**<br>
El siguiente paso es configurar un destino de telemetría, que será el servidor o la plataforma donde recibirás los datos de telemetría. Puede ser un servidor de gestión o una herramienta de monitoreo de red. Puedes configurar el destino de telemetría con una dirección IP y el puerto UDP que desees utilizar. Por ejemplo:

```
switch(config)# telemetry destination 1
switch(config-telemetry-dest)# address <ip_servidor> port <puerto_udp>
switch(config-telemetry-dest)# protocol udp
```

Reemplaza `<ip_servidor>` con la dirección IP del servidor de destino y `<puerto_udp>` con el puerto UDP que utilizarás para la transmisión de datos de telemetría.

**Paso 4: Crear un grupo de sensores**<br>
Un grupo de sensores te permite agrupar los sensores que deseas monitorear y enviar a un destino de telemetría. Puedes crear un grupo de sensores y asignarle un ID único. Por ejemplo:

```
switch(config)# telemetry group 1
switch(config-telemetry-grp)# sensor-path
```

**Paso 5: Configurar sensores**<br>
Una vez que tienes un grupo de sensores, debes configurar los sensores específicos que deseas monitorear. Por ejemplo, puedes configurar un sensor para monitorear el uso de la CPU:

```
switch(config-telemetry-grp)# sensor 100
switch(config-telemetry-sensor)# path sys/cpu/5sec
```
{% include adsense-inarticlead.html %}


**Paso 6: Asociar un grupo de sensores con un destino**<br>
Después de configurar los sensores, debes asociar el grupo de sensores con el destino de telemetría que configuraste anteriormente:

```
switch(config-telemetry-grp)# destination 1
```

**Paso 7: Habilitar el grupo de sensores**<br>
Finalmente, habilita el grupo de sensores para comenzar a enviar datos de telemetría al destino configurado:

```
switch(config-telemetry-grp)# commit
```

**Paso 8: Verificar la configuración**<br>
Verifica que la configuración de telemetría se haya aplicado correctamente utilizando los siguientes comandos:

```
switch# show telemetry
switch# show telemetry destination
switch# show telemetry group
```

Estos comandos te mostrarán la configuración actual de telemetría, incluidos los destinos, grupos y sensores configurados.

**Paso 9: Monitorear la telemetría**<br>
Una vez que la telemetría esté configurada y habilitada, puedes comenzar a monitorear y analizar los datos recibidos en el servidor de destino. Utiliza la herramienta de monitoreo de red o plataforma de gestión para visualizar y analizar los datos de telemetría según tus necesidades.

Recuerda que los datos de telemetría proporcionan información valiosa sobre el rendimiento y el estado del switch Cisco Nexus 9k, lo que te permite tomar decisiones informadas y solucionar problemas de manera más efectiva.

**Referencias**

- Cisco Nexus 3000 and 9000 Series NX-API REST SDK User Guide and API Reference, Release 9.2x: [enlace](https://developer.cisco.com/docs/cisco-nexus-3000-and-9000-series-nx-api-rest-sdk-user-guide-and-api-reference-release-9-2x/#!configuring-telemetry)

