---
description: >-
  Diagnóstico y solución cuando un control de acceso facial no responde o
  pierde la conexión.
icon: plug-circle-xmark
---

# El control de acceso facial no responde (sin conexión)

Cuando un control de acceso facial no responde, hacé estos tres chequeos antes de decidir cómo seguir:

{% stepper %}
{% step %}
### Verificar que el programa esté abierto

Localizá el ícono del molinete (`SP Control de acceso`) en los iconos ocultos de la barra de tareas.

![Ícono de SP Control de acceso en los iconos ocultos de la barra de tareas](../.gitbook/assets/icono-sp-control-acceso-bandeja.png)

Si no lo encontrás ahí, buscalo también en el Administrador de tareas (`Ctrl + Shift + Esc`).
{% endstep %}

{% step %}
### Verificar el estado del túnel

Entrá al [panel de túneles de Cloudflare](https://one.dash.cloudflare.com/982b2c155832920d5daed442e2c2fc6c/access/tunnels?search=) y fijate que el túnel esté en estado `HEALTHY`. Si no estás logueado, ingresá con las credenciales de la cuenta de soporte.
{% endstep %}

{% step %}
### Verificar la conexión de los nodos

Comprobá que la IP de cada nodo responda al hacer `ping`, y que se pueda acceder a ella desde el navegador de la PC del cliente mediante AnyDesk.

{% hint style="danger" %}
Es fundamental que **todos** los nodos tengan conexión. Si falta uno solo, el programa no va a arrancar.
{% endhint %}
{% endstep %}
{% endstepper %}

Según el resultado de estos tres chequeos, el diagnóstico y la solución cambian:

<details>

<summary>Caso 1 — El programa NO está abierto y el túnel SÍ tiene conexión (Healthy)</summary>

Entrá al Administrador de tareas (`Ctrl + Shift + Esc`) y terminá todos los procesos `cloudflared.exe`. Repetí esto hasta que el túnel muestre estado `DOWN`.

</details>

<details>

<summary>Caso 2 — El programa SÍ está abierto y el túnel NO tiene conexión (Down)</summary>

Reiniciá la PC e intentá nuevamente. Si después de reiniciar el túnel sigue sin conexión, seguí los pasos del apartado **"El túnel ya existe"** de la [guía de instalación del control de acceso facial](control-acceso-reconocimiento-facial.md) y volvé a correr el setup para reconfigurar el túnel.

</details>

<details>

<summary>Caso 3 — El programa SÍ está abierto y el túnel SÍ tiene conexión (Healthy), pero no podemos conectarnos a los nodos</summary>

Este caso se da cuando, al intentar entrar al dominio del nodo desde **nuestro** navegador (no el del cliente), no conseguimos acceso. El formato del dominio es:

```
https://nodo{nro_nodo}-{cliente}-{nro_sede}.socioplusaccess.com.ar
```

Por ejemplo: `https://nodo1-argneuq009-1.socioplusaccess.com.ar`.

Verificá que los nodos tengan conexión mediante `ping` y que sean accesibles desde el navegador del cliente vía AnyDesk. Pedile al cliente que desconecte los nodos, que espere 5 minutos y que los vuelva a conectar. Repetí los tres chequeos del comienzo de esta página.

</details>

Después de aplicar la solución que corresponda, ejecutá nuevamente el programa con el archivo `startup_script.ps1` (clic derecho › `Ejecutar con PowerShell`, igual que en el paso "Ejecutar el script de inicio" de la guía de instalación). Esto asegura que se detecten los cambios en las IPs de los nodos, si los hubo, y que se actualice la configuración de cloudflared.

{% hint style="info" %}
Para saber la IP asociada a la MAC de cada nodo, podés usar el comando `arp -a` en una consola.
{% endhint %}
