---
title: Guía para Configurar Zoning en Switches Cisco MDS y Nexus Fiber Channel 
date: 2023-07-29 00:00:00 -05:00
image:
  path: "/assets/img/00003-3396673131.png"
  src: "/assets/img/00003-3396673131.png"
  alt: Zoning FiberChannel cisco nexus
categories: [Cisco, CCNP, Nexus]
tags: [bash, cisco, nexus, automation]     # TAG names should always be lowercase
---

# Guía para Configurar Zoning en Switches Cisco MDS y Nexus Fiber Channel

## Introducción

La configuración adecuada del zoning en switches Cisco MDS y Nexus es esencial para garantizar un rendimiento óptimo, seguridad y aislamiento entre dispositivos conectados a la red de almacenamiento. El zoning permite agrupar y restringir el acceso entre los servidores y los dispositivos de almacenamiento, evitando posibles conflictos y mejorando la eficiencia del sistema. En este blog post, te proporcionaremos una guía paso a paso para configurar Fiber Channel zoning en switches Cisco MDS y Nexus.

## I. Comprender los Conceptos Básicos

1. Zoning
El zoning es una técnica que divide la red de almacenamiento en segmentos lógicos para evitar que ciertos dispositivos accedan a otros. Hay dos tipos principales de zoning: "Zoning basado en puertos" (pWWN) y "Zoning basado en nombres de dispositivo" (NWWN). El primero utiliza las direcciones de puerto WWN (World Wide Name) y el segundo se basa en el nombre del dispositivo.

2. Zonas y Alias
Una zona es un conjunto de puertos que pueden comunicarse entre sí. Los alias son nombres que se asocian a las direcciones WWN o NWWN para hacer más fácil la identificación de dispositivos.

## II. Acceso al Switch y Verificación del Hardware

Antes de comenzar la configuración del zoning, asegúrate de tener acceso al switch Cisco MDS o Nexus y verifica que el hardware esté correctamente conectado y funcione correctamente. Si es necesario, actualiza el firmware del switch a la última versión compatible.

## III. Creación de Alias

1. Conecta a tu switch utilizando un cliente de terminal SSH o la interfaz de línea de comandos (CLI).

2. Verifica la lista actual de alias usando el siguiente comando:
   ```
   show fcalias
   ```

3. Crea alias para los dispositivos que quieras agrupar en las zonas utilizando el siguiente comando:
   ```
   fcalias name alias_name vsan <vsan_id>
   member pwwn <WWN_address>
   ```
   Sustituye `<alias_name>` con el nombre del alias deseado y `<WWN_address>` con el World Wide Name del dispositivo.

4. Verifica que los alias se hayan creado correctamente:
   ```
   show fcalias
   ```

## IV. Creación de Zonas

1. Verifica la lista actual de zonas utilizando el siguiente comando:
   ```
   show zone
   ```

2. Crea una nueva zona usando el siguiente comando:
   ```
   zone name zone_name vsan <vsan_id>
   ```
   Reemplaza `<zone_name>` con el nombre de la zona que desees.

3. Añade los alias a la zona recién creada utilizando el siguiente comando:
   ```
   zone name zone_name vsan <vsan_id>
   member fcalias alias_name
   ```
   Donde `<alias_name>` es el nombre del alias creado previamente.

4. Verifica que la zona se haya creado correctamente:
   ```
   show zone
   ```

## V. Activación del Zoning

1. Activa el zoning en el switch utilizando el siguiente comando:
   ```
   zoneset activate name zoneset_name vsan <vsan_id>
   ```
   Sustituye `<zoneset_name>` con el nombre del conjunto de zonas que deseas activar.

2. Verifica que el zoning se haya activado correctamente:
   ```
   show zoneset active
   ```

## VI. Guardar la Configuración

Una vez que hayas configurado el zoning, asegúrate de guardar la configuración para que los cambios sean persistentes después de reiniciar el switch. Utiliza el siguiente comando:
   ```
   copy running-config startup-config
   ```

# Configurar de Zoning usando Ansible

