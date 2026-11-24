---
title: "2. Proveedores y modelos de IA"
description: Cómo conectar OpenCode Zen (gratis), OpenAI (ChatGPT Plus/Pro), OpenCode Go y Ollama (modelos locales) paso a paso desde nivel cero.
---

# 2. Proveedores y modelos de IA

Una de las mayores ventajas de **OpenCode** es que **no estás atado a un único proveedor ni a un único modelo de Inteligencia Artificial**. Puedes conectar servicios gratuitos, tus suscripciones actuales o modelos que corren 100% en tu propio ordenador.

!!! note "¿Qué diferencia hay entre Proveedor, Modelo y API Key?"
    - **Proveedor (*Provider*)**: la empresa o servicio que aloja o sirve los modelos (por ejemplo, *OpenCode Zen*, *OpenAI*, *OpenCode Go*, *Ollama*).
    - **Modelo (*Model*)**: la IA concreta que procesa tu petición (por ejemplo, *Big Pickle*, *GPT-5.4*, *GLM-5.2*, *Qwen 3.5*).
    - **API Key**: una clave secreta personal que identifica tu cuenta ante un proveedor para que OpenCode pueda enviarle peticiones.

---

## Comparativa rápida de proveedores

| Proveedor | Costo | Internet requerido | Ideal para... |
| :--- | :--- | :--- | :--- |
| **OpenCode Zen** | Gratis (con modelos *Free*) / Pago por uso | Sí | Empezar el curso sin gastar dinero. |
| **OpenAI (ChatGPT)** | Incluido en tu plan **Plus / Pro** | Sí | Quienes ya pagan ChatGPT Plus/Pro. |
| **OpenCode Go** | $5 primer mes, luego $10/mes | Sí | Tener acceso económico a modelos *open-source* potentes (*GLM-5.2*, *DeepSeek*, *Qwen*, *Kimi*). |
| **Ollama** | 100% Gratis | Solo al descargar el modelo | Privacidad total o trabajar sin internet (requiere buena RAM y GPU). |

!!! danger "Advertencia crítica sobre Anthropic (Claude)"
    Si tienes una suscripción personal de **Claude (Anthropic)**, **NO la conectes a OpenCode**:

    1. Anthropic está bloqueando/baneando cuentas que conectan suscripciones personales a clientes externos.
    2. Además, el uso por fuera suele facturarse como *extra usage* a tarifas más altas.

    En el curso se omite intencionadamente la conexión con suscripciones de Anthropic para proteger tu cuenta.

---

## Cómo conectar cualquier proveedor (TUI vs CLI)

Antes de seguir los pasos de cada proveedor, ten en cuenta que existen **dos formas equivalentes** de abrir el asistente de conexión. Si `/connect` no te deja lanzarlo en la TUI (algo frecuente en algunas versiones o terminales de Windows, o en la extensión de VS Code), usa el comando de terminal CLI o la paleta de comandos:

=== "Desde la terminal (CLI — Alternativa infalible)"

    Sal de OpenCode (++ctrl+c++) y ejecuta en tu terminal normal (PowerShell, CMD o Bash):

    ```bash
    opencode auth login
    ```

    *(También funciona su alias `opencode providers login`.)*

    Para comprobar qué proveedores tienes conectados en cualquier momento:

    ```bash
    opencode auth list
    ```

=== "Dentro de la TUI (con `/connect` o atajos)"

    1. Escribe `/connect`, selecciónalo en el menú desplegable y pulsa ++enter++.
    2. **Si `/connect` no se ejecuta al pulsar Enter:**
       - Pulsa ++ctrl+p++ para abrir la paleta de comandos, escribe `Connect provider` y pulsa ++enter++.
       - O abre `/models` y pulsa ++ctrl+a++ (`Connect provider`).

---

## Opción 1: OpenCode Zen (Recomendado para empezar gratis)

**OpenCode Zen** es el proveedor oficial curado por el equipo de OpenCode. Incluye modelos gratuitos (marcados como `Free`) y modelos de pago.

