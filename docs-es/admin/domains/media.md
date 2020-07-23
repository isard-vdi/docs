<h1>Administrar medios</h1>

[TOC]

# Subir medios

Hay un botón Agregar nuevo que abrirá un nuevo formulario para que IsardVDI descargue medios desde una URL:

- **URL**: Pegue aquí cualquier URL de descarga de archivos ISO o disquete. (Los disquetes se pueden usar para agregar controladores de almacenamiento al instalar algunos sistemas operativos en el disco duro).

- **NOMBRE**: Propondrá por defecto el nombre del archivo extraído de la URL. Cámbiala si prefieres otra.
- **TIPO**: Seleccione el tipo de medio que se está cargand o (ISO / CDROM o disquete)
- **DESCRIPCIÓN**: Opcional
- **PERMITE**: Los roles, categorías, grupos y usuarios permitidos usar este medio. Por favor refiérase a permite el formulario. (../../user/allows.md#allows-form).

Cuando hagas clic en el botón **Cargar**, se iniciará y verás el progreso.


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

The URL to download mycdrom.iso in IsardVDI upload media will be **http://localhost:8000/mcdrom.iso**
