<h1>Hypervisores</h1>

[TOC]

# Tipos de hipervisores

## Hipervisor puro

Cuando edita o crea un hipervisor, puede verificar si será solo un hipervisor. En este modo, IsardVDI sólo usará el servidor para ejecutar escritorios virtuales. No se manejan operaciones de discos virtuales por este servidor.

## Operaciones de disco puro

En este modo, el servidor solo se utilizará para manejar las operaciones de disco de los escritorios virtuales.

## Operaciones mixtas de hipervisor + disco.

Este modo utilizará el servidor tanto para ejecutar escritorios virtuales como para ejecutar operaciones de disco virtual.

# Hipervisores externos

El método preferido para configurar un nuevo hipervisor es usar la imagen isard-genérica-hiper de la ventana acoplable, ya que mostrará todos los servicios necesarios en cualquier host de Linux con la ventana acoplable. Se requieren tres pasos:

1. Instalar el hipervisor KVM (se recomienda la ventana acoplable)
2. Añadir claves ssh para el nuevo hipervisor KVM
3. Crear un nuevo hipervisor en la interfaz de usuario web de IsardVDI.

## Instalar KVM hipervisor

### Docker Install (preferido)

La configuración rápida y fácil de un hipervisor externo se podría realizar utilizando la imagen external_hyper. En su servidor Linux instalado y limpio, será el contenedor de inicio del hipervisor KVM:

```
cd dockers/external_hyper
docker-compose up -d
```

Eso debería mostrar un hipervisor Isard completo en su servidor.

Los certificados reales en la ruta de IsardVDI / opt / isard / certs / default / deben copiarse en la nueva ruta creada por el host del servidor KVM / opt / isard / certs / default y el contenedor se debe reiniciar con el reinicio de la función docker-compose.

Ejecute este comando para restablecer las claves ssh y la contraseña:

```bash
docker exec -e PASSWORD=<YOUR_ROOT_PASSWORD> externalhyper_isard-hypervisor_1 bash -c '/reset-hyper.sh'
```

Después de eso, puede continuar agregando claves ssh para este hiper en su contenedor IsardVDI isard-app y crear un nuevo hipervisor en la interfaz de usuario:

```bash
docker exec -e HYPERVISOR=<IP|DNS> -e PASSWORD=<YOUR_ROOT_PASSWD> -e PORT=2022 isard_isard-app_1 bash -c '/add-hypervisor.sh'; history -d $((HISTCMD-1))
```

### Manual de instalación

Si desea utilizar otro host como el hipervisor KVM, asegúrese de que tenga:

- **libvirtd**:
  - * **servicio en ejecución**: ```systemctl status libvirtd```. Verifique con su distro cómo instalar un hipervisor KVM completo:

      * https://wiki.debian.org/KVM#Installation
      * https://help.ubuntu.com/community/KVM/Installation
      * https://wiki.centos.org/HowTos/KVM
      * https://wiki.alpinelinux.org/wiki/KVM

    **Certificados habilitados**: Actuales certificados de ruta en isardvdi: ```/opt/isard/certs/default/``` debe ser copiado a su nueva ruta de servidor KVM ```/etc/pki/libvirt-spice```. También se deben agregar algunas lineas a *libvirtd.conf* y *qemu.conf* para hacer uso de los certificados:

      * ```bash
        echo "listen_tls = 0" >> /etc/libvirt/libvirtd.conf;
        echo 'listen_tcp = 1' >> /etc/libvirt/libvirtd.conf;
        echo 'spice_listen = "0.0.0.0"' >> /etc/libvirt/qemu.conf
        echo 'spice_tls = 1' >> /etc/libvirt/qemu.conf
        ```

    * **Servidor websocket**: Para permitir la conexión a los espectadores a través de navegadores, también se debe configurar un servidor websocket. Deberías mirar el *start_proxy.py* included dockers/hypervisor.
- **Servicio ssh en ejecución y accesible para el usuario**: ```systemctl status sshd```
- **Rizo**: Se utilizará para obtener actualizaciones
- **qemu-kvm**: Deberías crear un enlace a tu qemu-system-x86_64 como este ```ln -s /usr/bin/qemu-system-x86_64 /usr/bin/qemu-kvm```

## Añadir claves ssh para el nuevo hipervisor

Antes debe agregar claves ssh para acceder a ese hipervisor. Proporcionamos un script dentro del contenedor de la aplicación isard para hacer esto:

```bash
docker exec -e HYPERVISOR=<IP|DNS> -e PASSWORD=<YOUR_ROOT_PASSWD> isard_isard-app_1 bash -c '/add-hypervisor.sh'
```

**Nota**: Hay parámetros opcionales que se pueden configurar:

- PUERTO: Por defecto es*22*
- USUARIO: Por defecto es *root*

Después de hacer esto, debe ir al menú de hipervisores y agregar un nuevo hipervisor con la misma IP / DNS. El motor intentará conectarse cuando habilites ese hipervisor.

**Ejemplo:** Tenga en cuenta que cuando se agrega ; history -d $((HISTCMD-1)) al comando para evitar que la contraseña se mantenga en nuestro historial

