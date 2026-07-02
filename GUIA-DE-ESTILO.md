# Guía de Estilo y Redacción — Documentación SocioPLUS

Este documento define el **criterio común** que debe seguir toda persona que escriba o edite la documentación de SocioPLUS. Su objetivo es que, sin importar quién redacte cada módulo, el resultado sea **uniforme, formal, claro e intuitivo**, de modo que cualquier usuario (administrador, recepcionista o dueño de gimnasio) pueda entender y ejecutar cada configuración sin ayuda externa.

Leé esta guía completa antes de redactar tu primer módulo. Funciona como onboarding: al terminarla, deberías poder escribir una página nueva que sea indistinguible en estilo de las ya existentes.

---

## 1. A quién le escribimos (público objetivo)

**Esta es la regla más importante de toda la guía: tenela presente en cada párrafo que escribas.**

La documentación de SocioPLUS está dirigida al **personal de los gimnasios y centros deportivos** —administradores, recepcionistas y dueños— que utiliza el sistema en su trabajo diario. Este público, en general, **no tiene conocimientos técnicos ni de informática**. Son personas expertas en la operación de su gimnasio, no en software.

Esto tiene consecuencias directas sobre cómo debés escribir:

- **No des nada por sabido.** Lo que para vos es obvio (un "campo desplegable", "exportar un CSV", "un período de presentación") puede no serlo para el lector. Explicá cada concepto la primera vez que aparece.
- **Evitá la jerga técnica.** No uses términos de programación, base de datos o informática. Si un término técnico es inevitable (por ejemplo, porque aparece así en la pantalla del sistema), explicalo en palabras simples.
- **Guialo de la mano.** El lector debe poder seguir las instrucciones aunque sea su primera vez en esa pantalla. Indicá exactamente dónde hacer clic, qué botón presionar y qué va a pasar después de cada acción.
- **Formal, pero siempre comprensible.** El tono es profesional y respetuoso, pero la prioridad número uno es que **se entienda**. Entre una redacción elegante y una redacción clara, elegí siempre la clara. Frases cortas, una idea por oración, lenguaje cotidiano.
- **Anticipá las dudas y los errores.** Como el lector no puede "deducir" lo que falta, aclará los prerrequisitos, las consecuencias de cada acción y qué hacer si algo sale mal.

Antes de dar por terminado un texto, hacete esta pregunta: *¿una recepcionista que nunca usó esta pantalla podría seguir estos pasos sin trabarse y sin tener que preguntarle a nadie?* Si la respuesta es no, simplificá.

---

## 2. Principios generales

La documentación de SocioPLUS se rige por cuatro pilares. Cada texto que escribas debe cumplirlos:

- **Formal pero cercana.** Usá un tono profesional y respetuoso, sin caer en lo rígido ni en lo coloquial. Evitá modismos, chistes y expresiones informales.
- **Explicativa.** No asumas que el usuario ya sabe. Explicá el *qué*, el *para qué* y el *cómo* de cada acción. Si un paso tiene una consecuencia, aclarala.
- **Intuitiva y accionable.** El usuario debe poder seguir las instrucciones mientras opera el sistema. Priorizá el paso a paso por sobre la descripción abstracta.
- **Consistente.** Mismos términos, misma estructura y mismo formato en todos los módulos. La consistencia es lo que convierte páginas sueltas en un manual.

---

## 3. Estructura de cada página de módulo

Toda página de módulo debe seguir esta estructura, en este orden:

### 3.1. Título del módulo (H1)

Una sola línea con el nombre del módulo. Ejemplo: `# Caja`

### 3.2. Párrafo introductorio

Un párrafo de 3 a 5 oraciones que explique **qué es el módulo** y **para qué sirve**. Debe responder: ¿qué resuelve esta herramienta y qué puede hacer el usuario desde acá? Redactado en tono de bienvenida y en segunda persona (ver sección 4).

### 3.3. Secciones de procedimiento (H3)

Cada tarea concreta se documenta en su propia sección, encabezada con un H3 que **empiece con un verbo en infinitivo** y describa la acción. Ejemplos:

- `### Cargar un gasto`
- `### Controlar los ingresos mensuales`
- `### Anular un comprobante`

Dentro de cada sección: el paso a paso, las rutas de menú, las notas y las imágenes que correspondan.

---

## 4. Cómo escribir un procedimiento (paso a paso)

Esta es la parte más importante de la documentación. Para mantener la coherencia, usamos **tres formatos según la complejidad del procedimiento**. Elegí el que corresponda en cada caso:

