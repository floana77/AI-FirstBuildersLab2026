# PRD-001: Proyección de Haber Jubilatorio CPBA — Pantalla única para que el afiliado proyecte su haber básico jubilatorio con un solo clic

## Contexto y Problema

**Situación actual.** El sitio de CPBA (https://www.cpbaonline.com.ar) tiene una pantalla de proyección de haber jubilatorio desactualizada y sin diseño de experiencia de usuario. Para llegar a la proyección el afiliado recorre 3 pantallas y espera [COMPLETAR: segundos actuales hasta obtener el reporte].

**Problema.** El proceso es lento y tiene exceso de pantallas, lo que genera fricción y una mala experiencia para el afiliado.

**Personas.**
- **Joven profesional (desde 26 años):** quiere estimar de forma rápida cuál será su haber el día que se jubile.
- **Profesional próximo a jubilarse (60/65 años):** necesita la misma estimación con una pantalla simple, legible y con botones amplios, porque puede tener menor destreza digital y cometer errores táctiles.

**Historia de usuario.** Como profesional afiliado a CPBA quiero generar la proyección de mi Haber Básico Jubilatorio para estimar cuál será mi haber el día que me jubile.

**Stakeholder clave:** Gerencia de Seguridad Social.

**Referencias visuales.**
- Pantalla actual: ![Pantalla actual](https://github.com/user-attachments/assets/6e3e4bcc-06fe-466c-94cc-d57cd687b2a2)
- Propuesta Opción A — Tarjeta única guiada: ![Opción A](https://github.com/user-attachments/assets/dc2b249c-39a0-4cd9-ad15-731edd62ab4c)
- Propuesta Opción A — Ya en condiciones de jubilarse: ![Opción A jubilable](https://github.com/user-attachments/assets/dbf58425-8333-43ae-8b2e-989a18cd319d)

## Objetivos

Ganar significa que el afiliado obtiene su proyección en una sola pantalla, en un solo clic y en menos de 2 segundos, sin nuevos servicios backend.

- **OBJ-01:** Reducir de 3 pantallas a 1 sola pantalla la generación de la proyección.
- **OBJ-02:** Lograr que el p95 de las proyecciones se genere en menos de 2 s y con un solo clic (con los valores propuestos por defecto), medido durante marzo 2027, el primer mes posterior al lanzamiento.
- **OBJ-03:** Liberar la nueva versión del módulo de proyección en producción antes del 28/02/2027, reutilizando los servicios backend actuales sin desarrollar nuevos endpoints.
- **OBJ-04:** [COMPLETAR: métrica de producto, por ejemplo % de afiliados que completan la proyección, reducción de consultas al soporte o satisfacción, con línea base y meta].

**Hitos:** release antes del 28/02/2027 · medición en marzo 2027 · cierre de la medición y reporte de resultados el 31/03/2027.

## Requerimientos Funcionales

- RF-01: El sistema debe consultar al ingresar, con los servicios backend actuales, los datos del afiliado, el estado de su matrícula y si está en condiciones de jubilarse.
- RF-02: El sistema debe mostrar la alerta "Ud. se encuentra en condiciones de jubilarse" y deshabilitar el botón Proyectar cuando el afiliado está en condiciones de jubilarse. La regla que define "en condiciones de jubilarse" la determina el servicio backend actual.
- RF-03: El sistema debe mostrar el botón "Inicie su trámite de Jubilación aquí" cuando la proyección esté deshabilitada por RF-02, y al hacer clic debe llevar al afiliado a https://www.cpbaonline.com.ar/apps/Main#/BeneficiosPrecarga/SolicitudJubilacion/Index.
- RF-04: El sistema debe mostrar un combo de tipo de beneficio con las opciones Jubilación Parcial y Jubilación Ordinaria, y la fecha mínima de jubilación del beneficio seleccionado. La edad mínima es 60 años en ambos beneficios; Jubilación Parcial además requiere 10 años de aportes ininterrumpidos y Jubilación Ordinaria requiere 30 años de aportes. La fecha mínima la calcula el servicio backend actual.
- RF-05: El sistema debe preseleccionar y resaltar al ingresar el beneficio con la menor fecha mínima y esa fecha como fecha de proyección. Si ambas fechas mínimas coinciden, debe preseleccionar Jubilación Ordinaria.
- RF-06: El sistema debe rechazar una fecha de jubilación menor a la fecha mínima, mostrar el mensaje "La fecha ingresada no puede ser menor a la fecha mínima de jubilación" y no generar la proyección.
- RF-07: El sistema debe bloquear la proyección cuando el estado de matrícula es "Fallecido", mostrar la alerta "No es posible realizar proyección para afiliados fallecidos" y deshabilitar el botón Proyectar.
- RF-08: El sistema debe generar el reporte de proyección en PDF y abrirlo en una nueva pestaña cuando la fecha elegida es mayor o igual a la fecha mínima de jubilación.
- RF-09: El sistema debe mostrar el mensaje "No pudimos generar su proyección. Intente nuevamente." y un botón Reintentar cuando el servicio backend devuelva error o no responda dentro de [COMPLETAR: segundos de espera máxima].

## Requerimientos No Funcionales

- RNF-01: Performance: el reporte se genera en menos de 2 s (p95), medido desde el clic en Proyectar hasta que el PDF se abre en la nueva pestaña, en producción.
- RNF-02: Usabilidad: la proyección se genera con 1 clic y en 1 sola pantalla cuando el afiliado acepta los valores propuestos por defecto.
- RNF-03: Responsive: la pantalla se ve sin scroll horizontal ni elementos superpuestos en resoluciones de 360×640 (celular) a 1024×768 (tablet), y el PDF se abre sin error en esas resoluciones (valores propuestos, a validar).
- RNF-04: Accesibilidad: contraste de texto ≥ 4,5:1 (WCAG 2.1 AA), texto base ≥ 16 px y botones y controles táctiles ≥ 44×44 px (valores propuestos, a validar).
- RNF-05: Compatibilidad: funciona en Chrome, Edge y Firefox, en su última versión estable, en escritorio, tablet y celular, y en Safari en iOS (iPhone y iPad), en la versión vigente de iOS.

## Criterios de Aceptación

- AC-01 (RF-01): Dado un afiliado autenticado con el login del sitio actual, cuando ingresa al módulo, entonces la pantalla obtiene datos, estado de matrícula y condición de jubilación con los servicios existentes, sin solicitarle datos al usuario y sin invocar ningún endpoint nuevo.
- AC-02 (RF-02): Dado un afiliado en condiciones de jubilarse, cuando ingresa al módulo, entonces se muestra la alerta "Ud. se encuentra en condiciones de jubilarse" y el botón Proyectar queda deshabilitado.
- AC-03 (RF-03): Dado un afiliado en condiciones de jubilarse, cuando ingresa al módulo, entonces se muestra el botón "Inicie su trámite de Jubilación aquí"; al hacer clic, el navegador va a https://www.cpbaonline.com.ar/apps/Main#/BeneficiosPrecarga/SolicitudJubilacion/Index.
- AC-04 (RF-04): Dado un afiliado activo que no está en condiciones de jubilarse, cuando ingresa al módulo, entonces se muestra un combo con exactamente 2 opciones (Jubilación Parcial y Jubilación Ordinaria) y la fecha mínima de jubilación del beneficio seleccionado; al cambiar de opción, la fecha mínima mostrada cambia al valor que devuelve el servicio para ese beneficio.
- AC-05 (RF-05): Dado un afiliado activo que no está en condiciones de jubilarse, cuando ingresa al módulo, entonces el beneficio con la menor fecha mínima y esa fecha aparecen preseleccionados y resaltados.
- AC-05b (RF-05): Dado un afiliado activo cuyas fechas mínimas de Jubilación Parcial y Jubilación Ordinaria son iguales, cuando ingresa al módulo, entonces Jubilación Ordinaria aparece preseleccionada.
- AC-06 (RF-06): Dado un afiliado activo con fecha mínima de jubilación F, cuando ingresa una fecha anterior a F (por ejemplo, F − 1 día) y hace clic en Proyectar, entonces se muestra "La fecha ingresada no puede ser menor a la fecha mínima de jubilación" y no se genera ningún PDF.
- AC-07 (RF-07): Dado un afiliado con estado de matrícula "Fallecido", cuando ingresa al módulo, entonces se muestra "No es posible realizar proyección para afiliados fallecidos" y el botón Proyectar queda deshabilitado.
- AC-08 (RF-08): Dado un afiliado activo con fecha mínima F, cuando elige la fecha F exacta y hace clic en Proyectar, entonces se abre una nueva pestaña con un PDF que contiene los mismos campos que el reporte que genera hoy el servicio actual para el mismo afiliado y fecha ([COMPLETAR: lista de campos]).
- AC-09 (RF-08): Dado un afiliado activo con fecha elegida mayor a F, cuando hace clic en Proyectar, entonces se abre una nueva pestaña con el PDF y la pestaña original de la pantalla se mantiene abierta.
- AC-10 (RF-09): Dado un afiliado activo y el servicio de proyección caído o sin respuesta, cuando hace clic en Proyectar, entonces se muestra "No pudimos generar su proyección. Intente nuevamente." con un botón Reintentar; al hacer clic en Reintentar se vuelve a invocar el servicio.
- AC-11 (RNF-01): Dado un afiliado activo con valores válidos, cuando se miden las proyecciones de un mes en producción, entonces el p95 del tiempo entre el clic en Proyectar y la apertura del PDF es menor a 2 s.
- AC-12 (RNF-02): Dado un afiliado activo con los valores propuestos por defecto, cuando genera la proyección, entonces el número de clics desde la carga de la pantalla hasta el PDF es exactamente 1 y el flujo ocurre en 1 sola pantalla.
- AC-13 (RNF-03): Dado un afiliado activo, cuando abre el módulo y genera la proyección en viewports de 360×640 y 1024×768, entonces no hay scroll horizontal ni elementos superpuestos y el PDF se abre sin error.
- AC-14 (RNF-04): Dada la pantalla del módulo, cuando se audita con una herramienta de accesibilidad, entonces todo el texto tiene contraste ≥ 4,5:1, el texto base es ≥ 16 px y todos los botones y controles miden ≥ 44×44 px.
- AC-15 (RNF-05): Dados Chrome, Edge y Firefox en su última versión estable y Safari en la versión vigente de iOS, cuando se ejecutan AC-02 a AC-10, entonces todos pasan en cada navegador.

## Fuera de Alcance

- Desarrollo de nuevos servicios o endpoints backend. En una segunda etapa, luego del lanzamiento del nuevo sitio, se actualizarán la API y los servicios backend, incluidos el cálculo del haber jubilatorio y la generación del PDF.
- Cambios en el cálculo del haber jubilatorio y en el contenido o diseño del reporte PDF.
- Desarrollo del login: lo provee el sitio actual.
- Proyección de cualquier beneficio distinto de Jubilación Ordinaria y Jubilación Parcial.
- Bloqueo de la proyección por estados de matrícula distintos de "Fallecido".
- El trámite de jubilación en sí: el botón "Inicie su trámite de Jubilación aquí" solo redirige.
- Uso de mockups en producción: son solo para desarrollo y QA.

## Riesgos y Dependencias

- Riesgo: los servicios backend actuales no están disponibles durante el desarrollo → mitigación: mockups solo para desarrollo y QA con datos personales del profesional, fechas mínimas por tipo de beneficio y un reporte PDF de prueba.
- Riesgo: los servicios backend actuales no están disponibles o fallan en producción → mitigación: RF-09 muestra un mensaje de error con botón Reintentar; nunca se muestran datos de mockup a un afiliado real.
- Riesgo: la latencia del backend actual, que no se modifica, impide cumplir p95 < 2 s → mitigación: medir la latencia actual del servicio [COMPLETAR: línea base] antes del release y escalar a la Gerencia de Seguridad Social si supera el objetivo.
- Riesgo: el PDF se descarga en vez de abrirse en una nueva pestaña en navegadores móviles → mitigación: probar AC-13 en los navegadores móviles soportados, incluido Safari en iOS, antes del release.
- Riesgo: los afiliados de 60 años o más cometen errores táctiles o no comprenden la pantalla → mitigación: cumplir RNF-04 y hacer una prueba de usabilidad con afiliados de 60 años o más antes del release.
- Riesgo: no llegar al release del 28/02/2027 → mitigación: alcance acotado a frontend y reutilización de servicios existentes; si no se llega, se sigue utilizando el sitio actual hasta liberar la nueva versión.
- Dependencia: el login y la sesión del sitio actual.
- Dependencia: los servicios backend actuales de datos del afiliado, estado de matrícula, condición de jubilación, fechas mínimas y generación del PDF.
- Dependencia: los diseños de pantalla aprobados (Opción A — Tarjeta única guiada).

