<h1>Admin FAQs & Troubleshooting</h1>

[TOC]

# Instalación

## Después de terminar de instalar el isard-hypervisor predeterminado estará deshabilitado

Si abre los detalles del hipervisor (haga clic en el botón +) verá mensajes del motor IsardVDI que pueden ser útiles para determinar qué sucedió. Si acaba de finalizar el asistente de instalación e isard-hypervisor está deshabilitado, probablemente el problema sea con las capacidades de virtualización.

Su hardware necesita tener la virtualización habilitada. Puede comprobarlo en su BIOS pero también en CLI:

```bash
egrep ‘(vmx|svm)’ /proc/cpuinfo
```

Si no ve nada en la salida, su CPU no tiene capacidades de virtualización o están deshabilitadas en BIOS. Verifique que su CPU tenga esa capacidad y que no esté deshabilitada en la BIOS.

También puede verificar esa capacidad dentro del contenedor en ejecución del hipervisor isard emitiendo el comando:

```bash
sudo docker exec -it isard-hypervisor virsh capabilities |grep "feature name"
```

# Espectadores

## Intenta conectarte a localhost o IP / nombre de host incorrecto

Deshabilite el hipervisor isard en el menú Hipervisores y verifique que el nombre de host del espectador sea correcto:

1. Deshabilitar el hipervisor isard en el menú del hipervisor
2. Editar hypervisor (botón editar detalles)
3. Comprobar / actualizar la dirección de los espectadores del cliente. Debe ser el nombre de host / IP donde se está ejecutando IsardVDI y debe ser accesible desde los clientes.
4. Habilite nuevamente el hipervisor isard e inicie un nuevo dominio para verificar la conexión del visor.

### Explicación

Probablemente hiciste el asistente usando 'localhost' o una dirección IP diferente a la que intentas conectar ahora con los espectadores. Eso llevó a un hipervisor isard configurado con 'localhost' o esa dirección IP del servidor incorrecta.

Siempre puede modificar la dirección del visor utilizada para los clientes que editan el hypervisor isard. Para editarlo, primero debe desactivar el hypervisor isard haciendo clic en el botón Habilitar / Deshabilitar:


![](../images/admin/faq/viewer_hyper_disabled.png)

y luego se habilitará el botón de edición para que pueda acceder al formulario de edición del hipervisor isard:

![](../images/admin/faq/viewer_hyper_form.png)

Hay dos IPs de visor que se pueden configurar:

- **Dirección del cliente**: Esta IP / dns será la utilizada en los espectadores de su red que se conectan a su servidor IsardVDI. Por lo tanto, debe configurarse con la dirección IP real de su servidor.
- **Dirección nat del cliente**: Esta dirección sólo tiene sentido si planea dar acceso a este servidor desde fuera de su organización a través de un enrutador NAT. Si este es su caso, también debe configurar aquí la dirección IP estática externa (o nombre de dominio o nombre de dominio dinámico) desde donde se puede acceder.
  - La opción de desplazamiento TCP de los espectadores del cliente está ahí para compensar el puerto predeterminado del rango del visor 5900-5949 accesible desde la red interna a otro rango desde el exterior a través del NAT. Por ejemplo, tenemos una configuración con seis hipervisores y todos son accesibles desde dentro con un rango predeterminado (5900-5949), pero configuramos reglas de NAT en nuestro enrutador externo para que cada uno de los hipervisores tenga un puerto de mapeo externo que no coincida. otros. IsardVDi intentará adivinar que el cliente se conectará desde dentro de su organización o desde afuera y configurará la dirección del espectador o la dirección nat del espectador dependiendo de cada conexión de cliente de origen.

  Por lo tanto, normalmente solo tiene que modificar la dirección de los espectadores del Cliente y habilitar nuevamente el hipervisor.

NOTA: *Al habilitar y deshabilitar los hipervisores, se recomienda reiniciar el contenedor de la aplicación isard, ya que este proceso no es completamente confiable ahora y podría fallar. Para reiniciarlo haz:*

```bash
sudo docker-compose restart isard-app
```

Esto no afecta a las máquinas virtuales actualmente iniciadas.

## ¿Cómo puedo obtener la contraseña cuando me conecto con el cliente VNC en Win?

Abra el visor del navegador vnc y tendrá la contraseña en la url como parámetro. Cópialo para usarlo con tu cliente vnc en Win.

# Actualizaciones

## ¿Qué usuario y contraseña tienen los dominios descargados?

Todos los dominios que pueden descargarse de las actualizaciones tienen, de forma predeterminada, el nombre de usuario y la contraseña pirineus. El usuario isard también tiene privilegios de superusuario.

# Certificados

## No se puede acceder a Isard después de reemplazar los certificados

El nuevo certificado se utilizará para acceder a su servidor web IsardVDI ahora. Verifique que lo esté usando en su navegador (normalmente habrá un icono de casillero antes de ingresar la URL).

- En caso de que no esté utilizando los nuevos registros de certificados, verifique los registros del contenedor de archivos de nginx al abrirlo con docker-compose (sin demonizarlo con -d). Allí podrás ver información sobre el certificado que se encuentra en la carpeta.

Verifique los certificados ahora en la carpeta predeterminada / opt / isard / certs / default. El código debería haber generado un ca-cert.pem (certificado de servidor extraído de fullchain). Puede eliminar todos los certificados, volver a colocar los nuevos y comenzar con la función docker-compose up.


## ¿Qué conexiones de espectador están encriptadas?

La información del certificado se mostrará en el menú Hipervisores -> Grupo predeterminado, que muestra que se encuentra en modo Seguro y la información del dominio tomada del certificado actualizado.

Puede conectarse a un escritorio en ejecución con el cliente de especias y ver [[Encrypted]] en la barra de la ventana. También conéctese a través de vnc o spice websockets y verifique que esté usando https URI con el certificado provisto. Estas son las conexiones de visor que se pueden cifrar mediante certificados:

- Cliente de especias (visor remoto)
- Spice websockets (navegador)
- VNC websockets (navegador)

Tenga en cuenta que el cliente VNC no se puede cifrar y debe usar un túnel como se describe en la sección de espectadores.

## Las conexiones del visor no están encriptadas.

En caso de que no esté utilizando certificados para acceder a los espectadores después de que haya reemplazado los certificados y el servidor web IsardVDI lo esté utilizando correctamente, debe haber un error server-cert.pem. IsardVDI extrae un ca-cert.pem del servidor-cert.pem dado. Compruebe que la clave ca-cert.pem extraída sea la correcta con:

- `openssl x509 -in /opt/isard/certs/default/ca-cert.pem -text`
- Verifique que tenga una cadena completa con su certificado de servidor primero y luego su cadena de certificados de CA raíz dentro de server-cert (**`cat myserver.pem ca-chain.pem > server-cert.pem`**)

Si está utilizando un hipervisor externo, compruebe que ha copiado los certificados en la carpeta correcta. (hypervisors.md#add-ssh-keys-for-new-hypervisor).
