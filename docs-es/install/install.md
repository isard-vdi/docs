<h1>Instalación</h1>

IsardVDI ahora funciona fuera de la caja con docker y docker-compose

[TOC]

# Requerimientos

Solo necesita tener instalado el servicio docker y el docker-compose:

- Instalar **Docker**: https://docs.docker.com/engine/installation/

  - Nota: la ventana acoplable 17.04 o más reciente es necesaria para la ventana acoplable compose.yml v3.2

    - ```bash
      sudo apt install docker
      ```

- Instalar **docker-compose**: https://docs.docker.com/compose/install/

Nota: docker-compose 1.12 o más necesario para docker-compose.yml v3.5. Puedes instalar la última versión usando pip3:

    - ```bash
      sudo apt install python3-pip -y
      sudo pip3 install docker-compose
      ```

**NOTA**: Su hardware necesita tener la virtualización habilitada. Puede comprobarlo en su BIOS pero también en CLI:

```
egrep ‘(vmx|svm)’ /proc/cpuinfo
```
Si no ve nada en la salida, su CPU no tiene capacidades de virtualización o están deshabilitadas en el BIOS.

# Inicio rápido

Para que aparezca IsardVDI, solo necesita descargar el archivo docker-compose.yml (o clonar el repositorio completo si desea compilar las imágenes usted mismo) y abrirlo:

```bash
wget https://raw.githubusercontent.com/isard-vdi/isard/master/docker-compose.yml
docker-compose pull
docker-compose up -d
```

Eso es todo, solo conéctese a https: // <ip | domain> del servidor y siga el asistente.


Nota: si se conecta con su navegador al asistente a través de localhost o 127.0.0.1, el nombre de host del espectador en isard-hypervisor se configurará para esa IP y nadie podrá abrir espectadores desde otras computadoras. Consulte la resolución de problemas del nombre de host del visor incorrecto para modificar la IP del visor.

# Insights

## Trayectorias mapeadas

IsardVDI creará las siguientes rutas en su sistema y las asignará dentro de los hipervisores y los contenedores de aplicaciones:

- **/opt/isard**: La carpeta principal que contendrá:
  - **bases**: Ruta donde se almacenarán las imágenes de la plantilla base. La ruta completa incluirá /opt/isard/bases/<role>/<category>/<group>/<username>/<base_disk_name.qcow2>`
  - **plantillas**: Ruta donde se almacenarán las imágenes de la plantilla del usuario. La ruta completa incluirá /opt/isard/templates/<role>/<category>/<group>/<username><tmpl_disk_name.qcow2>`
  - **grupos**: Ruta donde se almacenarán las imágenes de escritorio ejecutables. La ruta completa incluirá /opt/isard/group/<role>/<category>/<group>/<username>/<desktop_disk_name.qcow2>`
  - **medios**: Ruta donde se cargarán los medios (archivos iso y de disquete). La ruta completa incluirá /opt/isard/media/<role>/<category>/<group>/<username>/<media_ (iso | floppy).(Iso | fd)>
  - **copias de seguridad**: Las copias de seguridad de la base de datos creadas en la interfaz web utilizando el menú de configuración de copia de seguridad se almacenarán aquí.
  - **subidas**: (trabajo en progreso)
  - **registros**: Aquí tendrá registros para todos los contenedores. Tenga en cuenta que podrían crecer, por lo que deberían rotarse / eliminarse programáticamente.
  - **certificados**: Aquí se almacenan los certificados para la interfaz de usuario web y las conexiones del visor. También puede reemplazar los certificados autofirmados iniciales con sus comerciales / leemos encriptados siguiendo la guía de documentación sobre el reemplazo de certificados. En la versión actual, el sitio web de IsardVDI y los usuarios hacen uso de los mismos certificados almacenados en /opt/isard/certs/default/path location.

## Construye tus imágenes docker

Si prefiere crear sus imágenes de docker basadas en alpino IsardVDI, tiene que clonar el repositorio completo (clon git https://github.com/isard-vdi/isard.git) y encontrará las fuentes de docker en la carpeta docker:

- **alpine-pandas**: Esta imagen tiene las bibliotecas de python3 pandas y numpy. Las compilaciones de esas bibliotecas llevan mucho tiempo y, como son necesarias para los hipervisores y los contenedores de aplicaciones, decidimos crear esta imagen como base para esas imágenes.
- **hipervisor**: Tomará la imagen de los pandas alpinos como base y abrirá un hipervisor KVM / qemu completo con libvirt y un proxy websockets de Python para el acceso del navegador a los espectadores.
- **aplicaciones**: Agregará todas las bibliotecas necesarias para ejecutar el motor y la aplicación web Isard VDI código fuente contenido en la carpeta / src del repositorio.
- **nginx**: Tiene el servidor web https optimizado para socketio y websockets. También tiene fuentes para páginas de error y websocket vnc y visores de especias.
- **rethinkdb**: Usamos la imagen oficial rethinkdb de  https://hub.docker.com/_/rethinkdb/.

Proporcionamos un script de compilación para los estibadores. Solo necesitas agregar el parámetro de la versión:

```bash
./build-docker-images.sh 1.0.1
```

Después de crear imágenes desde la fuente, puedes iniciarlo con docker-compose up -d.

NOTA: compruebe la versión de los contenedores en el archivo docker-compose.yml para compilar la misma versión.

# Instalaciones de muestra

## Debian 9 Stretch

Con una nueva instalación de Debian 9 puede instalar docker y docker-compose con estos comandos.

### Instalar docker

```bash
apt-get remove docker docker-engine docker.io containerd runc
apt-get install     apt-transport-https     ca-certificates     curl     gnupg2     software-properties-common
curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"
apt-get update
apt-get install docker-ce
```

### Instalar docker-compose
```bash
apt install python3-pip
pip3 install docker-compose
```
## Fedora 28-29

Con una nueva instalación de Fedora 28-29, puede instalar la ventana acoplable y la ventana acoplable composa con estos comandos.

### Instalar docker

```bash
sudo dnf remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine

sudo dnf -y install dnf-plugins-core
sudo dnf config-manager \
    --add-repo \
    https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

### Instalar docker-compose
```bash
yum install python3-pip
pip3 install docker-compose
```

# Solución de problemas de instalación

## docker-compose up -d se niega a iniciar el hipervisor

Puede haber dos posibles fuentes para este problema. Uno es el uso de un servicio en su host que se encuentra en un puerto en el rango de puertos predeterminados utilizados por el hipervisor y los espectadores. Esos puertos son:

- 5900-5949: rango de puertos del visor de especias
- 55900-55949: puertos del visor del navegador web.

Puede verificar sus puertos de escucha emitiendo el comando netstat -tulpn y verificando si alguno de sus puertos de escucha se superpone con el rango de puertos del hipervisor.

No hay una solución fácil para esto sin cerrar su servicio antes de iniciar IsardVDI. Sucede que Chromium en Linux comienza a escuchar el servicio mDNS en el puerto 5353. Esto se puede resolver fácilmente usando otro navegador como Firefox para acceder a IsardVDI o iniciar contenedores de IsardVDI antes de abrir Chrome. Tenga en cuenta que esto ocurrirá solo si va a utilizar la misma computadora para ejecutar IsardVDI y acceder a ella a través de un navegador local en el servidor.

## Después de hacer el asistente, los detalles del hipervisor dicen que no hay virtualización disponible


Algunas CPU (en su mayoría antiguas) no tienen virtualización de hardware, otras la tienen pero está desactivada en BIOS. En el primer caso no hay nada que se pueda hacer. Si está deshabilitado en el BIOS, debe verificar VT-X o Virtualización o SVM y activarlo.

[More info in admin faqs](../admin/faq.md#after-finishing-install-the-default-isard-hypervisor-is-disabled)

# Instalación nested en KVM

Verifique la opción de virtualización anidada en su sistema operativo host:

- Procesadores Intel: ```cat /sys/module/kvm_intel/parameters/nested```
- Procesadores AMD: ```cat /sys/module/kvm_amd/parameters/nested```

Debería mostrar un **1** o **Y** si está habilitado.


Deberá habilitar la virtualización anidada en su sistema operativo host si aún no está activo.

## Nested virt en Procesadores Intel

### Live

Con todas las máquinas virtuales detenidas, elimine el módulo kvm_intel
```
modprobe -r kvm_intel
```
Y vuelve a cargarlo con opción nested:
```
modprobe kvm_intel nested=1
```

### Permanente

Cree el archivo ```/etc/modprobe.d/kvm.conf``` y agregue dentro de:
```
options kvm_intel nested=1
```

## Nested virt in AMD processors:

### Live

With all VMs stopped remove kvm_amd module
```
modprobe -r kvm_amd
```
Y vuelve a cargarlo con opción anidada
```
modprobe kvm_amd nested=1
```

###  Permanente

Cree el archivo ```/etc/modprobe.d/kvm.conf``` y agregue dentro de:
```
options kvm_amd nested=1
```

# Instalación de IsardVDI dentro de VMWare ESXi guest

Habilitar el paso de la CPU del host al invitado. Eso debería bastar.