### Pasos para conectarlo

1. Abre el asistente de conexión con `opencode auth login` (en tu terminal) o `/connect` / ++ctrl+p++ → `Connect provider` (dentro de la TUI).
2. Selecciona **`OpenCode Zen (Recommended)`**.
3. OpenCode te pedirá una **API Key** y te mostrará el enlace `https://opencode.ai/zen`. Abre ese enlace en tu navegador (puedes hacer ++ctrl+click++ o ++cmd+click++).
4. Haz clic en **Get started with Zen** e inicia sesión.
5. En el panel izquierdo, entra en **API Keys** → **Create API Key**, dale un nombre descriptivo (por ejemplo, `curso-opencode`) y pulsa **Create**.
6. Copia la clave generada, vuelve a la terminal, pégala y pulsa ++enter++.

### Cómo elegir o cambiar de modelo

1. Escribe `/models` en OpenCode y pulsa ++enter++.
2. Busca los modelos etiquetados como **`Free`** dentro de *OpenCode Zen* (en el curso se usa **`Big Pickle`** para las pruebas iniciales; ten en cuenta que la lista de modelos gratuitos puede cambiar con el tiempo).

??? info "¿Qué significa la etiqueta `Stealth` en algunos modelos (como Big Pickle)?"
    Significa que el proveedor no detalla públicamente sobre qué arquitectura base fue entrenado ese modelo. Además, no todos los modelos son **multimodales** (capaces de ver imágenes).

    **Ejemplo práctico probado en clase:**
    Si pegas una captura de pantalla en OpenCode (`Image 1`) y preguntas:
    ```text
    ¿Qué es lo que logras ver en esa imagen?
    ```
    *Big Pickle* responderá: *"No puedo procesar imágenes - este modelo no soporta entrada de imágenes."* Para analizar capturas o mockups necesitarás un modelo multimodal.

---

## Opción 2: OpenAI (ChatGPT Plus / Pro)

Si ya pagas una suscripción **ChatGPT Plus** o **Pro**, puedes aprovecharla dentro de OpenCode sin pagar tokens extra aparte.

!!! warning "Usa tu suscripción, evita la API Key de OpenAI"
    En el menú de OpenAI verás la opción de introducir una *API Key* manual. **Evítala si ya tienes suscripción**, porque el consumo por API Key se cobra por separado según los tokens usados y puede resultar más costoso.

### Pasos para conectarlo

1. Ejecuta `opencode auth login` en tu terminal (o `/connect` / ++ctrl+p++ → `Connect provider` dentro de OpenCode).
2. Escribe `openai` y selecciona **`OpenAI (ChatGPT Plus/Pro or API key)`**.
3. En *Select auth method*, elige **`ChatGPT Pro/Plus (browser)`**.
4. OpenCode mostrará una URL larga (`https://auth.openai.com/oauth/authorize?...`). Haz clic en ella (o cópiala con `c` y ábrela en tu navegador).
5. Inicia sesión con tu cuenta de ChatGPT y autoriza la conexión.
6. Vuelve a la terminal: la autenticación se completará automáticamente.
7. Verifica con `/models` que ya aparecen los modelos de OpenAI.

---

## Opción 3: OpenCode Go (Suscripción económica Open Source)

**OpenCode Go** es una suscripción de bajo costo (**$5 el primer mes**, luego **$10/mes**) que da acceso con límites generosos a modelos *open-source* de alto rendimiento para programación (como **GLM-5.2**, **GLM-5.1**, **Kimi K2.7 Code**, **DeepSeek V4 Flash/Pro**, **Qwen3.7 Plus/Max**, **MiniMax M2.7/M3**). Es el proveedor que el instructor utiliza en gran parte del curso.

!!! tip "Crédito adicional de $5 (Opcional)"
    En los recursos del módulo tienes un enlace de referido (`https://opencode.ai/go?ref=NP0DHYJCFX`) que otorga **$5 extra** de crédito de uso al suscribirte.

