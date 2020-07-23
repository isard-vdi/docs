<h1>Asistente</h1>

[TOC]

# Pasos

Conéctese con su navegador a su servidor IsardVDI en https://<ip|dns> y siga el asistente.


El navegador le pedirá que acepte el certificado generado autofirmado. Agregue una excepción, agregue el certificado a los certificados de confianza y se abrirá el asistente.

![](../images/wizard/firefox-untrusted.jpg)

A medida que el asistente abre un formulario modal, se mostrará el acuerdo de licencia y una casilla de verificación ya activada de forma predeterminada que permitirá a su instalación de IsardVDI descargar automáticamente los escritorios de demostración durante el asistente.

![](../images/wizard/license.png)

Siga los pasos para terminar su configuración inicial de IsardVDI.

## 1. Configuración

El asistente detectará la configuración automáticamente desde isard.conf.docker. Nada puede ser cambiado aquí.

![](../images/wizard/1.png)

Haga clic en el botón **Siguiente**.

## 2. Base de datos

La base de datos debe estar poblada ahora. Haga clic en el botón Rellenar base de datos y espere hasta que termine.

![](../images/wizard/2.png)

Haga clic en el botón **Siguiente** cuando termine de completar la base de datos.

## 3. Contraseña

El asistente obliga a configurar una nueva contraseña de usuario administrador. Haga clic en el botón Cambiar contraseña y rellénelo dos veces. Haga clic en el botón Actualizar para cerrar el formulario de actualización de contraseña.

![](../images/wizard/3.png)

Haga clic en el botón **Siguiente** cuando la contraseña se haya actualizado.

## 4. Hipervisor

IsardVDI viene con un hipervisor KVM acoplado y ahora el asistente comprobará si está en línea.

![](../images/wizard/5.png)

Click on **Next** button as it should be up and running already.

## 5. Motor

El asistente comprueba si el motor IsardVDI se ejecuta en segundo plano. Si falla, deje pasar unos segundos y haga clic en el botón Comprobar nuevamente. Cuando encuentre el motor en marcha, continuará con el siguiente paso.

![](/home/darta/jvinolas/docs/docs/images/wizard/4.png)

Haga clic en el botón **Siguiente** si ya encontró el motor.

## 6. Actualizaciones

Si marcó la casilla de verificación de registro en el primer modal, se mostrará una lista de recursos en línea disponibles para descargar. Puede registrarse más tarde desde el menú de actualizaciones.


![](../images/wizard/6.png)

Haga clic en el botón Finalizar y espere a que aparezca la página principal de inicio de sesión de IsardVDI. Inicie sesión con el nombre de usuario de administrador y la contraseña que proporcionó en el paso 3.

![](../images/wizard/login.png)

Ve a [first steps](first-steps.md)
