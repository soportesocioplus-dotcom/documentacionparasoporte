---
description: >-
  Cómo habilitar la apertura del molinete Tango desde otro tipo de control
  de acceso (facial, ZK9500, reloj, etc.).
icon: link
---

# Tango en otros controles de acceso

Es posible utilizar los molinetes de Tango en cualquier otro control de acceso de SocioPLUS (por ejemplo, reconocimiento facial, ZK9500 o reloj), usando el campo `comando_apertura` de la base de datos.

{% stepper %}
{% step %}
### Abrir la base de datos

Ingresá a la base de datos `SPControlAcceso.mdb` con Microsoft Access, y entrá a la tabla `sincroniza`.
{% endstep %}

{% step %}
### Habilitar el molinete de Tango

Configurá el campo `cc_molinete` en `1`.
{% endstep %}

{% step %}
### Indicar el ejecutable de apertura

En el campo `cc_apertura`, indicá el `.exe` que deja Tango para la apertura del molinete. Generalmente se encuentra en `C:\TangoAccess`.
{% endstep %}
{% endstepper %}
