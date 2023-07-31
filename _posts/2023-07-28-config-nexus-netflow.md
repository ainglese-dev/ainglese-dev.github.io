---
title: Como configurar Netflow en Cisco Nexus 
date: 2023-07-28 00:00:00 -05:00
image:
  path: "/assets/img/00001-2835769131.png"
  src: "/assets/img/00001-2835769131.png"
  alt: Netflow cisco nexus
categories: [Cisco, CCNP, Nexus]
tags: [bash, cisco, nexus, automation]     # TAG names should always be lowercase
---
# **Configuración de NetFlow en Cisco Nexus Switches**

NetFlow es una tecnología desarrollada por Cisco que proporciona información valiosa sobre el tráfico de red y el rendimiento de los dispositivos. Permite la recolección y análisis de datos de tráfico en tiempo real, lo que resulta crucial para la monitorización, el análisis de la red y la resolución de problemas. En este blog post, aprenderemos cómo configurar NetFlow en switches Cisco Nexus y exploraremos cinco ejemplos clásicos de uso para esta potente herramienta.

# **Configuración de NetFlow en Cisco Nexus: Paso a Paso**

Antes de empezar, asegúrate de tener acceso de administrador al switch Nexus y de contar con los privilegios necesarios para realizar cambios en la configuración. Sigue estos pasos para configurar NetFlow en tu switch:

## **Paso 1: Verificar la compatibilidad**

Verifica que tu modelo de switch Nexus soporte NetFlow y asegúrate de tener una versión de software que lo admita.

## **Paso 2: Habilitar NetFlow**

Ingresa al modo de configuración global y habilita NetFlow con el siguiente comando:
```
switch# configure terminal
switch(config)# feature netflow
```

## **Paso 3: Crear un flujo de NetFlow**

Define un flujo de NetFlow y asígnale un nombre.
```
switch(config)# flow exporter EXPORTER_NAME
switch(config-flow-exporter)# description Descripcion_del_exportador
switch(config-flow-exporter)# destination {IP_DEL_SERVIDOR} {PUERTO}
switch(config-flow-exporter)# transport udp {PUERTO}
switch(config-flow-exporter)# exit
```

## **Paso 4: Configurar los parámetros de exportación**

Configura los parámetros de exportación del flujo recién creado:
```
switch(config)# flow monitor MONITOR_NAME
switch(config-flow-monitor)# exporter EXPORTER_NAME
switch(config-flow-monitor)# record {cisco_sampler|netflow-original}
switch(config-flow-monitor)# exit
```

## **Paso 5: Aplicar el flujo de NetFlow en las interfaces**

Aplica el flujo de NetFlow que creaste anteriormente en las interfaces que deseas monitorear:
```
switch(config)# interface {tipo} {número_de_interfaz}
switch(config-if)# ip flow monitor MONITOR_NAME input
switch(config-if)# ip flow monitor MONITOR_NAME output
switch(config-if)# exit
```

## **Paso 6: Verificar la configuración**

Para verificar que la configuración de NetFlow se realizó correctamente, puedes usar el siguiente comando:
```
switch# show flow monitor MONITOR_NAME
```

Con esto, has configurado NetFlow en tu switch Cisco Nexus y estás listo para aprovechar sus ventajas.

## **5 Ejemplos Clásicos de Uso para NetFlow**

1. **Monitoreo del tráfico y Análisis de Ancho de Banda:**
NetFlow permite obtener información detallada sobre el tráfico que fluye a través de la red, como el origen y destino de los paquetes, el protocolo utilizado y el volumen de datos transferidos. Esto facilita el análisis del ancho de banda y la identificación de posibles cuellos de botella o abusos de la red.

2. **Detección de Problemas de Seguridad:**
Al analizar los datos de tráfico, NetFlow puede detectar patrones sospechosos, comportamientos maliciosos o intentos de intrusión en la red. Esto ayuda a los administradores a identificar y mitigar amenazas de seguridad en tiempo real.

