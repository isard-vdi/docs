<h1>Media</h1>

Desde el administrador de medios, puede cargar imágenes ISO y de disquete que le permitirán crear nuevos escritorios que arranquen desde él.

[TOC]

# Subir medios

Hay un botón **Agregar nuevo** que abrirá un nuevo formulario para que IsardVDI descargue medios desde una URL:

![](../images/media/media-upload.png)

- **URL**: Pegue aquí cualquier URL de descarga de archivos ISO o disquete. (Los disquetes se pueden usar para agregar controladores de almacenamiento al instalar algunos sistemas operativos en el disco duro).
- **NOMBRE**: Propondrá por defecto el nombre del archivo extraído de la URL. Cámbiala si prefieres otra.
- **TIPO**: seleccione el tipo de medio que se está cargando (ISO / CDROM o disquete)
- **DESCRIPCIÖN**: Opcional
- **PERMITE**: Los roles, categorías, grupos y usuarios permitidos usar este medio. Por favor refiérase a permite el formulario.

Cuando hagas clic en el botón **Cargar**, se iniciará y verás el progreso.

![](../images/media/media-downloading.png)

## Carga de medios desde el almacenamiento local

Si tiene su ISO localmente en su almacenamiento y desea subirlo a IsardVDI, puede crear un servidor web simple que pueda completar la url del archivo en el formulario de medios de carga en IsardVDI.

**Ejemplo de servidor web Python**

Si tiene el archivo *mycdrom.iso* en una carpeta, puede iniciar un servidor web de Python:

```
$ ls .
mycdrom.iso
$ python -m SimpleHTTPServer
Serving HTTP on 0.0.0.0 port 8000 ...
```

La URL para descargar mycdrom.iso en los medios de carga IsardVDI será:   **http://localhost:8000/mycdrom.iso**

# Crear nuevo escritorio a partir de medios cargados

Esta es la forma habitual de crear un escritorio nuevo y completo a partir de su ISO cargado. Verá un icono de escritorio verde junto a la carga finalizada.

![](../images/media/media-uploaded.png)

Al hacer clic en él, se mostrará una nueva creación de formulario de escritorio:

![](../images/media/media-desktop-form.png)

- **Nombre y descripción del escritorio**: Complete el nombre y la descripción del nuevo escritorio que se está creando.
- **Selected ISO/Floppy selecciona para iniciar**:  tiene la opción de verificar si se trata de una ISO propietaria de Win para instalar. Esto también agregará una segunda ISO con controladores optimizados para Win virtual (si el administrador ya descargó de las actualizaciones).
- **Seleccionar plantilla de sistema operativo**: Seleccione la plantilla que mejor se adapte a su instalación. No necesita ser la instalación exacta, solo una plantilla similar, ya que esto solo establecerá el hardware genérico simulado para este escritorio.
- **Ajustar la plantilla del SO Hardware**: puede configurar el hardware dentro de su cuota de usuario. De forma predeterminada, la opción de inicio se establecerá en CD / DVD y no podrá modificarse si planea crearla

Cuando termine y haga clic en el botón **Crear escritorio**, se creará un escritorio. Vaya al menú de Escritorios para comenzar, conéctese y comience la instalación desde el ISO seleccionado.


![](../images/media/debian-install.png)

***NOTA***: Cuando finaliza la instalación del sistema operativo, generalmente el sistema operativo invitado solicitará un reinicio. Simplemente puede apagar su sistema operativo invitado y luego editar el escritorio para cambiar el orden de arranque de CD / DVD a DISCO DURO. También puede eliminar la ISO de la sección de medios de escritorio de edición si ya no la necesita.


## Crear un escritorio de arranque de red (PXE)


Puede cargar cualquier archivo ISO (puede ser un archivo falso que termina con .iso que creó con el toque, por ejemplo) [creating new desktop from uploaded media](media.md#create-new-desktop-from-uploaded-media) y luego seguir el proceso de creación de un nuevo escritorio desde el medio cargado, pero seleccionando PXE en el cuadro de selección de Inicio en lugar de CD / DVD.