A continuación, te proporcionaré un ejemplo de playbook de Ansible para configurar el zoning en switches Cisco MDS y Nexus. Antes de ejecutar este playbook, asegúrate de tener acceso SSH habilitado en los switches y de que Ansible esté correctamente instalado y configurado para interactuar con los dispositivos. Además, modifica las variables según la configuración específica de tu red.

```yaml
---
- name: Configurar zoning en switches Cisco MDS y Nexus
  hosts: switches
  gather_facts: no
  vars:
    switch_username: tu_usuario_ssh
    switch_password: tu_contrasena_ssh
    vsan_id: 100
    zone_name: zona_ejemplo
    alias_name_server1: alias_servidor1
    alias_name_storage1: alias_almacenamiento1
    server1_wwn: "10:00:00:00:00:00:00:01"
    storage1_wwn: "20:00:00:00:00:00:00:01"

  tasks:
    - name: Crear alias para el servidor
      nxos_command:
        commands:
          - "conf t"
          - "fcalias name {{ alias_name_server1 }} vsan {{ vsan_id }}"
          - "member pwwn {{ server1_wwn }}"
          - "end"
      provider:
        host: "{{ inventory_hostname }}"
        username: "{{ switch_username }}"
        password: "{{ switch_password }}"
      register: alias_server1_result

    - name: Crear alias para el almacenamiento
      nxos_command:
        commands:
          - "conf t"
          - "fcalias name {{ alias_name_storage1 }} vsan {{ vsan_id }}"
          - "member pwwn {{ storage1_wwn }}"
          - "end"
      provider:
        host: "{{ inventory_hostname }}"
        username: "{{ switch_username }}"
        password: "{{ switch_password }}"
      register: alias_storage1_result

    - name: Crear zona y agregar alias
      nxos_command:
        commands:
          - "conf t"
          - "zone name {{ zone_name }} vsan {{ vsan_id }}"
          - "member fcalias {{ alias_name_server1 }}"
          - "member fcalias {{ alias_name_storage1 }}"
          - "end"
      provider:
        host: "{{ inventory_hostname }}"
        username: "{{ switch_username }}"
        password: "{{ switch_password }}"
      register: zone_creation_result

    - name: Activar zoning
      nxos_command:
        commands:
          - "conf t"
          - "zoneset activate name {{ zone_name }} vsan {{ vsan_id }}"
          - "end"
      provider:
        host: "{{ inventory_hostname }}"
        username: "{{ switch_username }}"
        password: "{{ switch_password }}"
      register: zoning_activation_result
```

## Explicación del playbook:

1. Definimos las variables necesarias, como el nombre de usuario y contraseña SSH para acceder a los switches, el ID de VSAN, el nombre de la zona y los alias que queremos crear para el servidor y el almacenamiento.

2. Usamos el módulo `nxos_command` para ejecutar comandos de configuración en los switches.

3. Primero, creamos los alias para el servidor y el almacenamiento usando los comandos `fcalias` en el contexto de configuración (`conf t`).

4. A continuación, creamos la zona y agregamos los alias recién creados a ella utilizando el comando `zone`.

5. Finalmente, activamos el zoning con el comando `zoneset activate`.

Para ejecutar este playbook, guárdalo en un archivo con extensión `.yml` (por ejemplo, `configurar_zoning.yml`) y ejecuta el siguiente comando desde la terminal:

```
ansible-playbook -i tu_inventario.txt configurar_zoning.yml
```

Recuerda reemplazar `tu_inventario.txt` con el nombre de tu archivo de inventario de Ansible, donde defines los hosts (switches) que deseas configurar. Además, asegúrate de que los switches sean accesibles mediante SSH y que las credenciales de usuario y contraseña sean correctas. Si utilizas claves SSH para la autenticación, ajusta la configuración en consecuencia.

# Conclusión

La correcta configuración del zoning en switches Cisco MDS y Nexus es fundamental para asegurar un rendimiento óptimo y una gestión eficiente de la red de almacenamiento. Mediante el uso de alias y zonas, es posible agrupar y aislar dispositivos según sus necesidades, lo que garantiza un entorno seguro y organizado. Recuerda siempre verificar y guardar la configuración después de realizar cambios importantes. Con esta guía, estás listo para mejorar la eficiencia y la seguridad de tu infraestructura de almacenamiento Fiber Channel. ¡Buena configuración!