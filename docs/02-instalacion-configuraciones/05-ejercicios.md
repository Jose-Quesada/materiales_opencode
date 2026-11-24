---
title: "5. Batería de ejercicios (Módulo 02)"
description: Ejercicios prácticos de dificultad incremental con pistas y soluciones ocultas sobre instalación, proveedores, memoria global y personalización del TUI.
---

# 5. Batería de ejercicios — Instalación y configuraciones

Pon a prueba todo lo aprendido en el **Módulo 02** (junto con los conceptos básicos de IA del Módulo 01). Los ejercicios están ordenados de **menor a mayor dificultad**.

!!! tip "Cómo aprovechar esta batería en clase"
    1. Intenta resolver cada ejercicio en tu propia terminal **antes** de abrir la solución.
    2. Si te bloqueas, despliega primero la **pista** (`Ver pista`).
    3. Al terminar, compara tu resultado con la **solución oculta** (`Ver solución`).

---

## Nivel 1 · Fundamentos y verificación del entorno

### Ejercicio 1: Instalación y carpeta segura

**Enunciado:**

1. Comprueba desde tu terminal qué versión de OpenCode tienes instalada y cómo ver su lista de comandos disponibles.
2. Un compañero quiere probar OpenCode rápido y ejecuta `opencode` directamente dentro de su carpeta de usuario (`C:\Users\Alumno` o `~`). Explica **por qué es peligroso** y qué comandos debes ejecutar para crear un entorno seguro llamado `practica-m2` dentro de `Desktop/OpenCode`.

??? tip "Ver pista"
    Recuerda los flags `--version` y `--help`, y piensa a qué archivos tiene acceso un agente de consola según el directorio desde el que lo lances.

??? success "Ver solución"
    1. **Comprobar versión y ayuda:**
       ```bash
       opencode --version
       opencode --help
       ```
    2. **Por qué es peligroso ejecutarlo en `~` o `C:\Users\Alumno`:**
       OpenCode es un **agente** con acceso a los archivos del directorio de trabajo actual y sus subcarpetas. Si lo lanzas en la raíz de tu usuario, podrá leer o modificar documentos personales, escritorio, descargas o configuraciones del sistema.
    3. **Crear y entrar en una carpeta segura:**
       - **Windows (PowerShell):**
         ```powershell
         mkdir "$HOME\Desktop\OpenCode\practica-m2" -Force
         cd "$HOME\Desktop\OpenCode\practica-m2"
         opencode
         ```
       - **macOS / Linux:**
         ```bash
         mkdir -p ~/Desktop/OpenCode/practica-m2
         cd ~/Desktop/OpenCode/practica-m2
         opencode
         ```

---

### Ejercicio 2: Primer `ping`, tokens y limpieza de sesión

**Enunciado:**

1. Abre OpenCode dentro de `practica-m2` y envía el mensaje `ping`.
2. Observa la barra de estado: aunque solo escribiste una palabra de 4 letras, el contador de contexto ya muestra alrededor de `9K–11K` tokens (aprox. 5 %). **Explica con tus palabras por qué ocurre esto** usando los conceptos de *token*, *System Prompt* y *ventana de contexto*.
3. Sin cerrar OpenCode, inicia una **sesión completamente limpia** usando el atajo de teclado con tecla líder.

??? tip "Ver pista"
    Recuerda que los LLMs son *stateless*: en cada turno OpenCode envía algo más que tu mensaje. Para la sesión nueva, recuerda la tecla líder ++ctrl+x++.

??? success "Ver solución"
    1. Al enviar `ping`, el modelo responde `pong`.
    2. **¿Por qué consume ~9K–11K tokens desde el primer mensaje?**
       Porque la **ventana de contexto** incluye todo lo que OpenCode envía automáticamente antes de tu mensaje: el **System Prompt** (reglas base del agente), las definiciones de **herramientas (*Tools*)**, las instrucciones globales/locales (`instructions.md`, `AGENTS.md`) y configuraciones activas (MCPs, Skills). Tu `ping` apenas ocupa 1 token, pero el contexto base se carga entero en cada turno.
    3. **Iniciar sesión limpia con atajo líder:**
       Pulsa ++ctrl+x++, **suelta ambas teclas** y pulsa ++n++ (equivale a escribir `/new`).

