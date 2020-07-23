<h1>Administrar escritorios</h1>

[TOC]

# Acciones de escritorio

Hay algunas acciones que el administrador puede realizar en cada escritorio, además de iniciarlo y detenerlo.

- **Plantilla**: Permite al administrador convertir cualquier escritorio del sistema en una plantilla compartida con otros.
- **Editar**: It is the generic desktop edit that also has the desktop user owner but now with the limits and permissions of the administrator.
- **Borrar**: Se eliminará el escritorio del usuario. No se puede recuperar.
- **XML**: Puedes ver el libvirt XML en bruto y modificarlo. Esto es práctico cuando.
- **Messages**: Mostrará el montón de mensajes JSON del motor asociados con este escritorio.
- **Events**: Se mostrará el montón de eventos JSON del motor asociados con este escritorio.

# Acciones globales

Hay un menú desplegable con una lista de acciones que se pueden realizar en un conjunto de escritorios. Los escritorios se pueden seleccionar haciendo clic sobre ellos. Si no se selecciona ningún escritorio, se llevará a una selección completa de conjuntos de escritorios, ten cuidado:

![](../../images/admin/domains/desktops/global_actions.png)

- **Alternar el estado**: Esta acción cambiará el estado de inicio y parada. Todos los escritorios seleccionados cambiarán su estado a la vez.
- **Stop no viewers**: Todos los escritorios iniciados en el conjunto de selección indicaron que el sistema detectó que nadie se conectará con un visor se detendrá.
- **Forzar detención**: Este como este el estado, se establecerá un estado fallido.

- **Borrar**: Establecerá todos los estados de escritorio seleccionados para eliminarlos si está en estado detenido o fallido. Eso iniciará en segundo plano todas las acciones para lograr la eliminación completa de los escritorios. Si algo falla en ese proceso, el escritorio permanecerá en estado de error. Siempre puede volver a activar la acción de eliminación al eliminar escritorios y se eliminarán de la base de datos ignorando el motor de fondo.


# Crear uno desde la ISO

Esta es la forma habitual de crear un nuevo escritorio completo a partir de su ISO cargado:


- **Nombre y descripción del escritorio**: Rellene el nombre y la descripción del nuevo escritorio que se está creando.

- **Selected ISO/Floppy to boot from**: Seleccione un medio ya cargado para instalar su nuevo escritorio. También tiene la opción de verificar si se trata de una ISO propietaria de Win para instalar. Esto también agregará una segunda ISO con controladores optimizados para Win virtual (si el administrador ya descargó de las actualizaciones).
/updates.md#recommended-updates)).

- **Seleccione la plantilla del SO**: Seleccione la plantilla que mejor se adapte a su instalación. No necesita ser la instalación exacta, solo una plantilla similar, ya que esto solo establecerá el hardware genérico simulado para este escritorio.
- **Ajuste de la plantilla del sistema operativo**: De forma predeterminada, la opción de inicio se establecerá en CD / DVD y no podrá modificarse si planea crearla a partir de ISO descargada.


Cuando termine y haga clic en el botón Crear escritorio, se creará un escritorio. Vaya al menú de Escritorios para comenzar, conéctese y comience la instalación desde el ISO seleccionado.

***NOTA***: Cuando finaliza la instalación del sistema operativo, generalmente el sistema operativo invitado solicitará un reinicio. Simplemente puede apagar su sistema operativo invitado y luego editar el escritorio para cambiar el orden de arranque de CD / DVD a DISCO DURO. También puede eliminar la ISO de la sección de medios de escritorio de edición si ya no la necesita.