| Situación | Formato a usar |
|---|---|
| Indicar una navegación simple (llegar a una pantalla) | **Breadcrumb** (3.1) |
| Explicar un proceso de varios pasos | **Stepper de GitBook** (3.2) |
| Detallar los campos de un formulario | **Tabla de campos** (3.3) |

Estos formatos se combinan: un stepper puede contener un breadcrumb dentro de un paso, y un paso puede cerrar con una tabla de campos.

### 4.1. Navegación simple → Breadcrumb

Cuando solo necesitás indicar cómo llegar a una pantalla, usá un **breadcrumb**: cada nivel de menú entre `código` en línea y el separador `›` entre niveles. No uses mayúsculas sostenidas ni flechas `→`.

```
Entrá a `Menú` › `Setup` › `Rubros Generales` › `Nuevo Rubro`.
```

Cuando exista un enlace directo a esa pantalla en la plataforma, enlazá el último nivel del breadcrumb:

```
Entrá a `Menú` › `Setup` › [`Rubros Generales`](https://gestion.socioplus.com.ar/rubrosGenerales) › `Nuevo Rubro`.
```

Usalo para instrucciones de una sola acción ("Para ver la planilla, entrá a `Caja` › `Planilla de Caja Mensual`").

### 4.2. Procesos complejos → Stepper de GitBook

Cuando un procedimiento tiene varios pasos sucesivos, usá el componente **stepper** de GitBook. Se renderiza con números en círculo y una línea que conecta los pasos, lo que guía visualmente al usuario. Cada paso lleva un título en H3 y su explicación:

```
{% stepper %}
{% step %}
### Crear el rubro
Entrá a `Setup` › `Rubros Generales` › `Nuevo Rubro`.
{% endstep %}

{% step %}
### Configurar el signo
En el campo Signo, seleccioná *Negativo*, ya que el rubro corresponde a un gasto y debe restar.
{% endstep %}

{% step %}
### Confirmar
Revisá los datos y hacé clic en **Confirmar**.
{% endstep %}
{% endstepper %}
```

Dentro de un paso podés incluir un breadcrumb, una imagen, una nota o una tabla de campos según haga falta.

### 4.3. Orden lógico y prerrequisitos

Presentá los pasos en el orden exacto en que el usuario debe ejecutarlos. Si una acción requiere algo previo (por ejemplo, "antes de cargar un gasto hay que crear un Centro de Costo"), indicalo **antes** del paso y enlazá a la sección o página donde se explica:

```
En primer lugar, debés crear un Centro de Costo. Para saber cómo hacerlo, [clickeá aquí](https://documentacionsocioplus.pages.dev/setup.html#costo).
```

### 4.4. Campos de formulario → Tabla

Cuando un formulario tenga varios campos para completar, presentalos en una **tabla** de dos columnas (campo y valor). Es más legible que enumerarlos en una línea:

| Campo | Valor |
|---|---|
| Tipo de Rubros | Movimientos varios |
| Habilitado | Sí |
| Signo | Negativo |
| Visible en los reportes | Sí |

Si un campo necesita aclaración, agregala entre paréntesis en la celda de valor (por ejemplo: *Negativo (porque es un gasto y resta)*).

### 4.5. Advertencias y consecuencias

Cuando una acción tenga un efecto importante o un error frecuente, no lo omitas. Aclaralo explícitamente (y considerá usar una nota destacada, ver sección 5):

> Un rubro NO PUEDE estar vinculado a más de un Centro de Costo, ya que de lo contrario los informes de las planillas de caja se verán afectados.

---

## 5. Voz, persona y tono

- **Persona:** escribí en **segunda persona**, dirigiéndote al usuario ("podrás gestionar", "deberás ingresar", "hacé clic en Confirmar"). Mantené la misma persona dentro de una misma página; no alternes entre "usted", "vos" y "ustedes".
- **Tiempo verbal:** usá presente y futuro simple para las instrucciones ("se abre la pantalla", "deberás seleccionar la sede").
- **Voz activa:** preferí la voz activa ("el sistema genera el comprobante") por sobre la pasiva ("el comprobante es generado").
- **Tono:** formal, cordial y orientado a la acción. Nunca condescendiente.

---

## 6. Notas, advertencias y elementos destacados

Para resaltar información importante usá los bloques de aviso de GitBook (*hints*). Reservalos para datos que el usuario no debe pasar por alto:

```
{% hint style="info" %}
Recordá que el rubro debe estar habilitado para que aparezca en los reportes.
{% endhint %}
```

