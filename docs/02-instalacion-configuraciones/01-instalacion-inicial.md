---
title: "1. Instalación inicial y primeros pasos"
description: Cómo instalar OpenCode desde cero, verificar la instalación, preparar una carpeta de trabajo segura y realizar tu primera prueba en el TUI.
---

# 1. Instalación inicial y primeros pasos

En esta guía vas a instalar **OpenCode** en tu equipo y a abrirlo por primera vez sin riesgo de tocar archivos importantes de tu ordenador.

---

## Paso 1: ¿Por qué usar OpenCode en la terminal?

Aunque OpenCode ofrece una aplicación de escritorio (*OpenCode Desktop*, en beta) y extensiones para editores como **VS Code, Cursor, Zed, Windsurf o VSCodium**, en el curso se recomienda encarecidamente usar **la versión de terminal**:

- **Independencia total**: funciona igual sin importar si programas en VS Code, Cursor, Xcode, Android Studio o cualquier otro editor.
- **Actualizaciones más rápidas**: el instalador oficial (`curl`) descarga las novedades directamente del equipo de OpenCode, mientras que gestores de terceros (como Homebrew) o extensiones suelen tardar más en actualizarse.
- **Menos limitaciones**: la interfaz de terminal (TUI) tiene acceso completo a todas las funciones y atajos del agente.

---

## Paso 2: Instalar OpenCode

Abre tu terminal (no hace falta abrirla como administrador) y ejecuta el comando correspondiente a tu sistema operativo:

=== "macOS / Linux / Git Bash (Recomendado)"

    ```bash
    curl -fsSL https://opencode.ai/install | bash
    ```

    !!! tip "Ventaja del comando `curl`"
        Este mismo comando sirve tanto para **instalar** OpenCode por primera vez como para **actualizarlo** a la última versión en el futuro.

=== "Windows (WSL / Chocolatey / Scoop)"

    En Windows, la documentación oficial recomienda usar **WSL** (*Windows Subsystem for Linux*) con el comando `curl` de la pestaña anterior. Si prefieres instalarlo de forma nativa en PowerShell:

    ```powershell
    # Con Chocolatey
    choco install opencode

    # O con Scoop
    scoop install opencode
    ```

=== "Node.js / Otros gestores"

    Si ya trabajas con JavaScript o prefieres otro gestor de paquetes:

    ```bash
    # npm
    npm install -g opencode-ai

    # bun
    bun add -g opencode-ai

    # Homebrew (macOS / Linux)
    brew install anomalyco/tap/opencode

    # Arch Linux (paru)
    paru -S opencode
    ```

---

## Paso 3: Verificar que se instaló correctamente

En la misma terminal, comprueba la versión instalada y los comandos disponibles:

```bash
opencode --version
```

Deberías ver un número de versión en pantalla (por ejemplo, `1.17.8` o superior). También puedes consultar la ayuda general:

```bash
opencode --help
```

---

## Paso 4: Crear una carpeta segura de pruebas

!!! danger "Regla de oro de seguridad"
    **Nunca** ejecutes `opencode` directamente en la carpeta raíz de tu usuario (`~` en macOS/Linux o `C:\Users\TuUsuario` en Windows). Si lo haces ahí, el agente tendrá acceso a **todos los archivos de tu computadora**, lo cual puede ser peligroso.

Crea una carpeta dedicada para tus prácticas (por ejemplo, en el Escritorio) y entra en ella antes de lanzar OpenCode:

=== "macOS / Linux"

    ```bash
    mkdir -p ~/Desktop/OpenCode/01-demo
    cd ~/Desktop/OpenCode/01-demo
    ```

=== "Windows (PowerShell)"

    ```powershell
    mkdir "$HOME\Desktop\OpenCode\01-demo" -Force
    cd "$HOME\Desktop\OpenCode\01-demo"
    ```

---

## Paso 5: Abrir OpenCode por primera vez (el TUI)

Una vez dentro de `01-demo`, ejecuta simplemente:

```bash
opencode
```

Tras unos segundos cargará la **TUI** (*Terminal User Interface*). Aunque esté dentro de la consola, puedes hacer clic con el ratón (por ejemplo, si haces clic en el logo de `opencode`, verás que parpadea).

### Primeros ajustes recomendados en el TUI

1. **Cambiar el tema visual**:
   Escribe `/theme` (o `/themes`) en el cuadro de texto y pulsa ++enter++. En el curso se utiliza **`tokyonight`**, pero puedes elegir el que más te guste.
2. **Ocultar o mostrar la barra lateral derecha**:
   La barra lateral muestra información útil (tokens consumidos, LSP, ruta actual), pero ocupa espacio cuando no la necesitas. Para ocultarla o volverla a mostrar, usa el atajo líder:
   - Pulsa ++ctrl+x++, suelta ambas teclas y luego pulsa ++b++.

---

## Paso 6: Tu primera prueba (`ping`) y el consumo de contexto

Antes de conectar proveedores de pago o locales, OpenCode incluye por defecto acceso a **OpenCode Zen** con modelos gratuitos (por ejemplo, *Big Pickle*).

Escribe en el cuadro de texto:

```text
ping
```

Pulsa ++enter++. El modelo responderá `pong` en un par de segundos.

!!! info "¿Por qué un simple `ping` ya muestra ~9.7K tokens (5%) en la barra inferior?"
    No te asustes al ver que la ventana de contexto marca alrededor de **9.7K tokens (5%)** tras un solo `ping`. Ese espacio no lo ocupa tu palabra `ping`, sino **todo el contexto inicial que OpenCode envía en cada conversación**:

    ```mermaid
    flowchart LR
        subgraph C["Ventana de contexto enviada al modelo"]
            direction TB
            P["Tu mensaje (ping)<br/>+ respuesta (pong)"]
            S["Instrucciones globales<br/>y del sistema"]
            A["Archivo AGENTS.md<br/>(si existe)"]
            M["MCPs, Skills y<br/>configuraciones"]
            P ~~~ S ~~~ A ~~~ M
        end
        C --> T["~9.7K tokens<br/>(5% inicial)"]
    ```

### Buena práctica desde el día 1: limpiar el contexto

Cada vez que termines una tarea y vayas a empezar algo distinto, inicia una **sesión limpia** para que el modelo no arrastre historial innecesario ni pierda precisión:

- Escribe `/new` y pulsa ++enter++, **o**
- Usa el atajo rápido: ++ctrl+x++ (soltar) y luego ++n++.

---

!!! success "Resumen de lo logrado"
    Ya tienes OpenCode instalado, verificado, ejecutándose dentro de una carpeta segura y sabes cómo cambiar el tema, ocultar la barra lateral y reiniciar sesiones limpias. En el siguiente paso conectarás tus **proveedores de modelos**.
