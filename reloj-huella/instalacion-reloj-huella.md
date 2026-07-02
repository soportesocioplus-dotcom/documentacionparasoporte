---
description: >-
  Guía técnica para el equipo de soporte: procedimiento de instalación del
  control de acceso con reloj de huella (con o sin display).
icon: clock
---

# Instalación con reloj (con y sin display)

Este documento describe el procedimiento de instalación del control de acceso con reloj de huella digital, donde el propio reloj toma y almacena las huellas (sin usar un sensor externo como el ZK9500). Está dirigido al equipo de soporte técnico encargado de realizar la instalación en la sede del cliente.

{% hint style="info" %}
Antes de instalar, verificá que la PC cumpla con los [requerimientos para una instalación](https://docs.google.com/document/d/1MI-zniR0nJHE0xqoOqe48sUwHTYPFEdu2MaFYw4moGE/edit?usp=drive_link).
{% endhint %}

{% hint style="warning" %}
La instalación de reloj es variable según la cuenta: siempre se configura de la misma forma general, pero cambia el modelo de reloj y si el enrolamiento se hace con el propio reloj o con un sensor aparte (por ejemplo, un ZK9500). Verificá cuál es el caso antes de empezar.
{% endhint %}

## Paso previo: cargar los nodos

Ingresá a [`gestion.socioplus.com.ar/soporte`](https://gestion.socioplus.com.ar/soporte), buscá al cliente y entrá. Repetí el ingreso, pero esta vez entrá a `Nodos`. Completá la información de los nodos previamente consultada al cliente, y asegurate de que se guarde correctamente en el cliente seleccionado.

![Formulario de creación de nodo con los campos Nodo ID, Nombre, Tipo, IP, MAC, Puerto, Dominio, usuario y contraseña](../.gitbook/assets/nodos-crear-nodo-formulario.png)

![Tabla de nodos cargados para una sede, con tipo, IP, puerto y credenciales](../.gitbook/assets/nodos-tabla-cargados.png)

Antes de empezar la instalación, abrí un Bloc de notas y respondé las preguntas que se indican en [PROCEDIMIENTO SOPORTE.docx](https://docs.google.com/document/d/1FiFYL9uSOERwpGWvrVg_pkBhZXl-l8K5/edit?usp=drive_link).

## Procedimiento de instalación

{% stepper %}
{% step %}
### Configurar el sistema

Andá a `Panel de control` › `Reloj y región` › `Cambiar formatos de fecha, hora o número`.

* **Formato:** verificá que esté configurado como `Español (Argentina)` (en caso de ser clientes de Argentina).
* **Configuración adicional** › pestaña **Fecha**: `Fecha corta` debe tener el formato `dd/MM/aaaa`.
* **Configuración adicional** › pestaña **Hora**: `Hora corta` debe tener el formato `HH:mm`, y `Hora larga` el formato `HH:mm:ss`.

![Configuración de formatos de fecha y hora, con Español (Argentina) y los formatos dd/MM/aaaa y HH:mm](../.gitbook/assets/reloj-huella-formato-fecha-hora.png)

Por último, hacé clic en **Aplicar** y en **Aceptar** para guardar los cambios.
{% endstep %}

{% step %}
### Descargar los drivers

Los drivers se obtienen del Drive de soporte, en la carpeta [`Configuración Molinetes Reloj`](https://drive.google.com/drive/folders/1Vi8fsGiWaY6hdmkIZ5xjUbKQsbjWZ91h?usp=drive_link). Descargá el archivo `socioPLUS_reloj.zip` y descomprimilo en la carpeta `Documentos`.
{% endstep %}

{% step %}
### Parametrizar el archivo config.ini

Abrí `config.ini` y completá los siguientes valores:

| TAG | Campo | Valor |
|---|---|---|
| `[Cliente]` | `Idcliente` | ID del cliente. Se consigue en [`gestion.socioplus.com.ar/soporte`](https://gestion.socioplus.com.ar/soporte), buscando al cliente. Ejemplo: `db82037fc12bfe1d50cb488f997e394` |
| `[Cliente]` | `Sede` | Número de sede a la que se le está instalando el sistema. Se consigue en el mismo panel: es el número que aparece entre paréntesis junto al nombre de la sede. Ejemplo: `CASA CENTRAL (1)` → `1` |
| `[Cliente]` | `cmd_exe` | Ruta del archivo `Access.exe`, dentro de la carpeta `socioPLUS`. Ejemplo: `C:\Documentos\socioPLUS\Access.exe` |
| `[BD]` | `Path` | Raíz donde se instaló el control de acceso. Ejemplo: `D:\Documentos\socioPLUS` |
| `[BD]` | `Path_datos` | Raíz de instalación + carpeta `datos`. Ejemplo: `D:\Documentos\socioPLUS\datos` |
| `[BD]` | `Path_imagenes` | Debe quedar en blanco |
| `[BD]` | `Path_fotos` | Raíz de instalación + carpeta `fotos`. Ejemplo: `D:\Documentos\socioPLUS\fotos` |

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
| `sincroniza` | Actualizar la sede, el ID de cliente, la IP del reloj y los campos `relojmodelo` y `accesocondeuda` (ver detalle abajo). |
| `sincroniza_nodos` | Contiene la información de los nodos. Por cada nodo se debe crear un registro con la sede, el ID del nodo, el nombre del nodo, su IP y su puerto. |
| `socios` | Debe estar en blanco. Si no lo está, eliminar sus movimientos. |
| `sociosHuella` | Debe estar en blanco. Si no lo está, eliminar sus movimientos. |

En la tabla `sincroniza`, completá también:

| Campo | Valor |
|---|---|
| `IP del reloj` | Solicitala previamente al cliente; se la provee quien le instaló el molinete y el reloj. |
| `relojmodelo` | `1` si el reloj tiene display, `0` si no tiene display |
| `accesocondeuda` | `1` si el socio puede ingresar con deuda, `0` si no puede ingresar con deuda |

{% hint style="warning" %}
Si la sede tiene más de un reloj, cargá en `sincroniza` la IP de **uno solo** de los relojes.
{% endhint %}

{% hint style="info" %}
Modelos de reloj **con display** compatibles (ZKTeco): F11, F19, F21, F22.

Modelo de reloj **sin display** compatible: MA300. El modelo ZKTeco 4500 todavía no tiene la información confirmada — consultá antes de instalarlo.
{% endhint %}

![Tabla sincroniza_nodos en Access, con los campos sede, nodo_id, nodo, ip y puerto cargados](../.gitbook/assets/zk9500-sincroniza-nodos-access.png)

Guardá los cambios y luego ejecutá `Compactar y reparar base de datos`.
{% endstep %}

{% step %}
### Instalar los archivos DLL

Copiá los archivos de la carpeta `DLL` a `C:\Windows\syswow64`.

1. Buscá `CMD` en Windows, hacé clic derecho y seleccioná **Ejecutar como administrador**.

![Menú de Windows con la opción Ejecutar como administrador señalada sobre Símbolo del sistema](../.gitbook/assets/zk9500-cmd-ejecutar-como-administrador.png)

2. Escribí `cd C:\Windows\SysWOW64` para moverte a esa carpeta. Si no funciona, usá el comando `cd` para ir navegando manualmente: `C:\` → `Windows` → `SysWOW64`.
3. Una vez en esa carpeta, ejecutá `regsvr32 (nombre del archivo dll)` y presioná **Enter**, repitiendo el comando para cada uno de los archivos que copiaste. Al terminar, cerrá la consola.
4. Desde la carpeta `Dll\time5.0`, ejecutá como administrador el archivo `setup.exe` y avanzá con **Siguiente** hasta finalizar la instalación.

Por último, creá un acceso directo del programa `SPControlAcceso.exe` en el escritorio.

{% hint style="danger" %}
El acceso directo debe ser del programa `SPControlAcceso.exe`. **No** del ejecutable de nodos web.
{% endhint %}
{% endstep %}
{% endstepper %}

## Antes de las pruebas: vincular los planes a los nodos

Para que los socios ingresen correctamente y el sistema web valide su acceso, sus contratos deben estar vinculados a los nodos correspondientes.

{% stepper %}
{% step %}
### Ingresar a la configuración del sistema

Entrá a `Menú` › `Setup` › `Configuración del sistema`.
{% endstep %}

{% step %}
### Editar el concepto

Localizá el concepto que vas a vincular y entrá a `Editar concepto` › `Deseo solo vincular planes a los nodos`.
{% endstep %}

{% step %}
### Seleccionar los nodos y guardar

Seleccioná el o los nodos por los cuales podrá ingresar el socio, y hacé clic en **Guardar** para aplicar los cambios.

![Pantalla de vinculación de nodos, con un nodo seleccionado y el botón Guardar señalado](../.gitbook/assets/vincular-nodos-guardar.png)
{% endstep %}
{% endstepper %}

## Pruebas

Enseñale a la persona encargada de asistirte a usar el programa SPacceso y a [enrolar la huella](como-enrolar-huellas-reloj.md) (al finalizar le vas a enviar un instructivo de cómo hacerlo).

{% stepper %}
{% step %}
### Elegir un socio de prueba

Consultá si hay un perfil creado sin deuda y con un contrato vigente. Si no hay ninguno, podés elegir un socio al azar del listado (que no sea de categoría Staff/Personal) y enrolarle la huella.
{% endstep %}

{% step %}
### Verificar el ingreso

Una vez enrolada la huella, verificá que el socio pueda ingresar y que el molinete abra correctamente. Cerrá el programa y [verificá si la huella se encuentra en el reloj](como-buscar-huellas-en-el-reloj.md).
{% endstep %}

{% step %}
### Repetir la comprobación

Repetí esta comprobación al menos 3 veces, para asegurarte de que todo esté funcionando correctamente.
{% endstep %}

{% step %}
### Cerrar la instalación

Una vez finalizadas las pruebas, pedile al cliente que envíe por mail el acta de conformidad, y guardala en [ACTAS DE CONFORMIDAD](https://drive.google.com/drive/folders/1xfpaaLYJxsa5M1msdlo3qoQ68zXNU4rz?usp=drive_link).
{% endstep %}
{% endstepper %}

{% hint style="danger" %}
Es importante hacer seguimiento de la instalación a las 24 horas de haberla realizado.
{% endhint %}
