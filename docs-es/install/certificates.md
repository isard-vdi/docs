<h1>Certificados</h1>

Los certificados SSL deben utilizarse para asegurar el acceso web y las conexiones a escritorios virtuales con los espectadores. IsardVDI generará un certificado genérico autofirmado predeterminado cuando se instala desde la primera vez. Además, si no hay un certificado presente, se generará un nuevo autofirmado para utilizarlo de forma predeterminada. Es por eso que los navegadores solicitarán la aceptación del certificado en el primer acceso a la web de IsardVDI.

[TOC]

# Gestionar certificados

Los certificados se almacenan en path /opt/isard/certs/ default, donde pueden ser reemplazados por nuevos. Los certificados deben ser los siguientes:

- **server-cert.pem**: Es la cadena completa de certificados con certificado raíz incluido.
- **server-key.pem**: Es la clave del servidor.

El ca-cert.pem se leerá desde el servidor-cert.pem existente, así que tenga en cuenta que el servidor-cert.pem debe contener en primera posición el certificado del servidor y después de eso la cadena de certificados de la entidad de validación (cadena)

## Certificado comercial

Siempre desactive IsardVDI antes de proceder a reemplazar el certificado:

```
docker-compose down
```

- **server-cert.pem**: puede cambiar el nombre de fullchain que le da su proveedor de certificados para que sea server-cert.pem o puede concatenar el certificado del servidor con chain **`cat myserver.pem ca-chain.pem > server-cert.pem`**
- **server-key.pem**: Por lo general, esa clave ya estará en un archivo. Solo renombralo

Coloque esos certificados con el nombre correcto en / opt / isard / certs / default (reemplace todos los que ya están en esa carpeta) e inicie IsardVDI de nuevo:


```
docker-compose up -d
```

Ahora puede conectarse al servidor IsardVDI utilizando el CN calificado que se proporciona con su certificado.

NOTA: Los certificados de Wilcard también se han validado con este procedimiento para que funcionen como se esperaba. Vea el ejemplo a continuación:

### Ejemplo de certificado SSL comodín

Por ejemplo, tiene un certificado comercial comodín de una empresa (digamos que compró * .isardvdi.com). Obtendrá estos archivos de su proveedor de certificados:

- **wildcard_isardvdi_com.crt.pem**: Su certificado de wilcard.
- **GandiStandardSSLCA2.pem**:  Este es el certificado intermedio de la autoridad de certificación (en este ejemplo, gandi.net es el proveedor). Siempre puede obtener este certificado copiando o exportando desde la configuración de certificado de su navegador o descargándolo de su página web (i.e. ```wget -q -O - https://www.gandi.net/static/CAs/GandiStandardSSLCA2.pem```)
- **wildcard_isardvdi_com.key.pem**: Su clave privada de certificado.

Necesitaremos transformar estos archivos en dos necesarios para IsardVDI:

- **server-cert.pem**: ```cat wildcard_isardvdi_com.crt.pem GandiStandardSSLCA2.pem > /opt/isard/certs/default/server-cert.pem```
- **server-key.pem**: ```mv wildcard_isardvdi_com.key.pem /opt/isard/certs/default/server-key.pem```

## Certificado de Letsencrypt

Siempre baje Isard VDI antes de proceder a reemplazar el certificado:

```
docker-compose down
```

- **server-cert.pem**: Es el fullchain.pem
- **server-key.pem**: Es el privkey.pem
Coloque esos certificados con el nombre correcto en / opt / isard / certs / default (reemplace todo lo que ya está en esa carpeta) e inicie IsardVDI de nuevo:


```
docker-compose up -d
```

Ahora puede conectarse al servidor IsardVDI utilizando el CN calificado que se proporciona con su certificado.

NOTA: Los certificados de host múltiple también se han validado con este procedimiento para que funcionen como se esperaba.

# Restablecer certificados

Si reemplazó los certificados y nada funcionó, se recomienda iniciar el proceso nuevamente restableciendo los certificados. Si manipula certificados en la carpeta, podría confundir el código de procesamiento del certificado IsardVDI.

Siempre puede hacer que su IsardVDI vuelva a funcionar con certificados autofirmados eliminando la carpeta / opt / isard / certs / default. IsardVDI generará y configurará un nuevo certificado autofirmado nuevamente. El procedimiento será:

```bash
docker-compose down
rm -rf /opt/isard/certs/default
docker-compose up
```

Es posible que haya hecho una copia de seguridad de sus certificados autofirmados que funcionaban anteriormente y ahora también podría copiarlos en la carpeta de certificados predeterminada en lugar de generar otros nuevos.

# Solucionar problemas de certificados

Please refer to the  [admin faq about certificates](../admin/faq.md#certificates) section.
