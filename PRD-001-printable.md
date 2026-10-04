# PRD-001: Proyección de Haber Jubilatorio
## Actualización tecnológica del módulo de proyección en CPBA

---

## 1. Contexto y Problema

**Sitio:** https://www.cpbaonline.com.ar

**Situación actual:** Los afiliados de CPBA cuentan con una web desactualizada, sin un diseño de experiencia de usuario adecuado para generar la proyección de su haber jubilatorio.

**Problema:** Lentitud en el proceso y exceso de pantallas para llegar al objetivo, lo que genera fricción y una mala experiencia para el afiliado.

**Personas (perfil de usuario):** Profesionales de CPBA de todas las edades, desde joven profesional (26 años) a adultos mayores próximos a jubilarse (60/65 años).

**Historia de usuario:**
- Como usuario de CPBA quiero generar la proyección de mi haber básico jubilatorio para estimar cuál será mi haber el día que me jubile.

**Stakeholder clave:** Gerencia de Seguridad Social

---

## 2. Objetivos

**Objetivo principal:** Modernizar el sitio web de CPBA para que los afiliados puedan proyectar su haber básico jubilatorio de forma simple y rápida.

### Objetivos específicos:

- **OBJ-01:** Reducir de 2 pantallas a 1 sola la generación de la proyección del haber jubilatorio, verificado en producción durante el período de medición.

- **OBJ-02:** Reducir de 2 clics a 1 la generación del reporte con los valores propuestos por defecto (RNF-02), verificado en producción durante el período de medición.

- **OBJ-03:** Reducir el tiempo de generación del reporte de unos 4 s (medido a mano, desde el clic hasta ver el PDF) a < 2 s (p95), verificado en producción durante el período de medición.

- **OBJ-04:** Aumentar la cantidad de proyecciones generadas por mes de 5,5 (mediana de los meses con datos de 2026) a ≥ 11, medido durante el período de medición.

**Hitos:**
- Release a producción: antes del 28/02/2027
- Período de medición: del 01/03/2027 al 31/03/2027

---

## 3. Requerimientos Funcionales

- **RF-01:** El sistema debe obtener del servicio backend actual las fechas mínimas de jubilación del profesional para Jubilación Ordinaria y Jubilación Parcial al ingresar al módulo.

- **RF-02:** El sistema debe mostrar un botón con el texto "Inicie su trámite de jubilación aquí" si al menos una de las dos fechas mínimas de jubilación es menor o igual a hoy.

- **RF-03:** El sistema debe permitir cambiar el tipo de beneficio propuesto entre Jubilación Ordinaria y Jubilación Parcial y, al cambiarlo, reemplazar la fecha de jubilación elegida por la fecha mínima del nuevo beneficio.

- **RF-04:** El sistema debe permitir elegir la fecha de jubilación solo desde un selector de fecha, sin ingreso manual, con los días anteriores a la fecha mínima del beneficio elegido deshabilitados.

- **RF-05:** El sistema debe mostrar la alerta "No es posible realizar proyección para afiliados con estado matricular fallecido" y deshabilitar el botón **Proyectar** si el estado de la matrícula es "fallecido".

- **RF-06:** El sistema debe mostrar el botón **Proyectar**, que genera el reporte de la proyección con el tipo de beneficio y la fecha de jubilación elegidos.

- **RF-07:** El sistema debe preseleccionar el tipo de beneficio con la menor fecha mínima de jubilación, junto con esa fecha mínima, aunque sea anterior a hoy. Si las fechas mínimas de Jubilación Ordinaria y Jubilación Parcial son iguales, preseleccionar Jubilación Ordinaria.

- **RF-08:** El sistema debe mostrar el reporte de la proyección en formato PDF. En navegadores de escritorio debe abrirlo en una nueva pestaña. En dispositivos móviles debe abrirlo en una nueva pestaña o en el visor de PDF del dispositivo.

- **RF-09:** El sistema debe mostrar un indicador de carga y deshabilitar el botón **Proyectar** mientras espera la respuesta del servicio de proyección.

- **RF-10:** El sistema debe mostrar el mensaje "No pudimos generar su proyección. Intente nuevamente." y volver a habilitar el botón **Proyectar** para reintentar si el servicio de proyección devuelve un error o no responde en 10 segundos.

- **RF-11:** El sistema debe mostrar la alerta "Ud. se encuentra en condiciones de jubilarse" y deshabilitar el botón **Proyectar** si las dos fechas mínimas de jubilación son menores o iguales a hoy.

- **RF-12:** El sistema debe permitir elegir una fecha de jubilación distinta de la propuesta.

---

## 4. Requerimientos No Funcionales

- **RNF-01:** Performance: el tiempo de generación del reporte, medido desde que el frontend llama al servicio de generación del PDF hasta que recibe la respuesta completa, es < 2 s (p95), medido en producción durante el período de medición.