```bash
root@server:~/isard# docker exec -e HYPERVISOR=192.168.0.114 -e PASSWORD=supersecret isard_isard-app_1 bash -c '/add-hypervisor.sh'; history -d $((HISTCMD-1))
fetch http://dl-cdn.alpinelinux.org/alpine/v3.8/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.8/community/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/edge/testing/x86_64/APKINDEX.tar.gz
(1/1) Installing sshpass (1.06-r0)
Executing busybox-1.28.4-r2.trigger
OK: 235 MiB in 127 packages
Trying to ssh into 192.168.0.114...
# 192.168.0.114:22 SSH-2.0-OpenSSH_7.6p1 Ubuntu-4ubuntu0.1
# 192.168.0.114:22 SSH-2.0-OpenSSH_7.6p1 Ubuntu-4ubuntu0.1
# 192.168.0.114:22 SSH-2.0-OpenSSH_7.6p1 Ubuntu-4ubuntu0.1
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/root/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
expr: warning: '^ERROR: ': using '^' as the first character
of a basic regular expression is not portable; it is ignored

/usr/bin/ssh-copy-id: WARNING: All keys were skipped because they already exist on the remote system.
		(if you think this is a mistake, you may want to use -f option)

    Hipervisor de acceso ssh otorgado.
    El acceso a 192.168.0.114 concedió y encontró el servicio libvirtd en ejecución.
    Ahora puede crear este hipervisor en la interfaz web de IsardVDI.
```

Si falla, desactive el hipervisor, edite los parámetros y habilítelo nuevamente. El motor IsardVDI verificará la conexión nuevamente. Puede ver el mensaje fallido en los detalles del hipervisor (haga clic en el botón + para ver los detalles).

Si el motor no está intentando conectarse nuevamente después de habilitar el nuevo hipervisor, siga para reiniciar el contenedor de la aplicación isard:

```bash
docker restart isard_isard-app_1
```

**NOTA**: Probablemente desee eliminar el hipervisor isard predeterminado después de crear un nuevo hipervisor externo. Desactive el hipervisor isard y elimínelo de los detalles del hipervisor. Una acción de eliminación forzada y el reinicio del contenedor de la aplicación isard se pueden realizar posteriormente.

## Añadir hipervisor en la interfaz de usuario web



# Conceptos de infraestructura

Cuando agrega un hipervisor externo, debe tener en cuenta (y configurar según sea necesario) de:

- **Discos de ruta**: Por defecto almacenará discos en /opt/isard.No es necesario crear esa carpeta, pero se recomienda que el almacenamiento rápido de IO (como el disco nvme) esté montado en esa ruta.
- **Rendimiento de la red**: Si va a utilizar un NAS, debe tener en cuenta que se debe utilizar una red rápida. Velocidad de red recomendada superior a 10Gbps.

## Discos de rendimiento

Como se usan diferentes rutas para las bases, plantillas y discos de grupos, se podría mejorar el rendimiento al montar diferentes discos de almacenamiento nvme en cada ruta:

- **/opt/isard/bases**: Allí residirán las imágenes base que crees. Por lo general, esas imágenes de disco se utilizarán como base para muchas plantillas y discos de usuario (grupos), por lo que el almacenamiento debe proporcionar un acceso de lectura lo más rápido posible.
- **/opt/isard/templates**: Las plantillas también se utilizarán como discos principales para múltiples discos de usuario (grupos). Así que mejor usa un disco de acceso de lectura rápida.
- **/opt/isard/groups**: Los escritorios en ejecución residirán aquí. Por lo tanto, aquí se realizará una gran cantidad de IOs simultáneas, por lo que el almacenamiento debe proporcionar la mayor cantidad posible de IOPS. Los discos NVME (o incluso las incursiones de NVME) brindarán el mejor rendimiento.

## Rendimiento de la red


La red de almacenamiento entre los hipervisores y los servidores de almacenamiento NAS debe ser de al menos 10 Gbps. Todos los hipervisores deben montar el almacenamiento en la ruta / isard y ese montaje debe ser el mismo para todos los hipervisores.

Como puede elegir al crear un hipervisor para ser un hipervisor puro, operaciones de disco puras o una mixta, puede agregar servidores de almacenamiento NAS como operaciones de disco puras. Esto traerá una manipulación más rápida de los discos, ya que IsardVDI hará uso del almacenamiento directo y los comandos en el NAS.

Contamos con una infraestructura IsardVDI con seis hipervisores y dos marcapasos con NAS que se hacen a sí mismos y que comparten el almacenamiento con NFSv4 y el rendimiento es muy bueno.

## Documentos técnicos de alta disponibilidad / alto rendimiento

Intentamos mantener todo nuestro conocimiento y experiencia en clústeres de alto rendimiento y disponibilidad mediante el uso de marcapasos, drbd, cachés de disco, migraciones en vivo, etc. en el sitio web de tecnología IsardVDI de thedocs.
 [thedocs](http://thedocs.isardvdi.com) IsardVDI tech website.

No se pierda nuestros videos [live virtual desktop and storage](http://thedocs.isardvdi.com/clusters/live-migration/) sobre la migración de almacenamiento y escritorio virtual en vivo que hicimos en nuestra infraestructura: migrar el escritorio virtual de un hipervisor a otro mientras AL MISMO TIEMPO migramos su almacenamiento de un almacenamiento NAS a otro sin que el usuario del escritorio virtual sea consciente de lo que sucedió!
