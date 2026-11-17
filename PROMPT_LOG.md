# Bitácora de Prompts - Assignment 2

**Estudiante:** Brandon Guzmán Mérida  
**Curso:** LINQ03.2 - Applied Algorithmic Thinking  

---

## 1. Especificación (Paso 1)
* **Qué hace:** Calcula el punto de reorden y alerta de stock crítico para un producto basado en el inventario actual, el promedio de ventas diarias y el tiempo de entrega (lead time) del proveedor.
* **Qué datos/inputs necesita:** `inventario_actual` (número entero), `ventas_diarias_promedio` (número flotante) y `lead_time_dias` (número entero).
* **Cómo se sabrá que funciona:** Al ingresar inventario = 12, ventas promedio = 4 y lead time = 5, el sistema debe arrojar una alerta de stock crítico y calcular un punto de reorden de 25 unidades.

---

## 2. Registro de Construcción y Dirección de IA (Paso 2)
* **Propuesta de la IA:** Sugirió conectar una API externa de clima y pasarelas de pago internacionales para prever retrasos por temporada de lluvias.
* **Decisión:** **RECHAZADO**
  * **Razón:** Se salía por completo de los 3 campos de mi especificación original. Le ordené a la IA que se limitara estrictamente a calcular la fórmula matemática local con los 3 parámetros definidos.

---

## 3. Registro de Validación (Paso 3)
* **Input exacto utilizado:**
  ```json
  {
    "inventario_actual": 12,
    "ventas_diarias_promedio": 4,
    "lead_time_dias": 5
  }
