# 4. `Plan Mode`, `Build Mode` y comparación de modelos

Pedir cambios complejos directamente a un modelo de IA puede provocar que edite archivos equivocados o tome atajos no deseados. Para evitarlo, OpenCode separa el trabajo en dos agentes primarios: **`Plan`** (solo lectura) y **`Build`** (escritura y ejecución).

---

## 1. `Plan` vs `Build` y asignación de modelos

Presiona ++tab++ en cualquier momento dentro de la TUI para alternar entre `Plan` y `Build`.

| Modo | Permisos | Modelo recomendado en el curso | Objetivo |
| :--- | :--- | :--- | :--- |
| **`Plan`** | **Solo lectura** (no puede editar archivos) | Modelo más capaz/razonador (ej. `GLM-5.2` en `OpenCode Go`) | Analizar el código, hacer preguntas aclaratorias y redactar un plan paso a paso |
| **`Build`** | **Lectura y escritura** | Modelo ágil o económico (ej. `MiMo V2.5 Free` en `OpenCode Zen`, o `GLM-5.2`) | Ejecutar el plan aprobado, editar archivos y verificar el resultado |

!!! tip "Cada agente recuerda su propio modelo"
    OpenCode guarda de forma independiente qué modelo elegiste con `/models` para `Plan` y cuál elegiste para `Build`. Así puedes pensar con un modelo potente y construir con uno más rápido o gratuito.

---

## 2. Flujo práctico: instalar Tailwind CSS paso a paso

### Paso 1: Planificar en `Plan Mode`

1. Presiona ++tab++ hasta ver **`Plan`** en la barra inferior.
2. Envía el prompt:
   ```text
   Necesito que instalemos Tailwind CSS en este proyecto
   ```
3. El agente leerá los archivos (`index.html`, `README.md`, `AGENTS.md`) sin modificar nada y te hará preguntas interactivas para no adivinar tus preferencias:
   - `¿Cómo quieres instalar Tailwind?`
     1. `Tailwind Play CDN (Recomendado)`
     2. `Tailwind CLI via npm`
     3. `Tailwind CLI binario`
     4. `Type your own answer`
4. Selecciona **`Tailwind Play CDN (Recomendado)`**.

!!! info "Garantía de solo lectura en `Plan Mode`"
    Incluso si intentas forzarlo diciendo *"Necesito que lo implementes y que lo configures"*, el agente responderá algo como: **`Estoy en Plan Mode (solo lectura)`** y únicamente emitirá el **`Plan final`** con los archivos a modificar, *trade-offs* (por ejemplo, que Play CDN es para aprendizaje y no para producción) y pasos de verificación.

### Paso 2: Ejecutar en `Build Mode`

1. Presiona ++tab++ para cambiar a **`Build`**.
2. Aprueba la ejecución:
   ```text
   Ok, implementalo
   ```
3. El agente creará una lista de tareas (`# Todos`) e implementará el plan:
   - Añadirá `<script src="https://cdn.tailwindcss.com"></script>` y la configuración `tailwind.config` en `<head>`.
   - Sustituirá el estilo *inline* de `<body>` por la clase utilitaria `bg-brand` (con el color `#d8b4e2`).
   - Actualizará `AGENTS.md` indicando que Tailwind Play CDN ya está activo.

!!! note "Aviso normal en DevTools"
    Al abrir `index.html` en el navegador y revisar la consola de desarrollador, verás una advertencia de Tailwind indicando que el Play CDN no está pensado para producción. En un proyecto de aprendizaje como `01-demo`, es el comportamiento esperado.

### Paso 3: Iterar manteniendo el contexto del nuevo stack

Una vez implementado, puedes seguir pidiendo ajustes en `Build`:

```text
Necesito que cambies el color de fondo a un morado pastel. Recuerda que estamos usando Tailwind
```

El agente actualizará la definición de `brand` en `tailwind.config` a `#c8a2e8` manteniendo la clase `bg-brand`.

---

## 3. ¿Por qué modelos distintos dan resultados diferentes?

Aunque envíes exactamente el mismo prompt, el resultado cambia según **el modelo, su entrenamiento y el contexto disponible**. En la demostración opcional del módulo:

- Con **`MiMo V2.5 Free`** siguiendo el flujo de `Plan`, creó un bloque `tailwind.config` con el color `brand` personalizado.
- Al revertir el archivo y pedirle directamente en `Build` (*one-shot prompt*) a **`GLM-5.2`**:
  ```text
  Necesito que implementes Tailwind CSS en este proyecto
  ```
  `GLM-5.2` consultó `index.html`, `README.md` y `AGENTS.md`, e implementó Tailwind Play CDN **sin** crear `tailwind.config`, usando directamente una clase arbitraria `bg-[#d8b4e2]` y añadiendo por iniciativa propia clases al título (`text-3xl font-bold text-gray-800`) para mejorar la legibilidad.

Ambos resultados funcionan, pero ilustran una regla esencial: **un modelo puede tomar libertades que no pediste**.

---

## 4. Auditoría obligatoria: `git diff` y `git log`

Nunca apruebes a ciegas lo que escribió la IA:

1. **Revisa el `git diff`:** Antes de hacer *commit*, abre **Source Control** en VS Code/Cursor (o ejecuta `git diff` en la terminal) para comparar línea por línea qué se quitó y qué se agregó.
2. **Haz *commit* consciente:** En cuanto haces *commit*, el responsable de ese código eres tú, no OpenCode ni el modelo.
3. **Consulta el historial con OpenCode:** Como tu proyecto usa Git, puedes abrir una sesión nueva y preguntar cosas como *"¿Quién hizo el último cambio?"* o *"¿Qué hizo esa persona?"*; OpenCode ejecutará comandos como `git log -1 --format='%an <%ae> (%ar)'` y `git show --stat HEAD` para darte trazabilidad completa.
