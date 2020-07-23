<h1>Grafana</h1>

[TOC]

# Introducción

Grafana es una interfaz web que permite mostrar cuadros de mandos con gráficos. Con esta ventana acoplable tendrá estadísticas en tiempo real sobre su IsardVDI.

![](../images/extras/grafana/sample.png)

# Instalación

En la carpeta extras / grafana está el archivo de composición acoplable. Puedes subir la ventana acoplable grafana tirando de la imagen:

```
docker-compose pull
docker-compose up -d
```

O puedes construirlo

```
./build.sh <image version>
```

# Configuración

En su interfaz web IsardVDI, como administrador, vaya al **menú de configuración** y active grafana. Compruebe que los parámetros son correctos (para una instalación independiente de IsardVDI, debería ser)

# Acceso

Conéctese a su servidor IsardVDI en el puerto 3000 para acceder a los paneles de grafana.

El usuario predeterminado es **admin** y la contraseña isardvdi. Es muy recomendable que cambie la **contraseña** predeterminada.

# Servidor remoto Grafana

Puedes poner tu grafana en otro servidor tirando (o construyendo) y ejecutando allí el yml remoto:

```
docker-compose pull
docker-compose -f remote-grafana.yml up -d
```

Luego debe modificar la configuración de grafana en el menú de configuración de IsardVDI para permitir el acceso a este host de grafana remoto. 