Criterio de uso de cada estilo:

- `style="info"` → aclaraciones y recordatorios útiles.
- `style="warning"` → acciones que pueden traer problemas si se hacen mal.
- `style="success"` → confirmaciones de que un proceso quedó correctamente realizado.
- `style="danger"` → acciones irreversibles o críticas (por ejemplo, eliminaciones).

No abuses de las notas: si todo está resaltado, nada lo está.

---

## 7. Imágenes y capturas

Las capturas de pantalla son parte esencial de la documentación. Reglas:

- Colocá la imagen **inmediatamente después** del paso que ilustra, no al final de la sección.
- Usá rutas consistentes. Las imágenes se referencian desde la carpeta publicada:

```
![Texto descriptivo](https://documentacionsocioplus.pages.dev/Imagenes/cajados.png)
```

- El **texto alternativo** (lo que va entre corchetes) debe describir qué muestra la imagen, no repetir "imagen" ni dejarse genérico. Por ejemplo `![Pantalla de Movimientos Varios](...)`, no `![imagen](...)`.
- Mostrá la pantalla relevante y, cuando ayude, marcá con un recuadro o flecha el campo o botón al que se refiere el texto.

---

## 8. Terminología y consistencia

Usá siempre los mismos términos para los mismos conceptos. Glosario de referencia:

- El producto se escribe **SocioPLUS** (con "PLUS" en mayúsculas).
- El organismo fiscal se nombra como **ARCA (ex-AFIP)** la primera vez que aparece en una página; luego, solo **ARCA**.
- Los nombres de **módulos, menús y botones** del sistema van en **mayúsculas** cuando se citan como elemento de la interfaz (SOCIOS, CAJA, CONFIRMAR).
- Mantené los nombres oficiales de cada módulo tal como figuran en el menú: Socios, Turnero, Débitos Automáticos, Caja, Factura Electrónica, Productos, Control de Acceso, Agenda Comercial, Planes de Entrenamiento, Aplicación Móvil, Setup.
- Evitá sinónimos para un mismo elemento. Si en un lado decís "comprobante", no lo llames "ticket" en otro.

---

## 9. Formato Markdown / GitBook

- Encabezados: `#` para el título del módulo, `###` para cada procedimiento. Evitá saltar niveles.
- Una sola idea por párrafo. Párrafos cortos antes que bloques extensos.
- Usá **negrita** para resaltar nombres de campos o conceptos clave, con moderación.
- Listas con viñetas para enumerar opciones o requisitos; listas numeradas solo cuando el orden de los pasos sea estricto.
- Enlaces internos (a otras páginas de la documentación) y enlaces a la plataforma (`gestion.socioplus.com.ar`) deben funcionar y abrir la pantalla correcta.
- **Correo de soporte:** toda mención a `soporte@socioplus.com.ar` debe escribirse como un hipervínculo `mailto:`, para que el usuario pueda abrir su correo con un solo clic. Escribilo así: `[**soporte@socioplus.com.ar**](mailto:soporte@socioplus.com.ar)`. La dirección va en negrita dentro del enlace, para que quede resaltada y a la vez sea clickeable.

---

## 10. Checklist antes de publicar

Antes de dar por terminada una página, verificá que cumpla todo lo siguiente:

- [ ] Tiene título H1 y un párrafo introductorio que explica qué es y para qué sirve el módulo.
- [ ] Cada procedimiento está en su propia sección H3 que empieza con un verbo.
- [ ] Las navegaciones simples usan breadcrumb (`código` con separador `›`); los procesos de varios pasos usan el stepper de GitBook; los formularios usan tabla de campos.
- [ ] Los enlaces a la plataforma y a otras páginas funcionan.
- [ ] Toda mención al correo de soporte está como hipervínculo `mailto:`.
- [ ] Las imágenes están ubicadas junto al paso que ilustran y tienen texto alternativo descriptivo.
- [ ] Se aclararon los prerrequisitos y las advertencias importantes.
- [ ] Se respetó la segunda persona y el tono formal en toda la página.
- [ ] El texto es comprensible para alguien sin conocimientos técnicos: sin jerga, con cada concepto explicado y los pasos guiados al detalle.
- [ ] La terminología coincide con la del resto de la documentación.
- [ ] No hay errores de ortografía ni de tipeo.

---

*Esta guía es un documento vivo. Si surge un caso no contemplado, resolvelo con el criterio más cercano a los principios de la sección 1 y proponé su incorporación a esta guía.*
