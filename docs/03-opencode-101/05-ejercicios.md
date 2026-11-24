---
title: "5. Batería de ejercicios (Módulo 03)"
description: Ejercicios prácticos de dificultad incremental con pistas y soluciones ocultas sobre TUI, contexto, /compact, /undo y /redo con Git, /init, AGENTS.md, .ignore, Plan Mode y auditoría.
---

# 5. Batería de ejercicios — OpenCode 101

Esta batería de ejercicios cubre **todos los conceptos y flujos de trabajo del Módulo 03** en dificultad progresiva, desde la navegación básica en la TUI hasta la planificación en `Plan Mode` y la auditoría profesional con Git.

!!! tip "Cómo aprovechar esta batería en clase"
    Trabaja sobre una carpeta de pruebas limpia (por ejemplo `01-demo-ejercicios`) para poder experimentar con libertad. Despliega la **pista** solo si te atascaste y abre la **solución** únicamente después de intentarlo.

---

## Nivel 1 · Navegación fluida en la TUI

### Ejercicio 1: Referencias físicas (`@`) y modo Shell (`!`)

**Enunciado:**

1. Crea una carpeta `01-demo-ejercicios` y dentro un archivo `index.html` con la plantilla básica HTML5 y un `<h1>Mi práctica</h1>`.
2. Abre OpenCode en esa carpeta y, usando el **modo Shell integrado** (sin salir de OpenCode), comprueba tu versión de Node (`node --version`) o de Git (`git --version`).
3. Sal del modo Shell y, en el siguiente mensaje normal, **adjunta físicamente `index.html`** usando `@` y pídele al agente que añada un párrafo debajo del `<h1>` mostrando la versión que acaba de salir en la consola.

??? tip "Ver pista"
    El modo Shell se activa escribiendo `!` al inicio del prompt y se desactiva con ++backspace++ en una línea vacía. Para referenciar un archivo físico, escribe `@ind` y pulsa ++tab++.

??? success "Ver solución"
    1. Crea `index.html` con `!` + ++tab++ en tu editor y añade `<h1>Mi práctica</h1>` dentro de `<body>`.
    2. En OpenCode, escribe:
       ```bash
       !node --version
       ```
       Pulsa ++enter++ (la salida, ej. `v24.9.0`, pasa a formar parte del historial de la conversación).
    3. Pulsa ++backspace++ para salir del modo Shell.
    4. Escribe `@index.html` (pulsa ++tab++ para que quede resaltado) y completa el prompt:
       ```text
       En @index.html añade un párrafo <p> debajo del h1 indicando la versión de Node que acaba de mostrarse.
       ```
       El modelo leerá la salida del comando `!` del historial y editará `index.html` correctamente.

---

### Ejercicio 2: Edición rápida y comandos sensibles al contexto

**Enunciado:**

1. Escribe una frase larga en la caja de entrada de OpenCode sin enviarla:
    - Borra solo la **última palabra** usando el atajo correcto de la TUI (recuerda que `Alt+Backspace` no funciona).
    - Luego borra **toda la línea hasta el inicio** con un solo atajo.
2. Abre una sesión completamente nueva (++ctrl+x++ ++n++) y escribe `/`. ¿Por qué **no aparecen** `/undo`, `/redo` ni `/compact` en la lista?
3. Muestra la barra lateral derecha (++ctrl+x++ ++b++) e identifica los 3 datos clave que debes vigilar mientras trabajas.

??? tip "Ver pista"
    Los atajos de edición de línea tipo Unix en la TUI usan la tecla `Ctrl` (++ctrl+w++ y ++ctrl+u++).

??? success "Ver solución"
    1. **Edición de texto en la TUI:**
       - Borrar la palabra anterior: ++ctrl+w++
       - Borrar desde el cursor hasta el inicio de la línea: ++ctrl+u++
    2. **¿Por qué no aparecen `/undo`, `/redo` ni `/compact`?**
       Porque el menú `/` de OpenCode es **sensible al contexto**: solo muestra comandos que tienen sentido en el estado actual. En una sesión vacía no hay historial que compactar ni mensajes/cambios que deshacer.
    3. **Datos clave en la barra lateral (++ctrl+x++ ++b++):**
       - **Tokens consumidos y porcentaje de contexto** (para no pasar del 45–50 %).
       - **Costo acumulado** de la sesión (`$0.00`, `$0.02`, etc.).
       - **Directorio de trabajo, estado LSP y versión de OpenCode.**

---

## Nivel 2 · Gestión del contexto, `/compact` y sesiones

### Ejercicio 3: Diagnóstico de la "zona tonta" (`/new` vs `/compact`)