### Pasos para conectarlo

1. Entra en `https://opencode.ai/go` y suscríbete en **Subscribe to Go**.
2. Ve a **API Keys** en el panel de OpenCode, crea una API Key (o reutiliza tu clave si ya tienes una) y cópiala.
3. Ejecuta `opencode auth login` en tu terminal (o `/connect` / ++ctrl+p++ → `Connect provider` en OpenCode) → selecciona **`OpenCode Go`**.
4. Pega tu API Key y pulsa ++enter++.
5. Ejecuta `/models` y selecciona el modelo de OpenCode Go que prefieras (por ejemplo, **`GLM-5.2`**).

---

## Opción 4: Ollama (Modelos locales en tu propio ordenador)

**Ollama** te permite descargar y ejecutar modelos de IA directamente en tu computadora, **sin costo** y **sin enviar código a la nube**. El rendimiento dependerá de tu **tarjeta gráfica (GPU)** y tu **memoria RAM**.

### Paso 1: Instalar Ollama

Descarga la aplicación visual desde [ollama.com](https://ollama.com/) para macOS, Windows o Linux, o instálala por terminal en macOS/Linux:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

Si abres la aplicación de Ollama, verás dos tipos de modelos en su selector:

- Con **icono de nube (☁️)**: se ejecutan en los servidores en la nube de Ollama (`:cloud`).
- Con **flecha hacia abajo (↓)**: se descargan y ejecutan **localmente** en tu máquina.

### Paso 2: Probar el modelo antes de conectarlo a OpenCode

Antes de integrarlo en OpenCode, prueba el modelo directamente en tu terminal (en el ejemplo del curso se usa `qwen3.5`):

```bash
ollama run qwen3.5
```

*(Si no lo tienes descargado, Ollama lo descargará automáticamente la primera vez).*

Cuando aparezca el prompt `>>>`, haz dos pruebas:

1. Escribe `ping` para medir cómo responde tu hardware.
2. Haz una pregunta real de programación en español:
   ```text
   ¿Cuál es la forma para trabajar peticiones asíncronas en JavaScript?
   ```
   Verás primero un bloque `Thinking...` (el razonamiento interno del modelo) y después la respuesta estructurada en español.
3. Para salir de Ollama, pulsa ++ctrl+d++ (o escribe `/bye`).

!!! warning "Si el comando `ollama` no se reconoce"
    Si ves un error de comando no encontrado tras instalar la app visual, cierra y vuelve a abrir tu terminal, o asegúrate de haber instalado la CLI de Ollama.

### Paso 3: Conectar Ollama con OpenCode en un solo comando

En la página del modelo en `ollama.com` (o directamente desde tu terminal dentro de tu carpeta de trabajo `01-demo`), ejecuta:

```bash
ollama launch opencode --model qwen3.5
```

1. Ollama te preguntará si deseas modificar tu archivo de configuración `~/.local/state/opencode/model.json` (guardando una copia de seguridad automática). Selecciona **`Yes`** y pulsa ++enter++.
2. Se abrirá OpenCode automáticamente usando **`qwen3.5 Ollama`**.
3. Haz la prueba con `ping`:
   - **La primera petición tarda más** (en el vídeo ~55 segundos) porque el modelo se está cargando en memoria/GPU ("calentando motores").
   - **A partir de la segunda petición** (`hola mundo`, `ping`), la respuesta baja a ~1.3–1.4 segundos.
4. Prueba a limpiar el contexto (++ctrl+x++ luego ++n++) y enviar otra pregunta:
   ```text
   hola mundo? ¿Cómo se dice "hola mundo" en diferentes idiomas?
   ```

### Paso 4: Volver a OpenCode Zen cuando quieras

Si no quieres consumir recursos de tu PC durante las siguientes prácticas, vuelve a cambiar de modelo en cualquier momento con `/models` y selecciona de nuevo **`Big Pickle` (OpenCode Zen — Free)**, seguido de ++ctrl+x++ y ++n++ para iniciar una sesión limpia.
