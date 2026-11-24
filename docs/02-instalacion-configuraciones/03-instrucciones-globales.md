---
title: "3. Instrucciones globales y memoria"
description: Dónde guarda OpenCode su configuración y memoria, cómo configurar opencode.jsonc e instructions.md globales, reiniciar con opencode -c y evitar errores de sintaxis JSON.
---

# 3. Instrucciones globales y memoria

Antes de empezar a programar aplicaciones con OpenCode, conviene darle **instrucciones globales**: reglas que el agente respetará **siempre**, en todos tus proyectos (por ejemplo: responder de forma concisa, escribir código en inglés pero comentarios en español, aplicar código limpio y principios SOLID).

---

## ¿Dónde guarda OpenCode su configuración y memoria?

En el vídeo se le hace directamente esta pregunta a OpenCode:

> *"¿En qué lugares OpenCode guarda su configuración o memoria, ya sea a nivel de proyecto o a nivel global?"*

OpenCode organiza sus archivos en **cuatro niveles**:

=== "1. Global (tu usuario)"

    Se aplica a **todos** tus proyectos:

    | Archivo / Carpeta | Propósito |
    | :--- | :--- |
    | `~/.config/opencode/opencode.json` (o `.jsonc`) | Configuración global (proveedores, instrucciones, herramientas). |
    | `~/.config/opencode/tui.json` | Configuración visual y sonora del TUI. |
    | `~/.config/opencode/AGENTS.md` | Reglas globales para los agentes. |
    | `~/.config/opencode/agents/`, `commands/`, `plugins/`, `skills/`, `themes/`, `tools/` | Extensiones y agentes personalizados globales. |
    | `~/.claude/CLAUDE.md` y `~/.claude/skills/` | Compatibilidad (*fallback*) si vienes de Claude Code. |

    *(En Windows, `~` equivale a tu carpeta de usuario: `C:\Users\TuUsuario\.config\opencode\`).*

=== "2. Proyecto (raíz del repo)"

    Se aplica **solo al proyecto actual** (y suele subirse a Git, salvo credenciales):

    | Archivo / Carpeta | Propósito |
    | :--- | :--- |
    | `opencode.json` (o `.jsonc`) | Configuración del proyecto. |
    | `tui.json` | Configuración del TUI específica del proyecto. |
    | `.opencode/agents/`, `commands/`, `plugins/`, `skills/`, `tools/`, `themes/` | Subdirectorios locales del proyecto. |
    | `AGENTS.md` | Instrucciones y reglas del proyecto. |
    | `CLAUDE.md` | *Fallback* automático si no existe `AGENTS.md`. |

=== "3. Datos / Memoria persistente"

    Archivos internos gestionados por OpenCode:

    - `~/.local/share/opencode/opencode.db` — Base de datos SQLite con sesiones y ajustes.
    - `~/.local/share/opencode/auth.json` — Credenciales y API keys de proveedores.
    - `~/.local/share/opencode/snapshot/` — Instantáneas internas para `/undo` y `/redo`.
    - `~/.local/share/opencode/repos/`, `storage/`, `tool-output/` — Repositorios rastreados y salidas de herramientas.

=== "4. Organizacional (Managed)"

    Para configuraciones gestionadas por empresa/sistema:

    - **macOS**: `/Library/Application Support/opencode/`
    - **Linux**: `/etc/opencode/`
    - **Windows**: `%ProgramData%\opencode`

!!! important "Regla de prioridad: Proyecto > Global"
    Las configuraciones se **fusionan** (no se borran por completo), pero **lo que definas a nivel de proyecto sobrescribe lo global**.
    Por ejemplo, si tu configuración global dice *"responde en español"* y un proyecto específico dice *"responde en inglés"*, en ese proyecto ganará la regla del proyecto.

---

## Paso a paso: Configurar `opencode.jsonc` e `instructions.md` globales

### Paso 1: Abrir la carpeta de configuración global

Abre tu carpeta de configuración global en el explorador de archivos:

=== "macOS"

    1. Abre **Finder**.
    2. Pulsa ++cmd+shift+g++ (*Ir a la carpeta*).
    3. Pega `~/.config/opencode/` y pulsa ++enter++.

=== "Windows"

    1. Abre el **Explorador de archivos** (o pulsa ++win+r++).
    2. Escribe `%USERPROFILE%\.config\opencode` y pulsa ++enter++.
    *(Si la carpeta no existe todavía, créala).*

=== "Linux"

    Navega a `~/.config/opencode/` desde tu gestor de archivos o terminal (`cd ~/.config/opencode`).

---

### Paso 2: Crear o editar `opencode.jsonc`

Dentro de `~/.config/opencode/`, abre `opencode.jsonc` (o `opencode.json`) con tu editor de código y asegúrate de incluir la referencia a `instructions.md`:

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": [
    "instructions.md"
  ]
}
```

!!! tip "¿Para qué sirve `$schema`?"
    La línea `"$schema": "https://opencode.ai/config.json"` permite que editores como VS Code o Cursor validen automáticamente tus opciones de configuración y te muestren autocompletado.

---

### Paso 3: Crear `instructions.md`

En esa misma carpeta (`~/.config/opencode/`), crea el archivo **`instructions.md`** y pega las instrucciones globales del curso (disponibles también en el Gist oficial enlazado en el módulo):

```markdown
Eres un agente de codificación experto en clean code.

- Responde de manera corta y concisa.
- Código en inglés (nombres de variables, funciones, clases, todo).
- Comentarios en español, solo cuando aporten valor.
- Prioriza simplicidad: código fácil de leer, funciones pequeñas, sin duplicación (DRY).
- Sigue principios SOLID donde aplique.
- Sin comentarios superfluos — el código debe ser auto-documentado en lo posible.
- Sin magia: evita expresiones crípticas, prefiere claridad.
- Buenas prácticas de seguridad cuando corresponda.
```

!!! warning "Mantén `instructions.md` corto y al grano"
    **No sobrecargues este archivo** con cientos de líneas. Todo el contenido de `instructions.md` viaja dentro de la ventana de contexto en **cada petición** que haces (por ejemplo, notarás que tu contexto inicial sube de ~9.7K a ~27.9K tokens / 14% con las configuraciones cargadas). Un archivo gigante desperdiciaría tokens y capacidad de razonamiento del modelo.

---

### Paso 4: Reiniciar OpenCode y restaurar la conversación (`opencode -c`)

Para que OpenCode lea los nuevos archivos globales:

1. Sal de OpenCode pulsando ++ctrl+c++ (un par de veces si hace falta).
2. Vuelve a arrancarlo con la bandera `-c` (*continue*):

```bash
opencode -c
```

La bandera **`-c`** abre OpenCode **restaurando automáticamente la última conversación** donde la dejaste. Si no aparece ningún error al iniciar, tu configuración global se cargó correctamente.

---

## Error común: Sintaxis inválida en `opencode.jsonc`

Si cometes un error de formato JSON (por ejemplo, dejar una coma sobrante —*trailing comma*— o olvidar una coma después de `"$schema"`):

```jsonc
{
  "$schema": "https://opencode.ai/config.json"  // <-- falta coma aquí
  "instructions": [
    "instructions.md",  // <-- coma sobrante
  ],                    // <-- coma sobrante
}
```

Al ejecutar `opencode -c`, OpenCode mostrará un mensaje claro indicando la línea y columna exacta del fallo:

```text
Config file at ~/.config/opencode/opencode.jsonc is not valid JSON(C):
--- Errors ---
CommaExpected at line 3, column 3
```

**Solución:** abre `opencode.jsonc`, corrige las comas sobrantes o faltantes hasta que no haya subrayados rojos en tu editor, guarda el archivo y vuelve a ejecutar `opencode -c`.
