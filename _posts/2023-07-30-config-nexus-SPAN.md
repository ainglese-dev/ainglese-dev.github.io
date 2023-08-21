---
title: Profundizando en la Monitorización del Tráfico de Red SPAN, RSPAN y ERSPAN
date: 2023-07-30 00:00:00 -05:00
image:
  path: "/assets/img/00005-3291037043.png"
  src: "/assets/img/00005-3291037043.png"
  alt: SPAN cisco nexus
categories: [Cisco, CCNP, Nexus]
tags: [cisco, nexus, automation]     # TAG names should always be lowercase
---
# **Profundizando en la Monitorización del Tráfico de Red: SPAN, RSPAN y ERSPAN**

## *Introducción*

En entornos de redes modernas, garantizar un rendimiento óptimo y solucionar problemas de red son fundamentales. Una de las herramientas esenciales para lograr estos objetivos es la tecnología de Switched Port Analyzer (SPAN). SPAN permite a los administradores de red monitorear y analizar el tráfico de red sin interrumpir el flujo normal de datos. En esta publicación del blog, exploraremos tres configuraciones de SPAN diferentes: SPAN, Remote SPAN (RSPAN) y Encapsulated Remote SPAN (ERSPAN), y aprenderemos cómo implementarlas en los switches Cisco Nexus 9000 Series.

# **1. SPAN (Local Switched Port Analyzer)**

SPAN, también conocido como Local SPAN, permite a los administradores de red monitorear el tráfico en un solo switch. Funciona copiando el tráfico de uno o más puertos fuente o VLAN y enviándolo a un puerto de destino designado, donde las herramientas de monitoreo pueden analizar los paquetes capturados. SPAN se puede utilizar para diversos fines, como monitoreo de red, análisis de seguridad y solución de problemas de rendimiento.

*Pasos de configuración:*

Para configurar SPAN en un switch Cisco Nexus 9000 Series, siga estos pasos:

1. Identificar la interfaz o VLAN fuente que desea monitorear:

```
switch# configure terminal
switch(config)# monitor session 1
switch(config-monitor)# source interface <interfaz-fuente>
switch(config-monitor)# source vlan <VLAN-fuente>
```

2. Especifique la interfaz de destino donde se enviará el tráfico reflejado:

```
switch(config-monitor)# destination interface <interfaz-destino>
```

3. (Opcional) Aplique un SPAN ACL para filtrar el tráfico reflejado en función de criterios específicos:

```
switch(config-monitor)# filter access-group <nombre-SPAN-ACL>
```

{% include adsense-inarticlead.html %}


4. Salga del modo de configuración y habilite la sesión de SPAN:

```
switch(config-monitor)# end
switch# show monitor session 1
switch# copy running-config startup-config
```

# **2. Remote SPAN (RSPAN)**

Remote SPAN, o RSPAN, extiende la capacidad de SPAN a través de varios switches, lo que permite a los administradores monitorear el tráfico en un switch remoto. Con RSPAN, el tráfico de origen de un switch se encapsula y se envía a través de la red a un puerto de destino en otro switch donde ocurre el monitoreo. Esto permite el monitoreo centralizado del tráfico y el análisis, incluso en entornos geográficamente dispersos.

*Pasos de configuración:*

Para configurar RSPAN en los switches Cisco Nexus 9000 Series, siga estos pasos:

1. Defina la VLAN de RSPAN que llevará el tráfico reflejado:

```
switch# configure terminal
switch(config)# vlan <VLAN-RSPAN>
switch(config-vlan)# remote-span
```

2. Identifique las interfaces o VLAN de origen que se deben monitorear:

```
switch(config)# monitor session 2
switch(config-monitor)# source interface <interfaz-fuente>
switch(config-monitor)# source vlan <VLAN-fuente>
```

3. Especifique la VLAN de RSPAN como destino para el tráfico reflejado:

```
switch(config-monitor)# destination remote vlan <VLAN-RSPAN>
```

4. Salga del modo de configuración y habilite la sesión de RSPAN:

```
switch(config-monitor)# end
switch# show monitor session 2
switch# copy running-config startup-config
```

# **3. Encapsulated Remote SPAN (ERSPAN)**

ERSPAN lleva RSPAN un paso más allá al encapsular el tráfico reflejado en paquetes de Generic Routing Encapsulation (GRE). Esto permite que el tráfico SPAN se transmita a través de una red IP, lo que hace posible monitorear el tráfico entre diferentes centros de datos o ubicaciones de monitoreo remoto.

*Pasos de configuración:*

Para configurar ERSPAN en los switches Cisco Nexus 9000 Series, siga estos pasos:

