# Initial Requirements (User Stories)

- As a user, I want to leave a review for a product I have purchased.
- Users can rate products from 1 to 5 stars.
- Reviews must be approved by an administrator before they are public.
- Users should see their reviews on the product page instantly after submitting them.
- The system should display the average rating on the product page.
- Reviews can have a title and a text of up to 500 characters.

## Análisis de Requerimientos Asistido por IA

### 1. Ambigüedades Detectadas
- No se especifica si los usuarios pueden editar o eliminar sus reseñas después de enviarlas.
- No queda claro si los usuarios pueden dejar más de una reseña por producto.
- "Instantly" puede referirse a la publicación inmediata o solo a la visualización privada antes de la aprobación.
- No se define qué sucede si la reseña es rechazada por el administrador.
- No se aclara si el título de la reseña es obligatorio u opcional.
- No se indica si el promedio de calificaciones incluye solo reseñas aprobadas o todas las enviadas.

### 2. Inconsistencias y Contradicciones
- Se indica que las reseñas deben ser aprobadas antes de ser públicas, pero también que los usuarios ven sus reseñas “instantáneamente” en la página del producto, lo que puede contradecir la moderación previa.
- No se especifica si la visualización instantánea es solo para el usuario que envió la reseña o para todos los usuarios.

### 3. Brechas funcionales (requerimientos faltantes)
- Falta definir el proceso de aprobación/rechazo por parte del administrador (notificaciones, motivos de rechazo, etc.).
- No se menciona si hay validaciones para evitar lenguaje ofensivo, spam o contenido inapropiado.
- No se especifica si los usuarios deben estar autenticados para dejar una reseña.
- No se indica si hay límites en la cantidad de reseñas por usuario/producto.
- No se contempla la posibilidad de reportar reseñas por otros usuarios.
- No se menciona si los usuarios pueden responder o comentar reseñas.
- No se define cómo se calcula y actualiza el promedio de calificaciones.
- No se aclara si la reseña puede contener imágenes u otros adjuntos.

### 4. Riesgos Potenciales
- **Negocio:** Reseñas negativas no moderadas pueden afectar la reputación del producto/marca.
- **Experiencia de usuario:** Retrasos en la aprobación pueden frustrar a los usuarios si no ven sus reseñas públicas rápidamente.
- **Compliance:** Falta de filtros puede permitir contenido ofensivo, difamatorio o ilegal.
- **Fraude:** Usuarios pueden manipular calificaciones creando cuentas falsas o dejando múltiples reseñas.
- **Privacidad:** No se menciona cómo se protege la identidad del usuario en las reseñas.
- **Moderación:** Sobrecarga para administradores si el volumen de reseñas es alto.

### Preguntas para Stakeholders
- ¿Los usuarios pueden editar o eliminar sus reseñas después de enviarlas?
- ¿La visualización “instantánea” es solo para el usuario que envió la reseña o para todos?
- ¿El promedio de calificaciones incluye solo reseñas aprobadas?
- ¿El título de la reseña es obligatorio?
- ¿Se permite adjuntar imágenes o archivos en las reseñas?
- ¿Qué sucede si una reseña es rechazada por el administrador?
- ¿Hay límites en la cantidad de reseñas por usuario/producto?
- ¿Qué mecanismos existen para prevenir fraude y contenido inapropiado?

## Requerimientos Optimización Propuesta y Casos de Prueba

### 1. Creación de Reseñas
- Solo usuarios autenticados que hayan comprado el producto pueden crear una reseña.
- Cada usuario puede dejar **una única reseña por producto**, editable/eliminable antes y después de la aprobación.
- La reseña debe incluir:
  - Calificación de 1 a 5 estrellas (**obligatorio**).
  - Título (**obligatorio**, hasta 100 caracteres).
  - Texto (**opcional**, hasta 500 caracteres).
  - Imágenes adjuntas (**opcional**, máx. 3; formatos JPG/PNG; hasta 2MB cada una).

**Criterios de Aceptación:**
- **Given** un usuario autenticado con compra válida, **When** intenta crear una reseña, **Then** el sistema permite la creación si cumple los requisitos de formato y cantidad.
- **Given** un usuario sin compra válida, **When** intenta crear una reseña, **Then** el sistema rechaza la acción y muestra un mensaje de error.
- **Given** una reseña con más de 500 caracteres en el texto, **When** intenta enviarla, **Then** el sistema rechaza la acción y muestra un mensaje de error.