**Enunciado:**
Decide qué acción tomarías en cada uno de estos 4 escenarios y justifica tu respuesta:

1. Acabas de terminar el diseño del encabezado (contexto al 28 %) y ahora vas a empezar una tarea totalmente distinta: configurar el formulario de contacto.
2. Llevas 45 minutos depurando un error complejo en el mismo módulo, el contexto ha llegado al **48 %** y todavía necesitas continuar con el mismo bug sin perder el hilo.
3. Un compañero lleva el contexto al **82 %** y se queja de que el modelo "se volvió tonto", ignora reglas y repite errores que ya habían corregido.
4. Tras ejecutar `/compact`, te das cuenta de que el modelo olvidó un requisito muy específico que mencionaste hace 20 mensajes. ¿Por qué pasó esto?

??? tip "Ver pista"
    Recuerda la regla del 45–50 %, la naturaleza *stateless* de los LLMs y quién decide qué información sobrevive en un `/compact`.

??? success "Ver solución"
    1. **Abrir nueva sesión (`/new` o ++ctrl+x++ ++n++):** Como cambias de tarea, arrastrar el historial del encabezado solo consume tokens y añade ruido innecesario.
    2. **Ejecutar `/compact`:** Estás cerca del límite recomendado del 45–50 % y necesitas mantener el hilo de la **misma tarea**. `/compact` resume el historial en un reporte estructurado (`Goal`, `Progress`, `Key Decisions`, `Next Steps`) y libera espacio.
    3. **Está en la "zona tonta" (*context rot*):** Al superar ampliamente el 50 %, el exceso de historial degrada la precisión del modelo. Debe compactar o, preferiblemente, abrir una nueva sesión con un resumen limpio de lo que falta.
    4. **Limitación de `/compact`:** Es el propio **LLM** quien decide qué considera importante conservar en el resumen. Por eso detalles muy concretos pueden perderse y siempre es mejor usar sesiones cortas y enfocadas.

---

### Ejercicio 4: Compactar y organizar sesiones

**Enunciado:**

1. En una sesión con al menos 3–4 intercambios sobre `index.html`, ejecuta `/compact` y compara el porcentaje/tokens **antes y después** en la barra lateral.
2. Abre el gestor de sesiones con ++ctrl+x++ ++l++:
    - Renombra tu sesión actual a `Practica HTML y Contexto`.
    - Crea una sesión rápida de prueba (`ping`), vuelve al gestor y **elimínala**.
3. Sal de OpenCode (++ctrl+c++) y vuelve a abrir directamente la última sesión desde la terminal.

??? tip "Ver pista"
    Dentro del modal `Sessions` (++ctrl+x++ ++l++), fíjate en la leyenda inferior: ++ctrl+r++ renombra y ++ctrl+d++ elimina.

??? success "Ver solución"
    1. Escribe `/compact` y pulsa ++enter++. Verás el reporte estructurado (`Goal`, `Constraints & Preferences`, `Progress`, `Key Decisions`, `Next Steps`, `Relevant Files`) y una caída fuerte del contador de tokens (por ejemplo, de `10K (5%)` a `1.4K (1%)`).
    2. Pulsa ++ctrl+x++ y luego ++l++ para abrir `Sessions`:
       - Selecciona la sesión y pulsa ++ctrl+r++ para renombrarla a `Practica HTML y Contexto`.
       - Abre una nueva sesión (++ctrl+x++ ++n++), envía `ping`, vuelve a ++ctrl+x++ ++l++, selecciona la sesión de prueba y pulsa ++ctrl+d++ para borrarla.
    3. Sal con ++ctrl+c++ y retoma la última sesión desde consola con:
       ```bash
       opencode -c
       ```

---

## Nivel 3 · `/undo` y `/redo` con Git, `/init`, `AGENTS.md` y `.ignore`

### Ejercicio 5: El experimento de `/undo` sin Git vs con Git

**Enunciado:**

1. En una carpeta **sin Git**, pídele a OpenCode que cambie el fondo de `@index.html` a azul pastel y luego a rosa pastel. Ejecuta `/undo` (++ctrl+x++ ++u++). ¿Qué ocurre con el chat y qué ocurre con `index.html`?
2. Ahora arregla el repositorio para que `/undo` y `/redo` **sí reviertan archivos en disco**. Enumera todos los pasos obligatorios (sin olvidar el paso tras el *commit*).
3. Haz 3 cambios consecutivos de color (`rosa pastel` &rarr; `morado pastel` &rarr; `verde pastel`), retrocede 2 pasos con ++ctrl+x++ ++u++ y avanza 1 paso con ++ctrl+x++ ++r++.
4. ¿Por qué no debes usar `/undo` como sustituto de Git en proyectos reales?

