# 📄 Product Requirements Document (PRD)

## 1. Resumen Ejecutivo
- **Nombre del producto/proyecto:** Actualización tecnológica: Proyección de Haber Jubilatorio en CPBA
- **Objetivo principal:** Actualizar el sitio web para proyectar el haber básico jubilatorio
- **Stakeholders clave:**  Gerencia de Seguridad Social

---

## 2. Contexto y Problema
- **Situación actual:**  Los afiliados de CPBA cuentan con una web desactualizada, sin diseño de experiencia de usuario cuando quieren generar la proyección de su haber jubilatorio.
- **Problema que se busca resolver:**  mejorar la experiencia del usuario, actualizar la tecnológicamente el sitio actual.
- **Impacto del problema en usuarios/negocio:**  lentitud, demasiadas pantallas para llegar al objetivo.

---

## 3. Objetivos del Producto
- **Objetivos principales (SMART):**  
  - [ ] Mejorar la experiencia de usuario.
  - [ ] Actualizar el sitio web de la proyección del haber básico Jubilatorio.
  

---

## 4. Alcance
- **Incluido en el alcance:**  Actualización front end y reutilización de servicios actuales.
- **Excluido del alcance:** desarrollo de servicios nuevos backend. 

---

## 5. Usuarios y Casos de Uso
- **Perfil de usuario:**  profesionales de CPBA
- **User stories (ejemplo):**  
  - Como [tipo de usuario], quiero [acción] para [beneficio].
  - Como usuario de CPBA quiero generar la proyección de mi Haber Básico Jubilatorio para estimar cuál será mi haber cuando me jubile.

---

## 6. Requerimientos Funcionales
- **Funcionalidad 1:** Al ingresar a la web el sistema valida si el profesional está en condiciones de jubilarse, si se puede jubilar debe mostrar una alerta indicando la situación y no permite proyectar
- **Funcionalidad 2:** Una vez que ingresa el profesional debe proponer tipo de beneficio ( Jubilación Parcial u Ordinaria) y fecha mínima de Jubilación.  
- **Funcionalidad 3:** El sistema no debe permitir proyectar con fechas menores a la fecha mínima de Jubilacion( cumpleaños 60)
- **Funcionalidad 4:** El sistema no debe permitir proyectar si el afiliado presenta estado de la matricula fallecido.
- **Funcionalidad 5:** Si la fecha de jubilación elegida es >= a la fecha mínima de Jubilación, el sistema debe mostrar un reporte de la proyección en formato pdf y que se pueda descargar.
---

## 7. Requerimientos No Funcionales
- **Performance:**  Generación del reporte en menos de 2s
-  **Experiencia:** Cantidad mínimas de clics para generar el reporte : 1. Al ingresar a la web las fechas y tipo de beneficio  son propuestas por el sistema y el usuario realiza clic en un botón para generar el reporte de proyección.

## 8. Casos de Prueba

| Nº | Funcionalidad | Caso de prueba | Datos de entrada | Resultado esperado |
|----|---------------|----------------|------------------|--------------------|
| 1  | Validación de condición de jubilación | Ingreso al sistema con profesional que cumple requisitos de jubilación | Usuario con edad ≥ 60 y aportes completos | Se muestra alerta “Ud. se encuentra en condiciones de jubilarse” y el botón **Proyectar** se deshabilita |
| 2  | Selección de tipo de beneficio | Profesional accede al módulo de proyección | Usuario activo con matrícula vigente | Se despliega combo con opciones **Jubilación Parcial** y **Jubilación Ordinaria**, y se muestra la **fecha mínima de jubilación** calculada |
| 3  | Validación de fecha mínima | Profesional intenta proyectar con fecha anterior al cumpleaños 60 | Fecha ingresada < fecha mínima de jubilación | Se muestra mensaje de error “La fecha ingresada no puede ser menor a la fecha mínima de jubilación” y no se genera proyección |
| 4  | Validación de estado de matrícula | Profesional con estado “Fallecido” intenta proyectar | Estado de matrícula = Fallecido | Se muestra alerta “No es posible realizar proyección para afiliados fallecidos” y se bloquea el botón **Proyectar** |
| 5  | Generación de reporte PDF | Profesional proyecta correctamente con datos válidos | Fecha ≥ fecha mínima, estado activo | Se genera reporte en formato **PDF**, se muestra botón **Descargar**, y el archivo contiene los datos de la proyección |

## 9. Pantalla actual

<img width="1340" height="872" alt="image" src="https://github.com/user-attachments/assets/6e3e4bcc-06fe-466c-94cc-d57cd687b2a2" />


