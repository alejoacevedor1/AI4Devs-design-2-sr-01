# CU-01: Crear y Publicar Vacante

## 1. Roles Involucrados

- **Reclutador**  
  Usuario encargado de crear y publicar la vacante en el sistema LTI.
- **Sistema LTI**  
  Componente responsable de validar datos, guardar la vacante, publicarla en canales externos y emitir el evento `job.published`.

---

## 2. Precondición

- El usuario está autenticado y posee el rol **RECRUITER**.

---

## 3. Happy Path (Flujo Principal)

1. El reclutador abre el formulario **Nueva vacante** desde el dashboard.
2. Completa los campos obligatorios:
   - **Título**
   - **Descripción**
   - **Requerimientos**
   - **Tipo de contrato**
   - **Salario**
   - **Localización**
3. Selecciona uno o más canales de publicación:
   - LinkedIn
   - Portal interno
4. Hace clic en **Publicar**.
5. El sistema:
   1. Persiste la vacante en la base de datos.
   2. Publica en los canales seleccionados.
   3. Emite el evento `job.published`.
6. Se muestra el mensaje de confirmación **“Vacante publicada con éxito”**.
7. El reclutador es redirigido al listado de vacantes.

---

## 4. Criterios de Aceptación (Happy Path)

- El botón **Nueva vacante** está visible y accesible desde el dashboard.
- El formulario carga en menos de 1 segundo.
- Todos los campos obligatorios están claramente marcados (\*).
- El sistema valida en tiempo real:
  - Salario: debe ser numérico y mayor que 0.
  - Formatos de texto (sin caracteres prohibidos).
- Si falta o es inválido algún campo, se muestra un mensaje de error junto al campo.
- El botón **Publicar** solo se habilita cuando todos los datos son válidos.
- Si no se selecciona ningún canal, al hacer clic en **Publicar** aparece el error “Debe seleccionar al menos un canal”.
- Al publicar:
  - La vacante se guarda sin errores.
  - Se invocan las APIs de LinkedIn y/o portal interno según selección.
  - Se emite el evento `job.published`.
- Tras la publicación exitosa:
  1. Se muestra un toast/modal “Vacante publicada con éxito”.
  2. El usuario es redirigido al listado de vacantes.

---

## 5. Historias de Usuario

### Historia 1: Abrir formulario de nueva vacante

**Como** Reclutador  
**Quiero** abrir el formulario “Nueva vacante”  
**Para** iniciar el proceso de creación de una nueva vacante.

**Criterios de Aceptación**

1. Existe un botón o enlace **“Nueva vacante”** en el dashboard.
2. Al hacer clic, el formulario se muestra en < 1 s.
3. El encabezado del formulario indica **“Crear Nueva Vacante”**.

---

### Historia 2: Completar datos de la vacante

**Como** Reclutador  
**Quiero** rellenar los campos Título, Descripción, Requerimientos, Tipo de contrato, Salario y Localización  
**Para** que el sistema almacene toda la información necesaria.

**Criterios de Aceptación**

1. Los campos obligatorios están marcados con un asterisco (\*).
2. Validaciones en tiempo real:
   - **Salario**: numérico y > 0.
   - **Texto**: sin caracteres especiales inválidos.
3. Si hay error o campo vacío, se muestra mensaje junto al campo.
4. El botón **Publicar** queda habilitado solo cuando todos los datos son válidos.

---

### Historia 3: Seleccionar canales de publicación

**Como** Reclutador  
**Quiero** elegir los canales donde publicar la vacante (LinkedIn y/o Portal interno)  
**Para** alcanzar a los candidatos adecuados.

**Criterios de Aceptación**

1. Se muestran casillas de verificación para **LinkedIn** y **Portal Interno**.
2. Puedo seleccionar una o ambas.
3. Si no se selecciona ninguno, al publicar aparece “Debe seleccionar al menos un canal”.
4. Al seleccionar un canal, aparece texto de ayuda: “La vacante se publicará en [X]”.

---

### Historia 4: Publicar vacante y emitir evento

**Como** Reclutador  
**Quiero** hacer clic en **Publicar**  
**Para** guardar la vacante, publicarla en los canales elegidos y notificar al sistema.

**Criterios de Aceptación**

1. La vacante se persiste en la base de datos sin errores.
2. Se llaman las APIs de LinkedIn y/o portal interno según lo seleccionado.
3. El sistema emite el evento `job.published`.
4. Aparece un toast/modal **“Vacante publicada con éxito”**.
5. Tras la confirmación, el usuario es redirigido al listado de vacantes.

---

# Backlog de Producto – CU-01 “Crear y Publicar Vacante”

Este backlog está estructurado en sprints de alta prioridad y refleja las tareas necesarias para implementar las cuatro historias de usuario de CU-01. Cada ítem incluye un título, descripción y criterios mínimos de definición de terminado (DoD).

---

## **Sprint 1: Setup y Formulario Base**

1. **TICKET-001: Crear componente “Nueva Vacante” (UI)**

   - **Descripción:**
     - Añadir botón “Nueva vacante” en el dashboard de reclutador.
     - Enrutamiento a la pantalla/formulario de creación.
   - **DoD:**
     - Botón visible y accesible en dashboard.
     - Al hacer clic, se abre la pantalla “Crear Nueva Vacante”.

2. **TICKET-002: Maquetar formulario de vacante (UI)**

   - **Descripción:**
     - Crear fields: Título, Descripción, Requerimientos, Tipo de contrato, Salario, Localización.
     - Marcar los campos obligatorios con \*.
   - **DoD:**
     - Todos los campos renderizan con labels claros.
     - Asterisco junto a cada campo obligatorio.

