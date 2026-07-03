---
description: >-
  Guía técnica para el equipo de soporte: requisitos y procedimiento de
  instalación del molinete con control de acceso por lector QR.
icon: qrcode
---

# Instalación con lector QR

Este documento describe el procedimiento de instalación y configuración del control de acceso por lector QR, que opera mediante una base de datos local en Microsoft Access y controla el molinete a través de un puerto serie (COM). Está dirigido al equipo de soporte técnico encargado de realizar la instalación en la sede del cliente.

{% hint style="warning" %}
No se debe pactar un turno de instalación sin que el cliente haya confirmado por escrito el cumplimiento de los requisitos de la sección siguiente, e informado el nombre completo de la persona a cargo del puesto de trabajo durante la instalación.
{% endhint %}

## Requisitos previos a la instalación

El puesto de trabajo donde se realizará la instalación (o reinstalación) debe cumplir con los siguientes requisitos mínimos de hardware y software:

| Componente | Requisito |
|---|---|
| Procesador | Intel i3 o AMD Ryzen 3 (mínimo) — Intel i5 o AMD Ryzen 5 (recomendado) |
| Memoria RAM | 16 GB (mínimo) |
| Disco rígido | 500 GB (mínimo) |
| Sistema operativo | Windows 10 |
| Explorador web | Firefox (recomendado) |
| Software adicional | [AnyDesk](https://anydesk.com/es) instalado |
| Conectividad | Conexión a internet óptima y estable |

Además, se debe verificar en la sede:

* Todos los periféricos de la PC (teclado, mouse, lector, molinete, etc.) deben estar correctamente conectados y en óptimo funcionamiento.
* Debe permanecer una persona en la PC durante toda la instalación, para asistir al técnico y validar las pruebas necesarias.
* Se recomienda conexión a internet por cable Ethernet.

## Cómo realizar la instalación

Antes de empezar, abrí un Bloc de notas y respondé las preguntas que se indican en [PROCEDIMIENTO SOPORTE.docx](https://docs.google.com/document/d/1FiFYL9uSOERwpGWvrVg_pkBhZXl-l8K5/edit?usp=drive_link).

{% stepper %}
{% step %}
### Configurar el sistema

Andá a `Panel de control` › `Región` y configurá la fecha y hora del equipo:

* **Formato:** verificá que esté configurado como `Español (Argentina)` (en caso de ser clientes de Argentina).
* **Configuración adicional** › pestaña **Fecha**: `Fecha corta` debe tener el formato `dd/MM/aaaa`.
* **Configuración adicional** › pestaña **Hora**: `Hora corta` debe tener el formato `HH:mm`, y `Hora larga` el formato `HH:mm:ss`.

Por último, hacé clic en **Aplicar** y en **Aceptar** para guardar los cambios.
{% endstep %}

{% step %}
### Copiar los drivers

Creá una carpeta con el nombre `socioPLUS` en `Mis documentos` del equipo. Verificá en qué partición se encuentra.

Los drivers se obtienen del Drive del mail de soporte, en la carpeta [`Configuración de molinetes con lector QR`](https://drive.google.com/drive/folders/1h4GQugUAFw2d1aCSC8csbxGdoMKLW9cz). Descargá el archivo `SocioPLUS_AccessQR.rar` y descomprimilo en `Documentos`.
{% endstep %}

{% step %}
### Parametrizar el archivo config.ini

Abrí `config.ini` y completá los siguientes valores:

| TAG | Campo | Valor |
|---|---|---|
| `[Cliente]` | `Idcliente` | ID del cliente. Se consigue en [`gestion.socioplus.com.ar/soporte`](https://gestion.socioplus.com.ar/soporte), buscando al cliente. Ejemplo: `db82037fc12bfe1d50cb488f997e394` |
| `[Cliente]` | `Sede` | Número de sede a la que se le está instalando el sistema. Se consigue en el mismo panel. |
| `[Micro]` | `M1` | `100` |
| `[Micro]` | `M2` | `101` |
| `[Molinete]` | `Puerto` | Número de puerto COM asignado al dispositivo. Se verifica en el `Administrador de dispositivos` › `Puertos (COM y LPT)`. Por ejemplo, `COM1` → ingresar `1`. |
| `[Molinete]` | `Tiempo` | `50` por defecto (tiempo de apertura). Se puede ampliar si hace falta. |
| `[BD]` | `Path` | Raíz donde se instaló el control de acceso. Ejemplo: `D:\Documentos\socioPLUS` |
| `[BD]` | `Path_datos` | Raíz de instalación + carpeta `datos`. Ejemplo: `D:\Documentos\socioPLUS\datos` |
| `[BD]` | `Path_imagenes` | Debe quedar en blanco |
| `[BD]` | `Path_fotos` | Raíz de instalación + carpeta `fotos`. Ejemplo: `D:\Documentos\socioPLUS\fotos` |

![Administrador de dispositivos con el puerto de comunicaciones COM1 señalado dentro de Puertos (COM y LPT)](../.gitbook/assets/lector-qr-puerto-com-dispositivos.png)

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
| `sincroniza` | Actualizar la sede y el ID de cliente. |
| `sincroniza_nodos` | Debe estar en blanco. Si no lo está, eliminar sus movimientos. |
| `socios` | Debe estar en blanco. Si no lo está, eliminar sus movimientos. |
| `sociosHuella` | Debe estar en blanco. Si no lo está, eliminar sus movimientos. |
{% endstep %}

{% step %}
### Instalar los archivos DLL

Copiá los archivos de la carpeta `Dll` a `C:\Windows\syswow64`.

1. Buscá `CMD` en Windows, hacé clic derecho y seleccioná **Ejecutar como administrador**.
2. Si la consola no se abre directamente en `C:\Windows\syswow64`, navegá hasta ahí ejecutando `cd..` las veces que sea necesario para llegar a la raíz, y luego `cd Windows` y `cd syswow64`.
3. Una vez en esa carpeta, ejecutá `regsvr32 (nombre del archivo dll)` y presioná **Enter**, repitiendo el comando para cada uno de los archivos que copiaste. Al terminar, cerrá la consola.
{% endstep %}

{% step %}
### Ejecutar el programa y hacer las pruebas

Creá un acceso directo del programa `SPControlAcceso_multi_nodos_web.exe` en el escritorio y abrilo. Hacé las pruebas escaneando el QR de algunos socios para verificar que puedan ingresar.
{% endstep %}
{% endstepper %}
