<h1>Cuotas</h1>

Las cuotas pueden limitar los recursos utilizados por cada rol, categoría, grupo o usuario individualmente.

[TOC]

# Tipos de cuotas

Hay un conjunto de cuotas que se pueden modificar:

- **Escritorios**: Número máximo de escritorios que se pueden crear.
- **En ejecución**: La cantidad máxima de escritorios que el usuario puede ejecutar al mismo tiempo..
- **Plantillas**: Número máximo de plantillas que el usuario puede crear desde sus escritorios.
- **Media** (or Isos): Número máximo de medios (isos / disquetes) que el usuario puede cargar en el sistema
- **CPUs**: Número máximo de CPU virtuales que se pueden configurar en un escritorio.
- **Memoria**: Memoria máxima en MB que se puede configurar en un escritorio.

# Cuotas efectivas para el usuario.

Un usuario siempre se clasifica dentro de un rol, categoría y grupo, en ese orden de prioridad jerárquica. Esto significa que las cuotas efectivas para un usuario serán las más restrictivas de arriba a abajo.

Si modificamos la cuota para un usuario individualmente, entonces esa cuota anulará la que tenía anteriormente basada en la jerarquía de roles, categorías y grupos.