3. **TICKET-003: Validaciones de formulario (Front)**
   - **Descripción:**
     - Validar en tiempo real:
       - Título, Descripción, Requerimientos: no vacíos.
       - Salario: numérico y > 0.
     - Mostrar mensajes de error inline.
   - **DoD:**
     - Al ingresar valor inválido, aparece mensaje junto al field.
     - “Publicar” deshabilitado hasta que todo sea válido.

---

## **Sprint 2: Selección de Canales & Publicación**

4. **TICKET-004: Selección de canales de publicación (UI)**

   - **Descripción:**
     - Añadir checkboxes para “LinkedIn” y “Portal Interno”.
     - Mostrar texto de ayuda dinámico.
   - **DoD:**
     - Pueden seleccionarse 0–2 canales.
     - Al marcar/unmark, se actualiza texto “Se publicará en…”.

5. **TICKET-005: Validar selección de canales (Front)**

   - **Descripción:**
     - Si no hay canales seleccionados y el usuario da “Publicar”, mostrar error global.
   - **DoD:**
     - Error “Debe seleccionar al menos un canal” aparece en top del formulario.

6. **TICKET-006: Integración API – Crear vacante (Back)**

   - **Descripción:**
     - Endpoint `POST /jobs` que recibe payload con todos los campos + canales.
     - Persistir en base de datos.
   - **DoD:**
     - 201 Created con ID de vacante retornado.
     - Los datos en DB coinciden con el payload enviado.

7. **TICKET-007: Integración API – Publicar en LinkedIn y Portal Interno (Back)**
   - **Descripción:**
     - Consumir servicio de LinkedIn y portal interno según canales seleccionados.
     - Implementar adaptadores/retry básico.
   - **DoD:**
     - Llamadas a APIs externas realizadas sólo para canales marcados.
     - Logs de éxito/error disponibles.

---

## **Sprint 3: Eventos y UX Final**

8. **TICKET-008: Emisión de evento `job.published` (Back)**

   - **Descripción:**
     - Al finalizar creación + publicaciones externas, publicar evento al Event Bus.
   - **DoD:**
     - Mensaje en topic `jobs` con payload `{ jobId, channels, timestamp }`.

9. **TICKET-009: Toast/Modal de confirmación (UI)**

   - **Descripción:**
     - Mostrar toast o modal “Vacante publicada con éxito” tras respuesta exitosa.
     - Auto‐dismiss en 3s o acción de “Ver vacantes”.
   - **DoD:**
     - Feedback visible tras “Publicar”.
     - Usuario redirigido al listado tras dismiss.

10. **TICKET-010: Redirección al listado de vacantes (UI)**
    - **Descripción:**
      - Tras confirmación, navegar automáticamente al listado.
    - **DoD:**
      - URL actualiza a `/jobs`.
      - Listado recargado mostrando la nueva vacante.

---

## **Definición de Terminado (DoD) General para CU-01**

- Código revisado en PR y aprobado en code review.
- Unit tests (front + back) cubriendo validaciones y payloads.
- Documentación mínima de la API (`POST /jobs`) en swagger/OpenAPI.
- QA funcional: recorrido del happy path sin errores manuales.
- Integración continua: build y tests pasan sin warnings.
- Cumple criterios de aceptación de todas las historias de usuario.

---

> Una vez completados estos tickets, CU-01 estará implementado y probado. Podremos entonces cerrar este flujo y pasar a CU-02 o iterar sobre mejoras/no‐funcionales adicionales.

---

# Estimaciones de Esfuerzo – CU-01 “Crear y Publicar Vacante”

## Metodología de Estimación

Se combinó **Planning Poker** (con opiniones de esfuerzo óptimo, más probable y pesimista) y **Cálculo PERT** para obtener una estimación equilibrada en horas:

- **O**: Estimación optimista
- **M**: Estimación más probable
- **P**: Estimación pesimista

---

## Tabla de Estimaciones

| Ticket     | Descripción Breve                                       | O (h) | M (h) | P (h) | PERT (h) |
| ---------- | ------------------------------------------------------- | :---: | :---: | :---: | -------: |
| TICKET-001 | Crear componente “Nueva Vacante” (UI)                   |   3   |   4   |   6   |     4.17 |
| TICKET-002 | Maquetar formulario de vacante (UI)                     |   6   |   8   |  10   |     8.00 |
| TICKET-003 | Validaciones de formulario (Front)                      |   4   |   6   |   8   |     6.00 |
| TICKET-004 | Selección de canales de publicación (UI)                |   2   |   4   |   6   |     4.00 |
| TICKET-005 | Validar selección de canales (Front)                    |   2   |   3   |   5   |     3.17 |
| TICKET-006 | Integración API – Crear vacante (Back)                  |   8   |  12   |  16   |    12.00 |
| TICKET-007 | Integración API – Publicar en LinkedIn y Portal Interno |  10   |  16   |  24   |    16.33 |
| TICKET-008 | Emisión de evento `job.published` (Back)                |   4   |   6   |   8   |     6.00 |
| TICKET-009 | Toast/Modal de confirmación (UI)                        |   2   |   4   |   6   |     4.00 |
| TICKET-010 | Redirección al listado de vacantes (UI)                 |   1   |   2   |   3   |     2.00 |

---

**Total estimado (sumatoria PERT): ≈ 64 horas**

> Estas cifras deben validarse y ajustarse en la sesión de Planning Poker del equipo antes de comprometerse en el Sprint Planning.
