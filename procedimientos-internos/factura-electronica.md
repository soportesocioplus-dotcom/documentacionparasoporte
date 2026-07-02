---
description: >-
  Procedimiento interno para dar de alta el módulo de Factura Electrónica a
  un cliente: datos a solicitar, generación de certificados y coordinación
  del alta.
icon: file-invoice
---

# Alta de Factura Electrónica

Este documento describe el procedimiento interno que sigue el equipo de soporte cuando un cliente solicita comenzar a utilizar el módulo de Factura Electrónica. Cubre los datos que hay que pedirle al cliente, la generación de los archivos necesarios para el Certificado Fiscal, el envío a AFIP a través del cliente, y la coordinación final del alta con el responsable del módulo.

{% hint style="danger" %}
Antes de avanzar, verificá que el cliente tenga contratado el **Plan Plus** o sea **Empresa**. Si no cumple ninguna de las dos condiciones, no se debe avanzar con el procedimiento: hay que informarle que solicite el módulo escribiendo a [**info@socioplus.com.ar**](mailto:info@socioplus.com.ar).
{% endhint %}

## Precio del servicio

El costo del servicio para dar de alta la Factura Electrónica es de **$150.000 + IVA**.

## Procedimiento

{% stepper %}
{% step %}
### Solicitar los datos al cliente

Pedile al cliente los siguientes datos:

| Dato | Detalle |
|---|---|
| Nombre de Fantasía | Nombre comercial del cliente |
| CUIT | Sin guiones, se va a usar así en los comandos |
| Razón social | Razón social registrada en AFIP |
| Punto de Venta | Punto de venta a habilitar para la factura electrónica |
{% endstep %}

{% step %}
### Generar los archivos Pedido y Privada con OpenSSL

Una vez que el cliente envía sus datos, hay que generar dos archivos —`pedido` y `privada`— que el cliente va a necesitar para tramitar el Certificado Fiscal en AFIP junto con su contador.

1. Descargá el archivo `.rar` de OpenSSL.
2. Andá a la carpeta raíz de OpenSSL, dentro de `bin`, y ejecutá `openssl.exe`.
3. Ejecutá los siguientes comandos, en este orden:

```
genrsa -out privada 2048
req -new -config openssl.cnf -key privada -out pedido
```

4. El segundo comando va a pedir los datos a continuación, uno por uno. Completalos así:

| Dato solicitado | Valor a ingresar |
|---|---|
| País | `AR` |
| Razón social | Razón social del cliente |
| Nombre de Fantasía | Nombre de fantasía del cliente |
| CUIT | CUIT del cliente |

{% hint style="warning" %}
El CUIT se debe escribir **sin guiones**.
{% endhint %}
{% endstep %}

{% step %}
### Guardar y verificar los archivos generados

En la misma carpeta de OpenSSL, vas a encontrar los archivos `pedido` y `privada`. Verificá que la fecha y hora de ambos coincida con el momento en que ejecutaste los comandos.

Guardá los dos archivos en el Drive, en `Factura Electrónica` → creá una carpeta con el nombre del cliente. Esa carpeta debe contener también los datos solicitados al cliente y, más adelante, la constancia de alta de punto de venta/emisión.
{% endstep %}

{% step %}
### Enviar los archivos al cliente y solicitar el certificado

Enviale al cliente los archivos `pedido` y `privada` (en la misma cadena de mail donde te envió sus datos), y pedile que te devuelva:

* El **Certificado Fiscal**, con extensión `.CRT`, **sin renombrar**.
* La **Constancia de alta de punto de venta / emisión**.
* Si desea recibir un mail cada vez que se emite una factura (consultaselo directamente).
{% endstep %}

{% step %}
### Armar el correo y enviarlo al responsable del módulo

Antes de enviar nada, corroborá que todo lo que mandó el cliente esté correcto y completo. Armá un correo para [**info@socioplus.com.ar**](mailto:info@socioplus.com.ar) (Hugo) con:

* Los datos solicitados al cliente: Nombre de Fantasía, CUIT, Razón social y Punto de Venta.
* Los archivos `pedido` y `privada`.
* El Certificado Fiscal (`.crt`, sin renombrar).
* La Constancia de alta de punto de venta / emisión.
{% endstep %}

{% step %}
### Confirmar el alta con el cliente

Aguardá a que Hugo haga las pruebas correspondientes con el certificado. Cuando confirme que fueron exitosas, avisale al cliente y enviale los instructivos y videos explicativos del módulo de Factura Electrónica, incluyendo el instructivo de cómo cargar los datos impositivos en el perfil del socio.
{% endstep %}
{% endstepper %}
