---
title: Instalemos Synth Shell en ubuntu 
date: 2023-07-15 00:00:00 -05:00
image:
  path: "/assets/img/synth-shell-example.png"
  src: "/assets/img/synth-shell-example.png"
  alt: Desuperpacking Meta Android Superpacked APKs
categories: [linux, ubuntu, shell, bash, scripting]
tags: [Linux, Ubuntu, Shell, bash, scripting, synth-shell]     # TAG names should always be lowercase
---

# Instalemos Synth Shell en ubuntu !

### Pero primero, que shell tenemos? 

```console
> echo $SHELL

/bin/bash
```

El resultado que buscamos debe ser `/bin/bash`, en caso contrato es recomendado instalar bash en tu terminal como prerequisito.

### Que es BASH?

+ BASH (Bourne Again SHell) es un intérprete de comandos de Unix y un lenguaje de programación de scripts. Es ampliamente utilizado en sistemas Linux y macOS. BASH proporciona una interfaz de línea de comandos para ejecutar comandos y escribir scripts que automatizan tareas en el sistema operativo. Permite la manipulación de archivos y directorios, el control de procesos, el redireccionamiento de la entrada/salida y la programación condicional. Su sintaxis es similar a otros shells de Unix y ofrece una gran flexibilidad y potencia para los administradores y usuarios de sistemas.

### Que es Synth-shell?

+ Conforme a lo descrito por el autor en https://github.com/andresgongora/synth-shell, se puede interpretar en que synth-shell mejora la experiencia y la productividad de su terminal a través de una combinación de pequeños scripts bash.

### Que queremos lograr con estos scripts?

![Bash por defecto][def1]
![Ejemplo Synth shell][def2]

## Pasos a seguir

Paso 0. En caso de no tenerlo. Instalar bash en Ubuntu:

```console
> sudo apt install bash
> chsh
```
Con respecto al segundo comando, sigues instrucciones para establecer por defecto `/bin/bash`

Paso 1. Instalamos  y validamos Git:

```console
> sudo apt install git

> git --version

git version 2.34.1

```

Paso 2. Instalamos Synth-shell en cualquier directorio que usen (Documentos esta bien):
```console
> git clone --recursive https://github.com/andresgongora/synth-shell.git
> cd synth-shell
```

Paso 3. Aseguramos que el setup es ejecutable y continuamos con la instalacion:

```console
> sudo chmod +x setup.sh

> ./setup.sh

```

{% include adsense-inarticlead.html %}

Dependiendo como vaya corriendo el script, vas a ver un cuestionario relevante a una nueva instalacion que te permitira definir lo que va a instalar, yo acepte todo por defecto, quizas querras leer que si te beneficia y que no:

![Synth-shell assistance][img3]

🚧 NOTA: Este script puede variar conforme a la aplicacion evoluciona.

## No te gustan los colores por defect? editemos!

Regularmente puede tener colores que no sean compatibles con tus gustos, por lo tanto es posible cambiarlos sin mayor problema:

```bash
> gedit ~/.config/synth-shell/synth-shell-prompt.config
```

## Hablemos de los parametros includidos aqui y como editarlos

### Estructura del prompt:

 -  USER: shows the user's name.
 -  HOST: shows the host's name.## -   PWD: shows the current directory.
 -   GIT: if inside a git repository, shows the name of current branch.
 - PYENV: if inside a Python Virtual environment.
 -    TF: if inside a Terraform Workspace.
 - CLOCK: shows current time in H:M format.
 - INPUT: actual bash input.

### Colores permitidos:

- white black light-gray dark-gray
red green yellow blue cyan purple
light-red light-green light-yellow light-blue light-cyan light-purple

### Una opcion mas interesante aun es:

- Values in the range [0-255] for 256 bit colors. To check all number-color
pairs for your terminal, you may run the following snippet by HaleTom:

```console
curl -s https://gist.githubusercontent.com/HaleTom/89ffe32783f89f403bba96bd7bcd1263/raw/ | bash
```
### Impresionante resultado para tomar tu color preferido sin salir de bash:

![Color palette bash][img4]


# Para desinstalar completamente:

```console
> ./setup.sh
```
![Uninstalling][img5]

Y naturalmente escoger la opcion de desinstalar.

Igualmente podemos elimiar los archivos residuales generados por los scripts:

```console
> rm -r ~/.config/synth-shell/
```

[def1]: ../../assets/img/default_ubuntu_shell.png
[def2]: ../../assets/img/synth-shell-example.png
[img3]: ../../assets/img/synth-shell-assistance.png
[img4]: ../../assets/img/color-palette-bash.png
[img5]: ../../assets/img/unisnstall-example.png
    