- **RNF-02:** Usabilidad: 1 clic para generar el reporte, cuando se usan los valores propuestos por defecto (RF-07).

- **RNF-03:** Responsive: en anchos de pantalla de 360 px a 1920 px, la pantalla de proyección se muestra sin scroll horizontal, sin elementos superpuestos ni textos cortados, y con el botón **Proyectar** visible sin necesidad de scroll.

- **RNF-04:** Accesibilidad: como algunos usuarios son adultos mayores, en la pantalla de proyección el contraste entre texto y fondo es ≥ 4,5:1 para el texto normal y ≥ 3:1 para el texto grande (WCAG AA).

- **RNF-05:** Compatibilidad: 5 navegadores, en su última versión estable: Chrome, Edge y Firefox en escritorio, y Chrome en Android y Safari en iOS en celular.

---

## 5. Criterios de Aceptación

- **AC-01 (RF-01):** Dado un afiliado con sesión iniciada en el sitio actual, cuando ingresa al módulo, entonces la pantalla muestra las fechas mínimas de Jubilación Ordinaria y Jubilación Parcial obtenidas del backend.

- **AC-02 (RF-11, RF-02):** Dado un afiliado con las dos fechas mínimas iguales a hoy, cuando ingresa al módulo, entonces se muestra la alerta "Ud. se encuentra en condiciones de jubilarse", el botón **Proyectar** está deshabilitado y se muestra el botón "Inicie su trámite de jubilación aquí".

- **AC-03 (RF-11, RF-02):** Dado un afiliado con las dos fechas mínimas anteriores a hoy, cuando ingresa al módulo, entonces se ve lo mismo que en AC-02.

- **AC-04 (RF-02, RF-11):** Dado un afiliado con la fecha mínima de Jubilación Parcial igual a hoy y la de Jubilación Ordinaria igual a mañana, cuando ingresa al módulo, entonces se muestra el botón "Inicie su trámite de jubilación aquí" (no la alerta de "en condiciones de jubilarse") y el botón **Proyectar** está habilitado.

- **AC-05 (RF-02, RF-11):** Dado un afiliado con las dos fechas mínimas iguales a mañana, cuando ingresa al módulo, entonces no se muestran el botón de trámite ni la alerta, y el botón **Proyectar** está habilitado.

- **AC-06 (RF-07):** Dado un afiliado cuya fecha mínima de Jubilación Parcial es anterior a la de Ordinaria, cuando ingresa al módulo, entonces está preseleccionada Jubilación Parcial con su fecha mínima.

- **AC-07 (RF-07):** Dado un afiliado con las dos fechas mínimas iguales y posteriores a hoy, cuando ingresa al módulo, entonces está preseleccionada Jubilación Ordinaria con esa fecha.

- **AC-08 (RF-07):** Dado un afiliado con la fecha mínima de Jubilación Parcial anterior a hoy y la de Ordinaria posterior a hoy, cuando ingresa al módulo, entonces está preseleccionada Jubilación Parcial con su fecha (anterior a hoy).

- **AC-09 (RF-03):** Dado un afiliado con Jubilación Parcial preseleccionada y una fecha elegida distinta de la mínima, cuando cambia a Jubilación Ordinaria, entonces la fecha elegida pasa a ser la fecha mínima de Jubilación Ordinaria.

- **AC-10 (RF-12):** Dado un afiliado con la fecha mínima F preseleccionada, cuando elige en el selector la fecha F + 1 año, entonces la fecha elegida es F + 1 año.

- **AC-11 (RF-04):** Dado un beneficio elegido con fecha mínima F, cuando el afiliado abre el selector de fecha, entonces el día F está habilitado y el día F − 1 está deshabilitado.

- **AC-12 (RF-04):** Dado el selector de fecha, cuando el afiliado intenta escribir una fecha con el teclado, entonces la fecha elegida no cambia.

- **AC-13 (RF-05):** Dado un afiliado con estado de matrícula "fallecido", cuando ingresa al módulo, entonces se muestra la alerta "No es posible realizar proyección para afiliados con estado matricular fallecido" y el botón **Proyectar** está deshabilitado.

- **AC-14 (RF-05):** Dado un afiliado con estado "activa", "cancelado" o "suspendido" (una prueba por estado) y las dos fechas mínimas posteriores a hoy, cuando ingresa al módulo, entonces no se muestra la alerta de fallecido y el botón **Proyectar** está habilitado.

- **AC-15 (RF-06, RF-08):** Dado un afiliado con estado "activa" que eligió el beneficio B y la fecha D, cuando hace clic en **Proyectar** en un navegador de escritorio, entonces el PDF se abre en una nueva pestaña.

- **AC-16 (RF-08):** Dado el mismo afiliado de AC-15, cuando hace clic en **Proyectar** en Chrome en Android o en Safari en iOS, entonces el PDF se abre en una nueva pestaña o en el visor de PDF del dispositivo.