---

### 2. Moderación
- Todas las reseñas deben ser aprobadas por un administrador antes de ser públicas.
- El administrador puede aprobar o rechazar la reseña, indicando el motivo en caso de rechazo.
- El usuario recibe notificación sobre el estado de su reseña (aprobada/rechazada).

**Criterios de Aceptación:**
- **Given** una reseña enviada, **When** el administrador la aprueba, **Then** la reseña se publica y es visible para todos.
- **Given** una reseña enviada, **When** el administrador la rechaza, **Then** la reseña no se publica y el usuario recibe el motivo del rechazo.

---

### 3. Visualización
- El usuario que envió la reseña puede verla **instantáneamente** en la página del producto, marcada como “pendiente de aprobación”.
- Solo las reseñas aprobadas son visibles para otros usuarios.
- Las reseñas rechazadas no son visibles en la página del producto.

**Criterios de Aceptación:**
- **Given** una reseña pendiente, **When** el usuario accede a la página del producto, **Then** ve su reseña con estado “pendiente”.
- **Given** una reseña aprobada, **When** cualquier usuario accede a la página del producto, **Then** la reseña es visible.
- **Given** una reseña rechazada, **When** el usuario accede a la página del producto, **Then** la reseña no aparece.

---

### 4. Métricas
- El sistema muestra el promedio de calificaciones en la página del producto, **calculado solo con reseñas aprobadas**.
- El promedio se actualiza automáticamente al aprobar o eliminar una reseña.

**Criterios de Aceptación:**
- **Given** varias reseñas aprobadas, **When** se aprueba una nueva reseña, **Then** el promedio se recalcula y actualiza.
- **Given** una reseña eliminada, **When** se elimina, **Then** el promedio se recalcula y actualiza.

---

### 5. Reglas de Datos y Validaciones
- No se permite lenguaje ofensivo, spam ni contenido inapropiado en texto o imágenes.
- El sistema valida el formato y tamaño de los campos antes de guardar la reseña.
- Los usuarios pueden reportar reseñas inapropiadas.

**Criterios de Aceptación:**
- **Given** una reseña con lenguaje ofensivo, **When** se envía, **Then** el sistema rechaza la reseña y muestra un mensaje de error.
- **Given** una imagen fuera de formato/tamaño, **When** se adjunta, **Then** el sistema rechaza la imagen y muestra un mensaje de error.
- **Given** una reseña inapropiada, **When** otro usuario la reporta, **Then** el sistema notifica al administrador para revisión.

---

### 6. Permisos y Seguridad
- Solo usuarios autenticados pueden crear, editar o eliminar sus reseñas.
- Solo administradores pueden aprobar/rechazar reseñas.
- La identidad del usuario se protege y no se muestra públicamente, salvo el nombre de usuario.

**Criterios de Aceptación:**
- **Given** un usuario no autenticado, **When** intenta crear/editar/eliminar una reseña, **Then** el sistema rechaza la acción.
- **Given** un usuario autenticado, **When** accede a la página del producto, **Then** solo ve su nombre de usuario asociado a su reseña.

## Tabla de Casos de Prueba de Alto Nivel

| # | Caso de Prueba | Resultado Esperado |
|---|---|---|
| 1 | Usuario autenticado con compra válida crea reseña con 5 estrellas y texto | Reseña creada; visible para usuario; pendiente de aprobación. |
| 2 | Usuario autenticado con compra válida crea reseña con 1 estrella y texto | Reseña creada; visible para usuario; pendiente de aprobación. |
| 3 | Usuario intenta crear reseña con texto de 501 caracteres | Error de validación; reseña no creada. |
| 4 | Usuario intenta crear reseña sin título | Error de validación; reseña no creada. |
| 5 | Usuario adjunta imagen de 3MB en reseña | Error de validación; imagen rechazada. |
| 6 | Usuario sin compra válida intenta crear reseña | Acción rechazada; mensaje de error. |
| 7 | Usuario crea reseña; administrador la aprueba | Reseña visible para todos; promedio actualizado. |
| 8 | Usuario crea reseña; administrador la rechaza con motivo | Reseña no visible; usuario recibe motivo de rechazo. |
| 9 | Usuario edita reseña pendiente; cambia texto | Cambios guardados; estado sigue pendiente. |
|10 | Usuario reporta reseña ofensiva | Notificación enviada al administrador para revisión. |