1. Defina la VLAN de ERSPAN:

```
switch# configure terminal
switch(config)# vlan <VLAN-ERSPAN>
switch(config-vlan)# erspan-source
```

2. Especifique la dirección IP de destino de ERSPAN y parámetros opcionales:

```
switch(config)# monitor session 3
switch(config-monitor)# source interface <interfaz-fuente>
switch(config-monitor)# destination erspan-id <ID-ERSPAN> ip address <IP-destino> [vrf <nombre-VRF>]
```

3. (Opcional) Aplique un ERSPAN ACL para filtrar el tráfico reflejado:

```
switch(config-monitor)# filter access-group <nombre-ERSPAN-ACL>
```

4. Salga del modo de configuración y habilite la sesión de ERSPAN:

```
switch(config-monitor)# end
switch# show monitor session 3
switch# copy running-config startup-config
```

# Automatizando SPAN, RPAN y ERPAN

Puedo proporcionarte un ejemplo de un archivo Ansible playbook que podrías utilizar para configurar SPAN, RSPAN y ERSPAN en un switch Cisco Nexus 9000 Series. Ten en cuenta que debes adaptar este playbook a tu entorno específico, incluidas las direcciones IP, interfaces y VLAN que desees monitorear. Además, asegúrate de tener acceso SSH habilitado en el switch y las credenciales adecuadas para conectarte a él.

Antes de ejecutar el playbook, asegúrate de tener Ansible instalado en tu máquina local. Puedes instalar Ansible mediante el siguiente comando (en Linux):

```
sudo apt-get install ansible
```

Luego, crea un archivo llamado `span_config.yml` con el siguiente contenido:

```yaml
---
- name: Configurar SPAN en el Switch Cisco Nexus
  hosts: switches
  gather_facts: no
  connection: network_cli
  tasks:
    - name: Configurar SPAN (Local SPAN)
      nxos_command:
        commands:
          - "configure terminal"
          - "monitor session 1"
          - "source interface {{ source_interface }}"
          - "destination interface {{ destination_interface }}"
      vars:
        source_interface: "fuente_interface"
        destination_interface: "destino_interface"

    - name: Configurar RSPAN (Remote SPAN)
      nxos_command:
        commands:
          - "configure terminal"
          - "vlan {{ rspan_vlan }}"
          - "remote-span"
          - "monitor session 2"
          - "source interface {{ source_interface }}"
          - "destination remote vlan {{ rspan_vlan }}"
      vars:
        rspan_vlan: "RSPAN_VLAN"
        source_interface: "fuente_interface"

    - name: Configurar ERSPAN (Encapsulated Remote SPAN)
      nxos_command:
        commands:
          - "configure terminal"
          - "vlan {{ erspan_vlan }}"
          - "erspan-source"
          - "monitor session 3"
          - "source interface {{ source_interface }}"
          - "destination erspan-id {{ erspan_id }} ip address {{ destination_ip }} vrf {{ vrf_name }}"
      vars:
        erspan_vlan: "ERSPAN_VLAN"
        source_interface: "fuente_interface"
        erspan_id: "ERSPAN_ID"
        destination_ip: "IP_DESTINO"
        vrf_name: "NOMBRE_VRF"
```

Asegúrate de reemplazar `fuente_interface`, `destino_interface`, `RSPAN_VLAN`, `fuente_interface`, `ERSPAN_VLAN`, `ERSPAN_ID`, `IP_DESTINO` y `NOMBRE_VRF` con los valores adecuados para tu configuración.

Luego, crea un archivo llamado `hosts` con la dirección IP de tu switch Cisco Nexus:

```
[switches]
192.168.1.1
```

Finalmente, ejecuta el playbook con el siguiente comando:

```
ansible-playbook -i hosts span_config.yml
```

Este playbook configurará SPAN, RSPAN y ERSPAN en el switch Cisco Nexus 9000 Series con las opciones que hayas especificado en el archivo `span_config.yml`. Recuerda que siempre es importante realizar pruebas en un entorno de laboratorio antes de implementar configuraciones en un entorno de producción.


*Conclusión*

En conclusión, SPAN, RSPAN y ERSPAN son herramientas poderosas que brindan a los administradores de red la capacidad de monitorear y analizar el tráfico de la red para solucionar problemas, análisis de seguridad y optimización del rendimiento. Al comprender estas configuraciones e implementarlas en los switches Cisco Nexus 9000 Series, los profesionales de TI pueden obtener información valiosa sobre el comportamiento de su red, lo que lleva a una mejor toma de decisiones y una mayor estabilidad de la red en general.