??? tip "Ver pista"
    OpenCode solo crea instantáneas de archivos si detecta un repositorio Git inicializado con al menos un *commit*, y debes reiniciar `opencode` tras inicializarlo.

??? success "Ver solución"
    1. **Sin Git:** `/undo` muestra `1 message reverted` y deshace el mensaje en el chat, **pero `index.html` en disco NO cambia**.
    2. **Pasos obligatorios para activar reversión real:**
       ```bash
       # 1. Salir de OpenCode (Ctrl+C)
       # 2. Inicializar Git y crear commit inicial
       git init
       git add .
       git commit -m "first commit"
       # 3. Reiniciar OpenCode (obligatorio para que detecte Git)
       opencode
       ```
    3. Tras pedir los 3 colores, cada ++ctrl+x++ ++u++ mostrará algo como `index.html +1 -1` y revertirá tanto el mensaje como el color real en `index.html`. Con ++ctrl+x++ ++r++ volverá a aplicar el cambio (si ya no hay más pasos por rehacer, aparecerá la letra `r` en el prompt).
    4. **Por qué no sustituye a Git:** `/undo` depende de la sesión activa de OpenCode. Si borras o purgas la sesión, pierdes el historial de reversión. En proyectos reales siempre debes usar *commits* y ramas de Git.

---

### Ejercicio 6: `README.md`, `/init` y mantenimiento de `AGENTS.md`

**Enunciado:**

1. En una sesión limpia (++ctrl+x++ ++n++), crea un archivo `README.md` en la raíz del proyecto que mencione `HTML`, `CSS` y `Tailwind CSS` como tecnologías del proyecto (aunque todavía **no** hayas instalado Tailwind en `index.html`).
2. Ejecuta `/init` dentro de OpenCode y revisa el `AGENTS.md` generado. ¿Qué debería advertir sobre Tailwind CSS?
3. Supón que tu equipo tiene una guía de estilos de 400 líneas llamada `style-guide.md`. ¿Deberías copiar esas 400 líneas dentro de `AGENTS.md`? Explica cuál es la buena práctica y qué hacer con `AGENTS.md` en Git.

??? tip "Ver pista"
    Recuerda que `AGENTS.md` se carga al inicio de las conversaciones y consume contexto. Puedes usar `@archivo.md` dentro de `AGENTS.md`.

??? success "Ver solución"
    1. Crear `README.md` describiendo el proyecto y listando `HTML`, `CSS` y `Tailwind CSS`.
    2. Al ejecutar `/init`, OpenCode inspecciona `README.md`, `index.html`, `git status` y `ls -la`, y genera `AGENTS.md`. En la sección de `Stack` aclarará que **Tailwind CSS aparece en el `README.md` pero todavía no está instalado ni configurado**, evitando que futuros agentes asuman un CDN o *build step* inexistente.
    3. **NO copies las 400 líneas en `AGENTS.md`**, porque inflarías el contexto de todas las sesiones desde el primer turno. La buena práctica es mantener `AGENTS.md` conciso y **referenciar el archivo externo como `@style-guide.md`** para que el agente solo lo lea cuando necesite consultarlo. Además, debes **hacer commit de `AGENTS.md` en Git** y actualizarlo cuando cambie el stack.

---

### Ejercicio 7: Blindar archivos con `.gitignore` vs `.ignore`

**Enunciado:**
En tu proyecto `01-demo-ejercicios` necesitas cumplir dos requisitos distintos:

- **Requisito A:** Ni Git ni OpenCode deben rastrear ni sugerir `node_modules/` ni `.env`.
- **Requisito B:** Tienes una carpeta `auth/auth.js` con código sensible que **SÍ debe guardarse en Git**, pero **NO quieres que OpenCode la encuentre** en búsquedas automáticas con `@` o `grep`.

1. Crea los archivos de configuración necesarios para cumplir ambos requisitos.
2. Verifica con `@auth` en OpenCode y en `git status` que ambos requisitos funcionan.
3. *Bonus:* ¿Qué sintaxis usarías dentro de `.ignore` si en otro proyecto necesitaras que OpenCode **sí** pueda buscar dentro de `dist/` aunque esté en `.gitignore`?

??? tip "Ver pista"
    Diferencia entre `.gitignore` (ignora Git + OpenCode) y `.ignore` (ignora solo búsquedas de OpenCode/`ripgrep`, Git sigue rastreando). Para excepciones se usa `!`.

