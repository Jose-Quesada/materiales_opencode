---
title: "4. Personalización del TUI (tui.json)"
description: Cómo personalizar la interfaz de terminal de OpenCode con tui.json para activar notificaciones de escritorio y sonidos, más los atajos esenciales del módulo.
---

# 4. Personalización del TUI (`tui.json`)

Cuando OpenCode está trabajando en una tarea larga, es muy habitual cambiar de ventana (por ejemplo, ir al navegador o a tu editor de código). Con **`tui.json`** puedes configurar la interfaz de terminal para que **te avise con una notificación y un sonido sutil** cuando termine o cuando necesite que apruebes un permiso.

---

## Paso 1: Generar el archivo `tui.json` automáticamente

La forma más rápida de que OpenCode cree el archivo `tui.json` en la ruta correcta es cambiar de tema una vez:

1. Dentro de OpenCode, escribe `/themes` y pulsa ++enter++.
2. Elige cualquier tema (y si quieres, vuelve a seleccionar `tokyonight`).
3. Ese simple cambio crea automáticamente el archivo **`tui.json`** en tu carpeta de configuración global (`~/.config/opencode/tui.json`), con un contenido inicial parecido a este:

```json
{
  "$schema": "https://opencode.ai/tui.json",
  "theme": "tokyonight"
}
```

*(Si prefieres aplicarlo solo a un proyecto concreto, puedes crear `tui.json` en la raíz de ese proyecto en lugar de en `~/.config/opencode/`).*

---

## Paso 2: Activar notificaciones y sonidos (`attention`)

Abre `~/.config/opencode/tui.json` en tu editor de código y añade el bloque `"attention"` respetando el formato JSON válido (sin olvidar la coma después de `"theme": "tokyonight"`):

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

### ¿Qué hace cada opción?

| Propiedad | Valor | Función |
| :--- | :--- | :--- |
| `enabled` | `true` | Activa el sistema de avisos del TUI. |
| `notifications` | `true` | Muestra notificaciones del sistema operativo cuando OpenCode requiere atención. |
| `sound` | `true` | Reproduce avisos de audio al terminar una respuesta, ocurrir un error o pedir permiso. |
| `volume` | `0.4` | Volumen del sonido (de `0.0` a `1.0`). |
| `sound_pack` | `"opencode.default"` | Usa el paquete de sonidos sutiles integrado en OpenCode. |

!!! note "¿Por qué quitamos la propiedad `sounds` de la documentación?"
    En la documentación oficial de OpenCode verás un ejemplo con `"sounds": { "error": "./sounds/error.mp3" }`. Si no tienes ese archivo `.mp3` personalizado en tu carpeta, **elimina esa línea** para que use el `sound_pack` por defecto sin errores.

---

## Paso 3: Recargar OpenCode y probar el sonido

1. Guarda `tui.json`.
2. Sal de OpenCode con ++ctrl+c++ y vuelve a entrar con:
   ```bash
   opencode -c
   ```
3. Escribe `ping` y pulsa ++enter++.
4. Al responder `pong`, escucharás un sonido sutil (como unas burbujitas) indicando que OpenCode terminó su turno.

---

## Resumen de atajos esenciales vistos en el Módulo 2

En OpenCode muchos atajos funcionan con la **tecla líder** (++ctrl+x++): pulsas `Ctrl+X`, **sueltas ambas teclas** y luego pulsas la tecla de acción.

| Atajo | Acción |
| :--- | :--- |
| ++ctrl+x++ luego ++n++ | Iniciar una **nueva sesión** limpia (equivale a `/new`). |
| ++ctrl+x++ luego ++b++ | **Mostrar u ocultar** la barra lateral derecha. |
| ++ctrl+x++ luego ++u++ | Deshacer (*Undo*). |
| ++ctrl+x++ luego ++r++ | Rehacer (*Redo*). |
| ++ctrl+p++ | Abrir la **paleta de comandos** rápida. |
| ++ctrl+c++ | Salir de OpenCode (en la terminal). |
| `opencode -c` | Abrir OpenCode **continuando la última sesión**. |

---

## Checklist de verificación del Módulo 2

Antes de pasar al **Módulo 03 (OpenCode 101 - Primeros pasos)**, comprueba que tienes todo listo:

- [ ] `opencode --version` responde correctamente en tu terminal.
- [ ] Trabajas dentro de una carpeta dedicada (`OpenCode/01-demo`) y **no** en la raíz de tu usuario.
- [ ] Tienes al menos un proveedor conectado (`opencode auth login` o `/connect`) y un modelo funcional (`/models`).
- [ ] Creaste `~/.config/opencode/instructions.md` y lo enlazaste en `opencode.jsonc`.
- [ ] Configuraste `~/.config/opencode/tui.json` con tu tema y las alertas de `attention`.