- **AC-17 (RF-09):** Dado un servicio de proyección que tarda en responder, cuando el afiliado hace clic en **Proyectar**, entonces mientras espera se ve el indicador de carga, el botón **Proyectar** está deshabilitado y no puede hacer clic nuevamente.

- **AC-18 (RF-10):** Dado un servicio de proyección que devuelve un error, cuando el afiliado hace clic en **Proyectar**, entonces se muestra "No pudimos generar su proyección. Intente nuevamente." y el botón **Proyectar** vuelve a estar habilitado.

- **AC-19 (RF-10):** Dado un servicio de proyección que no responde, cuando el afiliado hace clic en **Proyectar**, entonces a los 10 s se muestra "No pudimos generar su proyección. Intente nuevamente." y el botón **Proyectar** vuelve a estar habilitado.

- **AC-20 (RNF-01):** Dado el período de medición en producción, cuando se calcula el tiempo desde que el frontend llama al servicio de generación del PDF hasta que recibe la respuesta completa, entonces el p95 es < 2 s.

- **AC-21 (RNF-02):** Dado un afiliado con estado "activa" y las dos fechas mínimas posteriores a hoy, cuando ingresa al módulo y, sin cambiar ningún valor, hace 1 clic en **Proyectar**, entonces se genera y abre el PDF.

- **AC-22 (RNF-03):** Dada la pantalla con los valores por defecto, cuando se ve en 360×640, 768×1024, 1024×768 y 1920×1080 px, entonces no hay scroll horizontal, elementos superpuestos ni textos cortados, y el botón **Proyectar** está visible.

- **AC-23 (RNF-04):** Dada la pantalla de proyección, cuando se audita con una herramienta automática de accesibilidad (por ejemplo, axe) y se miden los elementos, entonces el contraste es ≥ 4,5:1 para texto normal y ≥ 3:1 para texto grande.

- **AC-24 (RNF-05):** Dados los 5 navegadores de RNF-05, cuando se ejecutan los AC-01 a AC-19, entonces todos pasan en los 5.

---

## 6. Fuera de Alcance

- Cambios en el cálculo del haber jubilatorio y en el contenido o diseño del reporte PDF.
- Desarrollo del login: lo provee el sitio actual.
- Proyección de cualquier beneficio distinto de Jubilación Ordinaria y Jubilación Parcial.
- Desarrollo de nuevos servicios o endpoints backend. En una segunda etapa, luego del lanzamiento del nuevo sitio, se actualizarán la API y los servicios backend, incluidos el cálculo del haber jubilatorio y el reporte PDF.
- Uso de mockups o datos de prueba en producción: los mockups son solo para desarrollo y QA.

---

## 7. Riesgos y Dependencias

### Riesgos:

1. Los servicios backend actuales no están disponibles en los ambientes de desarrollo o QA
   - **Mitigación:** Creación de 2 mockups con datos personales del profesional, fechas mínimas de jubilación, estado de matrícula y reporte PDF.

2. En algún navegador móvil el PDF queda solo como descarga, en vez de abrirse en una nueva pestaña o en el visor de PDF del dispositivo (RF-08)
   - **Mitigación:** Probar AC-16 en Chrome en Android y Safari en iOS antes del lanzamiento.

3. Los afiliados de 60 años o más cometen errores táctiles o no comprenden la pantalla
   - **Mitigación:** Cumplir RNF-04 y hacer una prueba de usabilidad con afiliados de 60 años o más antes del lanzamiento.

4. Los servicios backend actuales, que este proyecto no modifica, pueden no alcanzar el tiempo de < 2 s (p95) de RNF-01 y OBJ-03
   - **Mitigación:** Validar con Infraestructura que los servicios actuales pueden soportar este requisito.

### Dependencias:

- El login y la sesión del sitio actual.
- Los servicios backend actuales de datos del afiliado, estado de matrícula, condición de jubilación, fechas mínimas y generación del PDF.
- Los diseños de pantalla aprobados (Opción A — Tarjeta única guiada).
- El sitio actual no muestra el acceso al módulo de proyección a los afiliados jubilados (indicador de jubilado que devuelve el backend), por lo que este módulo no contempla ese caso.

---

## 8. Anexos

### 8.1 Proyecciones generadas por mes (2026)

| Mes | Cantidad |
|---|---|
| Febrero | 4 |
| Marzo | sin datos |
| Abril | sin datos |
| Mayo | 4 |
| Junio | 5 |
| Julio | 12 |
| Agosto | 23 |
| Septiembre | 6 |

**Mediana de los meses con datos:** 5,5

---

## Notas para impresión

- Este documento está optimizado para impresión en tamaño A4 en orientación vertical.
- Se recomienda usar márgenes estándar (2,54 cm en todos los lados).
- Para mejor legibilidad, imprimir a color.
- Las imágenes adjuntas (pantalla actual y propuestas) deben ser referenciadas desde el repositorio de GitHub.

**Documento generado:** 2026-10-04
**Versión:** 1.0