3. **Identificación de Aplicaciones y Calidad de Servicio (QoS):**
NetFlow puede clasificar el tráfico por aplicación, lo que proporciona una visión clara de qué aplicaciones consumen más ancho de banda. Esto es útil para priorizar ciertos tipos de tráfico, mejorar la calidad de servicio y optimizar el rendimiento de aplicaciones críticas.

4. **Planificación de Capacidad y Optimización de la Red:**
El análisis histórico de datos de NetFlow permite a los administradores comprender cómo se utiliza la red con el tiempo y planificar adecuadamente la capacidad para futuros crecimientos. También ayuda a identificar áreas de mejora y optimizar la infraestructura de red existente.

5. **Facturación y Contabilidad:**
Para proveedores de servicios, NetFlow puede ser una herramienta valiosa para realizar el seguimiento del uso de la red por parte de diferentes clientes y aplicar políticas de facturación basadas en el consumo real de recursos.

# **Configurar Netflow usando Ansible**

Aquí tienes un ejemplo de un playbook de Ansible para configurar NetFlow en un Cisco Nexus Switch:

```yaml
---
- name: Configurar NetFlow en Nexus Switch
  hosts: tu_switch_nexus
  gather_facts: no
  connection: local

  vars:
    exporter_name: netflow_exporter
    server_ip: IP_DEL_SERVIDOR_NETFLOW
    server_port: PUERTO_DEL_SERVIDOR_NETFLOW
    monitor_name: netflow_monitor

  tasks:
    - name: Habilitar la función NetFlow
      nxos_command:
        commands:
          - feature netflow
      register: result
      ignore_errors: yes

    - name: Crear el flujo de NetFlow
      nxos_command:
        commands:
          - flow exporter {{ exporter_name }}
          - description Exportador_NetFlow
          - destination {{ server_ip }} {{ server_port }}
          - transport udp {{ server_port }}
      register: result

    - name: Configurar parámetros de exportación
      nxos_command:
        commands:
          - flow monitor {{ monitor_name }}
          - exporter {{ exporter_name }}
          - record netflow-original
      register: result

    - name: Aplicar el flujo de NetFlow en todas las interfaces
      nxos_command:
        commands:
          - interface range Ethernet1/1 - Ethernet1/48  # Personaliza el rango de interfaces según tus necesidades
          - ip flow monitor {{ monitor_name }} input
          - ip flow monitor {{ monitor_name }} output
      register: result

    - name: Guardar la configuración
      nxos_command:
        commands:
          - copy running-config startup-config
```

Asegúrate de reemplazar `tu_switch_nexus`, `IP_DEL_SERVIDOR_NETFLOW`, y `PUERTO_DEL_SERVIDOR_NETFLOW` con los valores apropiados para tu configuración. También, puedes personalizar el rango de interfaces en la tarea "Aplicar el flujo de NetFlow en todas las interfaces" según las interfaces que desees monitorear.

Guarda el contenido anterior en un archivo con extensión `.yml`, por ejemplo, `configurar_netflow.yml`, y ejecuta el playbook de Ansible con el siguiente comando:

```bash
ansible-playbook -i hosts_file configurar_netflow.yml
```

Asegúrate de tener acceso SSH habilitado en el switch Nexus y de que las credenciales de acceso estén definidas en el archivo `hosts_file` junto con la dirección IP del switch bajo el nombre `tu_switch_nexus`.


En resumen, la configuración de NetFlow en los switches Cisco Nexus ofrece una visibilidad profunda y en tiempo real del tráfico de red, lo que permite tomar decisiones más informadas y resolver problemas de manera eficiente. Esta tecnología es fundamental para mantener la seguridad, el rendimiento y la eficiencia en cualquier infraestructura de red. ¡Empieza a utilizar NetFlow y potencia tu gestión de red!