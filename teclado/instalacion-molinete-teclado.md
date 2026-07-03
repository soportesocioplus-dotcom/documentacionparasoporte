---
description: >-
  Guía técnica para el equipo de soporte: requisitos y procedimiento de
  instalación del molinete con control de acceso por teclado.
icon: keyboard
---

# Instalación de molinete con teclado

Este documento describe el procedimiento de instalación y configuración del control de acceso por teclado, que opera mediante una base de datos local en Microsoft Access y controla el molinete a través de un puerto serie (COM). Está dirigido al equipo de soporte técnico encargado de realizar la instalación en la sede del cliente.

## Requisitos previos a la instalación

Se recomienda que el puesto de trabajo cumpla con los siguientes requisitos:

| Componente | Requisito |
|---|---|
| Equipo | PC o notebook |
| Sistema operativo | Windows 10 |
| Disco | 250 GB o superior. Preferentemente sólido (SSD). Si se puede crear una partición para datos, es lo mejor. |
| Memoria RAM | 16 GB (mínimo) |
| Software adicional | Microsoft Access y [AnyDesk](https://anydesk.com/es) |

## Procedimiento de instalación

{% stepper %}
{% step %}
### Configurar el sistema

Andá a `Panel de control` › `Región` y configurá la fecha y hora del equipo:

* **Formato:** verificá que esté configurado como `Español (Argentina)`.
* **Configuración adicional** › pestaña **Fecha**: `Fecha corta` debe tener el formato `dd/MM/aaaa`.
* **Configuración adicional** › pestaña **Hora**: `Hora corta` debe tener el formato `HH:mm`, y `Hora larga` el formato `HH:mm:ss`.

Por último, hacé clic en **Aplicar** y en **Aceptar** para guardar los cambios.
{% endstep %}

{% step %}
### Copiar los drivers

Creá una carpeta con el nombre `socioPLUS`, en la partición de disco creada para datos o en `Mis documentos` del equipo.

Los drivers deben contener las siguientes carpetas y archivos:

| Carpeta | Contenido |
|---|---|
| `Datos` | Base de datos en formato Access `SPControlAcceso.mdb` |
| `Dll` | `iGrid250_75B4A91C.ocx`, `MSCOMM32.OCX`, `MSINET.OCX` |
| `Fotos` | `99999999.jpg`, `b64dec.exe`, `default.jpg`, `foto_socio.txt` |
| `Imágenes` | `default.jpg`, `molinete.ico` |

Además de esas carpetas, la instalación debe incluir:

* El archivo `config.ini`.
* El ejecutable `SPControlAcceso_multi_nodos_web.exe`.
{% endstep %}

{% step %}
### Parametrizar el archivo config.ini

Abrí `config.ini` y completá los siguientes valores:

| TAG | Campo | Valor |
|---|---|---|
| `[Cliente]` | `Sede` | Número de la sede que corresponde a la instalación. Acepta solo números. |
| `[Micro]` | `M1` | `100` |
| `[Micro]` | `M2` | `101` |
| `[Molinete]` | `Puerto` | Número de puerto COM asignado al dispositivo (se verifica en `Panel de control` › `Dispositivos`) |
| `[Molinete]` | `Tiempo` | `50` por defecto (tiempo de apertura). Se puede ampliar si hace falta. |
| `[BD]` | `Path` | Raíz donde se instaló el control de acceso. Ejemplo: `D:\socioPLUS` |
| `[BD]` | `Path_datos` | Raíz de instalación + carpeta `datos`. Ejemplo: `D:\socioPLUS\datos` |
| `[BD]` | `Path_imagenes` | Debe quedar en blanco |
| `[BD]` | `Path_fotos` | Raíz de instalación + carpeta `fotos`. Ejemplo: `D:\socioPLUS\fotos` |

{% hint style="warning" %}
Si los micros `M1` y `M2` están invertidos en la instalación física, hay que invertir también estos valores en el archivo.
{% endhint %}

{% hint style="danger" %}
No aumentes demasiado el valor de `Tiempo`: cuanto más tiempo de apertura tenga el molinete, más fácil es que se rompa.
{% endhint %}

Guardá el archivo una vez aplicados los cambios.
{% endstep %}

{% step %}
### Parametrizar la base de datos SPControlAcceso.mdb

Abrí la base de datos con Microsoft Access y revisá las siguientes tablas:

| Tabla | Qué verificar |
|---|---|
| `ingresos` | Debe estar en blanco. Si no lo está, eliminar sus movimientos. |
| `ingresos_bk` | Debe estar en blanco. Si no lo está, eliminar sus movimientos. |
| `movimientos` | Debe estar en blanco. Si no lo está, eliminar sus movimientos. |
| `sincroniza` | En el campo `sede`, colocar el número de la sede a instalar. Acepta solo números. |
| `sincroniza_nodos` | Si contiene información de nodos, actualizar el campo `sede` con el número de sede a instalar. Acepta solo números. |
| `socios` | Debe estar en blanco. Si no lo está, eliminar sus movimientos. |
{% endstep %}

{% step %}
### Instalar los archivos DLL

Copiá los tres archivos de la carpeta `Dll` (`iGrid250_75B4A91C.ocx`, `MSCOMM32.OCX`, `MSINET.OCX`) a la carpeta `C:\Windows\syswow64`.

Después, registralos desde la línea de comandos:

1. Abrí `CMD` con permisos de administrador.
2. Si la consola no se abre directamente en `C:\Windows\syswow64`, navegá hasta ahí ejecutando `cd..` las veces que sea necesario para llegar a la raíz, y luego `cd Windows` y `cd syswow64`.
3. Una vez en esa carpeta, ejecutá uno por uno los siguientes comandos, presionando **Enter** después de cada uno:

```
Regsvr32 iGrid250_75B4A91C.ocx
Regsvr32 MSCOMM32.OCX
Regsvr32 MSINET.OCX
```

4. Cerrá la ventana de línea de comandos.
{% endstep %}

{% step %}
### Ejecutar el programa

Creá un acceso directo del archivo `SPControlAcceso_multi_nodos_web.exe` en el escritorio de la PC.
{% endstep %}
{% endstepper %}
