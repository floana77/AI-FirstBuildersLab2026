# 📄 Product Requirements Document (PRD)

## 1. Resumen Ejecutivo
- **Nombre del producto/proyecto:** Actualización tecnológica: Proyección de Haber Jubilatorio en CPBA. https://www.cpbaonline.com.ar
- **Objetivo principal:** Modernizar el sitio web de CPBA para que los afiliados puedan proyectar su haber básico jubilatorio de forma simple y rápida.
- **Stakeholders clave:**  Gerencia de Seguridad Social. 

---

## 2. Contexto y Problema
- **Situación actual:** Los afiliados de CPBA cuentan con una web desactualizada, sin un diseño de experiencia de usuario adecuado para generar la proyección de su haber jubilatorio.
- **Problema que se busca resolver:** Mejorar la experiencia de usuario y modernizar tecnológicamente el sitio actual.
- **Impacto del problema en usuarios/negocio:**  Lentitud en el proceso y exceso de pantallas para llegar al objetivo, lo que genera fricción y una mala experiencia para el afiliado.

---

## 3. Objetivos del Producto
- **Objetivos principales:**  
 * Mantener en 1 sola pantalla  la generación proyección del haber jubilatorio, antes del 31/03/2027.
 * Lograr que el 100% de las proyecciones se generen en menos de 2 segundos y con un solo clic, medido durante el primer mes post-lanzamiento (abril 2027).
 * Liberar la nueva versión del módulo de proyección en producción antes del 28/02/2027, reutilizando los servicios backend actuales sin desarrollar nuevos endpoints.
  

---

## 4. Alcance
- **Incluido en el alcance:**  Actualización de frontend y reutilización de los servicios backend actuales. La proyección sólo estará disponible como es actualmente para los beneficios de Jubilación Ordinaria y Jubilación Parcial.
- **Excluido del alcance:**
   * Desarrollo de nuevos servicios backend. En una segunda etapa, luego del lanzamiento del nuevo site, se trabajará en la actualización de api y servicios backend, donde se incluye cálculo de haber jubilatorio y generación de reporte pdf.
   * El login no será desarrollado será provisto por el sitio actual. 

---

## 5. Usuarios y Casos de Uso
- **Perfil de usuario:**  profesionales de CPBA, todas la edades desde joven profesional (26 años) a adultos mayores próximos a Jubilarse (60/65 años).
- **Historia de usuario :**  
  - Como usuario de CPBA quiero generar la proyección de mi Haber Básico Jubilatorio para estimar cuál será mi haber el día que me jubile.

---

## 6. Requerimientos Funcionales
- **RF-01:** Al ingresar a la web el sistema obtiene y valida las fechas minimas de jubilacion del profesional (jub.ordinaria y Jub.Parcial) son mayores a hoy significa que NO está en condiciones de jubilarse, debe mostrar una alerta indicando la situación y no permite proyectar.
- **RF-02:** En caso de estar habilitada la proyección (con condicion jubilatoria ) debe mostrar un botón con el texto Inicie su trámite de Jubilación aquí.
- **RF-03:** Una vez que ingresa el profesional el sistema debe proponer tipo de beneficio ( Jubilación Parcial u Ordinaria) y fecha mínima de Jubilación.  
- **RF-04:** El sistema no debe permitir proyectar con fechas menores a la fecha mínima de Jubilación ( cumpleaños 60/65).
- **RF-05:** El sistema no debe permitir proyectar si el afiliado presenta estado de la matricula fallecido, debe informarlo con una alerta y no permite proyectar.
- **RF-06:** Si la fecha de jubilación elegida (por defecto es la minima) es >= a la fecha mínima de Jubilación, el sistema debe mostrar un botón para generar el reporte de la proyección.
---

## 7. Requerimientos No Funcionales
- **RNF-01:** Performance: generación del reporte en menos de 2s.
- **RNF-02:** Experiencia: Cantidad mínimas de clics para generar el reporte (1). Al ingresar a la web las fechas y tipo de beneficio  son propuestas por el sistema, por defecto , se debe posicionar o resaltar la menor fecha de jubilación según el tipo de beneficio, luego  el usuario sólo debe realizar un clic en el botón para generar el reporte de proyección.
-  **RNF-03:** Experiencia: tanto la pantalla como reporte deben ser visibles con claridad en resoluciones de tablet y celular.
-  **RNF-04:** Experiencia: como algunos usuarios son adultos mayores la pantalla debe ser simple, de alto contraste, con textos grandes y botones amplios que faciliten la lectura y reduzcan los errores táctiles.
-  **RNF-05:** Experiencia: el reporte de la proyección debe mostrarse en una nueva pestañay en formato pdf
  ---
## 8. Casos de Prueba/aceptación

| Nº | Funcionalidad | Caso de prueba | Datos de entrada | Resultado esperado |
|----|---------------|----------------|------------------|--------------------|
| 1  | Validación de condición de jubilación | Ingreso al sistema con profesional que cumple requisitos de jubilación | Fecha mínima de jubilación menor o igual a hoy | Se muestra alerta “Ud. se encuentra en condiciones de jubilarse” y el botón **Proyectar** se deshabilita |
| 2  | Validación profesional con condición de jubilación | Ingreso al sistema con profesional que cumple requisitos de jubilación | Fecha de jubilación mínima menor o igual a hoy | aparece un botón "Inicie su trámite de jubilación aquí |
| 3  | Selección de tipo de beneficio | Profesional accede al módulo de proyección | Usuario activo con matrícula vigente | Se despliega opciones **Jubilación Parcial** y **Jubilación Ordinaria**, y se muestra la **fecha mínima de jubilación** calculada |
| 4  | Validación de fecha mínima | Profesional intenta proyectar con fecha anterior al cumpleaños 60 | Fecha ingresada < fecha mínima de jubilación | Se muestra mensaje de error “La fecha ingresada no puede ser menor a la fecha mínima de jubilación” y no se genera proyección |
| 5  | Validación de estado de matrícula | Profesional con estado “Fallecido” intenta proyectar | Estado de matrícula = Fallecido | Se muestra alerta “No es posible realizar proyección para afiliados con estado matricular fallecido” y se bloquea el botón **Proyectar** |
| 6  | Generación de reporte PDF | Profesional proyecta correctamente con datos válidos | Fecha seleccionada > fecha mínima, estado activo | Se genera reporte en formato **PDF**,se muestra en una nueva pestaña y el archivo contiene los datos de la proyección |
---
## 9. Pantalla actual

<img width="1340" height="872" alt="image" src="https://github.com/user-attachments/assets/6e3e4bcc-06fe-466c-94cc-d57cd687b2a2" />

---
## 10. Propuestas iniciales de pantallas  
<img width="869" height="923" alt="Opción A — Tarjeta única guiada@1x" src="https://github.com/user-attachments/assets/dc2b249c-39a0-4cd9-ad15-731edd62ab4c" />
<img width="909" height="817" alt="Opción A — Ya en condiciones de jubilarse@1x" src="https://github.com/user-attachments/assets/dbf58425-8333-43ae-8b2e-989a18cd319d" />

---
## 11. Riesgos y Dependencias
- Riesgo: servicios actuales no disponibles → mitigación: creación de mockup (2) datos personales del profesional, fechas minimas de jubilacion por tipo y reporte pdf de prueba.
- Dependencia: actualmente se utiliza   servicios del backend y login del sitio actual.
  En caso de no funcionar los servicios o en una primera versión => mitigación creación de mockups.