??? success "Ver solución"
    1. **Para el Requisito A (`.gitignore`):**
       ```gitignore
       node_modules
       .env
       ```
    2. **Para el Requisito B (`.ignore` en la raíz del proyecto):**
       ```gitignore
       auth/*
       ```
    3. **Verificación:**
       - Al escribir `@auth` en OpenCode, `auth/auth.js` **no aparecerá** en el autocompletado.
       - Al ejecutar `git status`, `auth/auth.js` **sí aparecerá** como archivo rastreable para Git.
    4. **Bonus (incluir una carpeta ignorada por `.gitignore`):**
       Añadir con signo de exclamación en `.ignore`:
       ```gitignore
       !dist/
       ```

---

## Nivel 4 · `Plan Mode`, `Build Mode` y Auditoría de Modelos

### Ejercicio 8: Flujo profesional `Plan` &rarr; `Build` con modelos diferenciados

**Enunciado:**

1. Pulsa ++tab++ para entrar en **`Plan`** y selecciona con `/models` tu modelo más capaz/razonador.
2. Pide en `Plan`: *"Necesito que instalemos Tailwind CSS en este proyecto"*.
3. Responde a la pregunta interactiva eligiendo **Tailwind Play CDN**.
4. **Prueba de seguridad:** Todavía en `Plan`, dile *"Necesito que lo implementes ahora mismo"*. Comprueba qué responde.
5. Cambia a **`Build`** con ++tab++ (puedes asignar un modelo más ligero/gratuito para `Build`) y dile *"Ok, impleméntalo"*. Verifica que actualiza `index.html` y `AGENTS.md`.

??? tip "Ver pista"
    Recuerda que OpenCode guarda un modelo distinto para `Plan` y para `Build`, y que `Plan` es estrictamente de solo lectura.

??? success "Ver solución"
    1. ++tab++ &rarr; `Plan` &rarr; `/models` &rarr; elegir modelo razonador (ej. `GLM-5.2`).
    2. Al enviar el prompt, `Plan` lee `index.html`, `README.md` y `AGENTS.md` sin editar nada y pregunta cómo instalar Tailwind (`Play CDN`, `CLI via npm`, `CLI binario`).
    3. Al elegir **Tailwind Play CDN** y pedirle que lo implemente estando aún en `Plan`, **se negará a editar archivos** (`Plan Mode` es solo lectura) y entregará únicamente el plan final con *trade-offs* (Play CDN es solo para aprendizaje, no producción) y pasos de verificación.
    4. Pulsa ++tab++ para pasar a **`Build`**, envía `Ok, impleméntalo` y el agente creará una lista `# Todos`, añadirá `<script src="https://cdn.tailwindcss.com"></script>`, migrará los estilos inline a clases de Tailwind y actualizará `AGENTS.md`.

---

### Ejercicio 9 (Reto integrador): Comparativa de modelos y auditoría con Git

**Enunciado:**

1. En `Build`, haz un pequeño ajuste visual (por ejemplo: *"Añade una tarjeta centrada con mi nombre y un botón usando Tailwind"*).
2. Antes de hacer *commit*, abre **Source Control** en tu editor o ejecuta `git diff` en la terminal y revisa línea por línea:
    - ¿El modelo modificó algo que **no** le pediste (por ejemplo, cambiar el estilo del `<h1>` existente o añadir configuraciones extra)?
3. Si estás conforme con el código tras revisarlo y probarlo en el navegador, haz *commit* en Git.
4. Abre una **sesión nueva** (++ctrl+x++ ++n++) y pregúntale a OpenCode:
    - *"¿Quién hizo el último cambio?"*
    - *"¿Qué hizo esa persona?"*
   Observa qué comandos de Git ejecuta OpenCode automáticamente para responderte.

??? success "Ver solución"
    1. Al revisar `git diff` (o el visor de diferencias de VS Code / Cursor), podrás detectar cualquier "libertad" que el modelo haya tomado por su cuenta (como estilizar el `<h1>` o crear un `tailwind.config` no solicitado). Si algo no te convence, lo corriges antes del *commit*.
    2. Tras aprobarlo y hacer `git add . && git commit -m "Add profile card with Tailwind"`, **tú asumes la responsabilidad de ese código**.
    3. En una sesión nueva (++ctrl+x++ ++n++), al preguntar quién hizo el último cambio y qué modificó, OpenCode ejecutará comandos de Git similares a:
       ```bash
       git log -1 --format='%an <%ae> (%ar)'
       git show --stat HEAD
       ```
       y te devolverá el autor, la fecha, los archivos tocados y las líneas añadidas/eliminadas.
