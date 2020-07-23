# Isard**VDI**

Despliegue VDI de software libre basado en la virtualización con Linux KVM y docker.

## Inicio rápido

Get the docker-compose file and bring it up:

```
wget https://raw.githubusercontent.com/isard-vdi/isard/master/docker-compose.yml
docker-compose pull
docker-compose up -d
```

Browse to https://localhost and follow wizard. You are ready to use virtual desktops:

![](images/install.gif)

- Start **demo desktops** and connect to it using your browser and spice or
vnc protocol. Nothing to be installed, but already secured with certificates.
- Install virt-viewer and connect to it using the spice client. **Sound
and USB** transparent plug will be available.

Download new precreated desktops, isos and lots of resources from the **Updates** menu.

Create your own desktop using isos downloaded from Updates or **Media**
menu option. When you finish installing the operating system and
applications create a **Template** and decide which users or categories
you want to be able to create a desktop identical to that template. Thanks to the **incremental disk creation** all this can be done within
minutes.

<iframe src="https://drive.google.com/file/d/1tPL12yw3MEV5IEPL5by7z76zVVSNnAng/preview" width="640" height="480"></iframe>

Don't get tied to an 'stand-alone' installation in one server. You can
add more hypervisors to your **pool** and let IsardVDI decide where to
start each desktop. Each hypervisor needs only the KVM/qemu and libvirt
packages and SSH access. You should keep the storage shared between
those hypervisors.

We currenly manage a **large IsardVDI infrastructure** at Escola del
Treball in Barcelona. 3K students and teachers have IsardVDI available
from our self-made pacemaker dual nas cluster and six hypervisors,
ranging from top level intel server dual core mainboards to gigabyte
gaming ones.

We have experience in different **thin clients** that we use to lower renovation and
consumption costs at classrooms.

[IsardVDI Project website](http://www.isardvdi.com/)

### Authors
+ Josep Maria Viñolas Auquer
+ Alberto Larraz Dalmases

### Contributors
+ Daniel Criado Casas
+ Néfix Estrada

### Support/Contact
Please send us an email to info@isardvdi.com if you have any questions or fill in an issue.