---

## Nivel 2 · Proveedores, modelos y criterios de elección

### Ejercicio 3: Conectar un proveedor y resolver problemas con `/connect`

**Enunciado:**

1. Conecta el proveedor **OpenCode Zen** y selecciona un modelo gratuito (`Free`) mediante `/models`.
2. Imagina que estás en clase con Windows y al escribir `/connect` dentro de la TUI no te deja lanzar el comando. Indica **dos formas alternativas** de abrir el asistente de conexión de proveedores y el comando para listar tus proveedores conectados desde la terminal.

??? tip "Ver pista"
    Hay un comando de CLI fuera de la TUI (`opencode auth ...`) y también dos atajos dentro de la TUI (++ctrl+p++ y ++ctrl+a++).

??? success "Ver solución"
    1. **Conexión desde la terminal (CLI — alternativa infalible):**
       Sal de OpenCode (++ctrl+c++) y ejecuta:
       ```bash
       opencode auth login
       ```
       *(También sirve su alias `opencode providers login`.)* Selecciona **`OpenCode Zen (Recommended)`** y pega tu API Key creada en `https://opencode.ai/zen`.
    2. **Alternativas dentro de la TUI (sin escribir `/connect`):**
       - Pulsa ++ctrl+p++, escribe `Connect provider` y pulsa ++enter++.
       - O abre `/models` y pulsa ++ctrl+a++ (`Connect provider`).
    3. **Comprobar proveedores conectados desde terminal:**
       ```bash
       opencode auth list
       ```
    4. Por último, entra en OpenCode, ejecuta `/models` y elige un modelo etiquetado como **`Free`** (por ejemplo, `Big Pickle` o `MiMo V2.5 Free`).

---

### Ejercicio 4: Casos reales de elección de proveedor y modelo

**Enunciado:**
Para cada uno de los siguientes escenarios, indica **qué decisión tomarías y por qué**:

1. **Caso A:** Un alumno ya paga su suscripción personal de **Claude Pro (Anthropic)** y quiere conectarla a OpenCode para ahorrar dinero.
2. **Caso B:** Una desarrolladora tiene **ChatGPT Plus (OpenAI)** y duda entre introducir una *API Key* de OpenAI o usar el inicio de sesión *ChatGPT Pro/Plus (browser)*.
3. **Caso C:** Un equipo trabaja en un proyecto confidencial sin acceso a internet y dispone de un portátil con buena GPU y RAM.
4. **Caso D:** Alguien intenta pegar una captura de pantalla de un error visual usando el modelo gratuito `Big Pickle` (`Stealth`) y el modelo responde que no puede ver imágenes. ¿Qué ocurrió y qué necesita cambiar?

??? tip "Ver pista"
    Repasa las advertencias de Anthropic y OpenAI, el uso de Ollama local y el concepto de *multimodalidad*.

??? success "Ver solución"
    - **Caso A (Anthropic personal):** **NO conectarla.** Anthropic está bloqueando cuentas personales conectadas a clientes externos y puede facturar el uso como *extra usage* a tarifas más altas.
    - **Caso B (OpenAI Plus/Pro):** Elegir **`ChatGPT Pro/Plus (browser)`** (autenticación OAuth en navegador). Usar una *API Key* manual de OpenAI cobra el consumo por token aparte de tu suscripción mensual.
    - **Caso C (Sin internet y código privado):** Usar **Ollama** con un modelo descargado **localmente** (icono de flecha hacia abajo ↓, por ejemplo `ollama run qwen3.5`), ya que corre 100 % en su máquina sin enviar datos a la nube.
    - **Caso D (`Big Pickle` e imágenes):** No todos los modelos son **multimodales** (capaces de procesar imágenes además de texto), y la etiqueta `Stealth` indica que el proveedor no detalla su arquitectura interna. Debe cambiar con `/models` a un modelo que sí soporte entrada de imágenes.

