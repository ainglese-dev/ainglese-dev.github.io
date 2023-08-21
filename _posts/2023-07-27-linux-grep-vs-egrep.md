---
title: Linux grep vs egrep 
date: 2023-07-27 00:00:00 -05:00
image:
  path: "/assets/img/linux-on-a-router.png"
  src: "/assets/img/linux-on-a-router.png"
  alt: linux on a router
categories: [linux, CLI]
tags: [grep, egrep, linux]     # TAG names should always be lowercase
---

**Aprendamos la diferencia entre grep vs egrep**

En Linux, tanto `grep` como `egrep` son utilidades de línea de comandos que se utilizan para buscar patrones de texto en archivos o en la salida de otros comandos. Ambas herramientas se basan en expresiones regulares para realizar sus búsquedas, pero hay una diferencia clave entre ellas:

1. `grep`: Es la abreviatura de "Global Regular Expression Print". En su forma básica, `grep` realiza búsquedas utilizando expresiones regulares básicas (BRE). Las expresiones regulares básicas son un conjunto más limitado de patrones de búsqueda en comparación con las expresiones regulares extendidas (`egrep`). Por defecto, `grep` no interpreta los metacaracteres extendidos como ?, +, |, (), etc., a menos que se escapen con una barra invertida (\). Puedes activar el uso de expresiones regulares extendidas con la opción `-E`.

2. `egrep`: Es la abreviatura de "Extended Global Regular Expression Print". `egrep` es equivalente a `grep -E` y utiliza expresiones regulares extendidas (ERE). Las expresiones regulares extendidas ofrecen una mayor flexibilidad y permiten el uso de más metacaracteres sin necesidad de escaparlos con una barra invertida. Con `egrep`, puedes utilizar metacaracteres como ?, +, |, (), etc., directamente en tus patrones de búsqueda.

Ejemplo de `grep`:

Supongamos que tenemos un archivo llamado "archivo.txt" con el siguiente contenido:

```
apple
banana
orange
grape
pear
watermelon
```

Si queremos buscar todas las líneas que contienen la palabra "apple" o "orange", podemos usar `grep` de la siguiente manera:

```bash
grep 'apple\|orange' archivo.txt
```

Esto imprimirá las líneas que contienen "apple" o "orange" en el archivo:

```
apple
orange
```

Ejemplo de `egrep`:

Usando el mismo archivo "archivo.txt", podemos lograr el mismo resultado utilizando `egrep` sin necesidad de escapar el carácter "|":

```bash
egrep 'apple|orange' archivo.txt
```

El resultado será el mismo:

```
apple
orange
```

En resumen, la principal diferencia entre `grep` y `egrep` es la sintaxis de las expresiones regulares que utilizan. Si necesitas utilizar metacaracteres extendidos sin escapar, debes usar `egrep` o `grep -E`. Si solo necesitas patrones básicos, puedes usar `grep`.
