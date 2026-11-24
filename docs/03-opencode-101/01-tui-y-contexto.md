# 1. TUI, navegación y gestión del contexto

En esta guía aprenderás a moverte con soltura dentro de la **TUI (*Terminal User Interface*)** de OpenCode, a adjuntar archivos de forma exacta, a ejecutar comandos de sistema sin salir de la sesión y, sobre todo, a **controlar el crecimiento del contexto** para evitar que el modelo pierda precisión.

---

## 1. Preparación del proyecto demo (`01-demo`)

Para practicar desde cero sin afectar ningún proyecto real, crea una carpeta limpia y un archivo HTML básico:

=== "Terminal"

    ```bash
    mkdir 01-demo
    cd 01-demo
    ```

=== "Editor (VS Code / Cursor)"

    1. Abre la carpeta `01-demo` en tu editor.
    2. Crea el archivo `index.html`.
    3. Escribe `!` y presiona ++tab++ para generar la plantilla HTML5 básica.
    4. Dentro de `<body>`, agrega `<h1>Hello World</h1>`:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        <h1>Hello World</h1>
    </body>
    </html>
    ```

5. Abre una terminal integrada o externa **dentro de `01-demo`** y arranca OpenCode:

```bash
opencode
```

---

## 2. Dominar la TUI (*Terminal User Interface*)

La interfaz de terminal de OpenCode está pensada para trabajar casi sin usar el ratón. Estos son los elementos y atajos esenciales:

### Comandos sensibles al contexto (`/`)

Al escribir `/` en la caja de entrada se despliega la lista de comandos disponibles.

!!! note "Los comandos cambian según el estado de la sesión"
    En una sesión recién abierta **no verás** comandos como `/compact`, `/undo` o `/redo`. OpenCode es sensible al contexto y solo los muestra cuando ya existe historial de conversación o cambios que compactar o revertir.

### Agentes primarios y paleta de comandos

- **Cambiar agente primario (++tab++):** Alterna directamente entre los modos principales (por ejemplo, `Build` y `Plan`).
- **Paleta de comandos (++ctrl+p++):** Abre un buscador con *fuzzy search* (búsqueda aproximada) para ejecutar cualquier acción o comando de OpenCode rápidamente.

### Edición rápida de texto en el prompt

| Atajo | Acción | Nota práctica |
| :--- | :--- | :--- |
| ++ctrl+u++ | Borra toda la línea desde el cursor hasta el inicio | Ideal para descartar un prompt a medias |
| ++ctrl+w++ | Borra la palabra anterior al cursor | En la TUI, `Alt + Backspace` no funciona; usa siempre ++ctrl+w++ |
| Seleccionar con el ratón | Copia automáticamente al portapapeles | Al soltar la selección verás el aviso `Copied to clipboard` |

### Referenciar archivos físicos con `@`

Cuando quieras asegurarte de que el agente lea un archivo concreto en lugar de adivinar dónde está:

1. Escribe `@` seguido de parte del nombre (usa *fuzzy search*, por ejemplo `@index.html` o `@ind`).
2. Presiona ++tab++ o ++enter++ para seleccionarlo.
3. El nombre del archivo quedará **resaltado**, indicando que se adjunta como una referencia física directa al archivo en disco.

### Modo Shell integrado (`!`)

¿Necesitas comprobar una versión de Node, listar archivos o correr un script rápido sin cerrar OpenCode?

1. Escribe `!` como primer carácter del prompt. La barra cambiará a **modo Shell**.
2. Ejecuta cualquier comando de tu terminal, por ejemplo:

```bash
!node --version
# Salida ejemplo: v24.9.0
```

3. Presiona ++backspace++ con la línea vacía para salir del modo Shell y volver al chat normal.

!!! tip "El resultado entra en la conversación"
    La salida de cualquier comando ejecutado con `!` **pasa a formar parte del historial de la conversación**, por lo que el modelo de IA podrá leerla y usarla en tu siguiente pregunta (por ejemplo, si necesitas que analice un error de consola).

### Barra lateral de estado (++ctrl+x++ ++b++)

Presiona ++ctrl+x++ seguido de ++b++ para mostrar u ocultar el panel lateral derecho. Allí podrás supervisar en tiempo real:

- **Contexto consumido:** Cantidad exacta de tokens usados y porcentaje de la ventana (por ejemplo, `10.7K (5%)`).
- **Costo acumulado:** Gasto estimado de la sesión (por ejemplo, `$0.00` con modelos gratuitos o `$0.02` con modelos de pago).
- **Estado del LSP (*Language Server Protocol*):** Servidores de lenguaje activos.
- **Directorio de trabajo y versión de OpenCode** (por ejemplo, `1.17.8`).

---

## 3. ¿Cómo crece el contexto y la "zona tonta"?

Los modelos de lenguaje (LLMs) son **stateless** (*sin estado*): no tienen una base de datos interna persistente de tu proyecto. En cada turno que envías, OpenCode vuelve a mandar al modelo:

1. Las instrucciones del sistema (*system prompt*) y definiciones de herramientas.
2. El contenido de archivos de memoria como `AGENTS.md`.
3. **Todo el historial de mensajes anteriores** (tus prompts, las respuestas del modelo, los archivos leídos y las salidas de comandos `!`).

A medida que la conversación se alarga, la ventana de contexto se llena y el modelo entra en lo que en la práctica se conoce como la **"zona tonta"** (*dumb zone*): al tener demasiada información acumulada y ruidosa, su precisión cae, ignora instrucciones previas o empieza a cometer errores absurdos.

!!! warning "La regla de oro del 45–50 %"
    Mantén siempre el uso de contexto **por debajo del 50 %**. Cuando te acerques al **45 %**, toma una decisión:
    - Si vas a cambiar de tema o tarea: abre una **nueva sesión** (++ctrl+x++ ++n++). Es la opción más limpia y barata.
    - Si necesitas continuar exactamente con la misma tarea en curso: ejecuta **`/compact`**.

---

## 4. El comando `/compact`

Cuando ejecutas `/compact`, OpenCode pide al modelo que resuma toda la conversación actual en un reporte estructurado y reemplace el historial largo por ese resumen.

### Estructura del resumen generado

Al finalizar `/compact`, verás un documento Markdown con secciones como:

- **Goal:** Objetivo principal de la sesión.
- **Constraints & Preferences:** Restricciones y preferencias indicadas.
- **Progress (`Done`, `In Progress`, `Blocked`):** Qué se completó, qué está en curso y qué está bloqueado.
- **Key Decisions:** Decisiones técnicas tomadas.
- **Next Steps:** Próximos pasos inmediatos.
- **Critical Context & Relevant Files:** Datos críticos y archivos relevantes.

En sesiones reales, esto reduce drásticamente el consumo (por ejemplo, de `10.7K (5%)` a `1.4K (1%)`), liberando espacio para seguir trabajando.

!!! caution "Limitación importante de `/compact`"
    Es el propio **LLM** quien decide qué es importante conservar en el resumen y qué descartar. Si tenías un detalle muy específico en un mensaje antiguo, podría perderse tras compactar. Por eso, **siempre es preferible crear una sesión nueva (++ctrl+x++ ++n++) cuando cambias de tarea**.

---

## 5. Gestión de sesiones (`Ctrl+X L`, `Ctrl+R`, `opencode -c`)

OpenCode guarda tus conversaciones para que puedas retomarlas o organizarlas sin perder el hilo:

| Acción | Atajo / Comando | Uso recomendado |
| :--- | :--- | :--- |
| **Nueva sesión** | ++ctrl+x++ ++n++ (o `/new`) | Al iniciar una nueva funcionalidad o tarea independiente |
| **Listar / cambiar sesión** | ++ctrl+x++ ++l++ (o `/sessions`) | Abre el selector `Sessions` para volver a una conversación anterior |
| **Renombrar sesión** | ++ctrl+r++ (dentro de `Sessions`) | Pon nombres descriptivos (ej. `Instalar Tailwind CSS en proyecto`) |
| **Eliminar sesión** | ++ctrl+d++ (dentro de `Sessions`) | Borra sesiones de prueba (como `ping-pong`) |
| **Continuar última sesión** | `opencode -c` (en la terminal) | Abre OpenCode retomando directamente la última sesión activa del proyecto |