---

## Nivel 3 · Memoria global, prioridades y depuración

### Ejercicio 5: Configurar `opencode.jsonc` e `instructions.md` globales

**Enunciado:**

1. Localiza y abre tu carpeta de configuración global de OpenCode según tu sistema operativo.
2. Crea el archivo `opencode.jsonc` con el esquema oficial (`$schema`) enlazando un archivo llamado `instructions.md`.
3. Crea `instructions.md` con al menos 3 reglas claras (por ejemplo: respuestas concisas, código en inglés y comentarios en español).
4. Reinicia OpenCode retomando la última sesión desde la terminal y pídele: *"Crea una función para validar un email"* para verificar que cumple tus reglas.

??? tip "Ver pista"
    En Windows la ruta es `%USERPROFILE%\.config\opencode` y en macOS/Linux `~/.config/opencode/`. Para retomar la última sesión usa el flag `-c`.

??? success "Ver solución"
    1. **Carpeta global:**
       - Windows: `%USERPROFILE%\.config\opencode\`
       - macOS / Linux: `~/.config/opencode/`
    2. **Contenido de `opencode.jsonc`:**
       ```jsonc
       {
         "$schema": "https://opencode.ai/config.json",
         "instructions": [
           "instructions.md"
         ]
       }
       ```
    3. **Contenido de `instructions.md`:**
       ```markdown
       Eres un agente de codificación experto en clean code.

       - Responde de manera corta y concisa.
       - Código en inglés (nombres de variables, funciones y clases).
       - Comentarios en español, solo cuando aporten valor.
       ```
    4. **Reiniciar y comprobar:**
       Sal con ++ctrl+c++ y vuelve a entrar con:
       ```bash
       opencode -c
       ```
       Al pedir la función de validación de email, verás nombres en inglés (ej. `isValidEmail`), comentarios breves en español (si aplica) y una respuesta directa.

---

### Ejercicio 6: Depuración de configuración y regla de prioridad

**Enunciado:**
Un alumno modificó su `tui.json` y añadió un `AGENTS.md` en su proyecto, pero ahora OpenCode falla al arrancar y además no entiende qué regla se aplicará. Analiza estos dos problemas:

1. **Encuentra el error de sintaxis** en este `tui.json` y corrígelo:
   ```json
   {
     "$schema": "https://opencode.ai/tui.json",
     "theme": "tokyonight"
     "attention": {
       "enabled": true,
       "notifications": true,
       "sound": true,
       "volume": 0.4,
       "sound_pack": "opencode.default",
       "sounds": { "error": "./sounds/error.mp3" }
     }
   }
   ```
2. **Conflicto de reglas:** Su `instructions.md` global dice *"Responde siempre en español"*, pero el archivo `AGENTS.md` de un proyecto concreto dice *"Respond always in English"*. ¿En qué idioma responderá OpenCode dentro de ese proyecto y por qué?

??? tip "Ver pista"
    Fíjate en el final de la línea `"theme"` y en la propiedad `"sounds"` que apunta a un `.mp3` que no existe. Para el conflicto, recuerda la jerarquía de configuración.

??? success "Ver solución"
    1. **Dos problemas en `tui.json`:**
       - Falta una **coma (`,`)** al final de `"theme": "tokyonight"`.
       - La propiedad `"sounds": { "error": "./sounds/error.mp3" }` provocará error si ese archivo `.mp3` no existe físicamente; debe eliminarse para que use `"sound_pack": "opencode.default"`.
       - **Archivo corregido:**
         ```json
         {
           "$schema": "https://opencode.ai/tui.json",
           "theme": "tokyonight",
           "attention": {
             "enabled": true,
             "notifications": true,
             "sound": true,
             "volume": 0.4,
             "sound_pack": "opencode.default"
           }
         }
         ```
    2. **Idioma resultante:** Responderá **en inglés**, porque la regla de prioridad de OpenCode es **Proyecto > Global**: las configuraciones se fusionan, pero lo definido a nivel de proyecto (`AGENTS.md` / `opencode.jsonc` local) sobrescribe lo global en caso de conflicto.

---

## Nivel 4 · Personalización del TUI y Reto Integrador

### Ejercicio 7: Notificaciones y sonidos en `tui.json`

**Enunciado:**

1. Genera automáticamente `~/.config/opencode/tui.json` sin crear el archivo a mano.
2. Configúralo para activar notificaciones de escritorio y sonidos sutiles a volumen `0.4` con el paquete por defecto.
3. Recarga OpenCode con `opencode -c`, envía `ping` y verifica que suena el aviso al terminar.

??? tip "Ver pista"
    Cambiar el tema una vez con `/themes` crea automáticamente el archivo `tui.json` en la ruta correcta.

??? success "Ver solución"
    1. Dentro de OpenCode, ejecuta `/themes`, selecciona un tema (por ejemplo `tokyonight`) y pulsa ++enter++. Esto crea `~/.config/opencode/tui.json`.
    2. Edítalo añadiendo el bloque `"attention"`:
       ```json
       {
         "$schema": "https://opencode.ai/tui.json",
         "theme": "tokyonight",
         "attention": {
           "enabled": true,
           "notifications": true,
           "sound": true,
           "volume": 0.4,
           "sound_pack": "opencode.default"
         }
       }
       ```
    3. Guarda el archivo, sal con ++ctrl+c++, ejecuta `opencode -c`, escribe `ping` y comprueba el sonido sutil al recibir `pong`.

---

### Ejercicio 8 (Reto integrador): Puesta a punto completa y prueba de atajos

**Enunciado:**
Sin consultar los apuntes, realiza esta secuencia completa de principio a fin y comprueba cada paso:

1. Crea una carpeta limpia `Desktop/OpenCode/reto-m2` y entra en ella.
2. Verifica desde terminal qué proveedores tienes conectados.
3. Lanza OpenCode, oculta y vuelve a mostrar la barra lateral derecha con atajo de teclado.
4. Abre la paleta de comandos rápida y cambia el modelo activo.
5. Pregunta a OpenCode: *"¿En qué rutas guardas tu configuración global y de proyecto?"* y verifica que responde de forma concisa siguiendo tu `instructions.md`.
6. Limpia la sesión con atajo de teclado, sal de OpenCode y vuelve a entrar retomando esa última sesión.

??? success "Ver solución"
    1. `mkdir -p ~/Desktop/OpenCode/reto-m2 && cd ~/Desktop/OpenCode/reto-m2` (o `mkdir "$HOME\Desktop\OpenCode\reto-m2" -Force; cd "$HOME\Desktop\OpenCode\reto-m2"` en PowerShell).
    2. `opencode auth list`
    3. `opencode` &rarr; pulsa ++ctrl+x++ y luego ++b++ dos veces (oculta y vuelve a mostrar la barra lateral).
    4. Pulsa ++ctrl+p++, escribe `Switch model` (o usa `/models`) y elige tu modelo.
    5. Envía la pregunta y confirma que te menciona `~/.config/opencode/` (global), `AGENTS.md` / `opencode.jsonc` (proyecto) y `~/.local/share/opencode/` (datos/SQLite), respetando el estilo conciso de tu `instructions.md`.
    6. Pulsa ++ctrl+x++ y luego ++n++ (nueva sesión) &rarr; ++ctrl+c++ (salir) &rarr; `opencode -c` (retomar última sesión).
